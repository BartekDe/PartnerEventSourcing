#PartnerEventSourcing
###Requirements to run
- Composer
- PHP 7.4
- Symfony
- PostgreSQL server

###How to start
1. `composer install`
2. Set up environment variables in `.env` file - both `DATABASE_URL` and `POSTGRES_DSN` can point to the same database
3. `php bin/console doctrine:database:create`
4. `php bin/console doctrine:migrations:migrate`
5. `php bin/console create:event-stream`
6. `symfony serve`

###Description
Event sourcing for the Partner entity. <br>
Current state of Partner entity is stored in partner table.

There are five events related to Partner entity:
- `PartnerWasCreated`
- `PartnerNameChanged`
- `PartnerDescriptionChanged`
- `PartnerNipChanged`
- `PartnerWebpageChanged`

Each event stores the timestamp of its occurence, and the data that changed on the Partner entity plus the metadata

Events for entity are stored in the database table with the name generated by the prooph. <br>
The name of this table can be looked up in the `event_streams` table. When more entities would be added, name of the table storing events for the new entity would also be stored in `event_streams` table

###Endpoints

| Method   |      URL      | Description |  Request body |
|----------|:-------------:|-------------|:--------------|
| POST     | localhost:8000/partner   | Create new partner | <pre>{<br>    "name": "*string - max length 64 chars",<br>    "description": "*string - max length 255 chars",<br>    "nip": "*string - 10 chars",<br>    "webpage": "string - max length 255 chars"<br>}</pre>|
| PUT      | localhost:8000/partner/{partner-uuid} | Update partner with uuid = partner-uuid | <pre>{<br>    "name": "*string - max length 64 chars",<br>    "description": "*string - max length 255 chars",<br>    "nip": "*string - 10 chars",<br>    "webpage": "string - max length 255 chars"<br>}</pre> |
| GET      | localhost:8000/partner    | Get a list of all partners |
| GET      | localhost:8000/events/partner/{partner-uuid}    | Get events for the partner with uuid = partner-uuid

###Problems
Serialization of Partner entity includes empty array of recorded events even if there already are events for the entity