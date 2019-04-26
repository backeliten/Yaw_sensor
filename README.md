# Modell

1K0 097 655 D

ESP-DUOSENSOR

# Priser

Varierar friskt ifrån typ 200kr till upp till 3000kr.
Varav de sista är priser i sverige, lustigt nog tror man den är gjort av guld, men så är ju inte fallet.


# Pinout

1 - 12V 
2 - CAN L
3 - GND
4 - CAN H

Förslagsvis 12V, GND och CAN H, CAN L

# Kontakt
Lista ut vad det är för kontakt på den.

Det är samma serie som används på bredbandslambda fast mindre variant och enbart 4 pinnar.

# Uppbyggnad

- 1-axis Accelerometer
SCA610-C28H1A 

https://www.is-line.de/fileadmin/user_upload/is-line/sensorik/hersteller/datasheets/mu_sca610-e28h1a_datasheet.pdf

- CPU
Troligen en TMS320 

Märkt med TMS980 C122B117PZ
Övriga nummer: 1061413661 0
CB-58A1JKW

Ate

100-LQFP

tms320f28069??

- Regulator

0-9385.1D

Kan inte hitta något direkt info om den.
Men mest troligen en regulator som kan sätta olika state.
Ser ut att vara rätt advancerad för den design den sitter i.
Så den måste ha en egenskap som den efterfrågar som bara finns i den kapslingen.
Alternativt flera spänningar.

Får lägga på 12V och se vilka spänningar som dyker upp.

 - Gyro
 
Svårt att avgöra exakt vilket 


# Funktion

Har provat att lägga på spänning på enheten, provade först 5 volt och såg en svag ström.

Ökade till 7volt, vilket är mini spec för 12V system. Mycket riktigt surrade den igång. man kan höra ett svagt bakgrundssurr ifrån oscilatorn i gyrot. Vilket verkar ligga på runt 16khz, klassiskt TV tjut! :)

Provade att lägga proben på en av de förmodade CAN utgångarna, och mycket riktigt så fanns där datatrafik. 
Kollade baudrate och mycket riktigt så låg den på 500k, passar perfekt för CAN.
Så om nu andra kanalen är inverterad mot den första så är vi rätt säker på att det är CAN. 

Yes, de är helt klart inverterade vilket leder oss till CAN.

Koppla upp en CAN adapter och
Som gissat så var det en CAN device som skickar kontinuerlig data med nuvarande status värden på accel och gyro.

Har ännu inte kodat ner datat, men som man kan se så har jag hittat accelerometern.

Den skickar meddelande på 4 st CAN adresser:
130
131
132
133


130 - byte  5-7 är accelerometer, signed value. Så gissningsvis 

133 är oklart, men skickas några gånger under uppstart, för att sedan inte skickas nå mera.

131
132 är ännu inte tolkade, men om man tittar på mängden paket så verkar 130 vara själva realtidsdata övriga är statusflaggor eller beräknade värden. Kan vara typ hastighet kanske?

Att bedöma på rate:n på hur fort meddelanden skickas så är den mest troligen direkt kopplad till ESP enheten utan andra enheter på samma buss då den formligen floodar meddelande på CAN bussen.  Så man bör nog ha en isolerad CAN bus för den.



![20190423_214101.jpg](/pictures/20190423_214101.jpg)

