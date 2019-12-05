# Campsite Check-in Kata :tent:

You have been tasked with building a campsite check-in service for when people are checking in to a campsite. This service needs to produce a report such as the below, which can then be printed:

```
Check-in for: Avid Camper
Car Registration: REG 123
Check-out: 02/02/2019
---
INFORMATION:
---
Wildlife:
* Bear spotted 3 days ago
--
Rules:
* No food should be left unattended at site
--
Weather:
* Day 1 - Rain
--
Potential Costs:
* Shower - £1 for 5 minutes
* Wood - £5 for 10 logs
```

There are multiple services involved in order to supply the above informaton. Let's take a closer look at these services needed and their contracts.

## Campsite check-in service

Collaborating services: Wildlife service, Rules service, Weather service and Costs service.

This service will receive the following as input:

```
Name of camper: <name>
Car Registration (if applicable): <registration>
Check-out: <date>
```

Adding some real data to the above template looks like the below:

```
Name of camper: Some Camper
Car Registration (if applicable): REG 123
Check-out: 02/02/2019
```

The service will take all of the information from the collaborating services and then output something like:

```
Check-in for: Avid Camper
Car Registration: REG 123
Check-out: 02/02/2019
---
INFORMATION:
---
Wildlife:
* Bear spotted 3 days ago
--
Rules:
* No food should be left unattended at site
--
Weather:
* Day 1 - Rain
--
Potential Costs:
* Shower - £1 for 5 minutes
* Wood - £5 for 10 logs
```

If the camper has not driven, then the output would look like the below:

```
Check-in for: Avid Camper
Check-out: 02/02/2019
---
INFORMATION:
---
Wildlife:
* Bear spotted 3 days ago
--
Rules:
* No food should be left unattended at site
--
Weather:
* Day 1 - Rain
--
Potential Costs:
* Shower - £1 for 5 minutes
* Wood - £5 for 10 logs
```

In Java the interface might look something like the below:

```java
String checkIn(String camperInformation);
```

## Wildlife Service

Collaborating services: none.

This service is responsible for reporting any wildlife seen within a given period of time. It will return something like the below:

```
* 01/03/2019 - Bear spotted
```

If there is nothing to report, it will return the below instead:

```
Nothing to report
```

In Java the interface might look something like the below:

```java
String getWildlifeReportFor(Instant dateOfStay);
```

## Rules Service

Collaborating services: none.

This service is responsible for the rules of the campsite. It will return something like the below:

```
* No food should be left unattended at site
```

In the unlikely event there are no rules at the campsite, it will return the below:

```
There are no rules here
```

## Weather Service

Collaborating services: none.

This service is responsible for reporting the forecasted weather for a given period of time. It will return something like the below:

```
Between 01/02/2019 and 09/02/2019:
01/02/2019 - mostly cloudy
02/02/2019 - mostly cloudy
03/02/2019 - mostly cloudy
04/02/2019 - cloudy with some light rain
05/02/2019 - cloudy with some light rain
06/02/2019 - cloudy with some light rain
07/02/2019 - sunny with spells of light rain
08/02/2019 - clear skies
09/02/2019 - clear skies
```

If the dates requested are too far in the future then it will return the dates it knows about. For instance, if between the `01/02/2019` and `03/02/2019` were requested, and the service only knew the weather for the `1st` and `2nd` day in February, it would simply return the following:

```
Between 01/02/2019 and 02/02/2019 (weather from 03/02/2019 not known at this time):
01/02/2019 - mostly cloudy
02/02/2019 - mostly cloudy
```

Assume for all other cases it will be able to return a forecast.

In Java, the interface might look something like the below:

```java
String getWeatherFor(Instant from, Instant to);
```

## Costs Service

Collaborating services: none.

This service is responsible for reporting any potential costs that might be incurred whilst staying at the campsite. All costs are in local currency (pounds in the examples shown). The service will return something like the below:

```
* Shower - £1 for 5 minutes
* Wood - £5 for 10 logs
```

In Java, the interface might look something like the below:

```java
String getCostsForCampsite();
```

## Let's Code

You have read the briefing (I hope), now it is time to go and develop a solution. You might be wondering why everything returns strings. Well we are camping so going primitive seems appropriate :stuck_out_tongue::drum:

Also assume for now that the services only work for a single campsite, hence why no campsite id's are passed around.

### Examples

**Given** today's date is the `01/02/2019` 

**And** there was a bear spotted on the `20/01/2019`

**And** the rules stipulate that `no food should be left unattended at site`

**And** the weather for both the `01/02/2019` and the `02/02/2019` is `mostly cloudy`

**And** the potential costs incurred during the camper's stay are:

```
* Shower - £1 for 5 minutes
* Wood - £5 for 10 logs
```

**When** a camper checks in with the following details:

```
Name of camper: Avid Camper
Car Registration (if applicable): REG 123
Check-out: 02/02/2019
```

**Then** the campsite check-in service should produce the following report:

```
Check-in for: Avid Camper
Car Registration: REG 123
Check-out: 02/02/2019
---
INFORMATION:
---
Wildlife:
* Bear spotted 12 days ago
--
Rules:
* No food should be left unattended at site
--
Weather:
* Day 1 - mostly cloudy
* Day 2 - mostly cloudy
--
Potential Costs:
* Shower - £1 for 5 minutes
* Wood - £5 for 10 logs
```