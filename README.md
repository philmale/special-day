# special-day
Home Assistant script to work out if today is a 'special day'.
# Install
A script, just paste the script into Home Assistant. It sets two 'helpers' that you will need to create before you run it, `input_text.special_day_text` is a text helper (100 characters is fine) that will contain the text describing the 'day' and is worded to complete the phrase 'today is...', and `input_boolean.special_day` is a toggle helper that is turned on if today is a 'special day' and off if it isn't.

I run this script once a day in the morning (around 08:00 on a time trigger automation) to do notifications, change motion and outside light colours to match the day etc by watching and using the helpers.

It's run once a day, so it's not designed to be a super optimal script - it's written to be REALLY simple to read and add and play with over time - so everything is very 'wordy' for a reason.
