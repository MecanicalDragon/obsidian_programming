**ZoneId** is a representation of the time-zone such as *Europe/Paris*.
**ZoneOffset** extends ZoneId and defines the fixed offset of the current time-zone with GMT/UTC, such as *+02:00*.

| ZonedDateTime                                                                                             | OffsetDateTime                                                                              |
| --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| 2007-12-03T10:15:30+01:00 Europe/Paris                                                                    | 2007-12-03T10:15:30+01:00                                                                   |
| Holds LocalDateTime, ZoneId, and ZoneOffset.                                                              | Holds LocalDateTime and ZoneOffset only.                                                    |
| Fully DST-aware and handles daylight saving time adjustments, can be used to handle ambiguous date-times. | Preferable to store date-times in the database, cause hasn’t got excess region information. |
**DURATION** is a time-based amount of time, such as *34.5 seconds*.
>This class models a quantity or amount of time in terms of SECONDS and nanoseconds. It can be accessed using other duration-based units, such as MINUTES and HOURS. In addition, the DAYS unit can be used and is treated as exactly equal to 24 hours, thus ignoring daylight savings effects.

**PERIOD** is a date-based amount of time in the **ISO-8601** calendar system, such as *2 years, 3 months and 4 days*.
>This class models a quantity or amount of time in terms of years, months and days. The supported units of a period are YEARS, MONTHS and DAYS. All three fields are always present but may be set to zero.

<br>Durations and periods differ in their treatment of daylight savings time when added to ZonedDateTime. A Duration will add an exact number of seconds, thus a duration of one day is always exactly 24 hours. By contrast, a Period will add a conceptual day, trying to maintain the local time.<br>
For example, consider adding a period of one day and a duration of one day to 18:00 in the evening before a daylight savings gap. The Period will add the conceptual day and result in a ZonedDateTime at 18:00 the following day. By contrast, the Duration will add exactly 24 hours, resulting in a ZonedDateTime at 19:00 the following day (assuming a one-hour DST gap).

| Duration                                 | Period                                         |
| ---------------------------------------- | ---------------------------------------------- |
| Accepts DAYS, HOURS, MINUTES, SECONDS    | Accepts YEARS, MONTHS, WEEKS, DAYS             |
| P2DT4H32M45.54S                          | P2Y2M3D                                        |
| Day is not DST-aware and always 24 hours | Day is DST-aware, and hours in it are floating |
[Difference](https://stackoverflow.com/questions/32437550/whats-the-difference-between-instant-and-localdatetime/32443004#32443004)
