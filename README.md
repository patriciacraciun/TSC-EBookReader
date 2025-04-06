
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

## 🛠️ Diagrama Bloc

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

| Componenta                 | Link Cumparare                                                                                      | Datasheet                                                                                      |
|---------------------------|-----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| ESP32-C6-WROOM-1-N8       | https://ro.mouser.com/ProductDetail/Espressif-Systems/ESP32-C6-WROOM-1-N8                          | https://www.espressif.com/sites/default/files/documentation/esp32-c6_datasheet_en.pdf         |
| SD Card Holder 112A-TAAR  | https://store.comet.srl.ro/Catalogue/Product/43497                                                 | https://www.snapeda.com/parts/112A-TAAR-R03/Attend/datasheet/                                 |
| BME688 Sensor             | https://store.comet.srl.ro/Catalogue/Product/50164                                                 | https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf|
| RTC DS3231SN              | https://www.snapeda.com/parts/DS3231SN%23/Analog+Devices/view-part/?ref=eda                        | https://www.maximintegrated.com/en/products/digital/real-time-clocks/DS3231.html              |
| MCP73831 Charging IC      | https://www.snapeda.com/parts/MCP73831T-2ACI%2FOT/Microchip/datasheet/                             | https://ww1.microchip.com/downloads/en/DeviceDoc/20001984g.pdf                                |
| MAX17048G+T10 Fuel Gauge  | https://www.snapeda.com/parts/MAX17048G+T10/Analog+Devices/view-part/                              | https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17048-MAX17049.pdf     |
| W25Q512JVEIQ Flash        | https://www.snapeda.com/parts/W25Q512JVEIQ/Winbond+Electronics/view-part/                          | https://www.winbond.com/resource-files/W25Q512JV%20SPI%20RevB%2006252019%20KMS.pdf            |
| USBLC6-2SC6Y ESD Protect  | https://ro.mouser.com/ProductDetail/STMicroelectronics/USBLC6-2SC6Y                                | https://www.st.com/resource/en/datasheet/usblc6-2sc6y.pdf                                     |

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

