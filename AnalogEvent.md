Event handler for analog ports that can be used to read potentiometers or others sensors.

# Methods #

**AnalogEvent.addAnalogPort**(pin, onChange, hysteresis)

> Add a new analog port to event handler.

  * **pin**: Pin where the button is connected.
  * **onChange**: Method called on onChange events.
  * **hysteresis**: Minimum value change to launch an event.

**AnalogEvent.loop**()

> Event handler process loop. Must be added to main '**loop**()' function.

# Events #

  * **onChange**: Happens once when a new value is readed.

# Comments #

> The **hysteresis** parameter is useful to avoid too many oscillations when reading a value due to some noise. When defined as greater than 0, the **onChange** event is launched only when the value changes is greater or equal this parameter.

> Once the **onChange** occurs, the value must be readed from **AnalogPortInformation->value**. This value is obtained using **analogRead**() and stay between 0 to 1023 that represents the analog reference of input voltage. Look for more information in the  [Arduino Reference page](http://www.arduino.cc/en/Reference/AnalogRead).

# Example #

> In this example we configure the analog pin 1 to read values from a potentiometer with an hysteresis value of 3:

```
#include <AnalogEvent.h>

void setup() {
  AnalogEvent.addAnalogPort(1,        //potentiometer pin
                            onChange, //onChange event function
                            3);       //hysteresis
  
  Serial.begin(9600);
}

void loop() {
  AnalogEvent.loop();
}

void onChange(AnalogPortInformation* Sender) {
  Serial.print("Analog (pin:");
  Serial.print(Sender->pin);
  Serial.print(") changed to: ");
  Serial.print(Sender->value);
  Serial.println("!");
}
```