# find-time-specs
josh jones clay alvarez finn dempsey

# Find a Time Specs

This repo contains the specification for a simple scheduling app that allows users to create an event with various one-hour time slots submit their availability, and view the combined results.  We will deploy this application in a variety of ways using cloud technologies.

- [Use cases](use-cases.md) - Describes what users will experience with the running system. These cases form the basis for acceptance tests.
- [API](openapi.yaml) - Describes the HTTP/JSON contract between the web browser (client) and the backend (server). 
- [Data model](db.md) - Describes how data is stored in the `EVENTS`, `TIME_SLOTS`, and `AVAILABILITY` tables
- [Sample data](sample-data.md) - Example data covering important application scenarios. These examples are also provided in machine-readable [`sample-data.json`](sample-data.json) format for seeding a database and/or use with tests.
