# Find a Time Database Schema

 Three tables used by the Find a Time application: `EVENTS`, `TIME_SLOTS`, and `AVAILABILITY`.

Primary keys use integer IDs. Each event has between 2 and 10 possible one-hour time slots. Time slots belong to a specific event, and participant availability is linked to both the event and the selected time slot.

## EVENTS

Stores the basic information for each event.

| **Column** | **Type** | **Key / relationship** | **Purpose** |
| --- | --- | --- | --- |
| `event_id` | `INTEGER` | Primary key | Unique identifier for an event. |
| `event_name` | `VARCHAR(255)` | — | Name of the event. |
| `creator_name` | `VARCHAR(255)` | — | Name of the person who created the event. |

`EVENTS.event_id` is referenced by `TIME_SLOTS.event_id` and `AVAILABILITY.event_id`.

The response count is not saved in this table. The app gets it by counting the different response_id values for that event in the AVAILABILITY table.

## TIME_SLOTS

Stores the possible meeting times created for each event.

| **Column** | **Type** | **Key / relationship** | **Purpose** |
| --- | --- | --- | --- |
| `slot_id` | `INTEGER` | Primary key | Unique identifier for a time slot. |
| `event_id` | `INTEGER` | Linked to `EVENTS.event_id` | Event that the time slot belongs to. |
| `start_time` | `TIMESTAMP` | — | Start of the one-hour meeting block. |

Each time slot belongs to exactly one event. An event must have at least 2 time slots and may have at most 10.

Each time slot represents a one-hour block beginning at the top of the hour. The application does not need to store an end_time because it can be calculated as one hour after start_time.

Duplicate start times are not allowed within the same event.

## AVAILABILITY

Stores participant responses for an event.

| **Column** | **Type** | **Key / relationship** | **Purpose** |
| --- | --- | --- | --- |
| `availability_id` | `INTEGER` | Primary key | Unique identifier for an availability row. |
| `response_id` | `INTEGER` | Groups rows from one submission | Identifies the rows created by one participant response. |
| `event_id` | `INTEGER` | Linked to `EVENTS.event_id` | Event the participant is responding to. |
| `slot_id` | `INTEGER`, nullable | Linked to `TIME_SLOTS.slot_id` | Time the participant selected. `NULL` means none of the listed times work. |
| `participant_name` | `VARCHAR(255)` | — | Name entered by the participant. |

Only the time slots that a participant says they are available for are stored.

If a participant selects more than one time, the application creates multiple rows with the same `response_id`, `event_id`, and `participant_name`, but different `slot_id` values. This keeps all selections from one submission grouped together.


Relationships

An event can have many time slots and many availability responses.

EVENTS
  event_id
     |
     |---- TIME_SLOTS.event_id
     |
     |---- AVAILABILITY.event_id

TIME_SLOTS
  slot_id
     |
     |---- AVAILABILITY.slot_id
These connections let the app match each event with its time slots and see which people are available for each time.

The app does not save a yes or no for every person and every time. It only saves the times a person says they are available. If none of the times work, the app saves one row with NULL for the time slot.

The Results page uses this data to count how many people are available for each time and show their names.
