# Movie recommendation feature store homework

The solution is in [`hw_1.ipynb`](hw_1.ipynb). It models users, movies, genres, ratings, historical user features, online user features, and labeled training events. [`ER_diagram.png`](ER_diagram.png) shows the schema. The notebook contains the PostgreSQL DDL, sample data, point-in-time queries, normalization discussion, and reflection.

## Database and notebook setup

The SQL is written for **PostgreSQL 16**. Start PostgreSQL before running the notebook. Without `HW2_DATABASE_URL`, the setup cell connects to `localhost:5432` with the notebook's demonstration `postgres` credentials. Set the variable when your server uses another address, port, or account. The saved `SELECT version()` output is from PostgreSQL 17.11; rerun the notebook against version 16 for final verification.
If another PostgreSQL server uses port 5432, start version 16 on port 5433.
Check the connection and version before opening the notebook.
To use PostgreSQL 16 on port 5433, set a connection URL in the shell **before launching Jupyter**. Replace the username and password with credentials for your server. If your local server requires no password, use `postgresql+psycopg2://USER@localhost:5433/postgres` instead.

```sh
export HW2_DATABASE_URL='postgresql+psycopg2://USER:PASSWORD@localhost:5433/postgres'
```

The notebook setup cell uses the environment variable when present and otherwise connects to the existing local server.
If the package install asks for a kernel restart, restart it and rerun the setup cell. Then run the remaining cells from the top, beginning with `SELECT version()`. The DDL drops and recreates the `hw2_` tables, so rerunning it replaces their data. Saved outputs from PostgreSQL 17 should be refreshed before submission.

## What the notebook demonstrates

- A bridge table for the movie–genre many-to-many relationship and ratings as user–movie interactions.
- Separate offline history and online lookup tables for user features.
- Eighteen historical feature versions across three users, using `[valid_from, valid_to)` intervals.
- AS-OF lookups and a training-data join that select features available at `prediction_ts`.
- Label checks for ratings observed within a 720-hour horizon, plus overlap and leakage checks.
