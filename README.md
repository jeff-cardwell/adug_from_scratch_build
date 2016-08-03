# ADUG Composer Install Demo

## How to use this repository

1. Clone the repository

    ```
    $ git clone git@gitlab.com:adug/from-scratch.git MY_NEW_PROJECT
    ```

2. Change directory to your local copy

    ```
    $ cd MY_NEW_PROJECT
    ```

3. Install the project (will install Drupal as a dependency)

    ```
    ../MY_NEW_PROJECT$ composer install
    ```

## How to re-create these files without cloning the repository

1. Create a directory where your new Composer package will live, and change to that directory

    ```
    $ mkdir MY_NEW_PROJECT
    $ cd MY_NEW_PROJECT
    ```

2. Initialize new Composer project

    https://getcomposer.org/doc/03-cli.md#init

    ```
    $ composer init
    ```
    -- Note that you can skip the configuration questions by
        using `composer init --no-interaction` instead

    ###The following configuration wizard will be initiated

    ```
    Package name (<vendor>/<name>) [root/MY_NEW_PROJECT]:
    Description []:
    Author [FIRST_NAME LAST_NAME <EMAIL@DOMAIN.com>, n to skip]:
    Minimum Stability []:
    Package Type (e.g. library, project, metapackage, composer-plugin) []:
    License []:
    ```

    The answers used for the this project follow:

    ```
    Package name (<vendor>/<name>) [root/MY_NEW_PROJECT]: adug/from-scratch
    Description []:
    Author [FIRST_NAME LAST_NAME <EMAIL@DOMAIN.com>, n to skip]:
    Minimum Stability []: alpha
    Package Type (e.g. library, project, metapackage, composer-plugin) []: project
    License []:
    ```
3. Require the Drupal package repository

    ```
    $ composer config repositories.drupal composer https://packages.drupal.org/8
    ```
    -- Note

    https://github.com/drupal-composer/drupal-project/issues/175
    https://www.drupal.org/node/2718229

4. Require the following packages
    ```
    $ composer require composer/installers
    $ composer require drupal-composer/drupal-scaffold
    $ composer require drupal/core
    $ composer require drush/drush
    ```
    --Note what each of these is for...
        drupal-scaffold is because of drupal/core


1. Insert more steps

Wait.. :fingers_crossed: :coffee:

    `*` These settings are for informational purposes within your automatically generated composer.json file

    `**` Put relevent information about stability choices and may a link here

## Installation Instructions
