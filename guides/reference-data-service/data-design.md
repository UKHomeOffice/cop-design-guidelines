---
category: Reference Data Service
expires: 2020-12-31
order: 2
---

# Database schema design

In order for the Reference Data Service to display data sets, you need to create a database table. Once you create the table the reference data API will automatically expose the specified data. There are specific requirements when designing a database table.

You will put your schema definitions in the Github repository [here](https://github.com/UKHomeOffice/RefData)


First you need to create the table. The table must have a 'primary key' - this might be column name 'id'. It must also have 'validfrom' and 'validto'. Beyond this the structure of your database table will be dependent on your use case. You will need to consult your Data Architect.

```
CREATE TABLE ministry (
  id INTEGER NOT NULL PRIMARY KEY,
  name VARCHAR(60) NOT NULL,
  code VARCHAR(8) NOT NULL,
  validfrom TIMESTAMP WITH TIME ZONE,
  validto TIMESTAMP WITH TIME ZONE,
  updatedby VARCHAR(60) NULL
);
```
**Figure 1. Example database table**


Each Table **must** contain a comment in JSON format containing all the following entities:

* **description** - this is a non-technical description of what the table represents.
* **schemalastupdated** - date-time stamp of when the table schema was last updated.
* **dataversion** - sequential number representing the version of the schema.
* **owner** - a group email like `cop@homeoffice.gov.uk` for the owner responsible.


```
-- Table comment
COMMENT ON TABLE ministry IS '{"label": "Government ministries", "owner": "xyx@test.com", "description": "A list of departments, agencies and public bodies.", "schemalastupdated": "06/03/2019", "dataversion": 1}';
```
**Figure 2. Example of Table Comment**

Each Column **must** contain a comment in JSON format containing the following attributes:

* label
* description

One column **must** also contain:

* businesskey: true

```
-- Column comments
COMMENT ON COLUMN ministry.id IS '{"label": "Identifier", "description": "Database unique identity record."}';
COMMENT ON COLUMN ministry.name IS '{"label": "Name", "description": "The name of the branch or region."}';
COMMENT ON COLUMN ministry.code IS '{"label": "Code", "businesskey": "true", "description": "The code associated with the branch or region."}';
COMMENT ON COLUMN ministry.validfrom IS '{"label": "Valid from date", "description": "Item valid from date."}';
COMMENT ON COLUMN ministry.validto IS '{"label": "Valid to date", "description": "Item valid to date."}';
COMMENT ON COLUMN ministry.updatedby IS '{"label": "Updated By", "description": "Record updated by"}';
```
**Figure 3. Example Column Comments**

The following are optional attributes:

* aliases (comma separated list)
* minimumlength
* maximumlength
* minimumvalue
* maximumvalue




**There should be NO data CSVs committed into the schema definition repository, because they might contain sensitive reference data**

## Grants
When a user logs in via Keycloak, Keycloak will automatically assign a role based on the 'clientid'. This role will correspond to one of the three possible database GRANTS:

* serviceuser - This is used by Camunda to allow new records to be added to the Reference data, and current records to be updated.
* readonlyuser - This user can read all records.
* anonuser - This user should only be added to public tables where authentication is not needed to see the data.

This should be the default for all datasets unless they are deemed sensitive.

Each table must have at least two GRANTs.

```
-- GRANTs
GRANT SELECT,INSERT,UPDATE ON ministry TO ${serviceuser};
GRANT SELECT ON ministry TO ${readonlyuser};
GRANT SELECT ON ministry TO ${anonuser};
```
**Figure 4. Example GRANTS statement**

## Development

Database changes are applied using Flyway. It is a database version control tool. You can find more information on Flyway [here](https://flywaydb.org/documentation/).

Once you have committed the file containing your schema definition, if you subsequently discover a mistake in that file you cannot update it. Instead you have to create a new file with an 'alter' command to fix the mistake/s. **Do not update the existing file. This will cause an exception when Flyway tries to apply changes**  

The following describes how to run Flyway locally to test your schema definition before committing to the repository.


### Setting Up

You need to have cloned the schema definition repository, and have Docker available locally.

Go to `mini` folder - there is a `docker-compose.yml` file that performs the following:
1. Creates Postgres server - `localhost:5432`
2. Creates PostgREST API - `localhost:3000`
3. Applies all the flyway scripts on top of the local database.

Run the following command:

```bash
❯ docker compose up
...
mini_flyway_1 | Migrating schema "public" to version "115 - is81riskfactors"
mini_flyway_1 | Migrating schema "public" to version "116 - flightlookup"
mini_flyway_1 | Migrating schema "public" to version "117 - validation"
mini_flyway_1 | Successfully applied 117 migrations to schema "public" (execution time 00:04.489s)

```
The PostgREST server will run as long as your terminal session is open. If you are using a database that is running in docker then if you stop the session you will lose the data.

Because of the way [PostgREST caches schema](https://postgrest.org/en/v7.0.0/admin.html#schema-reloading), you need to run `docker-compose kill -s SIGUSR1 rest` after flyway has finished the job.


You will need the following environment variables in order to run the 'docker compose up':

* DB_HOSTNAME
* DB_PORT
* DB_NAME
* DB_DEFAULT_NAME
* DB_OPTIONS
* DB_JDBC_OPTIONS
* FLYWAY_USER
* FLYWAY_PASSWORD
* DB_OWNERNAME
* DB_OWNERPASSWORD
* DB_SCHEMA
* FLYWAY_PLACEHOLDERS_AUTHENTICATORUSER
* FLYWAY_PLACEHOLDERS_AUTHENTICATORPASSWORD
* FLYWAY_PLACEHOLDERS_ANONUSER
* FLYWAY_PLACEHOLDERS_SERVICEUSER
* FLYWAY_PLACEHOLDERS_READONLYUSER


### Making Changes

First, create a new flyway file in `schemas/reference`. Follow [flyway documentation](https://flywaydb.org/documentation/), if you are not familiar with this system, and use the existing schema definitions as examples to copy.  

Next go to `docker/flyway_reference_docker.conf`. Update the `flyway.target=` with your script version number e.g.`flyway.target=119`.

Finally raise a PR and wait for validation on the CI server to complete. If anything goes wrong, check the CI build logs. If there is an error you will find a detailed reason for the failure.
