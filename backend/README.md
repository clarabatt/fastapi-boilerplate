# Backend

This is a Python backend boilerplate project using FastAPI, SQLAlchemy, Alembic, and Docker.

## First time running the project

1.  Make sure you have Docker installed and running.
2.  Copy the `.env.example` file to `.env` and fill in the required values.
    - Don't use the port 5432 because it's Postgres default port and it may be already used by another service.
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

### Down docker containers

To stop and remove the docker containers, use the following command:

```bash
docker-compose down -v
```

## Running tests

To run the tests, use the following command:

```bash
pytest
```
