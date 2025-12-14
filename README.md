# ESP32 E-Ink Weather Dashboard

This project is an ESP32-based weather dashboard that displays information on a 7.5-inch e-ink display. It is based on [esphome-weatherman-dashboard](https://github.com/Madelena/esphome-weatherman-dashboard) by Madelena.

## Project Journey

This project began as a V1 weather dashboard, pulling data from Home Assistant to create a daily "at-a-glance" weather display. The goal was to have a low-power, high-contrast display that was always on.

The project then evolved into V2, a real-time transit display for Seattle's Link light rail, using data from the OneBusAway API. This required a significant refactor of the original codebase to handle the new data source and display layout.

### Key Learnings & Modifications

During the transition to the transit display, a couple of key hardware and configuration challenges emerged:

*   **Incorrect Display Model**: The initial configuration used a standard display model, but the specific Waveshare 7.5" V2 display required the `7.50inV2alt` model in ESPHome for proper operation.
*   **Inverted BUSY Pin**: The BUSY pin on this particular display is inverted. The ESPHome configuration required `inverted: true` for the `busy_pin` to ensure the ESP32 could correctly read the display's status.

Mine looks like this:  

![E-Ink Display](demo/demo.jpg)

## Hardware

This project uses the following hardware:
- ESP32 Board - [Amazon Link](https://www.amazon.com/dp/B07M5CNP3B)
- Waveshare 7.5inch E-Ink Display V2 (800×480 Resolution, SPI Interface) - [Amazon Link](https://www.amazon.com/dp/B075R4QY3L) 

## Prerequisites

### Setup ESPHome

Before you begin, you need to install ESPHome:

- Install ESPHome following the [official documentation](https://esphome.io/guides/installing_esphome.html)
- Home Assistant users can simply install the ESPHome add-on from the Add-on Store

## Setup and Configuration

### Initial Setup

1. Create a `secrets.yaml` file in the same directory as `eink.yaml`
2. Add your WiFi credentials to the `secrets.yaml` file:

```yaml
wifi_ssid: [YOUR_WIFI_SSID]
wifi_password: [YOUR_WIFI_PASSWORD]
```

### Updating and Flashing

To update and flash the configuration to your ESP32:

```bash
esphome run eink.yaml
```

### Wiring Instructions

Connect the ESP32 to the Waveshare E-Ink display using the following pin configuration. 

**Note**: The `BUSY` pin for this display is inverted. See the `eink.yaml` for the correct configuration (`inverted: true`).

| ESP32 Pin | E-Ink Display |
|-----------|---------------|
| GPIO13    | CLK (SPI Clock) |
| GPIO14    | MOSI (SPI Data) |
| GPIO15    | CS (Chip Select) |
| GPIO27    | DC (Data/Command) |
| GPIO25    | BUSY |
| GPIO26    | RST (Reset) |
| GPIO12    | Power Supply Pin |



## Project Files

- `eink.yaml`: ESPHome configuration for the e-ink display
- `configuration.yaml`: Home Assistant configuration for weather data integration

## Troubleshooting

### Display Issues
If you experience ghosting or the screen not displaying correctly, check the mode switch on the e-ink display board. Try switching from mode B to mode A - mode A has been confirmed to work better with this setup.

### Home Assistant Hourly Weather Entities
The weather forecast functionality requires proper setup in Home Assistant. You will need to:
1. Configure a weather integration with API access in Home Assistant
2. Add the necessary forecast sensors to your Home Assistant `configuration.yaml` file
3. Make sure the entity IDs in your `eink.yaml` file match those in Home Assistant

## Credits
This project is based on [esphome-weatherman-dashboard](https://github.com/Madelena/esphome-weatherman-dashboard).



