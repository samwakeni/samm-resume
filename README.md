# Samm Portfolio

A Laravel-based personal portfolio and online portfolio for **Samkelwe
"Samm" Wakeni**. The application is intended to present professional
experience, technical skills, projects, education, certifications,
services, and contact information through a public website, with a
protected administration area for managing the content.

> **Project status:** This README describes the intended application
> structure and setup. Confirm that the corresponding routes,
> migrations, models, seeders, and features have been implemented in the
> project before relying on every command or module below.

## Contents

-   [Overview](#overview)
-   [Technology stack](#technology-stack)
-   [Requirements](#requirements)
-   [Local installation](#local-installation)
-   [Environment configuration](#environment-configuration)
-   [Database setup](#database-setup)
-   [Run the application](#run-the-application)
-   [Application areas](#application-areas)
-   [Storage and uploads](#storage-and-uploads)
-   [Useful Artisan commands](#useful-artisan-commands)
-   [Testing](#testing)
-   [Production deployment checklist](#production-deployment-checklist)
-   [Security notes](#security-notes)
-   [Troubleshooting](#troubleshooting)

## Overview

Samm Portfolio has two main areas:

### Public website

The root URL (`/`) is the welcome page. It uses the supplied
green-and-black hero design as its visual reference and should include:

-   Profile photograph and professional name
-   Professional titles: **Full-Stack Developer \| Graphic Designer**
-   Social profile links
-   **Work with me** and **Download CV** actions
-   An **About me** indicator that navigates to the profile content
-   A discreet **Login** link in the top-right corner

The public resume and portfolio content can include:

-   Professional summary and profile
-   Employment history
-   Technical skills
-   Featured projects and project detail pages
-   Education
-   Courses and certifications
-   Services
-   Contact form

### Administration area

The administration area is protected by authentication. The login page
is at `/admin/login`, and successful authentication should redirect the
administrator to `/admin/dashboard`.

The dashboard is intended to manage profile details, experience,
projects, skills, education, certifications, services, social links,
resume documents, contact messages, and website settings. Public
registration should remain disabled.

## Technology stack

The intended stack is:

-   PHP 8.3 or later
-   Laravel 12
-   MySQL or MariaDB
-   Blade templates
-   Bootstrap 5+
-   JavaScript and jQuery
-   Font Awesome
-   Vite and NPM

The project uses Laravel's standard application conventions. Check
`composer.json` and `package.json` for the exact dependencies installed
in your copy.

## Requirements

Install the following before setting up the project:

-   PHP 8.3+
-   Composer
-   MySQL or MariaDB
-   Node.js and NPM
-   Laravel Herd for the local Windows development environment
-   Git (recommended)

PHP extensions should meet Laravel's requirements and those of the
packages installed by the project. If Composer reports a missing
extension, enable it in the PHP configuration used by Herd.

## Local installation

### 1. Open the project

Open the `samm-portfolio` directory in your terminal or code editor.

With Laravel Herd, place the project inside a directory that Herd
serves, or add the project directory to Herd's parked/linked sites as
appropriate for your installation.

### 2. Install PHP dependencies

``` bash
composer install
```

### 3. Create the environment file

If `.env` does not exist, copy the example file:

``` bash
# Windows PowerShell
Copy-Item .env.example .env

# macOS / Linux
cp .env.example .env
```

### 4. Generate the application key

``` bash
php artisan key:generate
```

### 5. Install frontend dependencies

``` bash
npm install
```

Build the frontend assets:

``` bash
npm run build
```

During active frontend development, use:

``` bash
npm run dev
```

Keep the Vite development process running while working on frontend
assets.

## Environment configuration

Update the relevant values in `.env`. The following is an example for a
local MySQL/MariaDB database; adjust the username, password, port, and
URL to match your machine.

``` dotenv
APP_NAME="Samm Portfolio"
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://samm-portfolio.test

LOG_CHANNEL=stack
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=samm_portfolio
DB_USERNAME=root
DB_PASSWORD=

SESSION_DRIVER=database
CACHE_STORE=database
QUEUE_CONNECTION=database
```

Notes:

-   `APP_KEY` is populated by `php artisan key:generate`.
-   XAMPP or MariaDB installations may use a port other than `3306`.
    Check the database server configuration.
-   Set `DB_PASSWORD` if your database account has a password.
-   Do not commit `.env` or production secrets to source control.
-   The session and cache drivers above require their corresponding
    database tables if those drivers are used. Check the migrations and
    your Laravel version/configuration. Alternatively, use drivers that
    are already configured for your environment.

After changing environment values, clear cached configuration:

``` bash
php artisan config:clear
```

## Database setup

### 1. Create the database

Create a database named `samm_portfolio` in phpMyAdmin or your preferred
database client.

You can also run:

``` sql
CREATE DATABASE samm_portfolio
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

### 2. Check the connection

``` bash
php artisan config:clear
php artisan migrate:status
```

If the command cannot connect, check that MySQL/MariaDB is running and
that the `.env` host, port, database name, username, and password are
correct.

### 3. Run migrations

``` bash
php artisan migrate
```

### 4. Seed initial data

If the project's seeders have been created and populated:

``` bash
php artisan db:seed
```

Or run migrations and seeders together:

``` bash
php artisan migrate --seed
```

The intended initial data may include the profile, work experience,
projects, skills, education, certifications, services, social links, and
website settings. Only data actually defined in the seeders will be
inserted.

If an administrator account is created by a seeder, use the credentials
configured by that seeder and change the password after the first login.
Do not assume default credentials.

> **Warning:** `php artisan migrate:fresh` drops all existing tables
> before recreating them. It deletes the data in those tables. Use it
> only on a disposable development database when you are certain the
> data can be lost.

## Run the application

With Herd configured to serve the project, open:

-   Public website: `http://samm-portfolio.test`
-   Administrator login: `http://samm-portfolio.test/admin/login`
-   Administrator dashboard:
    `http://samm-portfolio.test/admin/dashboard` (requires
    authentication)

If you are not using Herd's automatic site serving, you can start
Laravel's development server:

``` bash
php artisan serve
```

Then open the URL shown in the terminal, usually
`http://127.0.0.1:8000`.

## Application areas

The planned public routes are:

  Route                Purpose
  -------------------- ----------------------------
  `/`                  Welcome page
  `/about`             Professional profile
  `/resume`            Online resume
  `/experience`        Work history
  `/skills`            Technical skills
  `/projects`          Project listing
  `/projects/{slug}`   Project details
  `/education`         Education history
  `/certifications`    Courses and certifications
  `/services`          Professional services
  `/contact`           Contact form

The planned administration routes are grouped under `/admin` and should
be protected by authentication middleware.

  Area             Intended management
  ---------------- --------------------------------------------------------
  Dashboard        Portfolio overview and content statistics
  Profile          Name, titles, biography, photo, and contact visibility
  Experience       Employment history and responsibilities
  Projects         Project descriptions, links, technologies, and images
  Skills           Skill categories and individual skills
  Education        Qualifications and subjects
  Certifications   Courses, training, and certificates
  Services         Service descriptions and icons
  Social links     Professional social profiles
  resume           Upload and manage CV documents
  Messages         Contact form submissions
  Settings         Website metadata and appearance settings

Routes and features may differ until the relevant modules have been
implemented. Check `routes/web.php` for the routes available in the
current project.

## Storage and uploads

If the application stores public profile photographs, project images, or
other public assets on Laravel's public disk, create the storage
symlink:

``` bash
php artisan storage:link
```

Use Laravel's storage APIs for uploaded files. Validate file type and
size on the server, generate safe filenames, and avoid allowing uploaded
executable files to be served as code.

Keep private documents, backups, and sensitive files outside publicly
accessible directories. If the CV is intended to be publicly
downloadable, make that visibility explicit in the application.

## Useful Artisan commands

  -------------------------------------------------------------------------------------
  Command                                           Purpose
  ------------------------------------------------- -----------------------------------
  `php artisan about`                               Show application and environment
                                                    information

  `php artisan route:list`                          List registered routes

  `php artisan migrate:status`                      Show migration status

  `php artisan migrate`                             Run outstanding migrations

  `php artisan migrate --seed`                      Run migrations and seeders

  `php artisan db:seed`                             Run database seeders

  `php artisan make:model Example -m`               Create a model and migration

  `php artisan make:controller ExampleController`   Create a controller

  `php artisan optimize:clear`                      Clear framework caches

  `php artisan config:clear`                        Clear configuration cache

  `php artisan storage:link`                        Create the public storage symlink

  `php artisan test`                                Run automated tests
  -------------------------------------------------------------------------------------

## Testing

Run the automated test suite with:

``` bash
php artisan test
```

Before considering the application ready, test at least:

-   Public welcome page loads without authentication.
-   Login link opens `/admin/login`.
-   Unauthenticated users cannot access administration pages.
-   Valid administrator credentials open the dashboard.
-   Public registration is unavailable.
-   Each implemented content module can create, view, update, and delete
    records as intended.
-   Uploaded images and documents are validated and displayed/downloaded
    correctly.
-   Contact form validation and submission work.
-   Private contact details are not displayed when configured as hidden.
-   CV download works and does not expose unintended files.
-   Public pages work on desktop and mobile screen sizes.

## Production deployment checklist

Before deploying to a live server:

1.  Back up the database and uploaded files.
2.  Configure production database credentials in the hosting
    environment.
3.  Set `APP_ENV=production`.
4.  Set `APP_DEBUG=false`.
5.  Configure a secure `APP_KEY`.
6.  Configure production mail settings if contact notifications are
    enabled.
7.  Install PHP dependencies using the production Composer options
    supported by the hosting environment.
8.  Build frontend assets with `npm run build` before uploading, or
    build them in the deployment environment.
9.  Run migrations using `php artisan migrate --force`.
10. Create the storage symlink if supported by the hosting environment.
11. Ensure Laravel's `storage` and `bootstrap/cache` directories are
    writable by the application.
12. Configure the web server document root to point to Laravel's
    `public` directory.
13. Enable HTTPS.
14. Verify login, contact form, file uploads, CV downloads, and public
    pages.
15. Configure backups and monitor application logs.

Never expose the project root, `.env`, source files, or private storage
through the public web root.

## Security notes

-   Use strong, unique administrator passwords.
-   Keep Laravel, PHP, Composer packages, and NPM packages updated.
-   Do not commit `.env` files, API keys, passwords, or private CV files
    to a public repository.
-   Protect all administration routes with authentication and
    authorization.
-   Validate and authorize every create, update, delete, and upload
    operation on the server.
-   Keep CSRF protection enabled for state-changing web requests.
-   Apply login throttling and secure session settings.
-   Escape user-provided content in Blade views.
-   Restrict uploaded file types and sizes.
-   Avoid displaying private contact details unless explicitly enabled.
-   Disable debug mode in production.

## Troubleshooting

### Database connection refused

-   Confirm that MySQL/MariaDB is running.
-   Check `DB_HOST` and `DB_PORT`.
-   Verify the database exists.
-   Verify the database username and password.
-   Run `php artisan config:clear` after editing `.env`.

### `php artisan migrate` reports no database configured

-   Check that `.env` exists.
-   Confirm `DB_CONNECTION=mysql`.
-   Confirm the database settings are correct.
-   Clear cached configuration and retry.

### Vite manifest not found

Build the frontend assets:

``` bash
npm install
npm run build
```

### Uploaded images are not visible

Create the storage link:

``` bash
php artisan storage:link
```

Also verify the file's storage disk, path, and permissions.

### Changes to `.env` do not take effect

Run:

``` bash
php artisan optimize:clear
```

Then retry the relevant command.

------------------------------------------------------------------------

## Project information

**Project:** Samm Portfolio\
**Owner:** Samkelwe "Samm" Wakeni\
**Purpose:** Professional portfolio, online resume, and private content
management system\
**Local development domain:** `samm-portfolio.test`

For project-specific setup details, check the current `.env.example`,
`composer.json`, `package.json`, `routes/web.php`, migrations, and
seeders.
