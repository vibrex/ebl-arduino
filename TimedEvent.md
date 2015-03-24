Simple task scheduler.

# Methods #

**TimedEvent.addTimer**(intervalMillis, onEvent)

> Add a new timer and start it immediately.

  * **intervalMillis**: Time interval in miliseconds.
  * **onEvent**: Method called on onEvent events.

**TimedEvent.addTimer**(eventId, intervalMillis, onEvent)

> Add a new timer with an associated ID. In this method the event is not started immediately and must be controled with '**start**(eventId)' and '**stop**(eventId)' methods.

  * **eventId**: Associated event ID.
  * **intervalMillis**: Time interval in miliseconds.
  * **onEvent**: Method called on scheduled events.

**TimedEvent.start**(eventId)

> Start an event timer.

  * **eventId**: Associated event ID.

**TimedEvent.stop**(eventId)

> Stop an event timer.

  * **eventId**: Associated event ID.

**ButtonEvent.loop**()

> Event handler process loop. Must be added to main '**loop**()' function.

# Events #

  * **onEvent**: Happens once when the interval elapses.

# Example #

> In this example one event (event1) is used to control when the another one (event2) is started and stoped at regular intervals while they send data through Serial Monitor:

```
#include <TimedEvent.h>

#define CONTROLLED_TIMER 1

bool flag = true;

void setup() {
  Serial.begin(9600);

  //event 1
  TimedEvent.addTimer(5000, event1);
  //event 2
  TimedEvent.addTimer(CONTROLLED_TIMER, 500, event2);
  TimedEvent.start(CONTROLLED_TIMER);
}

void loop() {
  TimedEvent.loop();
}

void event1(TimerInformation* Sender) {
  Serial.print("event1: ");
  if (flag=!flag) {
    TimedEvent.start(CONTROLLED_TIMER);
    Serial.println("START!");
  } else {
    TimedEvent.stop(CONTROLLED_TIMER);
    Serial.println("STOP!");
  }
}

void event2(TimerInformation* Sender) {
    Serial.println("event2!!");  
}
```