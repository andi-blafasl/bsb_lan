BSB Boiler-System-Bus LAN Interface

ATTENTION:  
      There is no waranty that this system will not damage your heating system!

Author: Gero Schumacher (gero.schumacher@gmail.com)  
      Based on the code and work from many other developers (see Info section below). Many thanks!

License:
      You are free to use this software on your own risk. Please take care of the licenses of the used libraries and software.

Host System:  
The software is designed to run on an arduino mega2560 board with ethernet shield.
Because there are different pin assignments for different ethernet shields, you
may have to connect the BSB adapter to different pins and to change the pin assigment
in the software.  
The software is tested with the following components:        
- SainSmart MEGA2560 R3 Development Board  
- SainSmart Ethernet Schild für Arduino UNO MEGA Duemilanove Neu Version W5100  
- BSB-Interface (see BSB_adapter.pdf)  
For this configuration, pin A14 (68) is used as RX and pin A15 (69) is used as TX (see setting parameters below).
      
Target System:  
      Tested with various Elco and Brötje heating systems (see README).
      Communication should be possible with all systems that support the BSB interface. 

Getting started:
* Connect the CL+ and CL- connectors of the interface to the corresponding port of your heating system (look out for port names like BSB, FB, CL+/CL-, remote control).
* Download and install the most recent version of the Arduino IDE from https://www.arduino.cc/en/Main/Software (Windows, Mac and Linux are available).
* <del>Copy the contents of the BSB_lan libraries folder into your local Arduino libraries folder (My Documents\Arduino\libraries\ on Windows, ~/Documents/Arduino/libraries on Mac).</del> No longer necessary from version 0.34 onwards.
* Open the BSB_lan sketch by double-clicking on the BSB_lan.ino file in the BSB_lan folder. The corresponding BSB_lan_config.h and BSB_lan_defs.h files will be automatically loaded as well.
* Configure the IP-address in BSB_lan_config.h according to your network (default 192.168.178.88 will work with standard Fritz!Box routers, but check for address collision).
* Select "Arduino/Genuino Mega or Mega 2560" under Tools/Board.
* Select "ATmega 2560" under Tools/Processor.
* Select "AVRISP mkII" under Tools/Programmer.
* Select the corresponding USB/serial port under Tools/Port.
* Upload the sketch to the Arduino under Sketch/Upload.
* Open `http://<ip address from config>/` (or `http://<ip address from config>/<passkey>/` when using the passkey function, see below) to see if everything was compiled and uploaded correctly. A simple web-interface should appear.

Optionally configure the following parameters in BSB_lan_config.h:  
- MAC address of your ethernet shield. It can be normally found on a label at the shield:  
  `byte mac[] = {0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xEA };`  
- You only need to change the default IP- and MAC-address when using more than one interface in the same network.
- Ethernet port  
  `EthernetServer server(80);`  
- Pin assigment of the BSB adapter  
  `BSB bus(68,69);`  
- Activate the usage of the passkey functionality (see below)  
  `#define PASSKEY  "1234"`  
- BSB address (default is 0x06=RGT1, but can be overwritten in the bus initialization)  
  `BSB bus(68,69,<my_addr>);`
  If you already have an RGT1 installed, you can type in the following to address the adapter as RGT2: `BSB bus(68,69,7);`
