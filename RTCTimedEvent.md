RTC based time scheduler, similar to Unix Cron. It's currently supporting only DS1307 based RTC circuits while connected to Arduino's I2C ports (analog 4 for SDA and analog 5 for SCL).

# Properties #

**RTCTimedEvent.initialCapacity**

> Define the initial buffer size. It will allocated after first call to **RTCTimedEvent.addTimer**() and must be at least the size of **RTCTimerInformation** structure to be used. Any subsequent calls to **RTCTimedEvent.addTimer**() will call realloc() to obtain only the missing space.

**RTCTimedEvent.time**

> Structure that storage the RTC data for read and write.

  * **hour**: Hours from 0 to 23.
  * **minute**: Minutes from 0 to 59.
  * **second**: Seconds from 0 to 59.
  * **day**: Day of month from 1 to 31.
  * **month**: Month from 1 to 12.
  * **year**: Years.
  * **dayOfWeek**: Day of week from 1 to 7.

# Methods #

**RTCTimedEvent.addTimer**(minute, hour, day, month, dayOfWeek, onEvent)

> Add a new timer. The event will be thrown when the current time matches all parameters. The wildcard TIMER\_ANY may be used for each parameter that must to be ignored.

  * **minute**: Minute from 0 to 59.
  * **hour**: Hour from 0 to 23.
  * **day**: Day of month from 1 to 31.
  * **month**: Month from 1 to 12.
  * **dayOfWeek**: Day of week from 1 to 7.
  * **onEvent**: Method called on onEvent events.

**RTCTimedEvent.addTimer**(eventId, minute, hour, day, month, dayOfWeek, onEvent)

> Add a new timer with an associated ID. The event will be thrown when the current time matches all parameters. The wildcard TIMER\_ANY may be used for each parameter that must to be ignored.

  * **eventId**: Associated event ID.
  * **minute**: Minute from 0 to 59.
  * **hour**: Hour from 0 to 23.
  * **day**: Day of month from 1 to 31.
  * **month**: Month from 1 to 12.
  * **dayOfWeek**: Day of week from 1 to 7.
  * **onEvent**: Method called on scheduled events.

**RTCTimedEvent.clear**()

> Remove all existing escheduled event.

**RTCTimedEvent.loop**()

> Event handler process loop. Must be added to main '**loop**()' function.

**RTCTimedEvent.readRTC**()

> Read date and time from RTC circuit to **RTCTimedEvent.time**.

**RTCTimedEvent.writeRTC**()

> Write date and time from **RTCTimedEvent.time** to RTC circuit.

# Events #

  * **onEvent**: Happens once when all parameters matches the current time read from RTC circuit.

# Comments #

> Native Arduino Wire library is needed for I2C communication. Don't forget to also include Wire.h on top of sketch toguether with RTCTimedEvent.h.

> It's a good practice to set the total ammount of memory needed to **RTCTimedEvent.initialCapacity** before adding any timer. This example show how to allocate an initial buffer space for 5 timers:

```
RTCTimedEvent.initialCapacity = sizeof(RTCTimerInformation)*5;
```

# Example #

```
#include <RTCTimedEvent.h>
#include <Wire.h> //needed for I2C

void setup() {
  Serial.begin(9600);

  //set time to 31/12/2010 23:58:50
  RTCTimedEvent.time.second = 50;
  RTCTimedEvent.time.minute = 58;
  RTCTimedEvent.time.hour = 23;
  RTCTimedEvent.time.dayOfWeek  = 6;
  RTCTimedEvent.time.day = 31;
  RTCTimedEvent.time.month = 12;
  RTCTimedEvent.time.year = 2010;
  RTCTimedEvent.writeRTC();

  //initial buffer for 3 timers
  RTCTimedEvent.initialCapacity = sizeof(RTCTimerInformation)*3;

  //event for every day
  RTCTimedEvent.addTimer(0,         //minute
                         0,         //hour
                         TIMER_ANY, //day fo week
                         TIMER_ANY, //day
                         TIMER_ANY, //month
                         dayCall);

  //event for every hour
  RTCTimedEvent.addTimer(0,         //minute
                         TIMER_ANY, //hour
                         TIMER_ANY, //day fo week
                         TIMER_ANY, //day
                         TIMER_ANY, //month
                         hourCall);

  //event for every minute
  RTCTimedEvent.addTimer(TIMER_ANY, //minute
                         TIMER_ANY, //hour
                         TIMER_ANY, //day fo week
                         TIMER_ANY, //day
                         TIMER_ANY, //month
                         minuteCall);
}

void loop() {
  RTCTimedEvent.loop();
}

void minuteCall(RTCTimerInformation* Sender) {
  Serial.print("New minute: ");

  Serial.print(RTCTimedEvent.time.hour, DEC);
  Serial.print(":");
  Serial.print(RTCTimedEvent.time.minute, DEC);
  Serial.print(":");
  Serial.print(RTCTimedEvent.time.second, DEC);
  Serial.print("  ");
  Serial.print(RTCTimedEvent.time.month, DEC);
  Serial.print("/");
  Serial.print(RTCTimedEvent.time.day, DEC);
  Serial.print("/");
  Serial.println(RTCTimedEvent.time.year, DEC);
}

void hourCall(RTCTimerInformation* Sender) {
    Serial.print("New hour! ");
}

void dayCall(RTCTimerInformation* Sender) {
    Serial.print("New day! ");
}
```

It's return that could be observed at Serial Monitor must be:
<pre>
New minute: 23:59:0  12/31/2010<br>
New day! New hour! New minute: 0:0:0  1/1/2011<br>
New minute: 0:1:0  1/1/2011<br>
New minute: 0:2:0  1/1/2011<br>
...<br>
...<br>
New minute: 0:58:0  1/1/2011<br>
New minute: 0:59:0  1/1/2011<br>
New hour! New minute: 1:0:0  1/1/2011<br>
New minute: 1:1:0  1/1/2011<br>
</pre>