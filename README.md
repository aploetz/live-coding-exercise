# live-coding-exercise
This is the repo that I'll use for my live-coding application.

## 1. Astra DB

For this exercise, we're going to use DataStax Astra DB (which is free).

### Register for an Astra DB account

If you do not have an account yet, register and sign in to Astra DB: This is FREE and NO CREDIT CARD is required. [https://astra.datastax.com](https://astra.datastax.com): You can use your `Github`, `Google` accounts or register with an `email`.

### Create an Astra DB instance on the "FREE" plan

Follow this [guide](https://docs.datastax.com/en/astra/docs/creating-your-astra-database.html), to set up a pay as you go database with a free $25 monthly credit. You will find below recommended values to enter:

- **For the database name** - `workshops`

- **For the keyspace name** - `live_coding`

_You can technically use whatever name(s) you want and update the code to reflect the keyspace. This is really to get you on a happy path for the first run._

- **For provider and region**: For Astra DB, select GCP as a provider and then the related region is where your database will reside physically (choose one close to you or your users).

- **Create the database**. Review all the fields to make sure they are as shown, and click the `Create Database` button.

### Create the Astra DB token

Following the [Manage Application Tokens docs](https://docs.datastax.com/en/astra/docs/manage-application-tokens.html) create a token with `Database Admnistrator` roles.

- Go the `Organization Settings`

- Go to `Token Management`

- Pick the role `Database Administrator` on the select box

- Click Generate token

- `appToken:` We will use it as a api token Key to interact with APIs.

#### Save your DB token locally

To know more about roles of each token you can have a look to [this video.](https://www.youtube.com/watch?v=TUTCLsBuUd4&list=PL2g2h-wyI4SpWK1G3UaxXhzZc6aUFXbvL&index=8)

**Note: Make sure you don't close the window accidentally or otherwise - if you close this window before you copy the values, the application token is lost forever. They won't be available later for security reasons.**

> **⚠️ Important**
> ```
> I will show you on screen how to create a token
> but will have to destroy to token immediately for security reasons.
> ```

We are now set with the database and credentials and will incorporate them into the application as we will see below.

## 2. Cassandra Data Model

Let's build a quick model to host data on Nerd Holidays.

### Open CQL Console on Astra OR use the new [Astra CLI](https://www.datastax.com/blog/introducing-cassandra-astra-cli)

```sql
use live_coding;
```

Copy/paste the DDL.

### Table Schema
```sql
CREATE TABLE nerd_holidays (
  year_bucket int,
  event_date date,
  name text,
  id UUID,
  PRIMARY KEY ((year_bucket), event_date, id)
) WITH CLUSTERING ORDER BY (event_date ASC, id ASC);
```

Copy/paste the DML.

### Table Data
```sql
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-01-02',UUID(),'Sci-Fi Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-01-02',UUID(),'Isaac Asimov''s Birthday');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-01-15',UUID(),'Apple Computer Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-02-07',UUID(),'e Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-03-14',UUID(),'Pi Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-03-25',UUID(),'Fall of Sauron Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-04-05',UUID(),'First Contact Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-04-30',UUID(),'International Tabletop Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-05-04',UUID(),'Star Wars Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-05-25',UUID(),'Towel Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-06-16',UUID(),'Captain Picard Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-06-28',UUID(),'CAPS LOCK DAY');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-07-28',UUID(),'SysAdmin Appreciation Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-08-04',UUID(),'International Beer Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-08-12',UUID(),'IBM PC Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-09-19',UUID(),'Talk Like a Pirate Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-09-22',UUID(),'Hobbit Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-10-02',UUID(),'Ada Lovelace Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-10-15',UUID(),'Marty McFly Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-11-23',UUID(),'Tardis Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-11-23',UUID(),'Fibonacci Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-11-30',UUID(),'Computer Security Day');
INSERT INTO nerd_holidays (year_bucket, event_date, id, name)
VALUES (2023,'2023-12-21',UUID(),'Rush 2112 Day');
```

## 3. The `.env` File

Edit your `.env` file with your Astra DB credentials.  You should have the token from above.  You can get the Database ID and Region from the Astra Dashboard.

```bash
export ASTRA_DB_ID=
export ASTRA_DB_REGION=
export ASTRA_DB_APP_TOKEN=
export ASTRA_DB_KEYSPACE=live-coding
```

Instantiate those environment variables:

Mac/Linux:
```bash
source .env
```

On Windows, you can use an `env.bat` file for the same effect:
```bash
set ASTRA_DB_ID=
set ASTRA_DB_REGION=
set ASTRA_DB_APP_TOKEN=
set ASTRA_DB_KEYSPACE=live-coding
```

