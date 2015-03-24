Event handler for buttons (tactile switches) on digital or analog (in series with resistors) ports.

# Methods #

**ButtonEvent.addButton**(pin, onDown, onUp, onHold, holdMillisWaitonDouble, doubleMillisWait)

> Add a new digital port connected button to event handler.

  * **pin**: Digital pin where the button is connected.
  * **onDown**: Method called on onDown events.
  * **onUp**: Method called on onUp events.
  * **onHold**: Method called on onHold events.
  * **holdMillisWait**: Hold time in miliseconds.
  * **onDouble**: Method called on onDouble events.
  * **doubleMillisWait**:  Double time in miliseconds.

**ButtonEvent.addButton**(pin, analogValue, deviation, onDown, onUp, onHold, holdMillisWaitonDouble, doubleMillisWait)

> Add a new analog port connected button to event handler. Automatically enable internal pullup resistor to given port.

  * **pin**: Analog pin where the button is connected.
  * **analogValue**: Analog readings for button.
  * **deviation**: Reading deviation for upper and lower values.
  * **onDown**: Method called on onDown events.
  * **onUp**: Method called on onUp events.
  * **onHold**: Method called on onHold events.
  * **holdMillisWait**: Hold time in miliseconds.
  * **onDouble**: Method called on onDouble events.
  * **doubleMillisWait**:  Double time in miliseconds.

**ButtonEvent.loop**()

> Event handler process loop. Must be added to main '**loop**()' function.

# Events #

  * **onDown**: Happens once when the button is pressed.
  * **onUp**: Happens once when the button is released.
  * **onHold**: Happens once when the button is keep pressed during the time expressed through **holdMillisWait** parameter.
  * **onDouble**: Happens once when the button is pressed at second time during the time expressed through **doubleMillisWait** parameter.

# Comments #

> Passing a **NULL** parameter to event method in **addButton** or setting it's wait time to **0** will disable the event.

> The **onDouble** event override the second **onDown** when it happen within the period specified in **doubleMillisWait**. The **onUp** event ocurrs normaly when the button is released from double event.

> In order to use series of button you need to calculate the correct voltage drop for each button position. You may also try to use the [AnalogEvent](http://code.google.com/p/ebl-arduino/wiki/AnalogEvent) to obtain these values.

# Example #

> In this example we configure the digital port 12 as a button with all events enabled:

```
#include <ButtonEvent.h>

void setup() {
  ButtonEvent.addButton(12,       //button pin
                        onDown,   //onDown event function
                        onUp,     //onUp event function
                        onHold,   //onHold event function
                        1000,     //hold time in milliseconds
                        onDouble, //onDouble event function
                        200);     //double time interval

  Serial.begin(9600);
}

void loop() {
  ButtonEvent.loop();
}

void onDown(ButtonInformation* Sender) {
  Serial.print("Button (pin:");
  Serial.print(Sender->pin);
  Serial.println(") down!");
}

void onUp(ButtonInformation* Sender) {
  Serial.print("Button (pin:");
  Serial.print(Sender->pin);
  Serial.println(") up!");
}

void onHold(ButtonInformation* Sender) {
  Serial.print("Button (pin:");
  Serial.print(Sender->pin);
  Serial.print(") hold for ");
  Serial.print(Sender->holdMillis);
  Serial.println("ms!");
}

void onDouble(ButtonInformation* Sender) {
  Serial.print("Button (pin:");
  Serial.print(Sender->pin);
  Serial.print(") double click in ");
  Serial.print(Sender->doubleMillis);
  Serial.println("ms!");
}
```

> In this example we configure three buttons in series with resistors connected to analog port 0:

```
#include <ButtonEvent.h>

short Button1 = 230;
short Button2 = 365;
short Button3 = 460;

void setup() {
  //initial buffer for 3 buttons
  ButtonEvent.initialCapacity = sizeof(ButtonInformation)*3;

  ButtonEvent.addButton(0,        //analog button pin
                        Button1,  //analog value
                        20,       //deviation
                        onDown,   //onDown event function
                        onUp,     //onUp event function
                        onHold,   //onHold event function
                        1000,     //hold time in milliseconds
                        onDouble, //onDouble event function
                        200);     //double time interval

  ButtonEvent.addButton(0,        //analog button pin
                        Button2,  //analog value
                        20,       //deviation
                        onDown,   //onDown event function
                        onUp,     //onUp event function
                        onHold,   //onHold event function
                        1000,     //hold time in milliseconds
                        onDouble, //onDouble event function
                        200);     //double time interval

  ButtonEvent.addButton(0,        //analog button pin
                        Button3,  //analog value
                        20,       //deviation
                        onDown,   //onDown event function
                        onUp,     //onUp event function
                        onHold,   //onHold event function
                        1000,     //hold time in milliseconds
                        onDouble, //onDouble event function
                        200);     //double time interval

  Serial.begin(9600);
}


void loop() {
  ButtonEvent.loop();
}

void onDown(ButtonInformation* Sender) {
  Serial.print("Button ");

  if (Sender->analogValue == Button1)
    Serial.print(1);
  else if (Sender->analogValue == Button2)
    Serial.print(2);
  else if (Sender->analogValue == Button3)
      Serial.print(3);

  Serial.println(" down!");
}

void onUp(ButtonInformation* Sender) {
  Serial.print("Button ");

  if (Sender->analogValue == Button1)
    Serial.print(1);
  else if (Sender->analogValue == Button2)
    Serial.print(2);
  else if (Sender->analogValue == Button3)
      Serial.print(3);

  Serial.println(" up!");
}

void onHold(ButtonInformation* Sender) {
  Serial.print("Button ");

  if (Sender->analogValue == Button1)
    Serial.print(1);
  else if (Sender->analogValue == Button2)
    Serial.print(2);
  else if (Sender->analogValue == Button3)
      Serial.print(3);

  Serial.print(" hold for ");
  Serial.print(Sender->holdMillis);
  Serial.println("ms!");
}

void onDouble(ButtonInformation* Sender) {
  Serial.print("Button ");

  if (Sender->analogValue == Button1)
    Serial.print(1);
  else if (Sender->analogValue == Button2)
    Serial.print(2);
  else if (Sender->analogValue == Button3)
      Serial.print(3);

  Serial.print(" double click in ");
  Serial.print(Sender->doubleMillis);
  Serial.println("ms!");
}
```