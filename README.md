# PROIECT-TSC-2026

# InkTime Smartwatch PCB

## Descriere generala

Proiectul consta in realizarea unui PCB pentru un smartwatch open-source numit InkTime, bazat pe microcontroller-ul nRF52840. Scopul a fost integrarea tuturor componentelor necesare (procesare, senzori, alimentare, interfata utilizator) pe o placa compacta, care sa respecte atat constrangerile electrice cat si cele mecanice impuse de carcasa.

Design-ul a fost realizat in Fusion 360 Electronics, urmand schema electrica oferita si regulile de layout discutate la laborator. S-a pus accent pe rutare corecta, planuri de masa si integrarea componentelor intr-un spatiu limitat.

---

## Diagrama bloc

Componente principale ale sistemului:
- nRF52840 (microcontroller principal, BLE)
- IMU (accelerometru + giroscop)
- Display E-Paper
- PMIC (power management IC)
- Baterie Li-Po
- USB-C pentru incarcare si alimentare
- Motor haptic (feedback vibratii)

Conectari intre componente:
- IMU conectat la MCU prin I2C (SDA, SCL)
- Display conectat prin SPI (MOSI, SCK, CS, DC, RST)
- PMIC gestioneaza alimentarea intregului sistem
- USB-C conectat la circuitul de incarcare
- Motorul haptic este controlat prin GPIO

---

## BOM (Bill of Materials)

| Componenta | Tip | Link | Datasheet |
|-----------|-----|------|----------|
| nRF52840 | MCU | JLC | Nordic datasheet |
| IMU | Senzor | JLC | Datasheet |
| E-Paper | Display | JLC | Datasheet |
| PMIC | Power | JLC | Datasheet |
| USB-C Connector | Conector | JLC | Datasheet |
| Condensatori | Pasivi | JLC | Datasheet |
| Rezistente | Pasivi | JLC | Datasheet |

BOM-ul contine principalele componente utilizate in proiect. Majoritatea componentelor au fost alese din libraria JLC pentru a fi compatibile cu procesul de fabricatie.

---

## Functionalitate hardware

Sistemul este bazat pe nRF52840, care gestioneaza toate perifericele si comunicatiile. Comunicatia cu senzorii si display-ul se face prin interfete standard:

- SPI pentru display (viteza mai mare)
- I2C pentru IMU (simplu si eficient)

Alimentarea sistemului se face de la o baterie Li-Po, printr-un circuit PMIC care asigura:
- stabilizarea tensiunii
- protectie la incarcare/descarcare
- distributia alimentarii catre componente

USB-C este utilizat pentru:
- incarcare baterie
- alimentare directa

Motorul haptic este folosit pentru feedback si este controlat printr-un pin GPIO.

Consum estimativ:
- MCU: consum redus in sleep, mai mare in BLE activ
- Display E-Paper: consum mic (update rar)
- IMU: consum redus pe I2C

---

## Pini folositi (nRF52840)

Interfata SPI:
- MOSI
- SCK
- CS (chip select)
- DC (data/command)
- RST (reset display)

Interfata I2C:
- SDA
- SCL

GPIO-uri:
- Haptic enable
- IMU interrupt
- PMIC interrupt
- Alte semnale de control

Alegerea pinilor a fost facuta astfel incat:
- sa permita rutare cat mai simpla
- sa evite intersectiile inutile
- sa respecte functionalitatea perifericelor

---

## Layout PCB

PCB-ul a fost proiectat pe 2 layere:
- Top layer pentru majoritatea componentelor si rutarii
- Bottom layer pentru rutare suplimentara si plan de masa

Au fost aplicate urmatoarele reguli:
- trasee de putere mai groase (ex: 3V3, VBUS)
- trasee de semnal min. 0.15 mm
- evitarea unghiurilor de 90 de grade
- plasarea condensatorilor de decuplare cat mai aproape de pini

Planuri de masa:
- polygon GND pe TOP
- polygon GND pe BOTTOM
- conectate prin via stitching

Antena:
- zona de sub antena fara cupru
- fara rutare sub antena
- keepout aplicat corect

---

## Probleme intampinate

Pe parcursul design-ului au aparut mai multe probleme:
- DRC errors (overlap, clearance)
- via-uri prea apropiate de pad-uri
- lipsa planuri de masa initial
- rutare dificila in zona BGA

Solutii:
- refacerea rutarii in zonele critice
- utilizarea via-urilor pentru escape routing
- adaugarea polygon-urilor de masa
- eliminarea masei sub antena
- aplicarea via stitching

Unele exceptii au ramas in zone foarte aglomerate, unde nu exista alta solutie viabila fara a schimba layout-ul complet.

---

## Mechanical

Modelul 3D include:
- PCB
- baterie Li-Po
- display
- carcasa

Dimensiunile au fost respectate conform datasheet-urilor. Bateria a fost modelata aproximativ (5.2 mm grosime) pentru a permite integrarea in carcasa.

---

## Imagini

- randari PCB 2D
- randari PCB 3D
- assembly complet (PCB + carcasa)

---

---

## Decizii de design si compromisuri

Pe parcursul realizarii PCB-ului au fost luate cateva decizii considerate acceptabile in contextul spatiului limitat si al complexitatii design-ului:

- Au fost acceptate cateva zone unde via-urile sunt foarte apropiate de pad-uri SMD, deoarece nu exista suficient spatiu pentru un escape routing clasic fara a redesena complet layout-ul.

- In anumite zone, traseele sunt foarte apropiate unele de altele (aproape de limita DRC), dar raman in tolerantele permise de regulile de fabricatie.

- Unele rutari au fost realizate folosind via-uri direct din pad (via-in-pad partial), pentru a permite scoaterea semnalelor din zone aglomerate (ex: zona MCU / pini multipli).

- Via stitching nu a fost aplicat uniform pe toata placa, ci doar in zonele relevante (in special in jurul planurilor de masa), pentru a evita aglomerarea inutila.

- In zona antenei a fost eliminat complet planul de masa si orice traseu, chiar daca acest lucru a complicat rutarea, pentru a respecta regulile RF.

- Dimensiunile unor componente (ex: baterie in model 3D) au fost ajustate usor pentru a permite integrarea in carcasa.

- Unele trasee par la 90 de grade in anumite vizualizari, dar in realitate sunt rutate la 45 de grade conform regulilor de design.

Aceste decizii au fost luate pentru a putea finaliza design-ul fara a compromite functionalitatea generala a sistemului.

## Concluzie

Design-ul final este functional si respecta majoritatea regulilor de proiectare PCB. Au fost corectate problemele identificate in review si s-au facut ajustari pentru a imbunatati layout-ul.

Chiar daca au existat limitari de spatiu, solutia finala este una utilizabila si pregatita pentru fabricatie.