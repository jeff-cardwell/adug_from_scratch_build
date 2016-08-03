# ADUG Basic Drupal Composer Install Demo

## How to use this repository

1. Clone the repository and change directory

    ```
    $ git clone git@gitlab.com:adug/from-scratch.git MY_NEW_PROJECT
    $ cd MY_NEW_PROJECT
    ```

2. Install the project (will install Drupal as a dependency)

    ```
    ../MY_NEW_PROJECT$ composer install
    ```

## How to re-create these files without cloning the repository

1. Create a directory where your new Composer package will live, and change to that directory

    ```
    $ mkdir MY_NEW_PROJECT
    $ cd MY_NEW_PROJECT
    ```

2. Initialize a new Composer project

    https://getcomposer.org/doc/03-cli.md#init

    ```
    $ composer init
    ```

    ###The following configuration wizard will be initiated
    -- Note that you can skip the configuration questions by
            using ```composer init --no-interaction``` instead

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
    -- Note that this is needed because Drupal packages are not not indexed by the "official" Composer packagist
    repository (and most likely they never *will* be - for a variety of reasons).  D.O. has made this alternative method available.  It seems like it will be the "official" Drupal.org
    supported way going forward.

    [Using Composer to install Drupal packages through Drupal.org](https://www.drupal.org/node/2718229)

    [[meta] Path forward for Composer support](https://www.drupal.org/node/2551607)

    [support for cmd-line installer support of Drupal.org beta repos, instead of packagist](https://github.com/drupal-composer/drupal-project/issues/175)

4. Require the following packages
    ```
    $ composer require composer/installers
    $ composer require drupal/core
    $ composer require drupal-composer/drupal-scaffold
    $ composer require drush/drush
    ```
    -- Note what each of these is for...

    1. `$ composer require composer/installers`

        > This is for PHP package authors to require in their composer.json. It will install their package to the correct
        location based on the specified package type."

        Basically it will tell Composer what directory your required
        dependencies should be installed into.

        https://github.com/composer/installers

        This list is of the pre-defined types of Drupal packages that ```composer/installers``` allow you to
        use (it can be found in the link above).  You'll be using these whenever you are creating composer.json files
        (for modules, themes, and whatnot) as the package `"type":`.

        - `drupal-core`
        - `drupal-module`
        - `drupal-theme`
        - `drupal-library`
        - `drupal-profile`
        - `drupal-drush`

    2. `$ composer require drupal/core`

        > This is a Git subtree split of Drupal 8's core directory which can be used to build the directory structure
        for a Drupal site and has the following advantages over pulling in the entire upstream Drupal repository:
        > - All the components of the Drupal site including Drupal, contributed modules and themes, as well as external
            libraries can be pulled in via Composer
        > - Drupal and any external libraries can be bootstrapped via Composer (i.e. without installing any modules for
            the external libraries)
        > - One has full control over index.php, .htaccess, robots.txt, etc. as those files will not be overridden by a
            Drupal core update"

        [drupal-core project README.md](https://github.com/drupal-composer/drupal-core/blob/master/README.md)

        Using this package instead of `drupal/drupal` essentially allows you separate updates of core from updates of
        scaffolding files.

    3. `$ composer require drupal-composer/drupal-scaffold`

        >"Composer plugin for automatically downloading Drupal scaffold files (like index.php, update.php, …) when using
        drupal/core via Composer."

        [drupal-scaffold project README.md](https://github.com/drupal-composer/drupal-scaffold/blob/master/README.md)

        We use this plugin because we are using `drupal/core`. This plugin allows you to update the Drupal core files
        *only* (without updating various scaffolding files that you may have customized).  You'll have to read the
        documentation at the above link to get more info.  We won't be manipulating the default actions at this time,
        but you might want investigate that depending on how your workflow is implemented.  Without writing our own
        scripts, this plugin is necessary when using `drupal/core` to move our scaffolding files to the appropriate
        places.

    4. `$ composer require drush/drush`

        why do we need a "local" drush installation?

5. Create a .gitignore file
    1. exclude vendor
    2. exclude core


1. Insert more steps

Wait.. :fingers_crossed: :coffee:

    `*` These settings are for informational purposes within your automatically generated composer.json file

    `**` Put relevent information about stability choices and may a link here

## Installation Instructions
