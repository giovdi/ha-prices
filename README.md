Work in progress

Add the following code to the `configuration.yaml` to set up the tariffs easily:

```

# Tariffs
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

input_select:
  tariff_current_band:
    name: Electricity Current Time Band
    icon: mdi:timeline-clock
    options:
      - F1
      - F2
      - F3
    initial: F1

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
