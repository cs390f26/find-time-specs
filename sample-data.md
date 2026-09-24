# Find a Time (Sample Data)

Sample payloads for each table using the database field names, integer IDs, and ISO 8601 timestamps.

**Machine-readable copy:** [`sample-data.json`](sample-data.json)

---

## 1: EVENTS

```json
[
  {
    "event_id": 1,
    "event_name": "Holiday Party",
    "creator_name": "Ben"
  },
  {
    "event_id": 2,
    "event_name": "CS 390 Study Group",
    "creator_name": "Joshua"
  },
  {
    "event_id": 3,
    "event_name": "Project Meeting",
    "creator_name": "Maya"
  }
]
```

---

## 2: TIME_SLOTS

```json
[
  {
    "slot_id": 1,
    "event_id": 1,
    "start_time": "2026-10-02T17:00:00-04:00"
  },
  {
    "slot_id": 2,
    "event_id": 1,
    "start_time": "2026-10-03T17:00:00-04:00"
  },
  {
    "slot_id": 3,
    "event_id": 2,
    "start_time": "2026-09-28T16:00:00-04:00"
  },
  {
    "slot_id": 4,
    "event_id": 2,
    "start_time": "2026-09-29T16:00:00-04:00"
  },
  {
    "slot_id": 5,
    "event_id": 2,
    "start_time": "2026-09-30T18:00:00-04:00"
  },
  {
    "slot_id": 6,
    "event_id": 3,
    "start_time": "2026-10-05T10:00:00-04:00"
  },
  {
    "slot_id": 7,
    "event_id": 3,
    "start_time": "2026-10-05T11:00:00-04:00"
  },
  {
    "slot_id": 8,
    "event_id": 3,
    "start_time": "2026-10-05T12:00:00-04:00"
  },
  {
    "slot_id": 9,
    "event_id": 3,
    "start_time": "2026-10-05T13:00:00-04:00"
  },
  {
    "slot_id": 10,
    "event_id": 3,
    "start_time": "2026-10-05T14:00:00-04:00"
  },
  {
    "slot_id": 11,
    "event_id": 3,
    "start_time": "2026-10-05T15:00:00-04:00"
  },
  {
    "slot_id": 12,
    "event_id": 3,
    "start_time": "2026-10-05T16:00:00-04:00"
  },
  {
    "slot_id": 13,
    "event_id": 3,
    "start_time": "2026-10-05T17:00:00-04:00"
  },
  {
    "slot_id": 14,
    "event_id": 3,
    "start_time": "2026-10-05T18:00:00-04:00"
  },
  {
    "slot_id": 15,
    "event_id": 3,
    "start_time": "2026-10-05T19:00:00-04:00"
  }
]
```

---

## 3: AVAILABILITY

```json
[
  {
    "availability_id": 1,
    "response_id": 1001,
    "event_id": 2,
    "slot_id": 3,
    "participant_name": "Clannys"
  },
  {
    "availability_id": 2,
    "response_id": 1001,
    "event_id": 2,
    "slot_id": 5,
    "participant_name": "Clannys"
  },
  {
    "availability_id": 3,
    "response_id": 1002,
    "event_id": 2,
    "slot_id": 3,
    "participant_name": "Alex"
  },
  {
    "availability_id": 4,
    "response_id": 1002,
    "event_id": 2,
    "slot_id": 4,
    "participant_name": "Alex"
  },
  {
    "availability_id": 5,
    "response_id": 1002,
    "event_id": 2,
    "slot_id": 5,
    "participant_name": "Alex"
  },
  {
    "availability_id": 6,
    "response_id": 1003,
    "event_id": 2,
    "slot_id": null,
    "participant_name": "Diego"
  },
  {
    "availability_id": 7,
    "response_id": 1004,
    "event_id": 3,
    "slot_id": 6,
    "participant_name": "Finn"
  }
]
```

---

## Sample Cases Covered

- **Holiday Party:** 2 time slots and no responses.
- **CS 390 Study Group:** participants available for some times, all times, and no times.
- **Project Meeting:** 10 time slots and a participant available for only one time.
- `response_id` groups multiple selected times from one submission.
- `slot_id: null` shows that a participant responded but none of the times work.
