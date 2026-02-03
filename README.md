# Alternatives-Dashboard
Grafana Dashboard auf Grundlage der von vieventlog erzeugten SQLite-Datenbank
# Grafana Dashboard Repository

Dieses Repository enthält ein Grafana-Dashboard als Classic-JSON-Dateien. Das Dashboard können direkt in eine eigene Grafana-Instanz importiert werden.

Hinweis: Das Grafana-Dashboard läuft bei mir auf meiner Synology-Datenstation in einem -Container. Nur diese Installation habe ich getestet. Die Installationshinweise für die anderen Umgebungen sind automatisiert erstellt worden und nicht getestet. 

----

## 1. Installation von Grafana

### Windows
1. Lade den Windows-Installer von der offiziellen Grafana-Website herunter: [https://grafana.com/grafana/download](https://grafana.com/grafana/download)
2. Installiere Grafana wie gewohnt.
3. Starte Grafana über das Startmenü. Standardmäßig läuft Grafana unter `http://localhost:3000`.
4. Standard-Login: Benutzername: `admin`, Passwort: `admin`

**Datenbank-Verwaltung:**
- Viessmann-Datenbank: `viessmann_events.db`
- Kostenloses Tool: [DB Browser for SQLite](https://sqlitebrowser.org/)

### macOS (Apple)
1. Mit Homebrew:
```bash
brew install grafana
brew services start grafana
2.	Alternativ: Download des macOS-Pakets von der Grafana-Website.
3.	Standard-Login: Benutzername: admin, Passwort: admin
4.  Datenbank-Verwaltung: Viessmann-Datenbank viessmann_events.db, Tool: DB Browser for SQLite

### Linux / Unix
1.	Debian/Ubuntu:
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update
sudo apt-get install grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
2.	Standard-Login: admin/admin
3.  Datenbank-Verwaltung: Viessmann-Datenbank viessmann_events.db, Tool: DB Browser for SQLite

### Raspberry Pi / ARM
1.	ARM-Paket herunterladen: https://grafana.com/grafana/download￼
2.	Installation per .deb:
sudo dpkg -i grafana_<version>_armhf.deb
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
3.	Standard-Login: admin/admin
4.  Datenbank-Verwaltung: Viessmann-Datenbank viessmann_events.db, Tool: DB Browser for SQLite

### Synology NAS
1. 	Installiere das Docker-Paket über das Synology-Paketzentrum (Image: z.B. grafana/grafana: latest
2.  Grafana-Container erstellen
<img width="1668" height="2224" alt="image" src="https://github.com/user-attachments/assets/b94f0ccf-431a-4ba9-acee-d804dfac6da7" />
3.  Grafana über Browser starten : http://deine_synology_ip:öffentlicher Port gem. Conatiner (z.B. 3030)
4.  SQLite-Plugin für Grafana installieren (Main Menü --> Datasource)
5.  Pad zur viessmann.events.db eintragen
6.  Standard-Login: admin/admin

Datenbank-Verwaltung: Viessmann-Datenbank z.B. /volume1/docker/viessmann_events.db, Tool: SQLite Browser auf Client

##  Logische views in der viessmann_events.db erzeugen
1.  Datenbank Verwaltungssoftware installieren (z.B. DB Browser for SQLite](https://sqlitebrowser.org/)
2.  Datenbank öffnen
3.  Beide logische Views auf die Tabelle temperature_snapshots erzeugen:
    a) temperature_snapshots_stat:

    CREATE VIEW 'temperature_snapshots_stat' as
    SELECT datetime  (
    substr(timestamp, 1, 19),'localtime') as 'timestamp_mez',
    thermal_power * 1000 / 60 * sample_interval as 'Wärmeleistung',
    compressor_power / 60 * sample_interval as 'elektrische_Leistung',
    thermal_power * 1000 / compressor_power as 'COP_calc',
    time('00:00', '+' || (compressor_active * sample_interval) || ' minutes') as 'Laufzeit',
    * from temperature_snapshots order by installation_id desc,
    datetime(substr(timestamp, 1, 19), 'localtime' )

    b) temperature_snapshots_takte_heute:

    CREATE VIEW 'temperature_snapshots_takte_heute' as
    SELECT datetime  (
    substr(timestamp, 1, 19), 'localtime') as timestamp_mez,
    installation_id,
    Compressor_active,
    LAG (
    compressor_active) over(order by installation_id desc,  datetime (
    substr(timestamp, 1, 19),
    'localtime') desc) as next_period,
    LAG (
    compressor_active) over(order by installation_id desc,  datetime (
    substr(timestamp, 1, 19),
    'localtime') desc )


##  Import des Dashboards
1.	Lade die Dashboard-JSON-Datei herunter: Viessmann Wärmepumpe – All-in-One Dashboard.json￼
2.	Öffne Grafana → Dashboard → Import → Upload JSON file
3.	Datei auswählen → Importieren

Hinweis: SQL-Statements für zusätzliche Views in viessmann_events.db müssen separat erstellt werden.

