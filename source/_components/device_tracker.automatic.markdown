---
layout: page
title: "Automatic"
description: "Instructions for how to integrate Automatic ODB readers into Home Assistant."
date: 2015-08-28 19:00
sidebar: true
comments: false
sharing: true
footer: true
logo: automatic.png
ha_category: Presence Detection
ha_release: 0.28
ha_iot_class: "Cloud Push"
---


The `automatic` platform offers presence detection by retrieving your car's information from the [Automatic](http://automatic.com/) cloud service.

To use Automatic with Home Assistant, first you must [create a free development account](https://developer.automatic.com/). Automatic will generate a Client ID and Secret for you to use in your Home Assistant configuration. You will also need to update your Event Delivery preferences to ensure Home Assistant can receive updates. On the developer page, under App Settings / Event Delivery, select "Websocket" for Event Delivery Preference.

Home Assistant will also take advantage of `scope:current_location` if available. This will allow Home Assistant to receive periodic location updates during a trip. In order to use this functionality, you must request the scope for your application from Automatic. Once `scope:current_location` is available, Home Assistant will automatically make use of it after the next restart.

Once your developer account is created, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
device_tracker:
  - platform: automatic
    client_id: 1234567
    secret: 0987654321
    username: your@email.com
    password: your_password
    devices:
      - 2007 Honda Element
      - 2004 Subaru Impreza
```

Configuration variables:

- **client_id** (*Required*): The OAuth client id (get from https://developer.automatic.com/).
- **secret** (*Required*): The OAuth client secret (get from https://developer.automatic.com/).
- **username** (*Required*): The username associated with your ODB reader.
- **password** (*Required*): The password for your given ODB reader account.
- **devices** (*Optional*): The list of vehicle display names you wish to track. If not provided, all vehicles will be tracked.

Home Assistant will also fire events when an update is received from Automatic. These can be used to trigger automations, as shown in the example below. A list of available event types can be found in the [Automatic Real-Time Events documentation](https://developer.automatic.com/api-reference/#real-time-events).


```yaml
# Example automatic event automation
automation:
  - trigger: 
      - platform: event
        event_type: automatic_update
        event_data:
          type: "ignition:on"
    action:
      - service: light.turn_off
```

See the [device tracker component page](/components/device_tracker/) for instructions how to configure the cars to be tracked.
