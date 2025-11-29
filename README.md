# Home Assistant Linky Tempo Integration

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/integration)
[![Version](https://img.shields.io/github/v/release/alltoon/LinkyTempo)](https://github.com/alltoon/LinkyTempo/releases)

[![French Version](https://img.shields.io/badge/Documentation-Français-blue?style=for-the-badge&logo=france)](README_fr.md)

This custom integration for Home Assistant retrieves your Linky power consumption and automatically distributes it across the **6 tariff periods of the EDF Tempo offer** (Blue/White/Red and Peak/Off-Peak Hours).

Unlike standard integrations, this one accurately reconstructs your history by cross-referencing your load curve with the historical Tempo color of the day.

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=alltoon&repository=https%3A%2F%2Fgithub.com%2Falltoon%2FLinkyTempo&category=Integrations)

## Features

*   🚀 **Easy setup** via the user interface (UI).
*   📊 **6 distinct energy sensors**, perfect for the Energy Dashboard.
*   🎨 **Automatic detection of the day's color** (via a community API).
*   🕒 **Precise time management**: respects Tempo schedules (Off-Peak from 10 PM to 6 AM).
*   💾 **Standalone**: No need for external sensors or MQTT.

## Prerequisites

1.  A **Linky** meter.
2.  An **Enedis access token**.
    *   This integration uses Bokub's proxy.
    *   👉 [Click here to generate your Token and find your PRM](https://conso.boris.sh/)

## Installation (HACS Recommended)

1.  **Open HACS** in Home Assistant.
2.  Go to **Integrations** > click the three dots in the top right > **Custom repositories**.
3.  Add this repository's URL: `https://github.com/alltoon/LinkyTempo`.
4.  Select the category: **Integration**.
5.  Click **Add**, find "Linky Tempo" in the list, and install it.
6.  **Restart Home Assistant**.

## Configuration

1.  Go to **Settings** > **Devices & Services**.
2.  Click **Add Integration** (bottom right).
3.  Search for **Linky Tempo**.
4.  Fill in the form:
    *   **PRM**: Your 14-digit Delivery Point ID.
    *   **Token**: Your access token (often starts with `ey...`).

> **Note:** Data is updated once a day (in the morning for the previous day). After the first installation, wait a few seconds for the sensors to be created. They will display yesterday's consumption.

## Display Tempo Colors with a Theme

To visually distinguish the Tempo days in your dashboards, you can use a dedicated theme.

1.  **Install the Theme**:
    *   Go to HACS > Front-end.
    *   Add this repository: `https://github.com/alltoon/LinkyTempoTheme`
    *   Install the "LinkyTempoTheme" theme.
2.  **Activate the Theme**:
    *   Go to your user profile (by clicking your name in the bottom left).
    *   Select **LinkyTempoTheme** from the "Theme" dropdown menu.

This will allow you to assign specific colors to your sensors based on the day's Tempo color.

## Energy Dashboard Setup

This is where the integration truly shines!

1.  Go to **Settings** > **Dashboards** > **Energy**.
2.  In the "Electricity consumption" section, click **Add a source**.
3.  Add your 6 new sensors one by one:
    *   `sensor.linky_tempo_bleu_heures_creuses`
    *   `sensor.linky_tempo_bleu_heures_pleines`
    *   `sensor.linky_tempo_blanc_heures_creuses`
    *   ...and so on for all 6.
4.  For each sensor, select **"Use a static price"** and enter the corresponding kWh tariff from your contract (e.g., 0.1296 for Blue Off-Peak).

## How It Works

This integration performs the following actions every 4 hours:
1.  Retrieves the **Load Curve** (30-minute intervals) for the previous day from the Enedis API (via Bokub's Proxy).
2.  Fetches the historical **Tempo Color** for each relevant day from `api-couleur-tempo.fr`.
3.  Calculates and allocates the Wh to the correct "bins" (e.g., if it's 11:00 PM on a WHITE day -> adds to `White Off-Peak`).
4.  Updates the sensors in Home Assistant.

## Credits

*   Based on the work of [Bokub](https://github.com/bokub/ha-linky) for API access.
*   Uses the [api-couleur-tempo.fr](https://www.api-couleur-tempo.fr/) API for color history.
