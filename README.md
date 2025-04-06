
# README - OpenBook - Proiect cu ESP32-C6 si E-Paper

## Descriere Generala

Acest proiect a fost realizat ca parte dintr-un curs practic de microprocesoare si reprezinta un sistem integrat cu afisaj E-Paper, conectivitate wireless si mai multi senzori. Totul este construit in jurul microcontrollerului ESP32-C6, care ofera suport pentru WiFi 6 si Bluetooth LE.

Am urmarit sa integrez mai multe componente utile intr-un design cat mai compact, usor de extins si eficient energetic. Proiectul include atat partea de incarcare si monitorizare a bateriei, cat si stocare externa si masurare a conditiilor de mediu.

---

## Etapele de Implementare

- Am inceput cu realizarea schemei electrice in EAGLE, conectand toate componentele relevante la ESP32-C6.
- Apoi am trecut la layout-ul PCB-ului in 2D, unde am acordat atentie speciala rutarii liniilor de date si alimentare.
- Am creat planuri de masa (GND plane) atat pe stratul superior, cat si inferior pentru stabilitate electrica si protectie EMI.
- Am folosit reguli custom pentru latimea traseelor de alimentare si distantele minime fata de alte semnale.
- Conectorii au fost pozitionati astfel incat sa fie usor accesibili (ex: Qwiic pentru senzori externi).
- Am adaugat pad-uri de test (TP) pentru debugging mai usor dupa asamblare.
- Am generat modele 3D pentru a verifica compatibilitatea mecanica si pozitionarea componentelor in carcasa.

---

## BOM (Bill Of Materials)

ESP32-C6-WROOM-1-N8 - https://ro.mouser.com/ProductDetail/Espressif-Systems/ESP32-C6-WROOM-1-N8  
Datasheet - https://www.espressif.com/sites/default/files/documentation/esp32-c6_datasheet_en.pdf

SD Card Holder 112A-TAAR-R03 - https://store.comet.srl.ro/Catalogue/Product/43497  
Datasheet - https://www.snapeda.com/parts/112A-TAAR-R03/Attend/datasheet/

BME688 Sensor - https://store.comet.srl.ro/Catalogue/Product/50164  
Datasheet - https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf

RTC DS3231SN - https://www.snapeda.com/parts/DS3231SN%23/Analog+Devices/view-part/?ref=eda  
Datasheet - https://www.maximintegrated.com/en/products/digital/real-time-clocks/DS3231.html

MCP73831 Charging Controller - https://www.snapeda.com/parts/MCP73831T-2ACI%2FOT/Microchip/datasheet/  
Datasheet - https://ww1.microchip.com/downloads/en/DeviceDoc/20001984g.pdf

MAX17048G+T10 Battery Gauge - https://www.snapeda.com/parts/MAX17048G+T10/Analog+Devices/view-part/  
Datasheet - https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17048-MAX17049.pdf

W25Q512JVEIQ 64MB Flash - https://www.snapeda.com/parts/W25Q512JVEIQ/Winbond+Electronics/view-part/  
Datasheet - https://www.winbond.com/resource-files/W25Q512JV%20SPI%20RevB%2006252019%20KMS.pdf

USBLC6-2SC6Y TVS - https://ro.mouser.com/ProductDetail/STMicroelectronics/USBLC6-2SC6Y  
Datasheet - https://www.st.com/resource/en/datasheet/usblc6-2sc6y.pdf

---

## Functionalitate Hardware

ESP32-C6 este creierul sistemului si controleaza toate celelalte module. El comunica cu:

- **Display-ul E-Paper** prin SPI (pini: IO7, IO6, IO10, IO5, IO23, IO3)
- **Cardul SD** pentru stocare externa (SPI, pin CS: IO4)
- **Memoria externa Flash NOR** (SPI, CS: IO12)
- **RTC-ul DS3231** si **senzorul BME688** prin I2C (SDA: IO21, SCL: IO22)
- **Monitorul de baterie MAX17048** prin I2C (SDA: IO19, SCL: IO20)

Am folosit un conector USB-C pentru alimentare si programare, protejat cu diode ESD. Tensiunea de 5V este coborata la 3.3V cu un LDO stabil (XC6220A331MR-G). Pentru incarcare am folosit un controller MCP73831, iar pentru estimarea starii bateriei - MAX17048, care trimite datele prin I2C.

Display-ul este controlat de un circuit dedicat si are o sursa separata de alimentare, cu MOSFET-uri care il decupleaza cand nu e folosit, ca sa economiseasca energie.

---

## Organizare si Design

Am incercat sa pastrez un design cat mai ordonat:
- toate traseele SPI sunt protejate cu diode TVS
- planul de masa continua asigura stabilitate si reduce zgomotul
- componentele sunt grupate logic: senzorii, zona de alimentare, zona de afisaj etc.
- pad-urile de test sunt etichetate si puse la marginea placii
- conectorul Qwiic ofera posibilitatea extinderii cu senzori noi, fara lipire

---

## ℹObservatii Finale

Proiectul este gandit modular, astfel incat sa poata fi adaptat usor pentru alte aplicatii (ex: afisaj meteo, cititor e-book, dashboard smart home etc). PCB-ul este pregatit pentru productia in serie mica si se poate integra usor intr-o carcasa printata 3D.


## Diagrama Bloc 

                    +------------------+
                    |     Baterie      |
                    +--------+---------+
                             |
                             v
                    +------------------+
                    |  Charging IC     |<--- USB-C
                    +--------+---------+
                             |
                             v
                    +------------------+
                    |       LDO        | --> 3.3V
                    +--------+---------+
                             |
      +----------+----------+-------------+-----------+-------------+
      |          |                        |           |             |
      v          v                        v           v             v
+----------+ +----------+        +----------------+ +----------+ +----------+
|  ESP32   | |  E-Paper |<------>|   BME688       | |   RTC    | |  Butoane |
|   -C6    | |  Display | (SPI)  | (I2C)           | (I2C)      | (Reset/IO)|
+----------+ +----------+        +----------------+ +----------+ +----------+
      |          ^
      |          |
      |          |
      |    +-----+----------+
      |    |    Flash NOR   |
      |    |     (SPI)      |
      |    +----------------+
      |
+----------------+
|    SD Card     |
|     (SPI)      |
+----------------+
