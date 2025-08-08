💡 Smart Room Switch with Presence Check
This Home Assistant blueprint automates a switch for a room, providing intelligent control that goes beyond a simple motion sensor. It's designed to make your home more intuitive by confirming a room is empty before it turns off the lights or other devices.

✨ Features
Intelligent Turn-On: The switch turns on when a door opens or motion is detected in the room.

Enhanced Presence Check: After the door closes, a delay is triggered. If no new motion is detected during that time, a message is played on a media device (e.g., a Google Home or Amazon Echo) to ask if someone is still in the room.

Response Handling: The automation now handles a "yes" response. If someone responds by creating motion after the question is asked, the system recognizes this as a positive response. The automation will then be canceled, and the switch will remain on.

Smart Turn-Off: If no motion is detected after the presence check, the switch is safely turned off.

Manual Override: The blueprint includes a check for manual operation. If you turn on the switch yourself (without the door opening or motion being detected), the automation won't interfere and will not turn the switch off unexpectedly.

📝 Prerequisites
Before using this blueprint, you will need the following devices and an input_boolean helper configured in Home Assistant:

Door/Window Sensor: A binary_sensor entity for your door.

Motion Sensor: A binary_sensor entity for your room.

Switch: The switch entity you want to control.

Media Player: A media_player entity that supports text-to-speech (TTS), like Google Cast devices or Amazon Echo devices.

Automation Flag Helper: You must create an input_boolean helper in Home Assistant and select it as an input for this blueprint. This helper acts as an internal flag to help the automation distinguish between a manual turn-on and an automatic turn-on.

⚙️ How It Works
This blueprint is built on a single automation with a choose action that responds to four different triggers. This design is highly efficient and keeps the logic centralized.

When the Switch Turns On:

The automation is triggered when the door sensor opens or when motion is detected.

It first checks if the switch is currently off to avoid running redundantly.

If the condition is met, it turns the switch on and sets the input_boolean helper to on. This flag is crucial—it tells the system, "I turned this on, so I'm in charge of turning it off later."

The Presence Check and Turn-Off Sequence:

This sequence begins when the door sensor closes.

It first checks if the input_boolean helper is on. This ensures the automation only proceeds if it was the one that turned the switch on. If the flag is off (meaning you turned the switch on manually), this whole sequence is skipped.

A short delay is triggered to give you time to leave the room.

After the delay, the automation waits for a few minutes for new motion to be detected.

If no motion is detected, it plays a TTS message on the media player asking, "Is anyone still in the room?"

It then waits for another 20 seconds. This is the part that handles your "yes" response. The blueprint assumes that if someone is in the room and hears the question, they will move slightly to acknowledge it. If motion is detected during this time, the automation is considered a "positive response" and the script ends, leaving the switch on.

If still no motion is detected after the question is asked, the switch is turned off, and the input_boolean helper is turned off to reset the system.

Handling Manual Operation:

This logic is triggered when the switch entity is turned on.

It uses a condition to check if both the door and motion sensors are off.

If this condition is true, it means the switch was activated manually (not by the blueprint's triggers).

In this case, the input_boolean helper is immediately turned off. This prevents the presence check and turn-off sequence from ever running, leaving you in full control of the switch.
