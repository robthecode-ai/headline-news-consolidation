## Public repository

https://github.com/robthecode-ai/headline-news-consolidation

# Headline News Center

A Django news application where Readers follow news, Journalists write
articles, and Editors manage publishers and approve articles.

## Requirements for virtual environment setup

- Python 3.13
- Git
- MariaDB installed and running

These instructions use Windows PowerShell.

## Setup with a virtual environment

### 1. Download the project

Open PowerShell in the folder where you want to download the project:

```powershell
git clone https://github.com/robthecode-ai/headline-news-consolidation.git
cd headline-news-consolidation
```

Run the remaining commands from this folder, which contains manage.py.

### 2. Create and activate the virtual environment

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

If PowerShell blocks activation, run this and try again:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
.\.venv\Scripts\Activate.ps1
```

### 3. Create the database

Log in using your MariaDB root password:

```powershell
mysql -u root -p
```

If PowerShell does not recognise mysql, find your MariaDB installation's
bin folder and run mysql.exe using its full path. For example:

```powershell
& 'C:\Program Files\MariaDB 12.3\bin\mysql.exe' -u root -p
```

Replace 12.3 with your installed version. Enter your password when
prompted; nothing appears while you type.

At the database prompt, run:

```sql
CREATE DATABASE news_db CHARACTER SET utf8mb4;
EXIT;
```

### 4. Configure local settings

Copy the example settings file:

```powershell
Copy-Item .env.example .env
```

Generate your Django secret key:

```powershell
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

Copy the generated key, then open .env:

```powershell
notepad .env
```

Replace these values and keep the quotes around the key and password:

```ini
DJANGO_SECRET_KEY='paste-your-generated-key-here'
NEWS_DB_NAME=news_db
NEWS_DB_USER=root
NEWS_DB_PASSWORD='your-MariaDB-password'
```

Use the database name you created in step 3. Leave the other settings
unchanged for the default local setup.

Save and close the file. Keep .env private; do not upload it to GitHub.

### 5. Prepare and start the application

Run these commands in order:

```powershell
python manage.py migrate
python manage.py setup_groups
python manage.py check
python manage.py runserver
```

Open http://127.0.0.1:8000/ in your browser.
Press Ctrl+C in the terminal to stop the server.

To start it again later, open PowerShell in the project folder and run:

```powershell
.\.venv\Scripts\Activate.ps1
python manage.py runserver
```

## Run with Docker Compose

Install Docker with Docker Compose. On Windows, open Docker Desktop
and wait until its engine is running.

Download the project using the instructions above. Open a terminal in
the project root, which contains compose.yaml.

### Configure Docker settings

On first setup, copy .env.example to .env. If .env already exists,
open it to update the required values.

Generate a random value with this command:

```powershell
docker run --rm python:3.13-slim python -c "import secrets; print(secrets.token_hex(32))"
```

Run it three times. Put a different generated value into each of these
settings in your private .env file:

```ini
DJANGO_SECRET_KEY='your-first-generated-value'
DOCKER_DB_PASSWORD='your-second-generated-value'
DOCKER_DB_ROOT_PASSWORD='your-third-generated-value'
```

Keep the remaining settings from .env.example. Keep real secrets in
.env and placeholder values in .env.example.

### Build and start

```powershell
docker compose up --build
```

Compose starts MariaDB, waits for its health check, applies Django
migrations, creates the application role groups, and starts Django's
development server.

Open http://127.0.0.1:8000/ and register an account.

Press Ctrl+C in the terminal to stop both services. Start them again
with:

```powershell
docker compose up
```

Named Docker volumes preserve database records and uploaded images
across container recreation. A fresh installation starts with an empty
application database.

## Sphinx documentation

Open docs/_build/html/index.html in a browser to read the generated
documentation.

With the project's virtual environment dependencies installed and .env
configured, rebuild the documentation from the project root:

```powershell
.\.venv\Scripts\sphinx-build.exe -M html docs docs/_build -W
```

The generated HTML is stored in docs/_build/html.

## Using the app

A fresh installation has no accounts or articles.
Register as a Reader, Journalist, or Editor using a unique email address.
Application roles do not grant Django admin access.

### Editors

Open My publishers to create a publisher and manage its team.
Journalists and other Editors must register before you can assign them.
Keep yourself selected when saving the team.

Use Review queue to approve your publishers' articles.
Editors can only browse and manage articles and newsletters from
publishers they belong to.

### Journalists

Use My articles to create and manage your articles.

For publisher-backed articles, an Editor must first add you to that
publisher's team. Select the publisher when writing the article,
then wait for editorial approval.

For independent articles, leave the publisher empty. Save the article,
then select Publish in My articles.

Editing a published article makes it unpublished again. Publisher-backed
articles need editorial approval again; independent articles can be
republished by their author.

### Readers

Use Subscriptions to follow publishers and Journalists.
My feed shows matching published articles, their sources, and which
subscriptions matched.

### Newsletters and notifications

Journalists and Editors can create newsletters.

Publisher newsletters may contain only approved articles from their
selected publisher. Editors must select a publisher they belong to.
Independent newsletters may contain only their Journalist author's
published independent articles.

Publication notifies subscribers and logs the article through the API.
Creating a newsletter also generates subscriber emails.
Emails print in the server terminal during local development.

Django admin is optional and is only for maintenance. To create an
administrator, run:

```powershell
python manage.py createsuperuser
```

Then visit http://127.0.0.1:8000/admin/.
Application roles do not grant admin access.

## API

POST your username and password to /api/token/. Use the returned token
in this request header:

```text
Authorization: Token YOUR_TOKEN
```

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET, POST | /api/articles/ | List approved articles or submit an article |
| GET | /api/articles/subscribed/ | View subscribed articles |
| GET, PUT, DELETE | /api/articles/<id>/ | View, update, or delete an article |
| POST | /api/approved/ | Log an approved article |

Use http://127.0.0.1:8000 before each endpoint.
Replace <id> with an article ID. Access depends on role and publisher
membership. Editors can only retrieve articles from their publishers.

## Tests

With the virtual environment activated, run:

```powershell
python manage.py test news --verbosity 2
```

Tests use a separate database. Your database account needs permission to
create and remove it. Tests capture emails and mock publication HTTP
requests, so the development server does not need to be running.

## Docker verification

Docker Compose was tested on Windows with Docker Desktop and in a fresh
GitHub Codespace.

Local testing covered publisher team setup, article submission and
approval, reader subscriptions, image uploads, and publication
notifications. Accounts, articles, and images survived container
recreation.

Codespaces testing covered registration, independent article publishing,
image display, and logging out and back in.

This project is configured for local development.