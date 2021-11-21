# docker_laravel_mailhog
docker-compose for Laravel with mailhog

## Summary
You can create minimum Laravel(LEMP) environment with mailhog using docker-compose.

## Prerequisites
Before using this repository, prepare your Laravel project directory having mailing function.

If you'd like to create a new one, follow the directions below:
1. Create brand new Laravel project directory.
```
docker run --rm --volume $PWD:/app composer create-project laravel/laravel example-app
```
2. Edit .env file.
```
-DB_HOST=127.0.0.1
+DB_HOST=mysql
-DB_USERNAME=root
-DB_PASSWORD=
+DB_USERNAME=test_user
+DB_PASSWORD=test_pass
-MAIL_FROM_ADDRESS=null
+MAIL_FROM_ADDRESS=test@test
```
3. Create users table.
```
docker-compose exec php sh
...
(after entering sh of php)
...
php artisan migrate
```
4. Install Laravel Breeze.
```
docker run --rm --v $PWD:/app composer require laravel/breeze
```
5. Publish the authentication files.
```
docker-compose exec php sh
...
(after entering sh of php)
...
php artisan breeze:install
```
6. Compile the assets.
```
docker run --rm  -v $PWD:/usr/src/app -w /usr/src/app node npm install && npm run dev
```
7. Register a new user.

    (1) Visit the top page(http://localhost/). 

    (2) Click "Register" link near the top-right corner(or visit http://localhost/register).

    (3) Fill in the blanks appropriately and click Register button.

    (4) Click the name of the new user and click "Log Out".

    (5) Click "Log in" link near the top-right corner(or visit http://localhost/login).

    (6) Click "Forgot your password?" link.

    (7) Put the email address you created at the step (3) and click EMAIL PASSWORD RESET LINK button.

See also https://github.com/Asano-Naoki/docker_laravel_minimum for possible errors.



## Usage
Send emails on you Laravel application and the emails are trapped automatically in mailhog. You can check the emails at http://localhost:8025/

NOTE:
You must set MAIL_FROM_ADDRESS a string which represents an email address(e.g. test@test) in your .env file to prevent the "Cannot send message without a sender address" error.


## Author
[Asano Naoki](https://asanonaoki.com/blog/)


## License
Under the MIT License. See [LICENSE](/LICENSE) for details.



