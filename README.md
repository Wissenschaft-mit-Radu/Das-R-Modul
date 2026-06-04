# Das-R-Modul

R‑Modul – Bauanleitung


Diese Anleitung beschreibt den Aufbau eines **30 MHz Resonanz-Induktionsmoduls für bis zu 50 W Ausgangsleistung**.

## Hinweis zu Messdaten

Das R‑Modul wurde erfolgreich aufgebaut und getestet. Ausdrücklich verzichte ich auf die Veröffentlichung detaillierter Messdaten, um Zweckentfremdung zu vermeiden. Wer nachbaut, kann eigene Messungen durchführen. Theoretische Grundlagen sind vollständig.

## 1. Theoretische Grundlagen

Klasse-E Oszillator (MOSFET) mit abgestimmter LC‑Resonanzspule (9 Windungen, 10 cm ⌀, 30 MHz). Nahfeldübertragung.

## 2. Werkzeuge & Materialien (BOM)

Werkzeuge: Lötkolben, Multimeter, Oszilloskop (100 MHz), Seitenschneider, Heißklebepistole.

| Pos. | Bauteil               | Wert / Typ                          | Anzahl | Preis (ca.) | Bezugsquelle        |
|------|-----------------------|-------------------------------------|--------|-------------|---------------------|
| 1    | MOSFET                | IRF510                              | 1      | 2–4 €       | Reichelt, Mouser    |
| 2    | Gate-Treiber          | TC4420 (oder diskret BC547/557)     | 1      | 3–8 €       | Reichelt, Mouser    |
| 3    | Resonanzkondensator   | 100 pF, 500 V, NP0/C0G              | 1      | 1–3 €       | Reichelt, Conrad    |
| 4    | Spulenkondensator     | 470 pF, 100 V, Keramik              | 1      | 0,50 €      | Reichelt, Conrad    |
| 5    | Spulenkondensator     | 10 nF, 100 V, Keramik               | 1      | 0,30 €      | Reichelt, Conrad    |
| 6    | Widerstände           | 10, 47, 100, 1k, 10k, 100k Ω       | je 1   | 0,10 €/St.  | Reichelt, Conrad    |
| 7    | Spulenkörper          | 3D‑Druck / Holz / Plexi             | 1      | variabel    | –                   |
| 8    | Kupferlackdraht       | 0,5 mm ∅, 2 m                       | 1      | 1 €         | Conrad, Reichelt    |
| 9    | Kühlkörper            | für IRF510, z. B. SK 104            | 1      | 2–3 €       | Reichelt, Conrad    |
| 10   | Platine               | Lochraster oder selbst geätzt       | 1      | 2–5 €       | Conrad, Reichelt    |
| 11   | Gleichrichterdioden   | 1N4148 (schnell)                    | 2      | 0,30 €/St.  | Reichelt, Mouser    |
| 12   | Elko                  | 100 µF, 63 V                        | 1      | 0,80 €      | Reichelt, Conrad    |
| 13   | Elko                  | 1000 µF, 35 V                       | 1      | 1,20 €      | Reichelt, Conrad    |
| 14   | GND-Brücken           | Draht / Lötbrücken                  | –      | 0,10 €      | –                   |
| 15   | DC‑Buchse             | 5,5 / 2,1 mm Hohlstecker            | 1      | 1,50 €      | Reichelt, Conrad    |

## 3. Schritt‑für‑Schritt

1. **Spule:** 9 Windungen, 10 cm ⌀, 0,5 mm CuL. Enden abisolieren, verzinne.
2. **Oszillator:** Lochraster – Klasse‑E mit IRF510 + Gate‑Treiber. Resonanzkreis: Spule parallel zu 100 pF (C_res). Feinabstimmung mit 470 pF (C_tune). Masseflächen großzügig.
3. **Spule anschließen** – kurze Leitungen.
4. **Kühlung** – IRF510 auf Kühlkörper.
5. **Inbetriebnahme (Low Power):** 12 V, Strombegrenzung 100 mA. Frequenz am Drain mit Oszilloskop prüfen (soll 30 MHz sein). Kondensator oder Windungszahl anpassen.
6. **Leistungsanpassung:** Spannung auf 48 V erhöhen, Last 50 Ω, Wirkungsgrad optimieren (10‑nF-Kondensator variieren).
7. **Endmontage** – isolieren, Gehäuse.

## 4. Schaltplan (textuell)

- +48 V → 1000 µF (+) → Drain IRF510.
- Gate‑Treiber versorgt mit +12 V, Ausgang über 10 Ω → Gate IRF510.
- Source IRF510 → GND.
- Drain → paralleler Schwingkreis: L (9 Wdg) // 100 pF (C_res) // 10 nF (C_load). Der Kreis über 470 pF (C_tune) mit GND verbunden.
- Senderspule am Schwingkreisausgang.

## 5. Messpunkte & erwartete Werte

| Messpunkt                 | Sollwert          | Toleranz |
|---------------------------|-------------------|----------|
| Versorgungsspannung       | 48 V DC           | ±2 V     |
| Ruhestrom (ohne Last)     | < 50 mA           | –        |
| Betriebsstrom (50 W)      | ca. 1,1 A         | ±20 %    |
| Frequenz am Drain         | 30,0 MHz          | ±1 %     |
| Ausgangsspannung (Spule)  | ca. 200 Vpp (LL)  | –        |
| Wirkungsgrad (5 cm)       | > 80 %            | –        |

## 6. Quellcode (Arduino-Dummy – nur externer Takt)

```cpp
void setup() { pinMode(9, OUTPUT); }
void loop() {}

## 7. Sicherheitshinweise

- **Hochspannung (>200 V)** am Drain – Lebensgefahr! Keine Berührung.
- **HF‑Strahlung** – Abstand zu Herzschrittmachern, empfindlicher Elektronik.
- **Heiße Bauteile** – Kühlung sicherstellen.
- **Abstimmung nur mit 12 V** durchführen, erst dann 48 V.

## 8. Optimierungshinweise

- Spulenform, Windungszahl, Durchmesser variieren, um Resonanz exakt zu treffen.
- Verlustarme Kondensatoren (NP0/C0G, Glimmer) verwenden. X7R zerstört Wirkungsgrad.
- Sende‑ und Empfangsspule identisch, Abstand < Durchmesser.
- Bei Dauerbetrieb unter Volllast Lüfter empfehlenswert.