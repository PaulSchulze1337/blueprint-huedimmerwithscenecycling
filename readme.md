# Time-Adaptive Hue Dimmer with Scene Cycling (supporting different types of scene definitions)

## Features

  * **Hue Dimmer Control:**  Provides intuitive control for turning lights on/off and adjusting brightness using your Hue Dimmer. Scene cycling is also implemented using the 'Off' button.
  * **Time-Integrated Adaptive Lighting:**  When you turn lights **ON** with the dimmer (after they were off), the blueprint automatically applies time-of-day adjusted color temperature and brightness. This ensures your lighting is comfortable and appropriate for the time of day, transitioning through Morning, Day, Evening, and Night settings. Brightness adjustments via the dimmer maintain the current color temperature.
  * **Versatile Customizable Scenes (Per-Scene Type Selection):** Define up to 7 distinct lighting scenes. For each scene, you can choose to activate it using:
      * **RGB Color:** Set a specific RGB color and brightness.
      * **Color Temperature (Kelvin):** Set a specific color temperature (in Kelvin) and brightness.
      * **Home Assistant Scene:** Trigger a pre-defined Home Assistant Scene, allowing you to control not just lights, but any entities within your Home Assistant setup as part of the scene.
  * **Scene Cycling:**  Cycle through your defined scenes using the **'Off' button (short press)** on the Hue dimmer. The first press after starting the automation activates Scene 1, subsequent presses cycle through Scene 2, Scene 3, ..., Scene 7, and then back to Scene 1. The current scene index is tracked using a helper entity.
  * **Simplified Time Settings:**  Configuration is straightforward with only start times required to define your Morning, Day, Evening, and Night lighting periods.
  * **Smooth Transitions:**  Option for smooth and pleasant transitions for all dimmer button actions and when applying the initial adaptive lighting settings. Separate, shorter transition times can be configured for brightness adjustments for a more responsive feel.

## Setup

To use this blueprint, you'll need to configure the following inputs in Home Assistant when creating an automation from this blueprint.  **At minimum, you must configure the first three mandatory fields**:

1.  **MQTT Topic (mandatory):**

      * Enter the MQTT topic your Hue Dimmer publishes to. This is usually in the format `zigbee2mqtt/{FRIENDLY_NAME}`.
      * *Example:* `zigbee2mqtt/living_room_dimmer`

2.  **Lights to be acted upon (mandatory):**

      * Select the lights that you want the Hue Dimmer to control. You can select individual lights, groups of lights, or areas containing lights.
      * This blueprint is designed to work with light entities.

3.  **Scene Index Input Text Entity ID (mandatory, creation REQUIRED):**

      * **Important:** You **MUST create an `input_text` helper entity** in Home Assistant to track the current scene index for scene cycling.
      * **Recommended Naming Convention:** For clarity and proper functionality, it's highly recommended to name your `input_text` helper using the following convention, based on your MQTT Topic:
        ```
        hue_dimmer_scene_index_{{ MQTT_TOPIC_FRIENDLY_NAME }}_input_text
        ```
        where `{{ MQTT_TOPIC_FRIENDLY_NAME }}` is derived from the last part of your MQTT Topic, with any invalid characters replaced by underscores.
      * **Example:** If your MQTT Topic is `zigbee2mqtt/bedroom_hue_dimmer`, create an `input_text` entity with the Entity ID: `hue_dimmer_scene_index_bedroom_hue_dimmer_input_text`.
      * Then, in the blueprint input, select this `input_text` entity.

4.  **Transition time (seconds) - On/Off and Initial State:**

      * *(Optional)*:  Set the transition time (in seconds) for turning lights ON/OFF and for the initial application of adaptive lighting settings when turning lights on.  Defaults to 5 seconds.

5.  **Brightness Transition time (seconds) - Brightness Adjustments:**

      * *(Optional)*: Set the transition time (in seconds) specifically for brightness adjustments using the dimmer buttons (Up/Down).  A shorter time (e.g., 1 second) is recommended for responsiveness. Defaults to 1 second.

6.  **Adaptive Lighting Settings (time periods, color temperature, brightness):** 
      * *(Optional)*: Customize the time periods and lighting settings for adaptive lighting:
          * **Morning Start Time, Day Start Time, Evening Start Time, Night Start Time:** Define the start times for each period.
          * **Morning Color Temperature, Day Color Temperature, Evening Color Temperature, Night Color Temperature:** Set the desired color temperature (in Kelvin) for each period.
          * **Morning Brightness, Day Brightness, Evening Brightness, Night Brightness:** Set the desired brightness percentage for each period.
          * Defaults are provided for a comfortable adaptive lighting experience.

7.  **Scene 1 Settings, Scene 2 Settings, ..., Scene 7 Settings:** 

      * *(Optional)*: Configure up to 7 lighting scenes. For each scene:
          * **Scene X Type:** Choose the scene type: "RGB Color", "Color Temperature (Kelvin)", or "Home Assistant Scene".
          * Depending on the Scene Type selected, configure the relevant settings:
              * **RGB Color Scenes:** Set the "Color of scene X (RGB)" and "Brightness of scene X".
              * **Color Temperature Scenes:** Set the "Color Temperature of scene X (Kelvin)" and "Brightness of scene X".
              * **Home Assistant Scenes:** Select a pre-defined "Home Assistant Scene for scene X".

## Usage

Once you've created an automation using this blueprint, here's how to control your lights with your Hue Dimmer:

  * **'On' Button (Press and Release):**
      * **If lights are OFF:** Turns lights ON with time-adaptive color temperature and brightness settings applied.
      * **If lights are ON:** Turns lights OFF with a transition.
  * **'On' Button (Hold):**
      * Turns lights ON with time-adaptive color temperature and brightness settings applied (even if lights were already on - override to re-apply adaptive settings).
  * **'Off' Button (Press and Release):**
      * **Scene Cycling:** Activates the next scene in your sequence (Scene 1 -\> Scene 2 -\> ... -\> Scene 7 -\> Scene 1). The first press after automation start activates Scene 1.
  * **'Dim Up' Button (Press and Release / Hold):**
      * Increases the brightness of the lights by 10%.
  * **'Dim Down' Button (Press and Release / Hold):**
      * Decreases the brightness of the lights by 10%.

## ⚠️ Important Notes and Disclaimer

  * **Zigbee2MQTT Required:** This blueprint is designed to work with a Philips Hue Dimmer connected to Home Assistant via Zigbee2MQTT. Ensure your dimmer is properly paired and publishing MQTT messages.
  * **Input Text Helper is Mandatory for Scene Cycling:** Scene cycling functionality will **not work** without properly creating and configuring the `input_text` helper entity as described in the setup.
  * **Default Scene Type is RGB:** If you use scene cycling without customizing scene settings, scenes will default to RGB color scenes, which might not be ideal for bulbs that don't support RGB color. Consider customizing your scene settings, especially for scene cycling.
  * **Disclaimer:** This software is provided "as is," without any warranties or guarantees of any kind, express or implied. Use at your own risk.

Enjoy your enhanced Hue Dimmer experience with time-adaptive lighting and scene control\!
