(Hundreds of possible actions!!) Works with MQTT Directly bypassing both Legacy and non-Legacy trigger issues. This Blueprint uses a Zigbee2MQTT built sensor to sort out the 58+ unique commands (118+ total) available from the Xiaomi Magic Cube Remote. (Some unique commands listed as + are not counted & available thru templating only. See the related document.)

## 📑 Changelog

* **2023-01-26**: First Release.  Based on Zigbee2MQTT-Xiaomi_Cube_Controller_MQTT_Triggered Blueprint
* **2023-01-26-1**: Add in the forgotten Scene Mode Hold function

## 📩 * Version Updates

Updates will be published on my [GIT repository](https://github.com/SirGoodenough/HA_Blueprints) with the rest of my Home Assistant Blueprint collection.

🔗 There is not an official version control system for Blueprints.  However I have found something that comes pretty close.  It is not perfect, but for **MOST** Blueprints, it does just fine.  I encourage you to check this script out and use it to easily check if I have updated this blueprint.

[koter84 Blueprint Update Script](https://gist.github.com/koter84/86790850aa63354bda56d041de31dc70#file-readme-md)

## 📩 Get Started

Updates will be published on my [GIT repository](https://github.com/SirGoodenough/HA_Blueprints) with the rest of my Home Assistant Blueprint collection.

I have 3 of these cubes. If you aren't careful they trigger and do all kinds of things just sitting on the desk. Awesome when you want it to do this, but a HUGE PITA if you don't.

A friend of mine came up with this and it works awesome. He didn't want people hassling him about remixes and such, so he let me put it on the Thingiverse. I printed one for each of my cubes and because it's now parked at a diagonal it will not trigger, unless you knock it on the floor or something. I highly suggest you print yourself one of these or find someone to print one for you.... It will make cube life much simpler...
https://www.thingiverse.com/thing:4536778

### Option 1: My Home Assistant

Click the badge to import this Blueprint

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FSirGoodenough%2FHA_Blueprints%2Fblob%2Fmaster%2FAutomations%2FZigbee2MQTT-Aqara-Magic-Cube-T1-Pro-CTP-R01-Xiaomi-Lumi.yaml)

# Please Click the 🧡 at the end of the Top Post if you find this Useful

### Option 2: Direct Link

Copy this link if you want to import the blueprint in your installation.
```https://github.com/SirGoodenough/HA_Blueprints/blob/master/Automations/Zigbee2MQTT-Aqara-Magic-Cube-T1-Pro-CTP-R01-Xiaomi-Lumi.yaml```

https://github.com/SirGoodenough/HA_Blueprints/blob/master/Automations/Zigbee2MQTT-Aqara-Magic-Cube-T1-Pro-CTP-R01-Xiaomi-Lumi.yaml

## 📖 Description

**NOTE:** This blueprint is for the PRO cube version ONLY and is not fully compatible
with the original version of the cube.  Links for those are in my [GIT repository](https://github.com/SirGoodenough/HA_Blueprints).
As of the time of this writing, you need the
PRO version of the cube running this Z2M Extension:
[CTPR01.js Z2m Extension Link](https://github.com/JJPro/CTP-R01-converter)

This cube has an ''Action'' mode (Shown below with this graphic: 🫳 ACTION MODE ONLY 🫳 )

and a ''Scene'' mode (Shown below with this graphic: 👀 SCENE MODE ONLY 👀 ).

Make sure the cube is in the mode you think it is in when triggering it.
The functions are separate.  IE Action - slide will do something different from Scene - slide.

The split out of functions gives you the ability to assign local scripts or functions
to do the things you want the remote to do.

Functions that are left empty will simply do nothing.

#### 🍎 There is a set of 36 + 18 actions that will trigger on specific actions on specific sides that are listed as **Group 1 🍎**

#### 🍊 There is a set of 6 + 3 actions that will trigger on specific actions on *ANY* side that are listed as **Group 2 🍊**

#### 🍐 There is a set of 30 + 30 actions that will trigger on cube flips to & from specific sides that are listed as **Group 3 🍐**

#### 🍩 There are 5 actions (shake & throw for each mode & hold in scene mode) that only occur once in each mode & are OK to be combined with any other group in that mode. 🍩

#### NOTICE: This cube *can* be triggered 118+ ways, but only 58+ are unique.

### IF YOU SEE --> TemplateError: Must provide a device or entity ID

If you get an error like that, The friendly_name in Z2M likely does not match the 
friendly_name on HA. To fix go into the Z2M ```Open web UI``` and set the 
friendly_name there. Setting this in just HA or in Z2M without ticking the update HA
box will cause this.

There is sample code to make the template sensor in the help file on GitHib named the same as this one and in the community page related to this.

#### Seeing the cube commands for training the operator

Within this blueprint there is an event handler that will latch the last command that the blueprint finds and sends that to the event buss. From there a simple Template sensor can grab it and show you the last action sent. This will help when setting up new functions and to troubleshoot strange behaviors.  Add an entity card in your dashboard for sensor.cube_last_action to see what actions occur as you move the cube.

```yaml
template:
  - trigger:
      - platform: event
        event_type: cube_last_action
    sensor:
      - name: "Cube Last Action"
        unique_id: Any-unique-string-here-MUST-be-unique
        icon: mdi:eye-refresh-outline
        attributes:
          friendly_name: "Cube Action"
        state: >
          {{ trigger.event.data.friendly_name }} - 
          {{ trigger.event.data.action }} - 
          {{ trigger.event.data.side }} frm 
          {{ trigger.event.data.last_side }}

```

#### Event Sensor in Action

![Sample Script Generation Dashboard Entry](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/cubesensor.gif?raw=true "Sensor in action")

If you wish to 'store' these events you can add this sensor to recorder and it will save them for you.

#### Programming actions

My 'suggestion' is that you do separate scripts for most, if not all of the actions you generate here. If you are using the UI editor for the simple things you are fine, but for more complicated things scripts may work better for you. This is my opinion and how I am using it, to each their own.  See my example dimmer script below...

#### Getting Tap and Flip actions to work

I have had reports of the 'tap' action working.  It was due to the lack of instructions provided by the manufacturer of the cube.  Tap actions on the cube are initiated by sharply tapping the cube 2x on a hard surface like this:

![Demo of Tap Action](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/Tap_Action.gif?raw=true "Demo of Tap Action")

In a similar manner, flips need to tap the surface at the end of the 90 or 180 flip.  Setting up the template sensor above will help train you in the force needed for all the actions.
_________________________

> This was 'forked' from 'https://community.home-assistant.io/t/z2m-xiaomi-cube-controller/263006'
V1.2 project authored by luckypoppy and the friends he pulled together to create the base.  
>
> I sincerely thank Him (Them) for their work. I felt there needed to be more documentation for rookie users to properly set this up. I had quite a few questions and when I saw a few questions in that chat from people struggling, I wanted to help. I also had a better idea for troubleshooting info.

_________________________

### ⚙️ Usage

#### 🛠 Installation

* Open Home Assistant with administrator privileges and on a Lovelace screen, click anywhere in the main entity area and type the letter ‘c’.  A selection box should pop up.  Type blue and select the button to navigate to blueprints.  You can also find blueprints by selecting configuration from the left menu and then blueprints from the center menu.
* Once there, click on the ‘Import Blueprint’ button in the lower right side of the main screen.
* In the ‘URL of the blueprint’ line type or paste in the URL of my Blueprint. I have the blueprint stored on my Public GitHub:

> ◦ https://github.com/SirGoodenough/HA_Blueprints

#### 🧬 To make the blueprint work it will need

To make the Blueprint work you will need a functional Magic Cube T1-PRO integrated to Home Assistant thru Zigbee2MQTT.

This version of the Blueprint uses MQTT to deal with cube interface duties.  This means that if you have Legacy triggers enabled on your setup or not, it will still work.  It also creates it's own number helper to track a variable needed to do all the tricks.  You as the user will not have to deal with that either.

You will also need the correct MQTT topic to talk to your device.

This blueprint has been known to freak out when there are spaces or any symbols in the MQTT Topic.  Make sure there are no spaces and there are all ASCII characters (upper case, lower case, & numbers) in the topic, (```  /  ``` is ok) (These are bad: _ = - > < .)
If there are, you will need to change the name of the cube to remove those characters.
>> Wise advice from: [HiveMQ](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/):
>>
>> ### Never use spaces in a topic
>>
>> A space is the natural enemy of every programmer. When things are not going the way they should, spaces make it much harder to read and debug topics. Just because something is allowed, doesn’t mean it should be used. UTF-8 has many different white space types, such uncommon characters should be avoided.
>>
>> ### Use only ASCII characters, avoid non printable characters
>>
>> Because non-ASCII UTF-8 characters often display incorrectly, it is very difficult to find typos or issues related to the character set. Unless it is absolutely necessary, we recommend avoiding the use of non-ASCII characters in a topic.

The topic can be found by going into devices and finding your cube device. [![Open your Home Assistant instance and show your devices.](https://my.home-assistant.io/badges/devices.svg)](https://my.home-assistant.io/redirect/devices/)

![Find Device](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/FindDevice.png?raw=true "Find Device")

Then clicking on ```MQTT INFO```:

![Click MQTT INFO](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/MQTTInfo.png?raw=true "Click MQTT INFO")

Then looking for the subscribed topic that looks like this... ```zigbee2nqtt/[your device name here]```:

![Find the subscribed topic](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/MQTTTopic.png?raw=true "Find the subscribed topic")

That is what belongs in the the topic question you get when setting up a new automation with this blueprint.

#### Template error

If you see an error similar to: **Error while executing automation automation.magic_cube_automations: TemplateError: Must provide a device or entity ID** there is a way to fix it.  It happens (usually) because the friendly_name that is in HA does not match the friendly_name that is in Z2M.
In order to fix I suggest you go into the Z2M web UI and change the friendly name.  Below is how to do this if you are using the Z2M Addon.

1. Open the Z2M Web UI:
![Open the Z2M Web UI](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/Z2M-WebUI.png?raw=true)

2: Select the change name icon in the row of the cube device:
![Select the change namr icon in the row of the cube device](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/Z2M-ChangeName.png?raw=true)

3: Change the name and be certain to tick the box to update HA at the same time:
![Change the name and be certain to tick the box to update HA at the same time](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/Z2M-Devicename.png?raw=true)

After this find the topic again the same way as above and it should work.

> 1. Click on 'Create Automation'  [![Open your Home Assistant instance and show your automations.](https://my.home-assistant.io/badges/automations.svg)](https://my.home-assistant.io/redirect/automations/) and 'Use Blueprint'
> 2. Add a Description so you can tell what this one is for
> 3. Use the Drop-downs to select the Entities for the listed purposes

Code Samples for the items in the next few sections can be found in my Home-Assistant Config here:  

* https://github.com/SirGoodenough/Home-Assistant-Config/blob/master/automation2/Z2M-Xiaomi_Cube_A2.yaml
* https://github.com/SirGoodenough/Home-Assistant-Config/blob/master/script2/cube_script.yaml

#### Setting Action Mode or Scene Mode on Cube

To change the mode of the cube, there is a switch in the configuration section of the Device panel. This can be pulled into the Dashboard wherever you like as well. There is a configuration window, opens once an hour on itself, only during which the cube will respond to mode switch. Mode switch will be scheduled to take effect when the window becomes available. You can also give it a throw action to force a respond! Otherwise, you may open lid and click LINK once to make the cube respond immediately. [Hard Switch]: Open lid and click LINK button 5 times.

![Change the Cube Mode](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/pro_cube_mode_change.png?raw=true)

## 🌞 Dimmer Control

If you are looking for a dimmer control to change brightness based on rotation, here's something I cobbled together from other community posts here and there. ( Credit https://community.home-assistant.io/u/yourigh/summary and others )

I did this with all the complicated stuff in a script that is called with data from the blueprint automation. Then the complicated part is all in 1 place and there is only 1 copy of it. The same script works for both increase and decrease of brightness because the angle in the cube goes positive when turning clockwise and negative when going counter clockwise.

In the blueprint automation:
[![Open your Home Assistant instance and show your automations.](https://my.home-assistant.io/badges/automations.svg)](https://my.home-assistant.io/redirect/automations/)

```yaml
rotate_cw_face_0:
  - service: script.cube_dimmer_control
    data:
      angle: '{{ trigger.payload_json.action_angle | float(0.0)}}'
      light: light.bulb1
rotate_ccw_face_0:
  - service: script.cube_dimmer_control
    data:
      angle: '{{ trigger.payload_json.action_angle | float(0.0)}}'
      light: light.bulb1
```

Then this is the script that's called to do the heavy lifting.   It works for both CW and CCW cube rotations.

In the script integration:
[![Open your Home Assistant instance and show your scripts.](https://my.home-assistant.io/badges/scripts.svg)](https://my.home-assistant.io/redirect/scripts/)

```yaml
cube_dimmer_control:
  description: Template Dimmer Control
  variables:
    angle:
    light:
  sequence:
    - service: light.turn_on
      data_template:
        entity_id: "{{ light }}"
        brightness_pct: >
          {% set step_size = angle * 0.4 %}
            {# Get brightness as a percent. #}
          {% set cb = (state_attr( light, 'brightness') | float(10) / 255.0) * 100.0 %}
          {% set new_brightness = cb | int(10) + step_size %}
          {% if new_brightness < 5 %}
            {# If it gets really low set to 0. Adjust if needed for your lights. #}
            0
          {% elif new_brightness <= 10 %}
            {# If it's not quite 0, set to a minimum working brightness. #}
            10
          {% elif 91 <= new_brightness < (90 + step_size) %}
            {# If it's almost full brightness, set to 100%. #}
            100
          {% else %}
            {# Send actual calculated value. #}
            {{ new_brightness }}
          {% endif %}
```

#### What this does...

* The script reduces the angle number to 40% of the rotation angle (you can change this, but 40% works well for my needs).
* It then grabs the current brightness from the light entity (as a % of the full scale 255 number).
* The new_brightness target is then calculated.
* It then checks if the light is already off, and if so, leaves it off.
* It makes sure the new_brightness is above, in my case, 10% so all the lights come on.
* It then makes sure that if new_brightness >90%, it is set to 100% and not over that.
* Finally it provides the calculated brightness %.

## 🚦 Color Control

I also extended this to controlling the color one octet (color) at a time. Each color will use both rotations on one side of the cube.  Colors are changed one at a time (Red or Green or Blue) and change the amount (positive ot negative) based on how far you rotate the cube.

In the blueprint automation:
[![Open your Home Assistant instance and show your automations.](https://my.home-assistant.io/badges/automations.svg)](https://my.home-assistant.io/redirect/automations/)

```yaml
## Side 5 green
rotate_cw_face_5:
  - service: script.cube_green_color_control
    data:
      angle: '{{ trigger.payload_json.action_angle | float(0.0)}}'
      light: light.grp_studio
rotate_ccw_face_5:
  - service: script.cube_green_color_control
    data:
      angle: '{{ trigger.payload_json.action_angle | float(0.0)}}'
      light: light.grp_studio
```

Then this is the script that's called to do the heavy lifting. It works for both CW and CCW cube rotations.

In the Script Integration:
[![Open your Home Assistant instance and show your scripts.](https://my.home-assistant.io/badges/scripts.svg)](https://my.home-assistant.io/redirect/scripts/)

```yaml
cube_green_color_control:
  description: Control the green part of the color
  variables:
    light:
    angle:
  sequence:
    - service: light.turn_on
      data:
        entity_id: "{{ light }}"
        rgb_color: >
          {% set step_size = angle * 0.6 %}
          {% set color = state_attr(light, 'rgb_color') %}
          {% set R = color[0] %}
          {% set G = color[1] %}
          {% set B = color[2] %}
          {% set new_green = G + step_size | int(0) %}
          {% if new_green < 0 %}
            {# Make sure it doesn't go negative #}
            {{ R, 0, B }}
          {% elif new_green > 255 %}
            {# Make sure it doesn't go over 255 #}
            {{ R, 255, B }}
          {% else %}
            {# Send the value calculated #}
            {{ R, new_green, B }}
          {% endif %}
```

#### What this does...

* The script reduces the angle number to 60% of the rotation angle (you can change this, but 60% works well for my needs).
* It then grabs the current colors from the light entity and puts them into a list.
* The new color target is then calculated.
* It makes sure the new color is not negative.
* It then makes sure that the new color is not over 255.
* Finally it provides the calculated color if not in the limits.

This can be used over and over for as many lights as you want to control.  But you **will** need to repeat this and modify it for red and blue color octets.

## 🔁 🔴 Using Rotate CW and CCW as a Short-Press / Long-Press Toggle

Not enough switch positions for you?  **How about a possible 24 more?**  I came up with some scripts you can add to Home Assistant and call for more actions.  One is for CW rotation < 100 degrees, another for > 100 degrees.  Also the same for CCW.  These are can be called from the Group 1 🍎 rotate actions and the Group 2 🍊 rotate actions.  Match the CW call/receive or the CCW call/receive pairs together or you are going to be sad. 😩

Here is a sample of what you put into the script Blueprint UI.  It will need to be a manual YAML edit and contain your specific variables.  What you see here is one from my config.

```yaml
service: script.cube_long_cw_toggle
data:
  angle: '{{ trigger.payload_json.action_angle | float(0.0)}}'
  entity: light.livingroomlight
```

And if you are editing it manually in an editor inside the Script calling yaml, this is the way it should look for rotate ce face 3, as an example.

```yaml
rotate_cw_face_3:
  - service: cube_long_cw_toggle
    data:
      angle: '{{ trigger.payload_json.action_angle | float(0.0)}}'
      entity: light.livingroomlight
```

You can also do this buy going full gui and picking the matching template out of the below section and filling it in similar to this:

![Full GUI Example](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/full_GUI_Example.png?raw=true "Full GUI Example")

These are the standalone scripts that are 'called' from the Script calling yaml shown above.  If you don't need all of these only install the ones you will use.

This is a [homeassistant.toggle action](https://www.home-assistant.io/integrations/homeassistant#service-homeassistanttoggle), so it can toggle anything that that service can handle.  Changing it to homeassistant.turn_on or homeassistant.turn_off would change the behavior slightly if this fits your needs better.

```yaml
script:
cube_short_cw_toggle:
  description: CW Short Press Toggle
  variables:
    entity:
    angle:
  sequence:
    - condition: template
      value_template: '{{ angle > 0 <= 100 }}'
    - service: homeassistant.toggle
      target:
        entity_id: '{{ entity }}'
cube_long_cw_toggle:
  description: CW Long Press Toggle
  variables:
    entity:
    angle:
  sequence:
    - condition: template
      value_template: '{{ angle > 100 }}'
    - service: homeassistant.toggle
      target:
        entity_id: '{{ entity }}'

cube_short_ccw_toggle:
  description: CCW Short Press Toggle
  variables:
    entity:
    angle:
  sequence:
    - condition: template
      value_template: '{{ angle < 0 >= -100 }}'
    - service: homeassistant.toggle
      target:
        entity_id: '{{ entity }}'
cube_long_ccw_toggle:
  description: CCW Long Press Toggle
  variables:
    entity:
    angle:
  sequence:
    - condition: template
      value_template: '{{ angle < -100 }}'
    - service: homeassistant.toggle
      target:
        entity_id: '{{ entity }}'
```

## Method to use Group 3 🍐 actions and not interfere with Group 1 🍎

Not enough switch positions for you still?  **How about another possible 30 + 30 more?** 

This is another 'action' that I stumbled upon.  I noticed if you turn the cube from side to side very gently, it will internally register as being on a new side, but the flip action doesn't register.  Then if you slide the cube, it will send out an action of slide on side 5 from side 2, or whatever side combo's you choose.  I used 5 from 2 in the example, but you can use any of them.

![Showing slide 5 from 2 on the Cube Action Sensor](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/Slide5From2.png?raw=true "Showing slide 5 from 2 on the Cube Action Sensor")

> NOTE: In this example the slide side 5 will also trigger. You may need to add a condition to prevent that from triggering. An example for this 5 from 2 example is to put this condition on the slide side 5 action ```{{ not last_side == 2 }}``` before the thing you want to do.  Or you can just not have a slide 5 action.

It is as simple as adding a condition of let's say 'slide' in one of the Group 3 🍐 slots.

Here is the GUI editor showing this.

![Full GUI Example Conditional side to side](https://github.com/SirGoodenough/HA_Blueprints/blob/master/images/conditionalSide2Side.png?raw=true "Full GUI Example conditional side to side")

Here is the YAML editor showing this:

```yaml
alias: Aqara Magic Cube test
description: stuff happens here
use_blueprint:
  path: SirGoodenough/Zigbee2MQTT - Xiaomi Cube Controller MQTT Triggered.yaml
  input:
    remote: sensor.yertle_action
    5_from_2:
      - condition: template
        value_template: '{{ action == "slide" }}'
      - service: light.toggle
        data: {}
        target:
          entity_id: light.kitchensink
```

## 🌞 ❄️ Troubleshooting tip

If you are troubleshooting and you want to see more traces back when doing so, here is a TIP I've found.
Manually edit the automation created with the ui editor (or manually with a text editor) and add the following to have this automation contain 10 traces instead of the normal 5.

```yaml
alias: aaaaaaa Test
description: 'See how to increase the number of Traces available''
trace:
  stored_traces: 10
use_blueprint:
.....
```

# 🌐 All My Blueprints

[Link to ALL my Blueprints](https://github.com/SirGoodenough/HA_Blueprints/blob/master/README.md)

Here is a list of each of my blueprints, a quick description and jump links to the Blueprints Exchange post...

## 🌀 Scripts

#### 🧯Broadlink on Script Blueprint

https://community.home-assistant.io/t/script-blueprint-to-turn-my-tv-on-and-put-it-into-the-correct-mode-for-the-input-device-i-want/338755

This is a SCRIPT Blueprint that uses my Broadlink RM3 to turn my TV on and get it into the correct mode, Pushes remote buttons in sequence.

#### 🧯Tasmota EZ Button Blueprint

This Script Blueprint generates 3 Buttons to help you manage your Tasmota installed base.  Restart All, Update a few, and Update all.

https://community.home-assistant.io/t/script-blueprint-that-generates-3-ez-buttons-to-manage-your-tasmota-cluster/376934

#### 🧯Play Media File Script Blueprint Blueprint

This is a SCRIPT Blueprint. This provides a way to play canned media files with the big long list of YAML entries but keep the main script or automation clean.

https://community.home-assistant.io/t/script-blueprint-to-play-media-player-files-not-an-automation-blueprint/371988

#### 🧯TTS All Message Blueprint

This script can use any of the 11 integrated TTS Platforms in Home Assistant to send a message to a media player.

https://community.home-assistant.io/t/tts-script-blueprint-for-all-11-ha-core-tts-flavors/400700

## 🔃 Automations

#### 🧯Auto Fan Control Blueprint

This Blueprint is for controlling a 3 speed fan based on a temperature sensor. Both fan % control & MQTT fan control versions.

https://community.home-assistant.io/t/auto-fan-temp-control-for-3-speed-fan-using-ha-fan-or-mqtt-integration/326419

#### 🧯Door Open TTS Cloud-Say Message Blueprint

This Blueprint is a TTS.cloud-say version of another Door Announcer I found in the HA Blueprint Exchange.

https://community.home-assistant.io/t/door-open-tts-cloud-say-announcer-nabu-casa-required/316046

#### 🧯Keypad Lock or puzzle Box Tool Blueprint

This Blueprint accepts 5 actions & when done in the right order, flips an input_boolean.

https://community.home-assistant.io/t/keypad-cipher-code-for-5-button-presses-before-you-turn-on-an-input-boolean/322385

#### 🧯Zigbee2MQTT - Xiaomi Cube Controller Blueprint

This Blueprint uses a Zigbee2MQTT built sensor to sort out the 38(+54) commands from the Xiaomi Magic Cube Remote.  

https://community.home-assistant.io/t/zigbee2mqtt-xiaomi-cube-controller/393203

#### 🧯Zigbee2MQTT - ZemiSmart ZM-RM02 Controller Blueprint

This Blueprint uses the Z2M (Zigbee2MQTT) imported Action sensor to sort out the 18 commands from the ZemiSmart ZM-RM02 Controller.

https://community.home-assistant.io/t/zigbee2mqtt-zemismart-zm-rm02-controller/412650

#### 🧯ZHA - Xiaomi Cube Controller Blueprint

This Blueprint uses a ZHA built sensor to sort out the 38(+54) commands from the Xiaomi Magic Cube Remote.  

https://community.home-assistant.io/t/zha-xiaomi-cube-controller/495975

#### 🧯Device_tracker Monitor & Notifier

This Blueprint Monitor's device_tracker entities that you choose & notifies you if they go offline. Then it gives you the opportunity to devise an action to deal with it.

https://community.home-assistant.io/t/device-tracker-monitor-notifier/500688

#### 🧯 Zigbee2MQTT Aqara Magic Cube T1-Pro CTP-R01 Xiaomi Lumi cagl02

This Blueprint gives you literally hundreds of actions available on the new Magic Cube.

https://community.home-assistant.io/t/zigbee2mqtt-aqara-magic-cube-t1-pro-ctp-r01-xiaomi-lumi-cagl02/525111

## 🤹🏾‍♂️ Contact Links or see my other work

What are we Fixing Today Homepage / Website: https://www.WhatAreWeFixing.Today/

Channel Link URL: (WhatAreWeFixingToday) https://bit.ly/WhatAreWeFixingTodaysYT

Discord Guild: (Sir_Goodenough#9683) https://discord.gg/Uhmhu3B

## 🧀 If you want to support me

Buy me Coffee: https://www.buymeacoffee.com/SirGoodenough

PayPal one-off donation link: https://www.paypal.me/SirGoodenough

#WhatAreWeFixingToday

#SirGoodEnough