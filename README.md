<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## Introduction

This is basically a guide on how I manage to develop and easy, step by step and rapid API from scratch using laravel. This guide is useful for people who are beginning to write their own laravel API. Let us begin. Assuming you already installed composer, nodejs and npm. Assuming also that you have been using latest version of PHP. PHP 7.4 as of this writing.

## Walkthrough
1. LARAVEL NEW - Create new project.
2. LARAVEL UI - Install laravel/ui for authentication scaffolding.
3. AUTHENTICATION - Install tymon/jwt-auth. This guide will use JWT for authentication bearer tokens.
4. DATABASE DESIGN - Focus on creation of your database structure first. You can go to [dbdesigner](https://www.dbdesigner.net/) to create your database design. It is easy and user friendly. You may use other designer as well. It is up to you.
5. MIGRATIONS - Once you have a good database design, you can now make use of it as pattern to build your migrations. Create each table migrations now.
6. FACTORY - In order to provide sample test records, create factories for each important master tables/models that you need.
7. 


## Create New App
- `laravel new ProjectNameAPI`

## Install Laravel UI with Authentication
1. `composer require laravel/ui`
2. `php artisan ui vue --auth`
3. `npm install`
4. `npm run dev`

## JWT Authentication

- Install JWT
1. `composer require tymon/jwt-auth`
2. `php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"`
3. `php artisan jwt:secret`

- Update your User Model

```
<?php

namespace App;

use Tymon\JWTAuth\Contracts\JWTSubject;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;

class User extends Authenticatable implements JWTSubject
{
    use HasFactory, Notifiable;

    // Rest omitted for brevity

    /**
     * Get the identifier that will be stored in the subject claim of the JWT.
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * Return a key value array, containing any custom claims to be added to the JWT.
     *
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }
}
```

- Configure the Auth guard

```
'defaults' => [
    'guard' => 'api',
    'passwords' => 'users',
],

...

'guards' => [
    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
    ],
],
```
