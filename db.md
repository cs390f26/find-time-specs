# DynamoDB Data Model

Find a Time uses one DynamoDB table to store events and participant responses.

The table uses:

- `event_id` as the partition key
- `record_id` as the sort key

Items with the same `event_id` belong to the same event.

---

## Access Patterns

The application needs to support these data operations:

- Create an event with 2–10 possible time slots
- List event summaries
- Get one event and its possible time slots
- Record one participant response
- Record a response when none of the proposed times work
- Count submitted responses for an event
- Group responses by time slot
- List participants available for each time slot
- List participants who responded that no time works
- For this phase, listing all events uses a table scan for EVENT items, while operations for a specific event use event_id to retrieve that event and its responses.

---

## Event Item

Each event has one item containing the event information and its possible meeting times.

Example:

```json
{
  "event_id": 2,
  "record_id": "EVENT",
  "record_type": "EVENT",
  "event_name": "CS 390 Study Group",
  "creator_name": "Joshua",
  "time_slots": [
    {
      "slot_id": 3,
      "start_time": "2026-09-28T16:00:00-04:00"
    },
    {
      "slot_id": 4,
      "start_time": "2026-09-29T16:00:00-04:00"
    },
    {
      "slot_id": 5,
      "start_time": "2026-09-30T18:00:00-04:00"
    }
  ]
}
```

### Event Item Fields

| Field | Type | Notes |
| --- | --- | --- |
| `event_id` | Number | Partition key and unique event identifier |
| `record_id` | String | Sort key; `"EVENT"` identifies the event item |
| `record_type` | String | `"EVENT"` |
| `event_name` | String | Required event name |
| `creator_name` | String | Required creator name |
| `time_slots` | List | Contains between 2 and 10 possible meeting times |

Each time slot contains:

| Field | Type | Notes |
| --- | --- | --- |
| `slot_id` | Number | Identifier for a time within the event |
| `start_time` | String | ISO 8601 start time for the one-hour block |

Rules:

- each event has between 2 and 10 time slots
- every time slot belongs to the event containing it
- each time begins at the top of the hour
- each time represents a one-hour block
- duplicate start times are not allowed within the same event

---

## Response Item

Each submitted availability response is stored as its own item.

Example:

```json
{
  "event_id": 2,
  "record_id": "RESPONSE#1001",
  "record_type": "RESPONSE",
  "response_id": 1001,
  "participant_name": "Clannys",
  "selected_slot_ids": [3, 5]
}
```

### Response Item Fields

| Field | Type | Notes |
| --- | --- | --- |
| `event_id` | Number | Partition key identifying the event |
| `record_id` | String | Sort key identifying the response |
| `record_type` | String | `"RESPONSE"` |
| `response_id` | Number | Unique identifier for one submitted response |
| `participant_name` | String | Name entered by the participant |
| `selected_slot_ids` | List | IDs of the event times the participant selected |

A response stores all of the participant's selected time slots in one item.

For example:

```json
{
  "participant_name": "Clannys",
  "selected_slot_ids": [3, 5]
}
```

means Clannys is available for time slots 3 and 5.

---

## Response With No Available Times

A participant may submit a response even when none of the proposed times work.

 the response uses an empty list:

```json
{
  "event_id": 2,
  "record_id": "RESPONSE#1003",
  "record_type": "RESPONSE",
  "response_id": 1003,
  "participant_name": "Diego",
  "selected_slot_ids": []
}
```

This distinguishes between:

- a participant who has not submitted a response, where no response item exists
- a participant who responded that none of the times work, where a response item exists with an empty `selected_slot_ids` list



---

## How the Items Are Related

Items for the same event share the same `event_id`.

Example:

```text
event_id = 2

record_id = EVENT
  CS 390 Study Group
  Joshua
  time slots 3, 4, 5

record_id = RESPONSE#1001
  Clannys
  selected slots 3, 5

record_id = RESPONSE#1002
  Alex
  selected slots 3, 4, 5

record_id = RESPONSE#1003
  Diego
  selected slots []
```

Because these items share the same partition key, the application can retrieve the event and its responses together.

---

## Results

The application calculates the Results page from the event item and its response items.

For each time slot, the application can:

- count how many responses contain that `slot_id`
- list the participant names whose responses contain that `slot_id`
- compare the counts to determine which time or times currently have the most availability

The total response count is the number of response items stored for the event.

A response with an empty `selected_slot_ids` list still counts as a submitted response but does not increase the availability count for any time slot.

---

## Summary



- the event item stores the event information and its possible times
- each participant submission is one response item
- all items for an event share the same `event_id`
- `selected_slot_ids` stores the times a participant can attend
- an empty `selected_slot_ids` list represents a participant who responded that none of the times work
