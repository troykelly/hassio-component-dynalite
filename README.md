
# Home Assistant Component - Philips Dynalite to MQTT

> Bridging the RS485 world of Philips Dynalite with the rest of the home
> automation world via MQTT.

Communicates with Philips Dynalite systems via an IP to RS485 controller.

This is a component for use with [Home Assistant](https://home-assistant.io/components/).

**Do not purchase a Philips Dynalite IP<->485 Bridge - just get something
reliable and robust yourself.**

## Installation
Until somebody wants to integrate this into Home Assistant - you will need to download or clone this repo and place the `dynalite` folder in your `custom_components` folder. Configuration is all via Home Assistant's config files - so please don't try and edit the contents of the component itself.

## Configuration
This is a messy one to configure because getting information out of Dynalite is near on impossible. You must know the configuration and topology of your network to be able to integrate with it - this isn't a "*plug and play*" scenario.

### Communication
The host and port of the RS485 to IP gateway is defined at the root of the sensor config, along with the MQTT discovery topic and device topic.
```yaml
sensors:
    - platform: dynalite
      log_level: DEBUG		#Turn this off when you have things working
      host: 10.10.10.10     #IP Address of IP to 485 Gateway
      port: 12345           #Port for gateway typically 12345
      discovery_topic: homeassistant
      device_topic: dynalite
```

### Areas
Areas are define under the `area` tag. Dynalite areas are numbered 1 through 255, you only need to define the areas you are using. At the very least every area needs a `name`.

Area's can have a `nodefault` tag (no default) which prevents them inheriting the default presets defined later.

Area's can also have `preset`'s defined that either augment or replace the default presets.

Area's can have a `fade` time defined in seconds that will be used as a default fade time for preset changes in that area.

To use the dynalite component you will need to add the following to your
configuration.yaml file.
```yaml
      area:
        '1':
          name: Unused
          nodefault: true
        '2':
          name: Garage
        '3':
          name: Bedroom
          fade: 5          
        '4':
          name: Living
          preset:
            '5':
              name: Table
            '6':
              name: Lounge
            '7':
              name: Feature
            '8':
              name: Relax
```

### Presets
Presets are optionally defined under `area`'s (as above) and are required to be defined in the root of the config under `preset`.

Preset's that you wish to call must be defined with at least a name.

Preset's may optionally also be defined with a `fade` time in seconds.
```yaml
      preset:
        '1':
          name: 'On'
          fade: 2
        '2':
          name: 70%
          fade: 2
        '3':
          name: 30%
          fade: 2
        '4':
          name: 'Off'
          fade: 2
        '9':
          name: Special Scene
          fade: 8
```

### Defaults
All default settings (which can be overridden per above) are defined here.

The default fade time is configured via the `fade` tag in seconds.
```yaml
      default:
        fade: 2
```

### Full Example Configuration
`configuration.yaml`
```yaml
sensors:
    - platform: dynalite
      log_level: DEBUG		#Turn this off when you have things working
      host: 10.10.10.10     #IP Address of IP to 485 Gateway
      port: 12345           #Port for gateway typically 12345
      discovery_topic: homeassistant
      device_topic: dynalite
      area:
        '1':
          name: Unused
          nodefault: true
        '2':
          name: Front Deck
        '3':
          name: Lower Hall
        '4':
          name: Living
          preset:
            '5':
              name: Table
            '6':
              name: Lounge
            '7':
              name: Feature
            '8':
              name: Relax
        '5':
          name: Kitchen
          preset:
            '5':
              name: General
            '6':
              name: Cook
        '6':
          name: Theatre
          preset:
            '5':
              name: General
            '6':
              name: Rear
            '7':
              name: Side
            '8':
              name: Movie
        '7':
          name: Garage Foyer
        '8':
          name: Bar
          preset:
            '5':
              name: General
            '6':
              name: Service
            '7':
              name: Wall
        '9':
          name: Bar Deck
        '10':
          name: Dining
        '11':
          name: Master Bed
          preset:
            '5':
              name: General
        '12':
          name: Master Ensuite
          preset:
            '5':
              name: Mirror
            '6':
              name: Mirror Heat
            '7':
              name: Mirror Heat Fan
            '8':
              name: Romance
        '13':
          name: External
        '14':
          name: Upper Balcony
        '15':
          name: Garage
        '16':
          name: Front Flood
        '17':
          name: Bedrooms
        '18':
          name: Rear Flood
          fade: 0
        '221':
          name: DOOR Garage Roller
          fade: 0
          nodefault: true
          preset:
            '4':
              name: Secure
            '15':
              name: Insecure
        '222':
          name: DOOR Vehicle Gate
          fade: 0
          nodefault: true
          preset:
            '4':
              name: Secure
            '15':
              name: Insecure
        '223':
          name: DOOR Pedestrian Gate
          fade: 0
          nodefault: true
          preset:
            '4':
              name: Secure
            '15':
              name: Insecure
        '224':
          name: DOOR Front Door
          fade: 0
          nodefault: true
          preset:
            '4':
              name: Secure
            '15':
              name: Insecure
        '225':
          name: DOOR Bar Deck
          fade: 0
          nodefault: true
          preset:
            '4':
              name: Secure
            '15':
              name: Insecure
        '254':
          name: Night State
          fade: 0
          nodefault: true
          preset:
            '1':
              name: Night
            '4':
              name: Day
        '255':
          name: M1 Comms
          fade: 0
          nodefault: true
          preset:
            '4':
              name: M1 Idle
            '20':
              name: DYNPIR Office Corner
            '21':
              name: DYNPIR Bar Deck
            '22':
              name: DYNPIR Pool Deck
            '23':
              name: DYNPIR Garage Drive
            '25':
              name: ARM External Away
            '26':
              name: ARM External Home
            '27':
              name: DISARM External
            '28':
              name: ARM Garage Away
            '29':
              name: ARM Garage Home
            '30':
              name: DISARM Garage
            '31':
              name: ARM Living Away
            '32':
              name: ARM Living Home
            '33':
              name: DISARM Living
            '34':
              name: ARM Bed Away
            '35':
              name: ARM Bed Home
            '36':
              name: DISARM Bed
            '37':
              name: Fire Alarm
            '38':
              name: REED Garage Drive
            '39':
              name: PIR Garage
            '40':
              name: PIR Foyer
            '41':
              name: PIR Dining
            '42':
              name: PIR Theatre
            '43':
              name: PIR Study
            '44':
              name: PIR Living
            '45':
              name: PIR Guest
            '46':
              name: PIR MBed Hall
            '47':
              name: PIR Bar
            '48':
              name: PIR Upper Bed
            '49':
              name: PIR Front Drive
            '50':
              name: REED Front Door
            '51':
              name: REED Bar Deck
            '52':
              name: REED Rear Door
            '53':
              name: REED Pool Deck
            '54':
              name: REED Living Door
            '55':
              name: Fn Arm Stay From Bed
            '56':
              name: Fn Arm Stay From Living
            '58':
              name: Panic
      preset:
        '1':
          name: 'On'
          fade: 2
        '2':
          name: 70%
          fade: 2
        '3':
          name: 30%
          fade: 2
        '4':
          name: 'Off'
          fade: 2
        '9':
          name: Home Idle
          fade: 8
        '10':
          name: Home Motion
          fade: 8
        '11':
          name: Away Idle
          fade: 8
        '12':
          name: Away Motion
          fade: 8
        '13':
          name: Sleep Idle
          fade: 8
        '14':
          name: Sleep Motion
          fade: 8
        '64':
          name: Reset Automation
          fade: 0
      default:
        fade: 2
```
