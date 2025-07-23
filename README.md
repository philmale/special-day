# special-day
Home Assistant script to work out if today is a 'special day'.
# Install
A script, just paste the script into Home Assistant. It sets two 'helpers' that you will need to create before you run it, `input_text.special_day_text` is a text helper (100 characters is fine) that will contain the text describing the 'day' and is worded to complete the phrase 'today is...', and `input_boolean.special_day` is a toggle helper that is turned on if today is a 'special day' and off if it isn't.

I run this script once a day in the morning (around 08:00 on a time trigger automation) to do notifications, change motion and outside light colours to match the day etc by watching and using the helpers.

It's run once a day, so it's not designed to be a super optimal script - it's written to be REALLY simple to read and add and play with over time - so everything is very 'wordy' for a reason.

The script has the following documentation built into the description:

  # Check For Special Day

  Check if today is a special day against a set of calculated and pre-set
  special days and set a text helper with the decription of the day, and
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
  It should be run daily or at specific times to ensure the helpers are updated.

  ## Requirements

  Requires two helper variables to be defined:
  ```text
    input_boolean.special_day - set to true if today is a special day, or false
    input_text.special_day_text - set to the text description, or empty
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
  1/4-1@Sun - Last Sunday before April
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
