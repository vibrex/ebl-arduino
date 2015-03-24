Simple LED functions with asynchronous capabilities.

# Methods #

**LedControl.turnOn**(pin)

> Turn the LED on.

  * **pin**: Pin where the LED is connected.

**LedControl.turnOn**(pin, delayMilliseconds)

> Turn the LED on smoothly like a "fade in" effect. **Depends on PWM**.

  * **pin**: Pin where the LED is connected.
  * **delayMilliseconds**: Time in milliseconds to become completely power-on.

**LedControl.turnOff**(pin)

> Turn the LED off.

  * **pin**: Pin where the LED is connected.

**LedControl.turnOff**(pin, delayMilliseconds)

> Turn the LED off smoothly like a "fade out" effect. **Depends on PWM**.

  * **pin**: Pin where the LED is connected.
  * **delayMilliseconds**: Time in milliseconds to become completely power-off.

**LedControl.turnPercent**(pin, percent)

> Set a custom power level for a LED. **Depends on PWM**.

  * **pin**: Pin where the LED is connected.
  * **percent**: Level of power from 0 to 100.

**LedControl.blink**(pin, times, intervalMilliseconds)

> Blinks the LED.

  * **pin**: Pin where the LED is connected.
  * **times**: Times to blink.
  * **intervalMilliseconds**: Interval between blinks in miliseconds.

**LedControl.blink**(pin, times, intervalMilliseconds, delayMilliseconds)

> Blinks the LED smoothly with "fade in" and "fade out" effect. **Depends on PWM**.

  * **pin**: Pin where the LED is connected.
  * **times**: Times to blink.
  * **intervalMilliseconds**: Interval between blinks in miliseconds.
  * **delayMilliseconds**: Time in milliseconds to become completely power-on and power-off.

**LedControl.startBlink**(pin, intervalMilliseconds)

> Start blinking the LED asynchronous.

  * **pin**: Pin where the LED is connected.
  * **intervalMilliseconds**: Interval between blinks in miliseconds.

**LedControl.startBlink**(pin, intervalMilliseconds, delayMilliseconds)

> Start blinking the LED asynchronous with with "fade in" and "fade out" effect. **Depends on PWM**.

  * **pin**: Pin where the LED is connected.
  * **intervalMilliseconds**: Interval between blinks in miliseconds.
  * **delayMilliseconds**: Time in milliseconds to become completely power-on and power-off.

**LedControl.stopBlink**(pin)

> Stop any asynchronous blinking.

  * **pin**: Pin where the LED is connected.

**LedControl.loop**()

> Event handler process loop. Must be added to main '**loop**()' function to enable asynchronous capabilities.

# Comments #

> Don't forget to check the capable ports for PWM functions.

> The **LedControl.loop**() function is needed only to enable asynchronous functions, in this case avoid using any blocking function in main '**loop**()'.

# Example #

> In this example two LEDs blinking together in different manners. The LED in pin 9 is configured to 500ms of fade in/out with 1s of interval, and the LED at pin 8 is a simple blinker with 200ms of interval:

```
#include <LedControl.h>

void setup() {
  LedControl.startBlink(9,1000,500);
  LedControl.startBlink(8,200);
}

void loop() {
  LedControl.loop();
}
```