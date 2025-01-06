# Bash, GitHub und Unix-Konzepte: Ein Lernprojekt

## Inhalt
1. [Bash-Befehle und Aufgaben](#bash-befehle-und-aufgaben)
2. [Dateianalyse mit SHA-256](#dateianalyse-mit-sha-256)
3. [GitHub-Logfile-Erstellung](#github-logfile-erstellung)
4. [Software-Updates auf macOS](#software-updates-auf-macos)
5. [Unix-Grundlagen](#unix-grundlagen)

---

## Bash-Befehle und Aufgaben

### Übersicht
- **`find`**: Dateisuche und Filterung.
- **`sort`**: Sortieren von Daten.
- **`uniq`**: Entfernen von Duplikaten und Zählen von Häufigkeiten.
- **`awk`**: Verarbeitung von Textdaten.
- **`sha256sum`**: Berechnung von Hashes.

### Beispiele und Anwendungsfälle
1. **Finden und Hashen von Dateien**:
   ```bash
   find . -type f -name "*.png" -exec sha256sum {} +
   ```
   - Sucht alle `.png`-Dateien und berechnet ihre SHA-256-Hashes.

2. **Sortieren und Zählen mit `uniq`**:
   ```bash
   find . -type f -name "*.png" -exec sha256sum {} + | sort | uniq -c
   ```
   - Zeigt an, wie oft jede Datei (nach Hash) vorkommt.

3. **Mit Dateinamen gruppieren (`awk`)**:
   ```bash
   find . -type f -name "*.png" -exec sha256sum {} + | sort | awk '{count[$1]++; files[$1] = files[$1] " " $2} END {for (hash in count) print count[hash], hash, files[hash]}'
   ```
   - Gruppiert Hashes und zeigt zugehörige Dateinamen an.

---

## Dateianalyse mit SHA-256

### Grundlagen
- **SHA-256**: Ein kryptografischer Algorithmus zur Erstellung eindeutiger Fingerabdrücke für Dateien.
- Anwendungen: Dateivergleich, Duplikaterkennung, Datenintegrität.

### Vorgehen zur Versionsanalyse
1. **Hashes berechnen**:
   ```bash
   sha256sum *.png > logfile.txt
   ```
   - Speichert die Hashes aller `.png`-Dateien in `logfile.txt`.

2. **Versionen zählen**:
   ```bash
   find . -type f -name "*.png" -exec sha256sum {} + | sort | uniq -c | sort -nr
   ```

3. **Hashes mit Dateinamen anzeigen**:
   ```bash
   find . -type f -name "*.png" -exec sha256sum {} + | sort | awk '{count[$1]++; files[$1] = files[$1] " " $2} END {for (hash in count) print count[hash], hash, files[hash]}' | sort -nr
   ```

---

## GitHub-Logfile-Erstellung

### Manuelles Hochladen
1. **Markdown-Dateien hochladen**:
   - Navigiere in das Repository → "Add file" → "Upload files".

2. **Logfile generieren**:
   ```bash
   git log --pretty=format:"%h %an %ad %s" > logfile.txt
   ```

3. **Dateien committen**:
   ```bash
   git add logfile.txt ergebnisse.md
   git commit -m "Ergebnisse und Logfile hinzugefügt"
   git push origin main
   ```

### Alternative: GitHub API
- Abruf der Commit-Historie:
  ```bash
  curl -s "https://api.github.com/repos/username/repository/commits" > logfile.json
  ```

---

## Software-Updates auf macOS

### Aktualisierung von `uniq` und Core Utilities
1. **Homebrew installieren**:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **GNU Core Utilities installieren**:
   ```bash
   brew install coreutils
   ```

3. **GNU-Versionen verwenden**:
   - Standard: `guniq` statt `uniq`.
   - Optional: GNU-Tools als Standard setzen:
     ```bash
     export PATH="/opt/homebrew/opt/coreutils/libexec/gnubin:$PATH"
     ```

---

## Unix-Grundlagen

### Wichtige Konzepte
1. **Pipes und Redirection**:
   - Weiterleitung von Ausgaben zu Eingaben:
     ```bash
     command1 | command2
     ```

2. **Dateisuche mit `find`**:
   - Beispiel:
     ```bash
     find /path -type f -name "*.txt"
     ```

3. **Sortieren und Filtern**:
   - Sortieren:
     ```bash
     sort file.txt
     ```
   - Duplikate entfernen:
     ```bash
     uniq
     ```

---

### Abschluss
Dieses Projekt dokumentiert die Grundlagen von Bash-Befehlen, Datei- und Versionsanalyse mit SHA-256, sowie praktische GitHub-Arbeiten und Software-Updates. Es dient als Lernressource und Nachschlagewerk für Unix-basierte Workflows.

