# Use Cases

Find a Time lets anyone create an event, submit availability for an event, and view the current results. There is no sign-in. Participants enter a name when submitting availability.

An **event** has a name, a creator, and between **2 and 10 possible one-hour time slots**. Each time slot begins at the top of the hour. The Submit Availability page and the Results page are separate views.



---

## Browse Events

Someone opens the application to see what events exist.

They see a list of all events. Each event shows the event name, creator name, and number of submitted responses. When there are no events yet, the list is empty.

Each event provides a way to open the Submit Availability page and a way to open the Results page. The home page also provides a way to create a new event.

### Scenarios

- **Empty list** — There are no events. The list has no rows. Create Event is still available.
- **List with data** — Each event shows the event name, creator name, and response count.
- **Open availability** — Choosing an event opens its Submit Availability page.
- **Open results** — Choosing Results opens that event's Results page.
- **Create event** — Choosing Create Event opens the Create Event page.

---

## Create an Event

Someone wants to create a new event and provide possible meeting times.

They provide:

- an event name
- their name as the event creator
- between **2 and 10** possible meeting times
- each meeting time starts at the top of the hour and represents a one-hour block
- duplicate start times are not allowed within the same event

The system creates the event and its possible time slots.

If the event name or creator name is blank, there are fewer than 2 or more than 10 time slots, a time is invalid, or the same start time is entered more than once, creation fails and the user is told the input is invalid.

### Scenarios

- **Successful create** — The user enters a valid event name, creator name, and 2–10 valid time slots. The event is created and can be opened for availability.
- **Minimum time slots** — The user creates an event with exactly 2 possible times. The event is valid.
- **Maximum time slots** — The user creates an event with exactly 10 possible times. The event is valid.
- **Invalid input** — The event name or creator name is blank, there are too few or too many time slots, or the time choices are invalid. The event is not created.
- **Duplicate time** — The same start time is entered more than once for the event. The event is not created.

---

## Open an Event to Submit Availability

Someone chooses an event because they want to provide their availability.

They see:

- the event name
- the creator name
- all of the event's possible time slots
- a place to enter their name
- checkboxes for choosing the times that work
- a **Submit Availability** button

The page does **not** show the current results. Results are displayed on a separate page.

If the event does not exist, the user is told it was not found.

### Scenarios

- **Availability page** — The event exists. The user sees the event name, creator, and all of its possible times.
- **No responses yet** — The event can still be opened and answered even if nobody has submitted availability yet.
- **Unknown event** — The event does not exist. The user is told the event was not found.

---

## Submit Availability

Someone has an event open and wants to submit the times they are available.

They enter their name and select the time slots that work for them. The system records only the selected times.

A participant may also submit the form with no time slots selected. This means they responded, but none of the proposed times work.

After a successful submission, the system confirms that the response was saved. The participant can then view the separate Results page.

If the participant name is blank, the event does not exist, or a submitted time slot does not belong to the event, the response is not saved.

### Scenarios

- **Available for some times** — The participant selects some of the event's time slots and submits them successfully.
- **Available for all times** — The participant selects every listed time and submits successfully.
- **Available for one time** — The participant selects one listed time and submits successfully.
- **Available for no times** — The participant submits with no time slots selected. The system still records that they responded.
- **Blank participant name** — The response is rejected and the participant is asked to enter a name.
- **Invalid time slot** — A submitted time slot does not belong to the event. The response is rejected.
- **Unknown event** — The event does not exist. The response is not saved.

---

## View Results

Someone wants to see the current availability for an event.

They open the Results page and see the event name, creator name, and total number of submitted responses.

For each possible time, the page shows:

- the time slot
- how many participants are available
- the names of the participants who are available

The page also shows participants who responded that none of the proposed times work.

This summary lets users compare the time slots and see which time or times currently work for the most people.

If the event has no responses yet, all time slots show zero availability.

If the event does not exist, the user is told it was not found.

### Scenarios

- **No responses** — The event exists, but nobody has submitted availability. Each time shows zero available participants.
- **Results with responses** — The page groups responses by time slot and shows the count and names for each time.
- **None available response** — A participant who submitted that none of the times work is shown separately from the time-slot availability.
- **Tied best times** — Two or more time slots may have the same highest availability.
- **Unknown event** — The event does not exist. The user is told the event was not found.

---

## Validation and Error Cases

| **Use case** | **Expected behavior** |
| --- | --- |
| Event name is blank | Do not create the event. Tell the user the event name is required. |
| Creator name is blank | Do not create the event. Tell the user the creator name is required. |
| Fewer than 2 or more than 10 time slots are entered | Do not create the event. Tell the user to choose between 2 and 10 times. |
| The same time is entered more than once | Do not create the event. Tell the user to choose different times. |
| Participant name is blank | Do not save the response. Ask the participant to enter a name. |
| No time slots are selected | Save the response as "none of these times work." |
| A submitted time slot does not belong to the event | Reject the response as invalid. |
| The event does not exist | Tell the user the event was not found. |
| The server or database is unavailable | Show an error and do not report the action as successful. |
