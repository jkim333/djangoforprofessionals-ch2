# Key points from chapter 2 of [Django for Professionals](https://djangoforprofessionals.com/):

- `docker-compose up -d*` -> detached mode, that runs containers in the background
- `docker-compose down` -> to stop the running Docker container
- `docker-compose logs` -> to see the current output and debug any issues
- When working within Docker, we must preface traditional commands with `docker-compose exec [service]`; e.g. docker-compose exec web python manage.py createsuperuser
- To switch over to PostgreSQL, there are three additional steps:
  1. Install a database adapter, psycopg2, so Python can talk to PostgreSQL
  2. Update the DATABASE config in our settings.py file
  3. Install and run PostgreSQL locally
- `docker-compose exec web pipenv install psycopg2-binary==2.8.5` -> to install Psycopg wihtin our Docker host
- Install packages within Docker rather than locally to avoide potential Pipfile.lock conflicts. The Pipfile.lock generation depends heavily on the OS being used. We've specified our entire OS within Docker, including using Python 3.8. But if we install psycopg2 locally on our computer, which has a different environment, the resulting Pipfile.lock file will also be different. Buth then the volumes mount in our docker-compose.yml file, which automatically syncs the local and Docker filesystems, will cause the local Pipfile.lock to overwrite the version within Docker. This can create Pipfile.lock conflicts.
- Always install new software packages within Docker rather than locally!
- To install a new software package:
  1. Install it within Docker
  2. Stop the containers
  3. Force an image rebuild (`docker-compose up -d --build`)
  4. Start the container up again.
- The key point is that with Docker, we don't need to be in a local virtual environment anymore. Docker is our virtual environment. The Docker host essentially replaces our local operating system and within it we can run multiple containers (e.g. our web app and our database)
