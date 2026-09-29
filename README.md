# Find Time Specs

This repo contains the specification for a simple scheduling app that allows users to create an event with **2–10 possible one-hour time slots**, submit their availability, and view the combined results. There is no sign-in. We will deploy this application in a variety of ways using cloud technologies.

- [Use cases](use-cases.md) - Describes what users will experience with the running system, including browsing events, creating an event, submitting availability, viewing results, and handling validation and error cases. These cases form the basis for acceptance tests.
- [API](openapi.yaml) - Describes the HTTP/JSON contract between the web browser and backend, including request and response data, validation rules, and error responses.
- [Data model](db.md) - Describes how events and participant responses are stored as DynamoDB items using an event partition key and separate event and response records.
- [Sample data](sample-data.md) - Provides example DynamoDB items covering no responses, partial availability, full availability, one available time, and a participant who responds that none of the proposed times work. A machine-readable version is provided in [`sample-data.json`](sample-data.json).
- [Views](views.md) - Describes the four application views and what information each view needs: Home/Event List, Create Event, Submit Availability, and Event Results.
- [UI mocks](mockups/) - Contains static HTML/CSS pages for the four views of the system.
