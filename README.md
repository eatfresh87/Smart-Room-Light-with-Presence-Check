💡 Smart Room Switch with Presence Check
This Home Assistant blueprint automates a switch for a room, providing intelligent control that goes beyond a simple motion sensor. It's designed to make your home more intuitive by confirming a room is empty before it turns off the lights or other devices.

✨ Features
Intelligent Turn-On: The switch turns on when a door opens or motion is detected.

Enhanced Presence Check: After the door closes, a delay is triggered. If no motion is detected, a message is played on a media device (e.g., a Google Home) to ask if someone is still in the room. If motion is detected after the question, the automation is canceled, and the switch remains on.

Manual Override: If you turn on the switch manually, the automation will not interfere.

Door Open Reminder: If the door is left open for a specified period, the blueprint will turn the switch back on as a visual reminder. If there is no motion, the system will reset, returning manual control to the user.

📝 Prerequisites
Before using this blueprint, you will need the following devices and an input_boolean helper configured in Home Assistant:

Door/Window Sensor: A binary_sensor entity for your door.

Motion Sensor: A binary_sensor entity for your room.

Switch: The switch entity you want to control.

Media Player: A media_player entity that supports text-to-speech (TTS), like Google Cast devices or Amazon Echo devices.

Automation Flag Helper: You must create an input_boolean helper in Home Assistant and select it as an input for this blueprint. This helper acts as an internal flag to help the automation distinguish between a manual turn-on and an automatic turn-on.

⚙️ How It Works
This blueprint is a single automation with a choose action that responds to different triggers to manage the switch's behavior.

When the Switch Turns On:

The automation is triggered when the door opens or motion is detected.

It checks if the switch is currently off. If it is, it turns the switch on and sets the input_boolean helper to on.

The Presence Check and Turn-Off Sequence:

This sequence begins when the door closes and the input_boolean helper is on.

A short delay gives you time to leave the room.

The automation then waits for a few minutes for new motion to be detected.

If no motion is detected, it plays a TTS message on the media player asking, "Is anyone still in the room?"

It then waits for another 20 seconds. If motion is detected during this time, the automation assumes someone is there and stops.

If still no motion is detected, the switch is turned off, and the input_boolean helper is turned off to reset the system.

Handling Manual Operation:

This logic is triggered when the switch is turned on.

It checks if both the door and motion sensors are off, which indicates a manual turn-on.

If this is the case, the input_boolean helper is immediately turned off, preventing the automation from ever turning the switch off.

The Door Open Reminder:

This new trigger activates if the door has been open for a specified duration (door_open_reminder_timeout).

The automation turns the switch on as a visual reminder to close the door.

It then waits for a short, fixed delay (30 seconds) and checks the motion sensor.

If there is no motion, it assumes the room is now empty, and the input_boolean helper is reset to off, returning manual control to the user.
