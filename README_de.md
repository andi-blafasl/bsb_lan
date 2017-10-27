# BSB-LPB-LAN

English version can be found <A HREF="https://github.com/fredlcore/bsb_lan/blob/master/README.md">here</A>. 

LAN-Adapter für den Boiler-System-Bus (BSB) und den Local Process Bus (LPB) - entwickelt für einen Arduino Mega 2560 mit Ethernet-Shield zur web-basierten Kontrolle über Elco Thision, Brötje und baugleiche Heizungssysteme, einschließlich Logging-Funktion auf Micro-SD-Karte.

Prinzipiell sollte diese Kombination aus Hard- und Software mit allen Heizungssystemen lauffähig sein, deren Steuerung über einen BSB oder LPB verfügt. Unter realen Umständen erwies sich die Kombination auf folgenden Systemen als lauffähig:
 - Brötje ISR-SSR 
 - Brötje ISR-ZR1 
 - Brötje NovoCondens BOB 20 (Ölbrenner)
 - Brötje NovoCondens SOB 22C (Ölbrenner)
 - Brötje NovoCondens SOB 26C [ISR + RVS43.222/100] (Ölbrenner)
 - Brötje SensoTherm BSW-K (Wärmepumpe)
 - Brötje WBS 22E (Gasbrenner)
 - Brötje WGB-U 15H (Gasbrenner)
 - Brötje WGB 15 E [LMS14] (Gasbrenner)
 - Brötje WGB S 17/20 E EcoTherm Plus (Gasbrenner)
 - Brötje WGB EVO 20H (Gasbrenner)
 - Brötje WGB Pro EVO 20C [LMU75] (Gasbrenner)
 - Elco Straton (Ölbrenner)
 - Elco Straton 21 [LOGON B G2Z2 + RVS63.283/160] (Ölbrenner)
 - Elco Thision S 17.1 [LMU74.100A136 + AVS37.394/136] (Gasbrenner)
 - Elco Thision S 25.1 [LOGON B G2Z2 (RSV63.283/360) + LOGON B MM (AVS75.390/260)] (Gasbrenner)
 - Fujitsu Waterstage (WSYK160DC9, RVS21.827/127) (Wärmepumpe)

Bitte beachten: Generelle Parameter sollten bei allen Systemen funktionieren, spezifischere Parameter werden sukzessiv hinzugefügt. Deine Hilfe könnte gebraucht werden, wenn du ein Heizungssystem hast, welches mit dem BSB oder dem LPB arbeitet, wofür wir jedoch noch keine spezifischeren Parameter hinzugefügt haben. Erfahre <A HREF="https://github.com/fredlcore/bsb_lan/blob/master/FAQ_de.md#mein-heizungssystem-verf%C3%BCgt-%C3%BCber-parameter-die-von-der-software-bisher-nicht-unterst%C3%BCtzt-werden-kann-ich-behilflich-sein-diese-parameter-hinzuzuf%C3%BCgen">hier</A>, wie du uns helfen kannst.

Instruktionen, wie die Software konfiguriert werden muss etc. können dem <A HREF="https://github.com/fredlcore/bsb_lan/blob/master/HOWTO_de.md">HowTo</A> entnommen werden.<BR>
Die Forendiskussion die zum Bau dieses Adapters geführt hat, kann <A HREF="https://forum.fhem.de/index.php?topic=29762.new;topicseen#new">hier</A> nachvollzogen werden.<BR>

Einige Adapter-Bausätze, die direkt auf einen Arduino Mega2560 passen, sind noch von einer Sammelbestellung her verfügbar. Bei Interesse kontaktiere bitte bsb (ät) code-it.de .

Web-Interface screenshots:
<img src="https://github.com/fredlcore/bsb_lan/blob/master/schematics/Web-Interface.png" size="50%">
<img src="https://github.com/fredlcore/bsb_lan/blob/master/schematics/Web-Interface2.png" size="50%">

BSB-Adapterplatine unbestückt, mit den Anschlüssen CL+ und CL-:
<img src="https://github.com/fredlcore/bsb_lan/blob/master/schematics/BSB-Board%20plain.jpg" size="50%">

BSB-Adapterplatine bestückt:
<img src="https://github.com/fredlcore/bsb_lan/blob/master/schematics/BSB-Board.jpg" size="50%">

BSB-Adapterplatine und Ethernet-Shield auf dem Arduino Mega2560:
<img src="https://github.com/fredlcore/bsb_lan/blob/master/schematics/BSB-Board%20on%20Arduino%20Mega%202560.jpg" size="50%">