- You can restrict access to the adapter to read-only, so that you can not set or change certain parameters of the heater itself by accessing it via the adapter. To achieve this, you have to set the flag in the concerning line (#define DEFAULT_FLAG 0) to FL_RONLY:  
  `#define DEFAULT_FLAG FL_RONLY;`
- You can set the language of the webinterface of the adapter to english by deactivating the concerning definement:
  `//#define LANG_DE;`
        
Web-Interface:  
      A simple website is displayed when the server is accessed by its simple URL 
      without any parameters.  
      e.g. `http://<ip-of-server>`  
      To protect the system for unwanted access you can enable the passkey feature (very simple and not really secure!).  
      If the passkey feature is enabled (see below), the URL has to contain the defined passkey as first element
      e.g. `http://<ip-of-server>/<passkey>/`    - to see the help. Don't forget the trailing slash after the passkey! 
      The urls in the below examples have to be exented, if the passeky feature is turned on.  

      In addition to the web-interface, all functions can also be directly accessed via URL commands, this is especially useful when 
      using the device in home automation systems such as FHEM.
      
      All heating system parameters are accessed by line numbers. A nearly complete description can be found in systemhandbuch_isr.pdf.  
      Some lines are 'virtual', i.e. they were added to simplify the access to complex settings like time programms.  
      The parameters are grouped in categories according to the submenu items when accessing your boiler system from the display.  

      List all categories:
      http://<ip-of-server>/K
        This command does not communicate with the boiler system. It is a software internal feature.

      List all enum values for parameter x
      http://<ip-of-server>/E<x>
        This command does not communicate with the boiler system. It is a software internal feature.
        The command is only available for parameters of the type VT_ENUM.

      Query all values from category x: 
        http://<ip-of-server>/K<x>

      Query value for line x
        http://<ip-of-server>/<x>

      Query value for a range of lines (from line x up to line y)
        http://<ip-of-server>/<x>-<y>

      Multiple queries can be concatenated
        e.g. http://<ip-of-server>/K11/8000/8003/8005/8300/8301/8730-8732/8820

      Query for the reset value of parameter x
        http://<ip-of-server>/R<x>
        In the display there is a reset option for some parameters. A reset is performed by asking the boiler for the 
        reset value and setting it afterwards.

      Set value v for parameter x
        http://<ip-of-server>/S<x>=<v>
        Attention: This feature is not extensively tested. So be careful what are you doing and do it on your own 
        risk. The format of the value depends on its type. Some parameters can be disabled. 
        To set a parameter to 'disable', just send an empty value
        http://<ip-of-server>/S<x>=
        The description of the value formats will be added here. Until then have a look at the source code (function
        set).
         
      Send INF message for parameter x with value v
        http://<ip-of-server>/I<x>=<v>
        Some values cannot be set directly. The heating system is informed by a TYPE_INF message, e.g. the room temp:
        http://<ip-of-server>/I10000=19.5  // room temperature is 19.5 degree.

      Set verbosity level n
        http://<ip-of-server>/V<n>
        The default verbosity level is 0. When setting it to 1 the bus is monitored and all data is additionally 
        printed in raw hex format.
        The verbose output affects both the serial console of the mega2560 as well as (optional) logging bus data to SD card, 
        so this can fill up your card pretty fast! The html output is kept unchanged.

      Activate bus monitor
        http://<ip-of-server>/M<n>
        When setting it to 1 all bytes on the bus monitored. Telegrams are recognized by a character break 
        condition. Every Telegramm is printed in hex format to serial output with a timestamp in milliseconds. 
        The monitor output only affects the serial console of the mega2560. The html output is kept unchanged.
        
      Set/query GPIO pins
        http://<ip-of-server>/G<xx>[=<y>]
        Returns the current status of GPIO pin xx (0 or 1). Can be used to set the pin to 0 (LOW) or 1 (HIGH). 
        Reserved pins which are not allowed to be written can be defined in BSB_lan_config.h in variable GPIO_exclude.
      
      Show 24h averages of selected parameters
        http://<ip-of-server>/A[=parameter1,...,parameter20]
        Initially define parameters you want to generate rolling 24h averages from in BSB_lan_config.h in 
        variable avg_parameters.
        During runtime, you can use "/A=[parameter1],...,[parameter20]" to set (up to 20) new parameters.
      
      Query values of ds18b20 temperature sensors
        http://<ip-of-server>/T
        Returns temperature of optionally connected ds18b20 temperature sensors.
        
      Query values of DHT22 temperature sensors
        http://<ip-of-server>/H
        Returns temperature of optionally connected DHT22 temperature/humidity sensors.
      
      Accumulated duration of burner
        http://<ip-of-server>/B
        Query accumulated duration of burner on status (in seconds) captured from broadcast messages. Use /B0 to reset.

      Activate/deactivate logging to microSD-card
      In general, the activation/deactivation of the logging founds place by the definement in the BSB_lan_config.h-file. If the function is active, you can deactivate the function during runtime by using the following parameters:
        http://<ip-of-server>/L=0,0
        For activation, you can just set a new interval and the desired parameters (see configure log file).
        
      Configure log file
        http://<ip-of-server>/L=<x>[,<parameter1>,<...>,<parameter20>]
        Set logging interval to x seconds and (optionally) set logging parameters to [parameter1], [parameter2] etc. 
        during runtime. Logging parameters needs to be activated by uncommenting the LOGGER directive 
        in BSB_lan_config.h and can be configured initially with variables log_parameters and log_interval.
        
      Configure logging of bus telegrams
        http://<ip-of-server>/LU=<x>
        When logging bus telegrams (logging parameter 30000), log only unknown command IDs (x=1) or all (x=0) telegrams.
        http://<ip-of-server>/LB=<x>
        When logging bus telegrams (logging parameter 30000), log only broadcast (x=1) or all (x=0) telegrams.

      Display log file
        http://<ip-of-server>/D
        Shows the content of datalog.txt file on the Ethernet shield's micro SD card slot. 
        Use /D0 to reset datalog.txt including writing a proper CSV file header 
        (recommended on first use before logging starts).

      Reset Arduino
        http://<ip-of-server>/X
        Resets the Arduino after pausing for 8 seconds (#define RESET in BSB_lan_config.h).

Open issues
- Add more command ids to the table.
          Only the known command ids from the threads listed above and the tested boiler system (ELCO) are contents of the table.
          Any user with a different boiler system can set the verbosity to 1 and decode the missing command ids simply by accessing the 
          sytem via
          the display.
          Cause we want to provide a general working system for all boiler configurations working with BSB. Any help and feedback is 
          appreciated!
          
- Test and complete the set funcionality
          With the current implementation a lot of values can be already set. But there is still some testing needed and some parameter 
          types have to be added.

- Introduce valid ranges for parameters
          To make the access safer when setting values for parameters, the valid ranges should be added to the command table

- Test and maybe extend the system to work with LPB instead of BSB.

- Decode DE telegrams. Maybe they contain some status information and we can use them without querying.

- Add support of error messages sent by the boiler system

Further Info:  
      http://www.mikrocontroller.net/topic/218643  
      http://blog.dest-unreach.be/2012/12/14/reverse-engineering-the-elco-heating-protocol  
      http://forum.fhem.de/index.php/topic,29762.0.html  
      systemhandbuch_isr.pdf
