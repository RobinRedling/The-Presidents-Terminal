# The President's Terminal - *Eat or Be Eaten*

Ein textbasiertes Terminal-Strategiespiel, entwickelt als mein erstes größeres Python-Projekt zum Lernen der Programmierung.

## Über das Projekt

The President's Terminal ist ein Idle/Clicker-Strategiespiel, das komplett im Terminal läuft. Als Präsident baust du deine Armee auf, eroberst Regionen und verwaltest deine Ressourcen. Das Spiel kombiniert Echtzeit-Mechaniken mit strategischen Entscheidungen.

Dieses Projekt ist während meiner ersten Schritte in der Programmierung entstanden. Ich habe Python von Grund auf gelernt und dabei mit KI-Tools experimentiert, um Konzepte zu verstehen und Best Practices kennenzulernen.

## Features

- **Echtzeit-Gameplay**: Soldaten werden kontinuierlich rekrutiert, Gold fließt pro Sekunde
- **8 Regionen**: Erobere Gebiete mit steigendem Schwierigkeitsgrad
- **Upgrade-System**: Verbessere Rekrutierungsrate und Geschwindigkeit
- **Moral-Mechanik**: Halte die Moral hoch, sonst desertieren Soldaten
- **Präsidenten-Lifestyle**: Kaufe Luxusgüter vom Privatjet bis zur eigenen Insel
- **Safe-System**: Sichere dein Gold vor Verlusten
- **Savegame**: Speichere und lade deinen Spielstand (JSON)
- **Detaillierte Statistiken**: Kriegsarchiv mit K/D-Rate und allen wichtigen Zahlen

## Installation & Start

### Voraussetzungen
- Python 3.x (getestet auf Windows)
- Keine externen Dependencies erforderlich (nur Standard-Bibliothek)

### Spiel starten

1. Repository klonen oder Datei herunterladen:
```bash
git clone https://github.com/DEIN-USERNAME/the-presidents-terminal.git
cd the-presidents-terminal
```

2. Spiel starten:
```bash
python The_President_s_Terminal.py
```

Das war's! Keine Installation von Packages notwendig.

## Wie man spielt

### Spielstart
- Beim ersten Start erscheint ein ASCII-Art Titel
- System-Initialisierung läuft durch
- Gib deinen Namen ein oder lade einen bestehenden Spielstand

### Hauptmenü
Das Spiel läuft in Echtzeit - Gold und Soldaten werden automatisch generiert!

**Haupt-Upgrades:**
- `1` / `11` / `111`: Rekruten erhöhen (1x / bis 50% / Maximum)
- `2` / `22` / `222`: Ausbildung beschleunigen
- `3`: Moral-Boost (+20%)
- `4`: Zur Landkarte (Regionen erobern)
- `5`: Kriegsarchiv (Statistiken)
- `6`: Präsidentenamt (Lifestyle & Safe)
- `7`: Spiel speichern

**Tipp**: Mit `11`, `111` oder `222` kannst du Upgrades in Bulk kaufen!

### Kampf-System
- Regionen haben unterschiedlich starke Armeen (100 bis 10.000.000 Soldaten)
- Bei Sieg: Region erobert, passive Goldbonus dauerhaft
- Bei Niederlage: Verlierst 90% deiner Armee, Region regeneriert nach 1 Stunde

### Moral-System
- Sinkt langsam über Zeit
- Unter 15% Moral: Soldaten desertieren kontinuierlich
- Steigt bei Siegen, fällt bei Niederlagen

## Spielstand

Spielstände werden automatisch in `savegame.json` gespeichert.

**Speichern**: Drücke `7` im Hauptmenü  
**Laden**: Beim Spielstart wird automatisch nach einem Savegame gefragt

## Statistiken

Das Kriegsarchiv trackt:
- Rekrutierte, gefallene & desertierte Soldaten
- Besiegte Gegner & K/D-Rate
- Gewonnene/verlorene Schlachten
- Gesamtes Gold (verdient & ausgegeben)
- Safe-Transaktionen
- Lifestyle-Ausgaben

## Technische Details

- **Sprache**: Python 3.x
- **Libraries**: `time`, `os`, `random`, `json` (Standard-Bibliothek)
- **Architektur**: Hauptloop mit Delta-Time basierter Echtzeit-Berechnung
- **Persistenz**: JSON-basierte Savegames
- **UI**: ASCII-Art & Terminal-Formatierung

### Code-Struktur
```
- Konfiguration & Daten (Zeilen 1-79)
- Hilfsfunktionen (Zeilen 81-125)
- Kampf-Logik (Zeilen 127-151)
- UI & Screens (Zeilen 153-230)
- Hauptloop (Zeilen 232-455)
```

## Lernprozess

Dieses Projekt war mein erstes größeres Python-Projekt. Während der Entwicklung habe ich gelernt:

- **Python-Grundlagen**: Dictionaries, Listen, Funktionen, Loops
- **Datenpersistenz**: JSON lesen/schreiben
- **Zeit-Management**: Delta-Time für Echtzeit-Mechaniken
- **Code-Organisation**: Funktionen strukturieren, Daten trennen
- **Terminal-UI**: Formatierung & ASCII-Art
- **Game Design**: Balance, Progression, Feedback-Loops

Ich habe während der Entwicklung mit KI-Tools (Claude) gearbeitet, um:
- Konzepte zu verstehen und erklärt zu bekommen
- Best Practices zu lernen
- Bugs zu debuggen und Code zu verbessern
- Von Anfänger zu einem funktionierenden Projekt zu kommen

## Bekannte Einschränkungen

- Terminal muss mindestens 80 Zeichen breit sein
- Keine Farben (nur Mono-Text)
- Balancing ist nicht perfekt (war Lernprojekt)
- Code könnte noch weiter refactored werden

## Lizenz

Dieses Projekt steht unter der MIT-Lizenz - siehe LICENSE Datei für Details.

## Contributing

Da dies ein Lernprojekt ist, nehme ich gerne Feedback und Verbesserungsvorschläge entgegen!
Fühle dich frei, Issues zu öffnen oder Pull Requests zu erstellen.

## Autor

Erstellt als Lernprojekt für Python-Grundlagen.

---

*Hinweis: Dieses Projekt diente ausschließlich Lernzwecken. Der Code ist nicht produktionsreif, aber funktional und spielbar!*
