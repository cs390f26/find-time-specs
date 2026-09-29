# Find a Time Sample Data

This file shows example DynamoDB items used by Find a Time.

**Machine-readable copy:** [`sample-data.json`](sample-data.json)

The table uses:

- `event_id` as the partition key
- `record_id` as the sort key

Items with the same `event_id` belong to the same event.

---

## Holiday Party

This event shows the minimum allowed number of time slots and has no participant responses.

### Event Item

```json
{
  "event_id": 1,
  "record_id": "EVENT",
  "record_type": "EVENT",
  "event_name": "Holiday Party",
  "creator_name": "Ben",
  "time_slots": [
    {
      "slot_id": 1,
      "start_time": "2026-10-02T17:00:00-04:00"
    },
    {
      "slot_id": 2,
      "start_time": "2026-10-03T17:00:00-04:00"
    }
  ]
}
```

There are no response items for this event.

---

## CS 390 Study Group

This event has three possible times and demonstrates participants who are available for some, all, and none of the times.

### Event Item

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

### Clannys — Available for Some Times

```json
{
  "event_id": 2,
  "record_id": "RESPONSE#1001",
  "record_type": "RESPONSE",
  "response_id": 1001,
  "participant_name": "Clannys",
  "selected_slot_ids": [
    3,
    5
  ]
}
```

Clannys is available for time slots 3 and 5.

### Alex — Available for All Times

```json
{
  "event_id": 2,
  "record_id": "RESPONSE#1002",
  "record_type": "RESPONSE",
  "response_id": 1002,
  "participant_name": "Alex",
  "selected_slot_ids": [
    3,
    4,
    5
  ]
}
```

Alex is available for every proposed time.

### Diego — Available for No Times

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

The empty `selected_slot_ids` list means Diego submitted a response, but none of the proposed times work.



---

## Project Meeting

This event shows the maximum allowed number of time slots and a participant who is available for only one time.

### Event Item

```json
{
  "event_id": 3,
  "record_id": "EVENT",
  "record_type": "EVENT",
  "event_name": "Project Meeting",
  "creator_name": "Maya",
  "time_slots": [
    {
      "slot_id": 6,
      "start_time": "2026-10-05T10:00:00-04:00"
    },
    {
      "slot_id": 7,
      "start_time": "2026-10-05T11:00:00-04:00"
    },
    {
      "slot_id": 8,
      "start_time": "2026-10-05T12:00:00-04:00"
    },
    {
      "slot_id": 9,
      "start_time": "2026-10-05T13:00:00-04:00"
    },
    {
      "slot_id": 10,
      "start_time": "2026-10-05T14:00:00-04:00"
    },
    {
      "slot_id": 11,
      "start_time": "2026-10-05T15:00:00-04:00"
    },
    {
      "slot_id": 12,
      "start_time": "2026-10-05T16:00:00-04:00"
    },
    {
      "slot_id": 13,
      "start_time": "2026-10-05T17:00:00-04:00"
    },
    {
      "slot_id": 14,
      "start_time": "2026-10-05T18:00:00-04:00"
    },
    {
      "slot_id": 15,
      "start_time": "2026-10-05T19:00:00-04:00"
    }
  ]
}
```

### Finn — Available for One Time

```json
{
  "event_id": 3,
  "record_id": "RESPONSE#1004",
  "record_type": "RESPONSE",
  "response_id": 1004,
  "participant_name": "Finn",
  "selected_slot_ids": [
    6
  ]
}
```

Finn is available for only time slot 6.

---

## Sample Cases Covered

- **Holiday Party**
  - exactly 2 time slots
  - no participant responses

- **CS 390 Study Group**
  - participant available for some times
  - participant available for all times
  - participant available for no times

- **Project Meeting**
  - exactly 10 time slots
  - participant available for only one time

- Each participant submission is stored as one response item.
- `selected_slot_ids` stores the times the participant can attend.
- An empty `selected_slot_ids` list represents a participant who responded that none of the times work.

