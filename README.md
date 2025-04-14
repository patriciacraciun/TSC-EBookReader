
# README 

## Descriere Generala

Acest proiect reprezinta un sistem integrat cu afisaj E-Paper, conectivitate wireless si mai multi senzori. Totul este construit in jurul microcontrollerului ESP32-C6, care ofera suport pentru WiFi 6 si Bluetooth LE.
Proiectul include atat partea de incarcare si monitorizare a bateriei, cat si stocare externa si masurare a conditiilor de mediu.

---

## Etapele de Implementare

- Am inceput cu realizarea schemei electrice, conectand toate componentele relevante la ESP32-C6.
- Apoi am trecut la layout-ul PCB-ului in 2D, unde am rutat liniile de date si alimentare.
- Am creat planuri de masa (GND plane) atat pe stratul superior, cat si inferior.
- Am folosit reguli custom pentru latimea traseelor de alimentare si distantele minime fata de alte semnale.
- Conectorii au fost pozitionati astfel incat sa fie usor accesibili (ex: Qwiic pentru senzori externi).
- Am adaugat pad-uri de test (TP) pentru debugging mai usor dupa asamblare.
- Am generat modele 3D pentru a verifica compatibilitatea mecanica si pozitionarea componentelor in carcasa.
---

![Screenshot 2025-04-14 141623](https://github.com/user-attachments/assets/b81c3772-2c33-4e30-8e0d-29deec015ea7)

![Screenshot 2025-04-14 141532](https://github.com/user-attachments/assets/a08d443f-f194-4cc3-ad77-3c7d399e2723)

![Screenshot 2025-04-14 141027](https://github.com/user-attachments/assets/a3fc8d4f-1cfa-48fe-b3b7-7fc9faa0895d)

![Screenshot 2025-04-14 140928](https://github.com/user-attachments/assets/cbd895ce-ce49-4e79-9625-77ca70f6cb3e)

![Screenshot 2025-04-14 140846](https://github.com/user-attachments/assets/82f70fa5-8ea2-4f6e-872e-7d7ba9e42f8b)



## Diagrama Bloc

```text
         +------------+
         |  Baterie   |
         +------------+
               |
               v
       +----------------+
       |  Charging IC   |<-- USB-C
       +----------------+
               |
               v
       +----------------+
       |      LDO       | --> 3.3V
       +----------------+
               |
      +--------+--------+--------+--------+--------+
      v        v        v        v        v
   +------+ +--------+ +------+ +------+ +--------+
   | ESP  | |Display | | RTC  | | BME  | | Butoane|
   | -C6  | |(SPI)   | |(I2C) | |(I2C) | |(GPIO)  |
   +------+ +--------+ +------+ +------+ +--------+
               |
               v
           +---------+
           | Flash   |
           |  NOR    |
           | (SPI)   |
           +---------+
               |
               v
           +---------+
           | SD Card |
           |  (SPI)  |
           +---------+
```

---

## BOM (Bill Of Materials)

| Componenta                        | Link Cumparare                                                                                                   | Datasheet                                                                                          
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------
| 112A-TAAR-R03 ATTEND              | https://store.comet.srl.ro/Catalogue/Product/43497/                                                              | https://www.snapeda.com/parts/112A-TAAR-R03/Attend/datasheet/                                    
| BD5229G-TR                        | https://www.digikey.com/en/products/detail/rohm-semiconductor/BD5229G-TR/3663792                                 | https://fscdn.rohm.com/en/products/databook/datasheet/ic/power/voltage_detector/bd52xxg-e.pdf    
| ESP32 WROVER BME680 Sensor        | https://store.comet.srl.ro/Catalogue/Product/50164/                                                              | https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf   
| LED CHG_LED                       | https://ro.mouser.com/ProductDetail/Kingbright/KP-1608SURCK?qs=2JU0tDl2GZ3FuyEWfBV1%2Fg==                        | https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/datasheet/                                 
| Condensator C0402                 | https://ro.mouser.com/c/passive-components/capacitors/ceramic-capacitors/?q=CC0402                               | https://componentsearchengine.com/Datasheets/2/CC0402MRX5R5BB106.pdf                             
| Condensator Tant                  | https://ro.mouser.com/ProductDetail/KYOCERA-AVX/TAJW107M010RNJ?qs=Wtp%252Bf%2FAeVqIH8v1VxV%252B1Rg%3D%3D         | https://ro.mouser.com/datasheet/2/40/TAJ-3165264.pdf                                             
| Q1 DMG2305UX-7                    | https://www.digikey.com/en/products/detail/diodes-incorporated/DMG2305UX-7/4340666                               | https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf                                           
| RTC MODULE DS3231SN#              | https://www.snapeda.com/parts/DS3231SN%23/Analog+Devices/view-part/?ref=eda                                      | https://www.snapeda.com/parts/DS3231SN%23/Analog%20Devices/datasheet/                            
| ESP32 C6 WROOM-1-N8               | https://ro.mouser.com/ProductDetail/Espressif-Systems/ESP32-C6-WROOM-1-N8?qs=8Wlm6%252BaMh8ST02Gmwp74cw%3D%3D    | https://www.snapeda.com/parts/ESP32-C6-WROOM-1-N8/Espressif%20Systems/datasheet/                 
| FH34SRJ-24S-0.5SH(99)             | https://ro.mouser.com/ProductDetail/Hirose-Connector/FH34SRJ-24S-0.5SH99?qs=vcbW%252B4%252BSTIpKBl5ap9J8Fw%3D%3D | https://www.snapeda.com/parts/FH34SRJ-24S-0.5SH(99)/Hirose%20Connector/datasheet/                
| MAX 17048G+T10                    | https://www.snapeda.com/parts/MAX17048G+T10/Analog+Devices/view-part/?ref=eda                                    | https://www.snapeda.com/parts/MAX17048G+T10/Analog%20Devices/datasheet/                          
| MBR0530 Schottky Diode            | https://www.snapeda.com/parts/MBR0530/Onsemi/view-part/?ref=snap                                                 | https://www.snapeda.com/parts/MBR0530/ON%20Semiconductor/datasheet/                              
| MCP73831 Power Management         | https://www.snapeda.com/parts/MCP73831T-2ACI/OT/Microchip/datasheet/                                             | https://www.snapeda.com/parts/MCP73831T-2ACI/OT/Microchip/datasheet/                             
| PFMF.050.1                        | https://ro.mouser.com/ProductDetail/EPCOS-TDK/B72520T0350K062?qs=dEfas%2FXlABIszF52uu7vrg%3D%3D                  | https://www.tdk-electronics.tdk.com/inf/75/db/CTVS_14/Surge_protection_series.pdf                
| PGB1010603MR                      | https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda                                         | https://www.snapeda.com/parts/PGB1010603MR/Littelfuse%20Inc./datasheet/                          
| QWIIC_RIGHT_ANGLE CONNECTOR       | https://ro.mouser.com/ProductDetail/SparkFun/PRT-14417?qs=wd5RIQLrsJhgdz%2FpmZ%2F3GQ==                           | https://ro.mouser.com/datasheet/2/813/Qwiic_Connector_Datasheet-1223982.pdf                      
| Rezistenta R0402                  | https://grabcad.com/library/resistor-0402-1)                                                                     | https://www.yageo.com/upload/media/product/products/datasheet/rchip/PYu-RC_Group_51_RoHS_L_12.pdf
| SD0805S020S1R0                    | https://ro.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D               | https://ro.mouser.com/datasheet/2/40/schottky-3165252.pdf                                        
| Si1308EDL-T1-GE3 MOSFET           | https://www.snapeda.com/parts/SI1308EDL-T1-GE3/Vishay+Siliconix/view-part/?welcome=home&ref=eda                  | https://www.snapeda.com/parts/SI1308EDL-T1-GE3/Vishay%20Siliconix/datasheet/                     
| USB4110-GF-A                      | https://ro.mouser.com/ProductDetail/GCT/USB4110-GF-A?qs=KUoIvG%2F9IlYiZvIXQjyJeA%3D%3D                           | https://ro.mouser.com/datasheet/2/837/GCT_USB4110_Product_Drawing___20k_cycles-3455479.pdf       
| USBLC6-2SC6Y                      | https://ro.mouser.com/ProductDetail/STMicroelectronics/USBLC6-2SC6Y?qs=gNDSiZmRJS%2FOgDexvXkdow%3D%3D            | https://ro.mouser.com/datasheet/2/389/usblc6_2sc6y-1852505.pdf                                   
| W25Q512JVEIQ                      | https://www.snapeda.com/parts/W25Q512JVEIQ/Winbond+Electronics/view-part/?ref=eda)                               | https://www.winbond.com/resource-files/W25Q512JV%20SPI%20RevB%2006252019%20KMS.pdf               
| XC6220A331MR-G                    | https://componentsearchengine.com/part-view/XC6220A331MR-G/Torex                                                 | https://product.torexsemi.com/system/files/series/xc6220.pdf                                     
| 744043680 BOBINA                  | https://ro.mouser.com/ProductDetail/Wurth-Elektronik/744043680?qs=PGXP4M47uW6VkZq%252BkzjrHA%3D%3D               | https://www.we-online.com/components/products/datasheet/744043680.pdf                            

---

## Functionalitate Hardware

Microcontrollerul ESP32-C6 gestioneaza toate functiile sistemului:

- **Display E-Paper**: conectat prin SPI. Este utilizat pentru afisajul principal.
- **BME688**: senzor de temperatura, umiditate, presiune si VOC, conectat prin I2C.
- **RTC DS3231**: modul de timp real, pentru pastrarea orei chiar si cand dispozitivul este oprit.
- **SD Card**: stocare externa, comunicare SPI.
- **Memorie NOR Flash**: pentru date si firmware, interfata SPI.
- **Charging controller (MCP73831)**: incarca bateria Li-Po prin USB-C.
- **Fuel gauge (MAX17048)**: monitorizeaza nivelul de incarcare al bateriei.
- **Qwiic Connector**: pentru senzori modulari, I2C.
- **Buton BOOT si RESET**: conectate la GPIO pentru programare si control.

---

## Alocarea Pinilor ESP32-C6

| Componenta                | Interfata | Pini ESP32-C6        | Observatii                          |
|--------------------------|-----------|----------------------|--------------------------------------|
| Display E-Paper          | SPI       | IO7, IO6, IO10, IO5, IO23, IO3 | MOSI, SCK, CS, DC, RST, BUSY       |
| BME688 + RTC             | I2C       | IO21 (SDA), IO22 (SCL) | Bus comun cu pull-up                |
| MAX17048 Battery Gauge   | I2C       | IO19 (SCL), IO20 (SDA)| Separat pentru alimentare baterie   |
| SD Card                  | SPI       | IO4 (CS) + MOSI/SCK/MISO| Partajate cu Flash/Display         |
| Flash NOR                | SPI       | IO12 (CS)             | Pe acelasi bus SPI                  |
| USB                      | USB       | IO13 (D+), IO12 (D-)  | Conexiune directa la USB-C          |
| Buton Reset              | GPIO      | EN                    | Reset hardware                      |
| Buton Boot               | GPIO      | IO9                   | Intrare manuala in modul bootloader |

---

## Alte Observatii

- Placa are pad-uri de test (TP) pentru debugging usor.
- Conectorul Qwiic permite extinderea cu alti senzori.
- Diodele ESD protejeaza liniile sensibile (USB, SPI, alimentare).
- Planurile de masa au fost implementate pe ambele straturi pentru stabilitate si protectie EMI.
- Rutarea a fost facuta manual, tinand cont de distantele minime si traseele de putere.
- PCB-ul este compact si poate fi reprodus usor in mai multe exemplare.
- Designul este compatibil cu carcase printate 3D.

