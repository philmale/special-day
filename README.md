# special-day
Home Assistant script to work out if today is a 'special day'.
# Install
A script, just paste the script into Home Assistant. It sets two 'helpers' that you will need to create before you run it, `input_text.special_day_text` is a text helper (100 characters is fine) that will contain the text describing the 'day' and is worded to complete the phrase 'today is...', and `input_boolean.special_day` is a toggle helper that is turned on if today is a 'special day' and off if it isn't.

I run this script once a day in the morning (around 08:00 on a time trigger automation) to do notifications, change motion and outside light colours to match the day etc by watching and using the helpers.

It's run once a day, so it's not designed to be a super optimal script - it's written to be REALLY simple to read and add and play with over time - so everything is very 'wordy' for a reason.

The script will check a datetime helper to make sure it only runs once a day. You can set the `force` field to make it run regardless.

If you create a calendar in Home Assistant called `Special Days` you can also put all day events in there and they will be picked up if you
uncomment the marked bit of the script. I use that to let the family add things and to track birthdays when you want them to fire at midnight.

Here is the automation I use to trigger the script, if it fires at midnight to say 'Happy Birthday' then it's still safe to run at 08:00 because the
datetime helper check will stop it running. If it hasn't been triggered at mignight then normal 08:00 execution happens.

This means things like birthdays result in the input_boolean.special_day being turned on at midnight from the Special Days calendar, and less
personal things like 'Bank Holiday' get notified at 08:00.

```yaml
alias: Special Day Controller
description: >-
  Check for a special day, either in the Special Days calendar, or by running
  the Check For Special Days script and set up the Special Day colours.
triggers:
  - trigger: time
    at: "08:00:00"
    id: Time
  - entity_id:
      - calendar.special_days
    trigger: state
    to: "on"
    from: "off"
    id: Calendar
  - entity_id: input_boolean.special_days_active
    trigger: state
    id: Switch
conditions: []
actions:
  - action: script.check_for_special_day
    data: {}
  - action: script.special_day_colour_set
    data: {}
  - action: script.special_day_announce
    metadata: {}
    data: {}
mode: single
```

The `script.special_day_colour_set` and `script.special_day_announce` are simple little scripts that set the colour of our outside house lights to 
match the special day when they come on in the evening, and the announce simply says 'Its `input_text.special_day_text`' over our Alexa's.

# Description

Check if today is a special day against a set of calculated and pre-set
special days and set a text helper with the description of the day, and
a boolean helper if the day is special.

The special days are defined in a dictionary in the script, and can be
modified to add or remove special days as needed. The script also
calculates the date of Easter for the current year, and uses that to
determine the dates of Easter-related special days.

## Features

- Calculates the date of Easter for the current year.
- Supports custom special days defined in a dictionary.
- Automatically updates helper variables in Home Assistant.

## Usage

This script should be used by triggering it in Home Assistant to check
for special days and set the appropriate helper variables.
It should be run daily at specific times to ensure the helpers are updated.

## Requirements

Requires three Home Assistnt helpers to be defined:
```text
  input_boolean.special_day - set to true if today is a special day, or false
  input_text.special_day_text - set to the text description, or empty
  input_datetime.special_day_last_checked - date of last execution
```

## Special Day Syntax

The special_days dictionary uses the following syntax to specify a date:
```text
<base> [ ± <count><unit>[@<weekday>] ]
```
where:
```text
  base        Base date or Easter (Easter, 1/5, 25/12)
  ±           Plus or minus
  <count>     Integer number (3, 1, 4)
  <unit>      d, w, or m (+3d, -1w)
  @<weekday>  Optional weekday for that offset (@Mon, @Sun)
```

Some examples:
```text
Easter+39d - 39 days after Easter
1/5+1@Mon - First Monday after 1st May
1/7+3w@Mon - Monday in the third week of July
25/12-4@Sun - 4th Sunday before Christmas
31/3-1@Sun - Last Sunday of March (use end-of-month base, not 1/4, to avoid edge case when 1 Apr is Sunday)
6/5+2w - 2 weeks after the 6th of May
```

## Example Automation

```yaml
alias: Daily Special Day Check
trigger:
  - platform: time
    at: '00:00:00'
action:
  - service: script.check_for_special_day
mode: single
``` 
