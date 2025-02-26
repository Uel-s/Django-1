# Render Deployment Steps

## Switching from SQLite to PostgreSQL

1. **Create a Render Account**: Sign up for a Render account and link it to your GitHub.
2. **Create a PostgreSQL Project**: 
   - Click on "New" and select "Postgres".
   - Fill in the required fields and set the region to Frankfurt, then click "Create".
3. **Install Required Packages**:
   ```bash
   pip install dj-database-url
   pip install psycopg2
   ```
4. **Update `settings.py`**:
   - Import the necessary modules:
     ```python
     import dj_database_url
     import os
     ```
   - Override the default SQLite database with PostgreSQL:
     ```python
     DATABASES["default"] = dj_database_url.parse("external-connection from render-postgres")
     ```
5. **Migrate the Database**:
   ```bash
   python manage.py migrate
   ```

## Download DBeaver Tool

1. **Connect DBeaver with PostgreSQL**:
   - Use the dropdown arrow above the file section to connect.
   - Fill in the fields extracted from the external connection string.
   - Example connection string breakdown:
     ```
     HOST: dpg-cuvb2ii3esus73bnme70-a.frankfurt-postgres.render.com
     DATABASE: puddle_vic6
     USERNAME: puddle_vic6_user
     PASSWORD: AS7h0sWqmXE2LnNaNx7VrHO6w9myhAve
     ```
   - Test the connection to ensure it is successful.

## Deploying to Web Service

1. **Create a New Web Service**:
   - Click "New" and select "Web Service", filling in the required fields without connecting yet.
2. **Update `settings.py`**:
   - Set the following environment variables:
     ```python
     DEBUG = os.environ.get("DEBUG", "False").lower() == "true"
     ALLOWED_HOSTS = os.environ.get("ALLOWED_HOSTS").split(" ")
     SECRET_KEY = os.environ.get("SECRET_KEY")
     database_url = os.environ.get("DATABASE_URL")
     ```
   - Change the database connection string:
     ```python
     DATABASES["default"] = dj_database_url.parse(database_url)
     ```
3. **Handle Migration Errors**: If an error occurs during migration, ensure the environment variables are set correctly.
4. **Install Gunicorn**:
   ```bash
   pip install gunicorn
   ```
5. **Update Requirements**:
   ```bash
   pip freeze > requirements.txt
   ```
6. **Set Environment Variables in Web Service**:
   - Add the following variables:
     ```python
     ALLOWED_HOSTS = domain_name
     DATABASE_URL = internal connection key from postgres
     DEBUG = True
     SECRET_KEY = any password
     ```
7. **Deploy the Application**: Click the deploy command to finalize the deployment.
