# Use Cases

Find a Time lets anyone create an event, submit availability for an event, and view the current results. There is no sign-in. Participants enter a name when submitting availability.

An **event** has a name, a creator, and between **2 and 10 possible one-hour time slots**. Each time slot begins at the top of the hour. The Submit Availability page and the Results page are separate views.

This phase does not include a separate finalized-event workflow.

---

## Browse Events

Someone opens the application to see what events exist.

They see a list of all events. Each event shows:

- the event name
- the creator name
- the number of submitted responses

If there are no events, the list is empty. This is a valid result and is not treated as an error.

Each event provides a way to open the Submit Availability page and the Results page. The home page also provides a way to create a new event.

### Scenarios

- **Empty list** — There are no events. The list has no rows, but Create Event is still available.
- **List with data** — Each event shows its name, creator, and response count.
- **Open availability** — Choosing an event opens its Submit Availability page.
- **Open results** — Choosing Results opens that event's Results page.
- **Create event** — Choosing Create Event opens the Create Event page.
- **Service unavailable** — If the application cannot reach a required service, the user is shown an error instead of an event list.

---

## Create an Event

Someone wants to create a new event and provide possible meeting times.

They provide:

- an event name
- their name as the event creator
- between **2 and 10** possible meeting times
- each meeting time starts at the top of the hour
- each meeting time represents a one-hour block
- no duplicate start times

If all information is valid, the event is created.

### Scenarios

- **Successful create** — The user enters a valid event name, creator name, and 2–10 valid times. The event is created.
- **Minimum time slots** — Exactly 2 possible times are allowed.
- **Maximum time slots** — Exactly 10 possible times are allowed.
- **Blank event name** — The event is not created. The user is told that an event name is required.
- **Blank creator name** — The event is not created. The user is told that a creator name is required.
- **Too few time slots** — Fewer than 2 times are rejected.
- **Too many time slots** — More than 10 times are rejected.
- **Duplicate time** — The same proposed time cannot appear more than once.
- **Invalid time** — A value that is not a valid date and time is rejected.
- **Time not on the hour** — A proposed time such as 4:30 PM is rejected because time slots must start at the top of the hour.
- **Server failure** — If the application fails unexpectedly, the event is not reported as successfully created.
- **Service unavailable** — If a required service such as the database is unavailable, the event is not reported as successfully created.

---

## Open an Event to Submit Availability

Someone chooses an event because they want to provide their availability.

They see:

- the event name
- the creator name
- all of the event's possible time slots
- a place to enter their name
- checkboxes for choosing the times that work
- a Submit Availability button

The page does **not** show the current results. Results are displayed on a separate page.

### Scenarios

- **Availability page** — The event exists. The user sees the event name, creator, and possible times.
- **No responses yet** — The event can still be opened even if nobody has responded yet.
- **Invalid event identifier** — The request is rejected because the event identifier is not valid.
- **Unknown event** — The identifier is valid, but no event with that identifier exists. The user is told the event was not found.
- **Server failure** — An unexpected application error is shown to the user.
- **Service unavailable** — The page cannot be loaded if a required service is temporarily unavailable.

---

## Submit Availability

Someone has an event open and wants to submit the times they are available.

They enter their name and select the time slots that work for them.

The system records the participant's response and the time slots they selected.

A participant may submit the form with no time slots selected. This means they responded, but none of the proposed times work.

After a successful submission, the system confirms that the response was saved.

### Scenarios

- **Available for some times** — The participant selects some of the event's time slots and submits successfully.
- **Available for all times** — The participant selects every listed time and submits successfully.
- **Available for one time** — The participant selects one listed time and submits successfully.
- **Available for no times** — The participant submits with no time slots selected. The response is still saved.
- **Blank participant name** — The response is rejected and the participant is asked to enter a name.
- **Duplicate selected time** — The same time slot cannot be submitted more than once in the same response.
- **Invalid time-slot identifier** — A submitted slot identifier must be valid.
- **Time slot does not belong to event** — A participant cannot submit a time slot from another event.
- **Invalid event identifier** — The response is rejected if the event identifier itself is invalid.
- **Unknown event** — The response is rejected if the event does not exist.
- **Server failure** — The application does not report the response as saved if an unexpected server error occurs.
- **Service unavailable** — The application does not report the response as saved if a required service is unavailable.

---

## View Results

Someone wants to see the current availability for an event.

They open the Results page and see:

- the event name
- the creator name
- the total number of submitted responses
- each proposed time
- how many participants are available for each time
- the names of the participants available for each time
- participants who responded that none of the proposed times work

The results allow users to compare the proposed times and see which time or times currently have the most availability.

### Scenarios

- **No responses** — The event exists, but nobody has submitted availability. The page still loads successfully and each time shows zero available participants.
- **Results with responses** — The page groups responses by time slot and shows the count and names for each time.
- **None available response** — A participant who submitted no selected times is shown separately from the time-slot availability.
- **Tied best times** — Two or more time slots may have the same highest availability.
- **Invalid event identifier** — The request is rejected because the event identifier is invalid.
- **Unknown event** — The event does not exist. The user is told it was not found.
- **Server failure** — An unexpected application error prevents the results from loading.
- **Service unavailable** — The results cannot be loaded if a required service is temporarily unavailable.

---

## Validation and Error Cases

| Case | Expected behavior |
| --- | --- |
| Event name is blank | Do not create the event. Tell the user the event name is required. |
| Creator name is blank | Do not create the event. Tell the user the creator name is required. |
| Time-slot list is missing | Do not create the event. Tell the user that time slots are required. |
| Fewer than 2 or more than 10 time slots are entered | Do not create the event. Tell the user to choose between 2 and 10 times. |
| The same proposed time appears more than once | Do not create the event. Tell the user duplicate times are not allowed. |
| A proposed time is invalid | Do not create the event. Tell the user to enter a valid date and time. |
| A proposed time does not start on the hour | Do not create the event. Tell the user that times must start at the top of the hour. |
| Participant name is blank | Do not save the response. Ask the participant to enter a name. |
| `selected_slot_ids` is missing | Reject the response because the availability request is incomplete. |
| No time slots are selected | Save the response as meaning none of the proposed times work. |
| The same slot is submitted more than once | Reject the response as invalid. |
| A submitted slot identifier is invalid | Reject the response as invalid. |
| A submitted slot does not belong to the event | Reject the response as invalid. |
| Event identifier is invalid | Reject the request as invalid. |
| Event identifier is valid but the event does not exist | Tell the user the event was not found. |
| Unexpected application/server failure | Show an error and do not report the action as successful. |
| Required service or database is unavailable | Show a temporary service error and do not report the action as successful. |
