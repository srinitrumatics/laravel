# ReadyPaid App: Server Side

This repository contains the server-side code for the ReadyPaid App, built using PHP and Laravel.

**Recommended IDE:** PHPStorm (VSCode should also work)

## Project Architecture

The ReadyPaid application consists of three main parts:

1.  **Client Side:** A Vue application that provides a custom HTML element `<readypaid-app>`.
2.  **Server Side (This Repo):** An API repository (written in PHP) that handles communication with Salesforce and other third-party services.
3.  **Integration Page:** The web page (e.g., a WordPress page or an e-commerce checkout page) that hosts the `<readypaid-app>` custom element.

This is an API server primarily, but it does have a few web pages used for testing and API documentation: 

- http://localhost:8000 contains API documentation.
- http://localhost:8000/rp1.html contains e-commerce integration page using local dev bundles. (readypaid.html used for staging)
- http://localhost:8000/rp2.html contains web integration page using local dev bundles. (rp3.html used for staging)

## Project Setup

- Install PHP, Node.js, Composer and MySQL. 
- Configure a `.env` file. Copy the `.env.example` file to `.env`. Enter your root password for MySQL. Set LOG_CHANNEL=stderr and LOG_LEVEL=debug
- Clone the Repository
- Install PHP dependencies: composer install
- Install PNPM since it's faster than npm: npm i -g pnpm
- Install JS dependencies: pnpm i
- Generate the application key: php artisan key:generate
- Set up the database tables: php artisan migrate
- Populate database tables: php artisan db:seed
- Run data population commands: 
  - php artisan UpdateStandardcStatesCommand
  - php artisan UpdatePickListCommand
- Start the dev server: php artisan serve

## Heroku

For Heroku MySQL, you can use JAWS DB extension. 
Most operations can be done in Heroku via their CLI. 
Download it and login with heroku login. 
You can run the above commands using the heroku client as well.
Make sure to run the migrations. 

```
heroku run php artisan migrate --app readypaid-api-demo
```

NOTE: Session table may need to be manually created. You can try: 

```
heroku run php artisan session:table --app readypaid-api-demo
```

but if it does not work, you can just copy the definition from production database.

In order to clear the cache: 

```
heroku run php artisan optimize:clear --app readypaid-api-demo
```

## Debugging

It is possible to set up PHP debugging with PHPStorm, but for a quick way, 
you can use Log::debug for writing messages to log and you can 
monitor them with:  

`tail -f storage/logs/laravel.log`

## Testing Partner Pages

- Set WEB_APP_DOMAIN=localhost:3000 and PARTNER_DOMAIN=localhost
- Set up a partner account in Salesforce. Mark it strategic partner.
- Upload a logo image to the partner related files in Salesforce. Name it partner_logo. Make it public. 
- Update ReadyPaid page of the partner record in Salesforce.
- Optionally white logo flag if the logo is white and requires dark background (like Hydrofarm).
