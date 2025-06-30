---
title: 待办、记录、日历的工作流
toc: true
tags:
    - life
date: 2025-06-18
dg-publish: true
---

# 基于warriors的工作流

我是指taskwarrior 和 timewarrior

## cal

khal 添加新任务

```
Usage: khal new [OPTIONS] [START [END | DELTA] [TIMEZONE] [SUMMARY] [::
                DESCRIPTION]]

  Create a new event from arguments.

  START and END can be either dates, times or datetimes, please have a look at
  the man page for details. Everything that cannot be interpreted as a
  (date)time or a timezone is assumed to be the event's summary, if two colons
  (::) are present, everything behind them is taken as the event's
  description.

Options:
  -a, --calendar CAL
  -i, --interactive      Add event interactively
  -l, --location TEXT    The location of the new event.
  -g, --categories TEXT  The categories of the new event, comma separated.
  -r, --repeat TEXT      Repeat event: daily, weekly, monthly or yearly.
  -u, --until TEXT       Stop an event repeating on this date.
  -f, --format TEXT      The format to print the event.
  --json TEXT            Fields to output in json
  -m, --alarms TEXT      Alarm times for the new event as DELTAs comma
                         separated
  --url TEXT             URI for the event.
  --help                 Show this message and exit.
```

example

```sh
khal new -g cat1,cat2 -m 15m 2025-06-22 12:00 2025-06-23 13:00 "cmdline summary" :: descrpition after double colons
```

## caldav ical

一个简单的周期任务
```ics
BEGIN:VCALENDAR
CALSCALE:GREGORIAN
PRODID:-//Apple Inc.//iPhone OS 18.5//EN
VERSION:2.0
BEGIN:VEVENT
CREATED:20250620T052320Z
DESCRIPTION:备注\n周期任务，每周，到7 20
DTEND;VALUE=DATE:20250621
DTSTAMP:20250622T035111Z
DTSTART;VALUE=DATE:20250620
LAST-MODIFIED:20250622T035110Z
RRULE:FREQ=DAILY;UNTIL=20250722;INTERVAL=3
SEQUENCE:1
SUMMARY:Test title add on arch
UID:B1DA3A78-7C4A-44FF-B6E9-6BCDA1D40C82
URL;VALUE=URI:
X-APPLE-CREATOR-IDENTITY:com.apple.mobilecal
X-APPLE-CREATOR-TEAM-IDENTITY:0000000000
TRANSP:OPAQUE
END:VEVENT
END:VCALENDAR
```
