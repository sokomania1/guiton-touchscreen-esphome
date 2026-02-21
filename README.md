# Guiton Touchscreen ESPHome + LVGL

ESPHome-Konfiguration für Guition 3.5" Display mit LVGL.

## Features
- ✨ **Lampen-Steuerung** mit Dimmer-Slider (orange Theme)
- 🌤️ **Wetter** (weather.freudenstadt)
- 🌡️ **Raumtemperaturen** (alle sensor.snf_xxx_temperature)
- 🔥 **Heizungssteuerung** (climate.sonoff1-3)

## Hardware
- Guition ESP32-Display 480x320
- Basierend auf [RyanEwen/esphome-lvgl](https://github.com/RyanEwen/esphome-lvgl)

## Installation
1. ESPHome Dashboard öffnen
2. `guition-35-display.yaml` importieren
3. Secrets anpassen (`secrets.yaml`)
4. Kompilieren & flashen

## Entities
- **Wetter**: `weather.freudenstadt`
- **Temperaturen**: `sensor.snf_xxx_temperature` (siehe esphome_entities.json)
- **Heizung**: `climate.sonoff1`, `climate.sonoff2`, `climate.sonoff3`
- **Lampen**: `light.alle_lichter_2`, `light.stehlampe_bibi`, etc.

## Navigation
- Footer: Prev/Home/Next Buttons
- Swipe zwischen Seiten

## Anpassungen
- Orange Buttons: `layouts/styles/` & Widget-Templates
- Neue Seiten: YAML in Root + include in `guition-35-display.yaml`

Fork von: https://github.com/sokomania1/esp_lvg_eigen
