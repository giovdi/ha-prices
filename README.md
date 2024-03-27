[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip) 95% - testing, almost stable!

# Setup

## 1. Basic configuration

Add the following code to the `configuration.yaml` to set up tariffs easily:

```
# Tariffs - will be used in the electricity configuration
input_number:
  tariff_f1:
    name: Electricity Tariff F1
    icon: mdi:cash
    initial: 0
    min: 0
    max: 5
    step: 0.001
    mode: box
    unit_of_measurement: EUR
  tariff_f2:
    name: Electricity Tariff F2
    icon: mdi:cash
    initial: 0
    min: 0
    max: 5
    step: 0.001
    mode: box
    unit_of_measurement: EUR
  tariff_f3:
    name: Electricity Tariff F3
    icon: mdi:cash
    initial: 0
    min: 0
    max: 5
    step: 0.001
    mode: box
    unit_of_measurement: EUR

# Select - will be used to help change the current tariff in case automation missing
input_select:
  tariff_current_band:
    name: Electricity Current Time Band
    icon: mdi:timeline-clock
    options:
      - F1
      - F2
      - F3
    initial: F1

# Sensor - Current price that can be used anywhere
sensor:
  - platform: template
    sensors:
      tariff_price_current:
        friendly_name: Electricity Current Price
        entity_id: sensor.tariff_current_price
        unit_of_measurement: EUR
        icon_template: "{{ 'mdi:cash' }}"
        value_template: >
          {% if states('sensor.tariff_current_band') == 'F1' %}
            {{ input_number.tariff_f1 }}
          {% elif states('sensor.tariff_current_band') == 'F2' %}
            {{ input_number.tariff_f2 }}
          {% elif states('sensor.tariff_current_band') == 'F3' %}
            {{ input_number.tariff_f3 }}
          {% else %}
            0
          {% endif %}
```

After changing the `configuration.yaml` file, restart Home Assistant

## 2. Set up tariff prices

Go to your Helpers page and configure _Electricity Tariff F1_,  _Electricity Tariff F2_ and  _Electricity Tariff F3_ helpers.

Shortcut:

[![Open your Home Assistant instance and show your helper entities.](https://my.home-assistant.io/badges/helpers.svg)](https://my.home-assistant.io/redirect/helpers/)

## 3. Add unit meters

Add all the unit meters to Home Assistnat through the helpers' page to automate them.

Don't forget to set _F1_, _F2_ and _F3_ as tariffs on each meter.

If you'd like to set up the unit meters through YAML, use the following example:

```
unit_meter:
  meter_tv:
    unique_id: meter_tv
    name: Counter TV
    source: sensor.shellyplus1pm_aaaaaaaaaaaa_switch_0_energy
    cycle: monthly
    tariffs:
      - F1
      - F2
      - F3
    always_available: true
```

If you have so many unit meters, you can use a separate `utility_meters.yaml` file and import it into `configuration.yaml` with this directive:  
`utility_meter: !include utility_meters.yaml`.  
Remove the `unit_meter:` root in this case.

If you configure the unit meters by YALM, restart the Home Assistant.

## 4. Set up the integration

Add this integration to your Home Assistant, using this button:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fgiovdi%2Fha-prices%2Fedit%2Fmain%2Fhome_tariffs.yaml)
