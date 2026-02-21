# Installation Guide

## 1. Voraussetzungen
- ESPHome 2024.11.0 oder neuer
- Home Assistant mit konfigurierten Entities:
  - `weather.freudenstadt`
  - `light.alle_lichter_2`, `light.stehlampe_bibi`, etc.
  - `sensor.snf_*_temperature` (siehe esphome_entities.json)
  - `climate.sonoff1`, `climate.sonoff2`, `climate.sonoff3`

## 2. Repo klonen
```bash
git clone https://github.com/sokomania1/guiton-touchscreen-esphome.git
cd guiton-touchscreen-esphome
```

## 3. Secrets konfigurieren
```bash
cp secrets.yaml.example secrets.yaml
nano secrets.yaml  # WiFi Credentials eintragen
```

## 4. Fonts hinzufügen (wichtig!)
Lade folgende Fonts herunter und kopiere sie nach `layouts/fonts/`:
- **Roboto-Regular.ttf** (Google Fonts)
- **materialdesignicons-webfont.ttf** (Material Design Icons)

Oder kopiere aus deinem Original-Repo: `esp_lvg_eigen/layouts/fonts/`

## 5. Hardware-Config anpassen
Bearbeite `guition-35-display.yaml`:
- `board:` Zeile auf dein ESP32-Board anpassen
- Display-Config hinzufügen (SPI/I2C Pins)

Beispiel Display-Config (nach `esp32:` Block):
```yaml
spi:
  clk_pin: GPIO14
  mosi_pin: GPIO13

display:
  - platform: ili9xxx
    model: ILI9488
    cs_pin: GPIO15
    dc_pin: GPIO2
    rotation: 90
    update_interval: 1s
```

## 6. Entity-IDs prüfen
Öffne `temperaturen.yaml` und ersetze Platzhalter:
```yaml
entity_id: sensor.snf_wohnzimmer_temperature  # Deine echte Entity-ID
```

Liste aller Entities in `esphome_entities.json` (falls vorhanden).

## 7. Kompilieren & Flashen

### Option A: ESPHome Dashboard
1. ESPHome Dashboard öffnen
2. "+ New Device" → "From existing YAML"
3. `guition-35-display.yaml` wählen
4. "Install" → "Wireless" (nach Erstflash)

### Option B: CLI
```bash
esphome compile guition-35-display.yaml
esphome upload guition-35-display.yaml
```

## 8. Troubleshooting

### Fehler: "Font not found"
→ Fonts in `layouts/fonts/` ablegen (siehe Schritt 4)

### Display bleibt schwarz
→ Display-Pins in YAML prüfen (SPI/I2C)

### Entities zeigen "unavailable"
→ Home Assistant API-Verbindung prüfen:
```yaml
api:
  encryption:
    key: !secret api_encryption_key
```

### Lampen-Slider reagieren nicht
→ `brightness` Attribut unterstützen? Teste:
```bash
ha sensor get light.stehlampe_bibi --attributes
```

## 9. Anpassungen

### Weitere Sensoren hinzufügen (temperaturen.yaml)
Dupliziere Sensor-Block und ändere IDs:
```yaml
- platform: homeassistant
  id: snf_temp_7  # Neue ID
  entity_id: sensor.snf_schlafzimmer_temperature
  on_value:
    - lvgl.label.update:
        id: temp_sensor_7  # Neue Label-ID
        text: !lambda ...
```

### Farben ändern
Orange Theme (`0xFF8C00`) in YAMLs suchen & ersetzen.

### Neue Seiten
1. Neue YAML erstellen (z.B. `medien.yaml`)
2. In `guition-35-display.yaml` einbinden:
   ```yaml
   packages:
     medien: !include medien.yaml
   ```

## Support
Fragen? Issues öffnen: https://github.com/sokomania1/guiton-touchscreen-esphome/issues
