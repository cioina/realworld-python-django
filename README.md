# ![RealWorld Example App](logo.png)

> ### Django Ninja + Postgres codebase containing real world examples (CRUD, auth, advanced patterns, etc) that adheres to the [RealWorld](https://github.com/realworld-apps/realworld) spec and API.


### [Demo (of another codebase)](https://demo.realworld.how/)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[RealWorld](https://github.com/realworld-apps/realworld)

This codebase was created to demonstrate a fully fledged fullstack application built with [Django Ninja](https://django-ninja.dev) including CRUD operations, authentication, routing, pagination, and more.

It is using the [realWorld-DjangoRestFramework](https://github.com/Sean-Miningah/realWorld-DjangoRestFramework) repository from [Sean-Miningah](https://github.com/Sean-Miningah/) as a base, as porting an app from [Django REST framework](https://www.django-rest-framework.org) to [Django Ninja](https://django-ninja.dev) is an interesting process, and helps you realize that you may do it on your own codebase without having to change anything related to your Django models.

So credit is due to [Sean-Miningah](https://github.com/Sean-Miningah/) for any initial code.

For more information on how to this works with other [frontends/backends](https://en.wikipedia.org/wiki/Frontend_and_backend), head over to the [RealWorld](https://github.com/realworld-apps/realworld) repo.

#### About [RealWorld](https://codebase.show/projects/realworld)
The [RealWorld Demo App](https://codebase.show/projects/realworld?category=backend&language=python) includes many implementations of the same project ([a Medium clone](https://demo.realworld.io/#/)), for which all [frontends](https://codebase.show/projects/realworld?category=frontend) and [backends](https://codebase.show/projects/realworld?category=backend) are supposed to be switchable from one another [as they all follow the same API](https://github.com/realworld-apps/realworld/tree/main/api).

It is supposed to [`reflect something similar to an early-stage startup's MVP`](https://realworld-docs.netlify.app/docs/implementation-creation/expectations), contrarily to some demo apps that are either too little or too much complex, and provide a good way to assert differences between frameworks.

This repository has been accepted as the reference implementation for [Django Ninja](https://django-ninja.dev), see [the Python category for the RealWorld Demo App](https://codebase.show/projects/realworld?category=backend&language=python).

#### About [Django Ninja](https://django-ninja.dev/)
[Django Ninja](https://django-ninja.dev/) is an overlay to [Django](https://www.djangoproject.com) which lets you create APIs while being heavily inspired by [FastAPI](https://fastapi.tiangolo.com). It means it tries to stay as simple as possible for the API creation, while letting you benefit from the whole [Django](https://www.djangoproject.com) ecosystem, including [its ORM](https://docs.djangoproject.com/en/5.0/topics/db/), [its auth system](https://docs.djangoproject.com/en/5.0/topics/auth/), even [its HTML templates](https://docs.djangoproject.com/en/5.0/topics/templates/) if you still need those...

[Django Ninja](https://django-ninja.dev/) is a very good alternative to [Django REST framework](https://www.django-rest-framework.org), as it tries to be less unnecessarily complex and more performant.

As [Django Ninja](https://django-ninja.dev/) (and by extension this repository) only covers the API part, you may then [connect a frontend to it](#connect-a-frontend) after [deploying](#deploying).

#### Code style
In the long term, this repository aims to target as many good practices expected in a Django Ninja project as possible.

#### Quick-and-dirty to production-ready
One of Django Ninja's strengths is enabling rapid prototyping with untyped dictionaries, then iterating toward proper schemas. Explore the codebase at [`f662a5d`](https://github.com/c4ffein/realworld-django-ninja/tree/f662a5d) for an example of a quick-and-dirty `comments` app implementation using raw dicts, broad exception handling, and minimal typing - the kind of code you might write for a fast PoC. The current version shows the refactored, production-ready approach with proper `ModelSchema` definitions and typed responses.

## Usage

1. Clone the Git repository

```shell
  git clone https://github.com/c4ffein/realworld-django-ninja.git
  cd realworld-django-ninja
```

2. Install postgres dependencies, e.g. on Debian

```shell
  sudo apt install -y postgresql-server-dev-all
```

3. Apply [Django migrations](https://docs.djangoproject.com/en/5.0/topics/migrations/)
```shell
  # Apply migrations to the SQLite database
  DEBUG=True make migrate
  # OR
  # Apply migrations to the specified PostgreSQL database
  DATABASE_URL=postgresql://user:password@netloc:port/dbname make migrate
```

4. Run Application
```shell
  # Run in `DEBUG` mode with default settings, connected to the SQLite database
  make run-debug
  # OR
  # Run outside of `DEBUG` mode, connected to the specified PostgreSQL database
  make run DATABASE_URL=postgresql://user:password@netloc:port/dbname ALLOWED_HOSTS=*
```

The [API Documentation](#api-documentation) should then be available at [http://localhost:8000/docs](http://localhost:8000/docs)

### Using Docker and Docker Compose instead

Run:
> docker compose up

If you want to rebuild the Docker Image:
> docker compose up --build

### Testing
- `make test-django`: Django Ninja tests, uses SQLite by default, specify `DATABASE_URL` to use PostgreSQL.
- `make test-postman`: API newman tests (needs the application running, `make submodule`, and `bun` installed).
- `make test-postman-with-managed-server`: same as the previous target, but runs and manages the server itself.

### Deploying
A [Django Ninja](https://django-ninja.dev/) project can be deployed just as any [Django](https://www.djangoproject.com/) project.  
[The documentation is near perfect.](https://docs.djangoproject.com/en/5.0/howto/deployment/)

### Connect a frontend
Choose a frontend from [codebase.show](https://codebase.show/projects/realworld) and configure it as required.  
Some are already included in [`fronts`](https://github.com/c4ffein/realworld-django-ninja/tree/master/fronts) as git submodules for your convenience:
- [khaledosman/react-redux-realworld-example-app](https://github.com/khaledosman/react-redux-realworld-example-app)
- [mutoe/vue3-realworld-example-app](https://github.com/mutoe/vue3-realworld-example-app)

You must `make submodule` to download those (or just `git pull --recurse-submodules`): the regular `git pull` doesn't get the updated submodule code when the reference in this repository is updated by someone else.

| Description | [react-redux-realworld-example-app](https://github.com/khaledosman/react-redux-realworld-example-app) | [vue3-realworld-example-app](https://github.com/mutoe/vue3-realworld-example-app) |
| -------------------- | ------------------------ | ---------------------- |
| Install dependencies | `make front-setup-react` | `make front-setup-vue` |
| Run frontend         | `make front-run-react`   | `make front-run-vue`   |
| Clean dependencies   | `make front-clean-react` | `make front-clean-vue` |

By default, the frontends try to reach the backend at `http://localhost:8000`, but this may be configured with the `API_URL` variable (either as `make front-run-XXX API_URL="http://26.42.13.37:8000"` or `API_URL="http://26.42.13.37:8000" make front-run-XXX`).


## API Documentation
An auto-generated API documentation using [Swagger](https://swagger.io/) is available at the `/docs` route.


## Divergence with the existing, and porting process
### Divergence with [realWorld-DjangoRestFramework](https://github.com/Sean-Miningah/realWorld-DjangoRestFramework)
- Contrarily to the [RealWorld API spec](https://github.com/realworld-apps/realworld/blob/main/api/openapi.yml), [realWorld-DjangoRestFramework](https://github.com/Sean-Miningah/realWorld-DjangoRestFramework) returns [HTTP 404](https://developer.mozilla.org/fr/docs/Web/HTTP/Status/404) for nearly anything that goes wrong. This was replaced by custom exception handlers on the [NinjaAPI](https://django-ninja.dev/guides/errors/) that return errors in the standard RealWorld format.


### Commits that are a good illustration of the migration from [Django REST framework](https://www.django-rest-framework.org) to [Django Ninja](https://django-ninja.dev)
Before the heaviest modifications, some small commits have been made with the intention to well present the migration process.  
*Please note that many tests were added after and not before the migration, as, even if in a real world scenario you would try to add as-much tests before (to ensure that you don't break anything), the goal here was mainly to provide a [Django Ninja](https://django-ninja.dev) of the [RealWorld demo app](https://github.com/realworld-apps/realworld).*
- [`df3f024a`](https://github.com/c4ffein/realworld-django-ninja/commit/df3f024a0fcbb5694de7d29d9a3cc3d50cde111c): Good example of migrating just one route.
  - Focused on the quick fix of the `/api/articles/tag` route in `articles/api.py`, and the modification of `articles/urls.py` that lets that route be handled by the [Django Ninja router](https://django-ninja.dev/guides/routers/).
  - UT is adapted, the existing [Django REST framework](https://www.django-rest-framework.org) `ViewSet` deleted.
- [`e5efe9c3`](https://github.com/c4ffein/realworld-django-ninja/commit/e5efe9c309b9fd46e5978ed09ce4e5fd119844e4): Migrating the `comments` app (tests pass but the route is broken as the [router](https://django-ninja.dev/guides/routers/) is only registered in [`069cb7a7`](https://github.com/c4ffein/realworld-django-ninja/commit/069cb7a7c607457b8275ff9db2bf542c1b85b9de)).
- [`45e472e0`](https://github.com/c4ffein/realworld-django-ninja/commit/45e472e0bd8fc72b4458dc55901dc37c0ff205fa): Most modifications of the `accounts` app.
- [`069cb7a7`](https://github.com/c4ffein/realworld-django-ninja/commit/069cb7a7c607457b8275ff9db2bf542c1b85b9de): Preparing the migration of `articles`.
- [`e7285493`](https://github.com/c4ffein/realworld-django-ninja/commit/e7285493d28abeff453c03003a6e8075cd106e21): Biggest chunk of modification towards the migration of the `articles` app.


## Contributing
If you would like to contribute to the project, please follow these guidelines:
- Fork the repository and create a new branch for your feature or bug fix.
- Make the necessary changes and commit them.
  - `make lint` your changes, otherwise they will be rejected by the CI.
- Push your changes to your forked repository.
- Submit a pull request to the main repository, explaining the changes you made and any additional information that might be helpful for review.

### Opened issues
You may check [the opened issues](https://github.com/c4ffein/realworld-django-ninja/issues) if you want to make your first contributions.

### Should I create an issue?
Seriously, yes, for anything that crosses your mind. This is early-stage, I'll consider any opinion.


## License
- The [original code](https://github.com/Sean-Miningah/realWorld-DjangoRestFramework) is a [Django REST framework](https://www.django-rest-framework.org/) project made by [Sean-Miningah](https://github.com/Sean-Miningah/) and released under the [MIT License](https://github.com/Sean-Miningah/realWorld-DjangoRestFramework/blob/master/LICENSE).
- This project is now also released for the most part under the [MIT License](https://github.com/c4ffein/realworld-django-ninja/blob/master/LICENSE) by [c4ffein](https://github.com/c4ffein/).


C:\Users\cioin\source\repos\python\realworld-python-django>uv sync
Using CPython 3.14.10 interpreter at: C:\Python314\python.exe
Creating virtual environment at: .venv
Resolved 99 packages in 1ms
Installed 76 packages in 10.66s
 + annotated-types==0.8.0
 + antlr4-python3-runtime==4.13.2
 + asgiref==3.13.0
 + attrs==26.1.0
 + aws-xray-sdk==2.16.0
 + blinker==1.9.0
 + boto3==1.43.123
 + botocore==1.43.123
 + certifi==2026.10.23
 + cffi==2.1.1
 + cfn-lint==1.59.0
 + charset-normalizer==3.5.2
 + click==8.5.0
 + cryptography==50.0.2
 + django==6.1.2
 + django-cors-headers==4.9.0
 + django-extensions==4.1
 + django-ninja==1.7.3
 + dnspython==2.9.1
 + docker==7.2.0
 + email-validator==2.3.0
 + flask==3.1.3
 + flask-cors==6.0.5
 + graphql-core==3.3.1
 + idna==3.20
 + itsdangerous==2.2.0
 + jinja2==3.1.6
 + jmespath==1.1.0
 + joserfc==1.7.7
 + jsonpatch==1.39
 + jsonpath-ng==1.15.0
 + jsonpointer==3.7
 + jsonschema==4.26.0
 + jsonschema-path==0.5.0
 + jsonschema-specifications==2025.9.1
 + jwtninja==2.0.0
 + lazy-object-proxy==1.13.0
 + markdown==3.11.2
 + markupsafe==3.0.4
 + moto==5.2.5
 + mpmath==1.3.0
 + networkx==3.7
 + openapi-schema-validator==0.9.0
 + openapi-spec-validator==0.9.0
 + parameterized==0.9.0
 + pathable==0.6.0
 + psycopg2==2.9.13
 + py-partiql-parser==0.6.3
 + pycparser==3.12
 + pydantic==2.14.2
 + pydantic-core==2.50.2
 + pydantic-settings==2.16.2
 + pyjwt==2.15.1
 + pyparsing==3.3.3
 + python-build-standalone==0.1.0 (from file:///C:/Users/cioin/source/repos/python/realworld-python-django)
 + python-dateutil==2.9.0.post0
 + python-dotenv==1.2.4
 + pywin32==312
 + pyyaml==6.0.3
 + referencing==0.37.0
 + regex==2026.9.29
 + requests==2.34.2
 + responses==0.26.3
 + rfc3339-validator==0.1.4
 + rpds-py==2026.9.1
 + s3transfer==0.19.3
 + six==1.17.0
 + sqlparse==0.6.1
 + sympy==1.14.0
 + typing-extensions==4.16.0
 + typing-inspection==0.4.4
 + tzdata==2026.6
 + urllib3==2.8.0
 + werkzeug==3.1.9
 + wrapt==2.5.2
 + xmltodict==1.0.4

C:\Users\cioin\source\repos\python\realworld-python-django>uv run python manage.py runserver 0.0.0.0:8000
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 30 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): accounts, admin, articles, auth, comments, contenttypes, jwt_ninja, sessions.
Run 'python manage.py migrate' to apply them.
November 02, 2026 - 12:34:02
Django version 6.1.2, using settings 'config.settings'
Starting WSGI development server at http://0.0.0.0:8000/
Quit the server with CTRL-BREAK.

WARNING: This is a development server. Do not use it in a production setting. Use a production WSGI or ASGI server instead.
For more information on production servers see: https://docs.djangoproject.com/en/6.1/howto/deployment/

C:\Users\cioin\source\repos\python\realworld-python-django>