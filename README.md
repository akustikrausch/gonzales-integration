```
 ██████╗  ██████╗ ███╗   ██╗███████╗ █████╗ ██╗     ███████╗███████╗
██╔════╝ ██╔═══██╗████╗  ██║╚══███╔╝██╔══██╗██║     ██╔════╝██╔════╝
██║  ███╗██║   ██║██╔██╗ ██║  ███╔╝ ███████║██║     █████╗  ███████╗
██║   ██║██║   ██║██║╚██╗██║ ███╔╝  ██╔══██║██║     ██╔══╝  ╚════██║
╚██████╔╝╚██████╔╝██║ ╚████║███████╗██║  ██║███████╗███████╗███████║
 ╚═════╝  ╚═════╝ ╚═╝  ╚═══╝╚══════╝╚═╝  ╚═╝╚══════╝╚══════╝╚══════╝
```

# Gonzales Integration for Home Assistant

[![hacs_badge](https://img.shields.io/badge/HACS-Default-41BDF5.svg)](https://github.com/hacs/integration)
[![GitHub Release](https://img.shields.io/github/release/akustikrausch/gonzales-integration.svg)](https://github.com/akustikrausch/gonzales-integration/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=akustikrausch&repository=gonzales-integration&category=integration)

[Deutsche Anleitung weiter unten](#deutsche-anleitung)

---

## What is this?

This is the **Home Assistant integration** for [Gonzales Speed Monitor](https://github.com/akustikrausch/gonzales). It connects to any Gonzales server -- whether running as a [Home Assistant Add-on](https://github.com/akustikrausch/gonzales-ha) or as a standalone deployment (Docker, Raspberry Pi, etc.) -- and exposes **20+ sensors**, **binary sensors**, **buttons**, and **services** for your automations and dashboards.

The integration is a generic API client. It polls the Gonzales REST API and translates the data into native Home Assistant entities.

### Gonzales Ecosystem

| Repository | What it is | You need this if... |
|-----------|------------|---------------------|
| **[gonzales](https://github.com/akustikrausch/gonzales)** | Backend, Web Dashboard, TUI, CLI, API, MCP Server | You run Gonzales standalone (Docker, Raspberry Pi, bare metal) |
| **[gonzales-ha](https://github.com/akustikrausch/gonzales-ha)** | Home Assistant Add-on (App) | You use HA OS/Supervised and want one-click installation |
| **[gonzales-integration](https://github.com/akustikrausch/gonzales-integration)** | Home Assistant Integration (HACS) | You run Gonzales standalone AND want HA sensors |

> **You're here!** This is the HACS integration. You need a running Gonzales server to connect to -- either the [add-on](https://github.com/akustikrausch/gonzales-ha) or a [standalone installation](https://github.com/akustikrausch/gonzales).

---

## Installation

### Via HACS (Recommended)

1. Open **HACS** in your Home Assistant
2. Click **Integrations**
3. Click the **+ Explore & Download Repositories** button
4. Search for **Gonzales**
5. Click **Download**
6. Restart Home Assistant

### Manual Installation

1. Download the [latest release](https://github.com/akustikrausch/gonzales-integration/releases)
2. Copy the `custom_components/gonzales/` folder into your Home Assistant `config/custom_components/` directory
3. Restart Home Assistant

---

## Configuration

After installing, go to **Settings > Devices & Services > + Add Integration** and search for **Gonzales**.

### With the HA Add-on

If you have the [Gonzales Add-on](https://github.com/akustikrausch/gonzales-ha) installed, the integration auto-discovers it. Just click **Submit** to confirm.

If auto-discovery does not work, you need to enter the connection details manually:

1. Open the Gonzales web UI from the sidebar
2. Look at the URL: `http://homeassistant.local:8123/hassio/addon/546fc077_gonzales/ingress`
3. The slug is `546fc077_gonzales` -- replace the underscore with a dash: `546fc077-gonzales`
4. Enter this as the **Host**, port `8470` (or `8099`), leave API key empty

### Standalone Server (Docker, Raspberry Pi, etc.)

If you run Gonzales on a separate machine:

1. Enter the **IP address** or hostname of the Gonzales server
2. Enter the **port** (default: `8099`)
3. Enter your **API key** if authentication is enabled
4. Set the **update interval** (default: 60 seconds)

---

## Sensors

### Main Sensors

| Sensor | Entity ID | Description |
|--------|-----------|-------------|
| Download Speed | `sensor.gonzales_download_speed` | Latest download speed in Mbps |
| Upload Speed | `sensor.gonzales_upload_speed` | Latest upload speed in Mbps |
| Ping Latency | `sensor.gonzales_ping_latency` | Ping latency in ms |
| Ping Jitter | `sensor.gonzales_ping_jitter` | Ping jitter in ms |
| Packet Loss | `sensor.gonzales_packet_loss` | Packet loss percentage |
| Last Test Time | `sensor.gonzales_last_test_time` | Timestamp of last speed test |
| ISP Score | `sensor.gonzales_isp_score` | ISP performance score (0-100) |

### Diagnostic Sensors

| Sensor | Entity ID | Description |
|--------|-----------|-------------|
| Scheduler Status | `sensor.gonzales_scheduler_running` | running/stopped |
| Test in Progress | `sensor.gonzales_test_in_progress` | yes/no |
| Uptime | `sensor.gonzales_uptime` | Backend uptime in seconds |
| Total Measurements | `sensor.gonzales_total_measurements` | Total test count |
| Database Size | `sensor.gonzales_db_size` | Database size in bytes |

### Smart Scheduler Sensors

| Sensor | Entity ID | Description |
|--------|-----------|-------------|
| Smart Scheduler Phase | `sensor.gonzales_smart_scheduler_phase` | Current phase (learning/stable/aggressive) |
| Connection Stability | `sensor.gonzales_smart_scheduler_stability` | Stability score as percentage |
| Smart Scheduler Interval | `sensor.gonzales_smart_scheduler_interval` | Current adaptive interval in minutes |
| Data Budget Used | `sensor.gonzales_smart_scheduler_data_used` | Percentage of monthly data budget used |

### Root Cause Analysis Sensors

| Sensor | Entity ID | Description |
|--------|-----------|-------------|
| Network Health Score | `sensor.gonzales_network_health_score` | Overall network health (0-100) |
| Primary Network Issue | `sensor.gonzales_primary_issue` | Category of main detected issue |
| DNS Health | `sensor.gonzales_dns_health` | DNS layer health score (0-100) |
| Local Network Health | `sensor.gonzales_local_network_health` | Local network score (0-100) |
| ISP Backbone Health | `sensor.gonzales_isp_backbone_health` | ISP backbone score (0-100) |
| ISP Last Mile Health | `sensor.gonzales_isp_lastmile_health` | ISP last mile score (0-100) |

### Binary Sensor

| Sensor | Entity ID | Description |
|--------|-----------|-------------|
| Internet Outage | `binary_sensor.gonzales_internet_outage` | ON when outage detected |

### Button

| Entity | Entity ID | Description |
|--------|-----------|-------------|
| Run Speed Test | `button.gonzales_run_speed_test` | Trigger manual speed test |

---

## Services

| Service | Description |
|---------|-------------|
| `gonzales.run_speedtest` | Trigger a speed test (optional: `entry_id`) |
| `gonzales.set_interval` | Set test interval in minutes 1-1440 (required: `interval`, optional: `entry_id`) |

### Examples

**Trigger a speed test from an automation:**

```yaml
automation:
  - alias: "Morning Speed Test"
    trigger:
      - platform: time
        at: "06:00:00"
    action:
      - service: gonzales.run_speedtest
```

**Change the test interval dynamically:**

```yaml
automation:
  - alias: "Test More Often During Peak Hours"
    trigger:
      - platform: time
        at: "18:00:00"
    action:
      - service: gonzales.set_interval
        data:
          interval: 30
```

---

## Automation Examples

### Slow Internet Notification

```yaml
automation:
  - alias: "Slow Internet Alert"
    trigger:
      - platform: numeric_state
        entity_id: sensor.gonzales_download_speed
        below: 50
    action:
      - service: notify.mobile_app_your_phone
        data:
          title: "Slow Internet!"
          message: >
            Download speed is only
            {{ states('sensor.gonzales_download_speed') }} Mbps
```

### Internet Outage Detection

Gonzales uses smart 3-strike retry logic: three consecutive test failures confirm an outage, turning `binary_sensor.gonzales_internet_outage` to ON.

```yaml
automation:
  - alias: "Internet Outage Alert"
    trigger:
      - platform: state
        entity_id: binary_sensor.gonzales_internet_outage
        to: "on"
    action:
      - service: notify.mobile_app_your_phone
        data:
          title: "Internet Outage!"
          message: "Internet has been down for multiple consecutive tests."
      # Optional: restart router via smart plug
      - service: switch.turn_off
        entity_id: switch.router_plug
      - delay: "00:00:30"
      - service: switch.turn_on
        entity_id: switch.router_plug

  - alias: "Internet Restored"
    trigger:
      - platform: state
        entity_id: binary_sensor.gonzales_internet_outage
        to: "off"
    action:
      - service: notify.mobile_app_your_phone
        data:
          title: "Internet Restored"
          message: "Your internet connection is working again."
```

### Dynamic Interval Based on Time of Day

```yaml
automation:
  - alias: "Peak Hours: Test Every 30 Min"
    trigger:
      - platform: time
        at: "18:00:00"
    action:
      - service: gonzales.set_interval
        data:
          interval: 30

  - alias: "Off-Peak: Test Every 120 Min"
    trigger:
      - platform: time
        at: "00:00:00"
    action:
      - service: gonzales.set_interval
        data:
          interval: 120
```

---

## Dashboard Examples

### Entities Card

```yaml
type: entities
title: Internet Speed
entities:
  - entity: sensor.gonzales_download_speed
    name: Download
  - entity: sensor.gonzales_upload_speed
    name: Upload
  - entity: sensor.gonzales_ping_latency
    name: Ping
  - entity: sensor.gonzales_isp_score
    name: ISP Score
```

### Gauge Card

```yaml
type: gauge
entity: sensor.gonzales_download_speed
name: Download Speed
min: 0
max: 1000
severity:
  green: 850
  yellow: 500
  red: 0
```

### History Graph

```yaml
type: history-graph
title: Internet Speed History
hours_to_show: 24
entities:
  - entity: sensor.gonzales_download_speed
    name: Download
  - entity: sensor.gonzales_upload_speed
    name: Upload
```

---

## Troubleshooting

### Sensors show "unavailable"

Sensors need at least one completed speed test. Wait a few minutes or trigger a manual test via the `button.gonzales_run_speed_test` entity or the `gonzales.run_speedtest` service.

### Cannot connect to Gonzales

1. **Add-on users:** Make sure the Gonzales app is running (Settings > Apps > Gonzales)
2. **Standalone users:** Verify the IP address, port, and that the Gonzales server is reachable from Home Assistant
3. Check the Home Assistant logs for connection error details

### Auto-discovery does not find the add-on

This can happen if the Hassio API is temporarily unavailable. Try adding the integration manually:

1. Go to **Settings > Devices & Services > + Add Integration > Gonzales**
2. For **Host**, enter your add-on slug with dashes (e.g., `546fc077-gonzales`)
3. For **Port**, try `8470` or `8099`

### Speed test keeps failing

Check the Gonzales server logs. Common causes:
- No internet connection
- Firewall blocking Ookla test servers
- Temporary Ookla server issues

---

## Links

- [Gonzales Main Project](https://github.com/akustikrausch/gonzales) -- Full documentation, standalone setup, API reference, MCP server
- [Gonzales HA Add-on](https://github.com/akustikrausch/gonzales-ha) -- One-click Home Assistant app with Ingress dashboard
- [Report Issues](https://github.com/akustikrausch/gonzales-integration/issues)

---

## License

MIT License. See [LICENSE](LICENSE) for details.

The Ookla Speedtest CLI used by the Gonzales server is proprietary third-party software. By using Gonzales, you accept the [Ookla EULA](https://www.speedtest.net/about/eula). Speedtest is a trademark of Ookla, LLC.

---

---

# Deutsche Anleitung

## Was ist das?

Dies ist die **Home Assistant Integration** fuer [Gonzales Speed Monitor](https://github.com/akustikrausch/gonzales). Sie verbindet sich mit jedem Gonzales-Server -- egal ob als [Home Assistant Add-on](https://github.com/akustikrausch/gonzales-ha) oder als eigenstaendige Installation (Docker, Raspberry Pi, etc.) -- und stellt **20+ Sensoren**, **Binaersensoren**, **Buttons** und **Services** fuer deine Automationen und Dashboards bereit.

### Gonzales Ecosystem

| Repository | Was es ist | Du brauchst das wenn... |
|-----------|------------|------------------------|
| **[gonzales](https://github.com/akustikrausch/gonzales)** | Backend, Web Dashboard, TUI, CLI, API, MCP Server | Du Gonzales standalone betreibst (Docker, Raspberry Pi, Bare Metal) |
| **[gonzales-ha](https://github.com/akustikrausch/gonzales-ha)** | Home Assistant Add-on (App) | Du HA OS/Supervised nutzt und Ein-Klick-Installation willst |
| **[gonzales-integration](https://github.com/akustikrausch/gonzales-integration)** | Home Assistant Integration (HACS) | Du Gonzales standalone betreibst UND HA-Sensoren willst |

> **Du bist hier!** Dies ist die HACS-Integration. Du brauchst einen laufenden Gonzales-Server -- entweder das [Add-on](https://github.com/akustikrausch/gonzales-ha) oder eine [Standalone-Installation](https://github.com/akustikrausch/gonzales).

Die Integration ist ein generischer API-Client. Sie pollt die Gonzales REST API und uebersetzt die Daten in native Home Assistant Entities.

---

## Installation

### Ueber HACS (Empfohlen)

1. Oeffne **HACS** in deinem Home Assistant
2. Klicke auf **Integrations**
3. Klicke auf **+ Explore & Download Repositories**
4. Suche nach **Gonzales**
5. Klicke **Download**
6. Starte Home Assistant neu

### Manuelle Installation

1. Lade das [neueste Release](https://github.com/akustikrausch/gonzales-integration/releases) herunter
2. Kopiere den Ordner `custom_components/gonzales/` in dein Home Assistant `config/custom_components/` Verzeichnis
3. Starte Home Assistant neu

---

## Konfiguration

Gehe nach der Installation zu **Einstellungen > Geraete & Dienste > + Integration hinzufuegen** und suche nach **Gonzales**.

### Mit dem HA Add-on

Wenn du das [Gonzales Add-on](https://github.com/akustikrausch/gonzales-ha) installiert hast, erkennt die Integration es automatisch. Klicke einfach **Absenden** zum Bestaetigen.

Falls die Auto-Erkennung nicht funktioniert, musst du die Verbindungsdaten manuell eingeben:

1. Oeffne die Gonzales Web-Oberflaeche ueber die Seitenleiste
2. Schau in die URL: `http://homeassistant.local:8123/hassio/addon/546fc077_gonzales/ingress`
3. Der Slug ist `546fc077_gonzales` -- ersetze den Unterstrich durch einen Bindestrich: `546fc077-gonzales`
4. Gib dies als **Host** ein, Port `8470` (oder `8099`), API-Key leer lassen

### Eigenstaendiger Server (Docker, Raspberry Pi, etc.)

Wenn du Gonzales auf einem separaten Geraet betreibst:

1. Gib die **IP-Adresse** oder den Hostnamen des Gonzales-Servers ein
2. Gib den **Port** ein (Standard: `8099`)
3. Gib deinen **API-Key** ein, falls Authentifizierung aktiviert ist
4. Setze das **Update-Intervall** (Standard: 60 Sekunden)

---

## Sensoren

### Hauptsensoren

| Sensor | Entity ID | Beschreibung |
|--------|-----------|--------------|
| Download Speed | `sensor.gonzales_download_speed` | Aktuelle Download-Geschwindigkeit in Mbps |
| Upload Speed | `sensor.gonzales_upload_speed` | Aktuelle Upload-Geschwindigkeit in Mbps |
| Ping Latency | `sensor.gonzales_ping_latency` | Ping-Latenz in ms |
| Ping Jitter | `sensor.gonzales_ping_jitter` | Ping-Jitter in ms |
| Packet Loss | `sensor.gonzales_packet_loss` | Paketverlust in Prozent |
| Last Test Time | `sensor.gonzales_last_test_time` | Zeitstempel des letzten Speedtests |
| ISP Score | `sensor.gonzales_isp_score` | ISP-Leistungsbewertung (0-100) |

### Diagnose-Sensoren

| Sensor | Entity ID | Beschreibung |
|--------|-----------|--------------|
| Scheduler Status | `sensor.gonzales_scheduler_running` | running/stopped |
| Test in Progress | `sensor.gonzales_test_in_progress` | yes/no |
| Uptime | `sensor.gonzales_uptime` | Backend-Laufzeit in Sekunden |
| Total Measurements | `sensor.gonzales_total_measurements` | Gesamtanzahl der Tests |
| Database Size | `sensor.gonzales_db_size` | Datenbankgroesse in Bytes |

### Smart-Scheduler-Sensoren

| Sensor | Entity ID | Beschreibung |
|--------|-----------|--------------|
| Smart Scheduler Phase | `sensor.gonzales_smart_scheduler_phase` | Aktuelle Phase (learning/stable/aggressive) |
| Connection Stability | `sensor.gonzales_smart_scheduler_stability` | Stabilitaetsbewertung in Prozent |
| Smart Scheduler Interval | `sensor.gonzales_smart_scheduler_interval` | Aktuelles adaptives Intervall in Minuten |
| Data Budget Used | `sensor.gonzales_smart_scheduler_data_used` | Prozent des monatlichen Datenbudgets verbraucht |

### Root-Cause-Analyse-Sensoren

| Sensor | Entity ID | Beschreibung |
|--------|-----------|--------------|
| Network Health Score | `sensor.gonzales_network_health_score` | Netzwerk-Gesundheitsbewertung (0-100) |
| Primary Network Issue | `sensor.gonzales_primary_issue` | Kategorie des erkannten Hauptproblems |
| DNS Health | `sensor.gonzales_dns_health` | DNS-Gesundheitsbewertung (0-100) |
| Local Network Health | `sensor.gonzales_local_network_health` | Lokales Netzwerk Bewertung (0-100) |
| ISP Backbone Health | `sensor.gonzales_isp_backbone_health` | ISP-Backbone-Bewertung (0-100) |
| ISP Last Mile Health | `sensor.gonzales_isp_lastmile_health` | ISP-Last-Mile-Bewertung (0-100) |

### Binaerer Sensor

| Sensor | Entity ID | Beschreibung |
|--------|-----------|--------------|
| Internet Outage | `binary_sensor.gonzales_internet_outage` | AN wenn Ausfall erkannt |

### Button

| Entity | Entity ID | Beschreibung |
|--------|-----------|--------------|
| Run Speed Test | `button.gonzales_run_speed_test` | Manuellen Speedtest ausloesen |

---

## Services (Dienste)

| Service | Beschreibung |
|---------|--------------|
| `gonzales.run_speedtest` | Speedtest ausloesen (optional: `entry_id`) |
| `gonzales.set_interval` | Testintervall in Minuten setzen, 1-1440 (erforderlich: `interval`, optional: `entry_id`) |

### Beispiele

**Speedtest per Automation ausloesen:**

```yaml
automation:
  - alias: "Morgens Speedtest"
    trigger:
      - platform: time
        at: "06:00:00"
    action:
      - service: gonzales.run_speedtest
```

**Testintervall dynamisch aendern:**

```yaml
automation:
  - alias: "Haeufiger testen zu Stosszeiten"
    trigger:
      - platform: time
        at: "18:00:00"
    action:
      - service: gonzales.set_interval
        data:
          interval: 30
```

---

## Automations-Beispiele

### Benachrichtigung bei langsamem Internet

```yaml
automation:
  - alias: "Langsames Internet Warnung"
    trigger:
      - platform: numeric_state
        entity_id: sensor.gonzales_download_speed
        below: 50
    action:
      - service: notify.mobile_app_dein_handy
        data:
          title: "Langsames Internet!"
          message: >
            Download-Geschwindigkeit ist nur
            {{ states('sensor.gonzales_download_speed') }} Mbps
```

### Internet-Ausfall-Erkennung

Gonzales nutzt intelligente 3-Strike-Retry-Logik: drei aufeinanderfolgende Testfehler bestaetigen einen Ausfall und `binary_sensor.gonzales_internet_outage` schaltet auf AN.

```yaml
automation:
  - alias: "Internet Ausfall Warnung"
    trigger:
      - platform: state
        entity_id: binary_sensor.gonzales_internet_outage
        to: "on"
    action:
      - service: notify.mobile_app_dein_handy
        data:
          title: "Internet Ausfall!"
          message: "Internet ist bei mehreren aufeinanderfolgenden Tests ausgefallen."
      # Optional: Router ueber Smart Plug neustarten
      - service: switch.turn_off
        entity_id: switch.router_steckdose
      - delay: "00:00:30"
      - service: switch.turn_on
        entity_id: switch.router_steckdose

  - alias: "Internet Wiederhergestellt"
    trigger:
      - platform: state
        entity_id: binary_sensor.gonzales_internet_outage
        to: "off"
    action:
      - service: notify.mobile_app_dein_handy
        data:
          title: "Internet Wiederhergestellt"
          message: "Deine Internetverbindung funktioniert wieder."
```

### Dynamisches Intervall nach Tageszeit

```yaml
automation:
  - alias: "Stosszeiten: Alle 30 Min testen"
    trigger:
      - platform: time
        at: "18:00:00"
    action:
      - service: gonzales.set_interval
        data:
          interval: 30

  - alias: "Nebenzeiten: Alle 120 Min testen"
    trigger:
      - platform: time
        at: "00:00:00"
    action:
      - service: gonzales.set_interval
        data:
          interval: 120
```

---

## Dashboard-Beispiele

### Entitaeten-Karte

```yaml
type: entities
title: Internet Geschwindigkeit
entities:
  - entity: sensor.gonzales_download_speed
    name: Download
  - entity: sensor.gonzales_upload_speed
    name: Upload
  - entity: sensor.gonzales_ping_latency
    name: Ping
  - entity: sensor.gonzales_isp_score
    name: ISP Bewertung
```

### Gauge-Karte

```yaml
type: gauge
entity: sensor.gonzales_download_speed
name: Download Speed
min: 0
max: 1000
severity:
  green: 850
  yellow: 500
  red: 0
```

### Verlaufsgraph

```yaml
type: history-graph
title: Internet Geschwindigkeitsverlauf
hours_to_show: 24
entities:
  - entity: sensor.gonzales_download_speed
    name: Download
  - entity: sensor.gonzales_upload_speed
    name: Upload
```

---

## Problemloesung

### Sensoren zeigen "nicht verfuegbar"

Sensoren brauchen mindestens einen abgeschlossenen Speedtest. Warte ein paar Minuten oder loese einen manuellen Test ueber die `button.gonzales_run_speed_test` Entity oder den `gonzales.run_speedtest` Service aus.

### Verbindung zu Gonzales nicht moeglich

1. **Add-on Nutzer:** Stelle sicher, dass die Gonzales App laeuft (Einstellungen > Apps > Gonzales)
2. **Standalone Nutzer:** Pruefe IP-Adresse, Port und ob der Gonzales-Server von Home Assistant aus erreichbar ist
3. Pruefe die Home Assistant Logs auf Verbindungsfehler

### Auto-Erkennung findet das Add-on nicht

Das kann passieren, wenn die Hassio API voruebergehend nicht verfuegbar ist. Fuege die Integration manuell hinzu:

1. Gehe zu **Einstellungen > Geraete & Dienste > + Integration hinzufuegen > Gonzales**
2. Gib als **Host** deinen Add-on Slug mit Bindestrichen ein (z.B. `546fc077-gonzales`)
3. Als **Port** probiere `8470` oder `8099`

### Speedtest schlaegt immer fehl

Pruefe die Gonzales Server-Logs. Haeufige Ursachen:
- Keine Internetverbindung
- Firewall blockiert Ookla Testserver
- Temporaere Ookla Server-Probleme

---

## Links

- [Gonzales Hauptprojekt](https://github.com/akustikrausch/gonzales) -- Vollstaendige Dokumentation, Standalone-Setup, API-Referenz, MCP-Server
- [Gonzales HA Add-on](https://github.com/akustikrausch/gonzales-ha) -- Ein-Klick Home Assistant App mit Ingress-Dashboard
- [Probleme melden](https://github.com/akustikrausch/gonzales-integration/issues)

---

## Lizenz

MIT-Lizenz. Siehe [LICENSE](LICENSE) fuer Details.

Die vom Gonzales-Server verwendete Ookla Speedtest CLI ist proprietaere Drittanbieter-Software. Mit der Nutzung von Gonzales akzeptierst du die [Ookla EULA](https://www.speedtest.net/about/eula). Speedtest ist eine Marke von Ookla, LLC.
