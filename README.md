# Teamup
A package designed to facilitate easy interaction with TeamUp API.

# Quickstart

## Installation 🛠️

From PyPI
py_teamup is available on PyPI. To install with `pip`, just run

```
pip install py_teamup
```

##  Setup 🔨

Review the `.env.example` file for the required/optional environment variables.
- The email and password are not actually used currently.
- Filling in all others are required beside `TEAMUP_DEFAULT_CALENDAR_ID` which is optional.
    - Working with one calendar and multiple sub_calendars seems more likely. Which is why I have it set statically in `TEAMUP_DEFAULT_CALDENDAR_ID`. Can get this value here from your own calendar:
    ![calendar_id](img/teamup_calendar_key_example.png)

> Get token and api key from teamup: https://teamup.com/api-keys/request

## Usage - Commands/Features

This covers the main use cases for the library.

```python
from teamup_connect import TeamUP

teamup = TeamUP()

# # Get calendar - returns dict full of calendar info.
calendar_info = teamup.get_calendar(calendar_key_or_id=DEFAULT_CALENDAR_ID)

# Get list of subcalendars - returns dict of subcalendars and their info.
sub_calendars = teamup.get_subcalendars(calendar_key_or_id=DEFAULT_CALENDAR_ID)
sub_calendar_ids = [calendar["id"] for calendar in sub_calendars]

# Or get a subcalendar by its name
sub_calendar_id = teamup.get_subcalendar_id_from_name(
    calendar_key_or_id=DEFAULT_CALENDAR_ID, calendar_name="Daily Routine"
)

# With a sub_calendar_id, we can now create an event for that calendar.
# Note: the teamup class has a CalendarEvent data class that is required
    # to create an event (for ease of use and validation).
event_to_create = teamup.CalendarEvent(subcalendar_ids=[sub_calendar_id], title="Test Event")

created_event = teamup.create_calendar_event(
    calendar_key_or_id=DEFAULT_CALENDAR_ID, calendar_event=event_to_create
)
created_event_id = created_event["id"]

# Delete the event created
event_undo_id = teamup.delete_calendar_event(
    calendar_key_or_id=DEFAULT_CALENDAR_ID, event_id=created_event_id
)

# Get all events for the subcalendar - This example uses the optional parameter with a date range
optional_params = {
    "startDate": "2024-07-18",
    "endDate": "2024-08-10",
}
events = teamup.get_calendar_events(calendar_key_or_id=DEFAULT_CALENDAR_ID, query=optional_params)


```

# Development  🔧
