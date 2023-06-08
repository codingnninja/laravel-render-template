# laravel-render-template

Do you want to host your Laravel application for FREE and stress free? This guide will take you step by step until your application is hosted on render.com. 

Now, let's get started.

## 1. Set up an account on render.com

Now, you need to [click to sign up](https://dashboard.render.com/login) on Render. It is preferable to sign up with GitHub since you will be using GitHub in the hosting process.

## 2. Force Laravel to accept only https

To force Laravel to allow https by default, make sure you `App/Provider/AppServiceProvider.php` contains the line and block of code below:

```
use Illuminate\Routing\UrlGenerator;
```

Then:

```
public function boot(UrlGenerator $url)
    {
        if (env('APP_ENV') == 'production') {
            $url->forceScheme('https');
        }
    }
```
Make sure your AppServiceProvider contains the code above.

## 3. Create a remote repository with laravel-render-template.

Here, you will use `Laravel-render-template`. So [click to create](https://github.com/codingnninja/laravel-render-template) a remote repo.

![Laravel render template](https://res.cloudinary.com/nyscapp/image/upload/v1686174427/create_laravel-render_template_hpcqts.png)

You need to click on `use this template` and then `create a new repository`. Fill the form and you're done setting up your remote repo.

## 4. Link local repository to remote repository.

It is time to merge the local and remote repo and push everything to the remote repo. In this case, you need to choose the option that fit your need.

### 4.1 Using a new Laravel project.

Do you remember the remote repository you created? Wait, I will tell you what to do with soon. 

Run the following commands line by line:

```sh
git init
git add .
git commit -m "first commit"
git branch -M main
```

![Github url](https://res.cloudinary.com/nyscapp/image/upload/v1686174426/laravel_render_repo_url_sztour.png)

Yeah, it is time to use the url of the repository you created and you can copy it like in the image above. Use it as a replacement for the `url` in the commands below:

```sh
git remote add origin https://github.com/OWNER/REPOSITORY.git
git pull && git push -u origin main
```
### 4.2 Using an existing project with no remote repo.

If you're working on an existing project you want to host, you need the commands below:

```sh
git remote add origin https://github.com/OWNER/REPOSITORY.git
git branch -M main
git pull && git push -u origin main
```
You need to replace the `url` above with the one you copied from your remote repository.

### 4.3 Using an existing project that is already connected to a remote repo.

To do this, you need to reset the remote `url` in your local repository by running the command below:

```sh
git remote set-url origin https://github.com/OWNER/REPOSITORY.git
git remote -v
git add .
git commit -m "first commit"
git pull && git push
```

## 5. Setting up host web service on render
[Click to create](https://dashboard.render.com/select-repo?type=web) a new web service on Render. Then you will be prompted on what to do next. You see Github and Gitlab logos, and `connect account` by the right-hand side of your screen.

Click `connect account` under the Github logo to link your Github account. Choose the repository you want to host after setting up the web service. The image below gives you an insight.

![Render web service section](https://res.cloudinary.com/nyscapp/image/upload/v1686211207/render_create_webserive_w4lh78.png)
## 6. Setting up Enviroment variables

> Make sure your Laravel application is already working.

Here, we will add `environment` variables to the render web service you created the other time. Navigate to the `dashboard` and you will see all the services you have created just like in the image below:

![Dashboard of a render account](https://res.cloudinary.com/nyscapp/image/upload/v1686211207/dashboard_seriveces_cclxzh.png)

Then, click on the web service you want to host on and navigate to `environment`. You should see a view that look like below:

![Render enviroment section](https://res.cloudinary.com/nyscapp/image/upload/v1686211206/laravel_render_environment_variable_chirww.png)

Now, add all the `environment` variables your application depends on one by one. Let's start with `APP_KEY`.

### Generate application key.

Run the code below in the root directory of your Laravel project and copy its output.

```sh
php artisan key:generate --show
```

Add `APP_KEY` as the key and the generated string as its value.
## 7. Setup a database (PostgreSQL & MySQL)

To use MySQL on render, you need to be a paid user so we're using PostgreSQL in this write up. [Create](https://dashboard.render.com/new/database) PostgreSQL database service on render.

![Create new PostgreSQL](https://res.cloudinary.com/nyscapp/image/upload/v1686174426/Laravel_render_postgresql_h5ynsn.png)

Make sure you enter your preferred `name`, `database name`, `user name` and `region`. The rest are optional. So look for `create database` button at the bottom of the page to complete the setup.

Remenber, you can use an external MySQL or ProgreSQL services. You need to follow similar process to set it up.

Note: Make sure the name of your database include something like `db` to differentiate the services on your dashboard but that is up to you.

### Adding database environment variables.

When you finished setting up PostreSQL above, you should have copied `Hostname`, `Port`, `Database`, `Username`, `Password` because you need to add them to the `enviroment` as variables.

![Dashboard of a render account](https://res.cloudinary.com/nyscapp/image/upload/v1686211207/dashboard_seriveces_cclxzh.png)

If you forgot to copy them, you can go back to your dashboard and select your database to copy the information.

![Database connection information](https://res.cloudinary.com/nyscapp/image/upload/v1686212167/database_render_info_hclqgy.png)


|  Key     | Value   |
| -------- | ------- |
| Hostname | dg2kl...|
| Database | urdbname|
| Username | username|
| Password | ********|

The keys and values above should the be added to the `environment` variables on render.com.

![Render enviroment section](https://res.cloudinary.com/nyscapp/image/upload/v1686211206/laravel_render_environment_variable_chirww.png)

```sh
'default' => env('DB_CONNECTION', 'pgsql')
```

And

```php
'pgsql' => [
            'driver' => 'pgsql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', 'Hostname'),
            'port' => env('DB_PORT', '5432'),
            'database' => env('DB_DATABASE', 'dbname'),
            'username' => env('DB_USERNAME', 'username'),
            'password' => env('DB_PASSWORD', '********'),
            'charset' => 'utf8',
            'prefix' => '',
            'prefix_indexes' => true,
            'schema' => 'public',
            'sslmode' => 'prefer',
        ]
```

Explanation on the content of `Laravel-render-template`.
