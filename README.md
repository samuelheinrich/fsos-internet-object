# fsos-internet-object
CLI Script to create an "Internet Object"

![image](https://user-images.githubusercontent.com/16579232/221982347-637ff6f6-c217-469a-864e-957b59d0ed8c.png)



Worum geht es?
Fortigate bietet eine Vielzahl von Möglichkeiten Traffic Richtung Internet zu routen, Nebst statischen Routen, werden auch dynamische Objekten unterstützt, wie beispielsweise FQDN Objekte oder Internet Services.

![image](https://user-images.githubusercontent.com/16579232/221981630-e8187ade-ea17-431f-913e-54be9d868917.png)

Möchte man „alles“ Richtung „Internet“ routen, greift man in der Regel zu einer Default Route 0.0.0.0/0 bzw.  zu „ANY -> Zone WAN“

Problem?
In der Regel führt das zu keinem Problem, denn bei statischem und Policybased Routing, greift jeweils die am meist spezifischeste Route.

ZB:

10.0.0.0/8 -> 1.1.1.1
10.0.0./24 -> 1.1.1.2    <– Gewinnt gegen 10.0.0.0/8

Leider hat dies im Zusammenhand mit SD-WAN Regeln einen entscheidenen Nachteil, denn hier lassen keine Zonen angeben, somit bedeutet „ANY“ gleichzeitig auch private Subnetze, welche intern geroutet werden sollen. Auch ein Konzept von „mehr spezifisch“ gibt es hier nicht.

![image](https://user-images.githubusercontent.com/16579232/221981909-cfa9b359-d0e0-4f04-8bc4-cd020450ce1a.png)

Besser wäre es in diesem Fall ein reines „Internet Object“ zu haben, welches die lokalen Subnetze nicht beinhaltet.

![image](https://user-images.githubusercontent.com/16579232/221983808-0fd41e1a-e531-4894-aac2-148a966e2430.png)



Wie muss so ein Objekt aussehen?
Ein solches Objekt müsste alle IP Ranges von 0.0.0.0 bis 255.255.255.255 beinhalten, jedoch sämtliche nicht internet relevanten Adressen ausschliessen.

Um welche Ranges geht es hier?
Nebst den Klassischen privaten Address spaces:

Class A: 10.0. 0.0 to 10.255.255.255

Class B: 172.16. 0.0 to 172.31.255.255

Class C: 192.168. 0.0 to 192.168.255.255

Gibt es noch weitere special Adressen, welche nicht routebar sind:

192.0.2.0/24 Assigned as TEST-NET-1, documentation and examples

198.51.100.0/24 Assigned as TEST-NET-2, documentation and examples

203.0.113.0/24 Assigned as TEST-NET-3, documentation and examples

224.0.0.0/4 Private Multicast (Former Class D network.)

Theoretisch würde hier auch noch 127.0.0.0/8 dazu gehören, da diese jedoch aus Natur bereits nicht ausserhalb eines Geräts kommunizieren, sind sie zu vernachlässigen.

Die Schwierigkeit ist nun die Subnetze unter und oberhalb dieser Ranges so zu definieren, dass es routing technisch abgebildet werden kann.


Dieses Repo bietet den Code zum Importieren des Objekts auf der Fortigate.
