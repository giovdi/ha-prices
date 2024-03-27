# Home Assistant Blueprint - Italian electricity tariffs

[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)

## Setup

⚠️ Please read everything carefully, otherwise the automation won't work properly!

### 1. Basic configuration

Add the following code to the `configuration.yaml` to set up tariffs easily:

**Tariffs**: we'll use them in Energy management to set prices for each counter
```
input_number:
  tariff_f1:
    name: Electricity Tariff F1
    icon: mdi:cash
    min: 0
    max: 5
    step: 0.001
    mode: box
    unit_of_measurement: EUR/kWh
  tariff_f2:
    name: Electricity Tariff F2
    icon: mdi:cash
    min: 0
    max: 5
    step: 0.001
    mode: box
    unit_of_measurement: EUR/kWh
  tariff_f3:
    name: Electricity Tariff F3
    icon: mdi:cash
    min: 0
    max: 5
    step: 0.001
    mode: box
    unit_of_measurement: EUR/kWh
```

**Current Time Band**: we'll use it in case of automation failure, to change the current band manually
```
input_select:
  tariff_current_band:
    name: Electricity Current Time Band
    icon: mdi:timeline-clock
    options:
      - F1
      - F2
      - F3
```

**Current price sensor**: it stores the current price that can be used anywhere
```
sensor:
  - platform: template
    sensors:
      tariff_price_current:
        friendly_name: Electricity Current Price
        unit_of_measurement: EUR/kWh
        device_class: monetary
        value_template: >
          {% if is_state("input_select.tariff_current_band", "F1") %}
            {{ states('input_number.tariff_f1') }}
          {% elif is_state("input_select.tariff_current_band", "F2") %}
            {{ states('input_number.tariff_f2') }}
          {% elif is_state("input_select.tariff_current_band", "F3") %}
            {{ states('input_number.tariff_f3') }}
          {% else %}
            0
          {% endif %}
```

After changing the `configuration.yaml` file, restart Home Assistant

### 2. Set up tariff prices

Go to your Helpers page and configure _Electricity Tariff F1_,  _Electricity Tariff F2_ and  _Electricity Tariff F3_ helpers.

Shortcut:

[![Open your Home Assistant instance and show your helper entities.](https://my.home-assistant.io/badges/helpers.svg)](https://my.home-assistant.io/redirect/helpers/)

### 3. Add Unit meters

Now you can add all the Unit meters to Home Assistnat through the helpers' page to automate them.

**Don't forget to set _F1_, _F2_ and _F3_ as tariffs on each meter!**

#### - One or few devices

If you have a small bunch of devices, you may prefer adding all of them to the Energy Panel.

If you'd like to set up the Unit meters through YAML, use the following example:

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

If you have so many Unit meters, you can use a separate `utility_meters.yaml` file and import it into `configuration.yaml` with this directive:  
`utility_meter: !include utility_meters.yaml`.  
Remove the `unit_meter:` root in this case.

If you configure the Unit meters by YALM, restart the Home Assistant.

#### - Multiple devices

If you, like me, have dozens of Shelly active, you may prefer using a single Unit meter to keep track of the consumption and then a sensor for each device.

Unfortunately, at this time (2023.04) it's not possible to track single devices without adding them to the Energy Panel.  
But I have a good workaround, have a look at the [![giovdi - ha-electricity-cost](https://img.shields.io/static/v1?label=giovdi&message=ha-electricity-cost&color=blue&logo=github)](https://github.com/giovdi/ha-electricity-cost) Blueprint.

If this fits you, you can use the following YAML to set up a single counter for all the devices:

TODO

### 4. Set up the integration

Add this integration to your Home Assistant, using this button:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fgiovdi%2Fha-prices%2Fedit%2Fmain%2Fhome_tariffs.yaml)

Finally, set up the automation and you're ready to go!

### 5. Add meters to the Energy Panel

Now you can track your consumption and your energy footprint with the Energy Panel!

## Issues or suggestions?

Feel free to open an Issue of fork this Blueprint to improve it!
