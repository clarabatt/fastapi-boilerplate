# Backend

This is a Python backend boilerplate project using FastAPI, SQLAlchemy, Alembic, and Docker.

## Prerequisites

1.  Make sure you have Python 3.8+ installed.
2.  Make sure you have Docker installed and running.

## Configuration

### Database

This project uses PostgreSQL as the database, but you can change it to any other database supported by SQLAlchemy. You will need to:

- Update the `docker-compose.yml` file to the desired database image.
- Update the `requirements.txt` file if you need to add any additional database drivers.
- Remove the `psycopg2-binary` dependency if you are not using PostgreSQL.
- Update the `DATABASE_URL` in the `.env` file to match the new database connection string.

## First time running the project

1.  Make sure you have Docker installed and running.
2.  Copy the `.env.example` file to `.env` and fill in the required values.
    > Don't use the port 5432 because it's Postgres default port and it may be already used by another service.
3.  Run the following command to start the database container. This will also create the database if it's the first time:

    ```bash
    docker-compose up -d db
    ```

4.  Create a virtual environment and activate it:

    ```bash
    python -m venv .venv
    source .venv/bin/activate
    ```

5.  Install the dependencies using `uv`:

    ```bash
    uv pip install -r requirements.txt
    ```

6.  Run the database migrations:

    ```bash
    alembic upgrade head
    ```

7.  Run the application:

    ```bash
    uvicorn app.main:app --reload
    ```

## Subsequent runs

1.  Make sure the database container is running:

    ```bash
    docker-compose up -d db
    ```

2.  Run the application:

    ```bash
    uvicorn app.main:app --reload
    ```

## Docker

### Start docker containers

To start the docker containers, use the following command:

```bash
docker-compose up -d
```

### Stop docker containers

To stop the docker containers, use the following command:

```bash
docker-compose stop
```

### Remove docker containers

```bash
docker-compose down
# or to remove volumes as well
docker-compose down -v
```

<details>
<summary>Click to see detailed explanation</summary>

The `docker-compose down` command:

- Stops and removes containers
- Removes networks created by the compose file
- Leaves volumes intact (preserves your data)

The `docker compose down -v` command:

- Does everything above, plus
- Removes named volumes declared in the volumes section of your compose file
- **This deletes the data stored in those volumes**

</details>

## Migrations

This project uses Alembic for database migrations. The migration files are located in the `alembic/versions` directory.

You can create a new migration automatically based on the changes made to your models using the following command after changing your models. If you created new models, make sure to import them in `app/models/base.py` so Alembic can detect the changes.
When you are done editing the models, run:

```bash
alembic revision --autogenerate -m "Your migration message"
```

This will create a new migration file in the `alembic/versions` directory.

> Review the generated migration file to ensure it accurately reflects the changes you made to your models.

To apply the migrations to the database, run:

```bash
alembic upgrade head
```

To downgrade the database to a previous migration, run:

```bash
alembic downgrade <revision>
```

> Replace `<revision>` with the revision ID you want to downgrade to.

### Resetting the database

To reset the database, you can drop all tables and reapply the migrations.

> This will delete all data in the database.

You can do this by running:

```bash
alembic downgrade base
alembic upgrade head
```

## Running tests

To run the tests, use the following command:

```bash
pytest
```
