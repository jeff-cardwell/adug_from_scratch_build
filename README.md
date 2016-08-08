# ADUG Basic Drupal Composer Install Demo

1. [The how and why this repository looks like it does](#how-to-re-create-these-files-without-cloning-the-repository)
2. [How to quickly utilize this repository to get your Drupal 8 site up and running.](#how-to-use-this-repository)



## How to re-create these files without cloning the repository

###Required Software
   - [Composer](https://getcomposer.org/doc/00-intro.md)
   -- [Composer Cheat Sheet](http://composer.json.jolicode.com/)
   - [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)



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
    -- Note that you can skip the configuration wizard by
            using ```composer init --no-interaction``` instead

    - `Package name (<vendor>/<name>) [root/MY_NEW_PROJECT]:`
        - stuff
    - `Description []:`
        - stuff
    - `Author [FIRST_NAME LAST_NAME <EMAIL@DOMAIN.com>, n to skip]:`
        - stuff
    - `Minimum Stability []:`
        - stuff
    - `Package Type (e.g. library, project, metapackage, composer-plugin) []:`
        - stuff
    - `License []:`
        - stuff

    The answers used for the this project follow:

    ```
    Package name (<vendor>/<name>) [root/MY_NEW_PROJECT]: adug/from-scratch
    Description []: A Drupal 8 composer project 'from scratch' (without using a third-party composer project template)
    Author [FIRST_NAME LAST_NAME <EMAIL@DOMAIN.com>, n to skip]:
    Minimum Stability []: dev
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

        Basically this tells Composer what directory your required
        dependencies should be installed into.

        https://github.com/composer/installers

        This list is of the pre-defined types of *Drupal* packages that ```composer/installers``` allows you to
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

        [Clarify why `drupal/core` is the preferred approach](https://github.com/drupal-composer/drupal-project/issues/172)

        Using this package instead of `drupal/drupal` essentially allows you to separate updates of core from updates of
        scaffolding files.

    3. `$ composer require drupal-composer/drupal-scaffold`

        >"Composer plugin for automatically downloading Drupal scaffold files (like index.php, update.php, …) when using
        drupal/core via Composer."

        [drupal-scaffold project README.md](https://github.com/drupal-composer/drupal-scaffold/blob/master/README.md)

        [drupal-project project README.md](https://github.com/drupal-composer/drupal-project#updating-drupal-core)

        We use this plugin because we are using `drupal/core`. This plugin allows you to update the Drupal core files *only* (without updating various scaffolding files that you may have customized).  You'll have to read the documentation at the above link to get more info.  We won't be manipulating the default actions at this time, but you might want investigate that depending on how your workflow is implemented.  Without writing our own scripts, this plugin is necessary when using `drupal/core` to move our scaffolding files to the appropriate places and directories (e.g. `index.php` to the web root).  Though we have not written our scripts, we will have to tell composer when to implement the `DrupalComposer\\DrupalScaffold\\Plugin::scaffold`. We'll do that by adding special language to the `composer.json` file below (keep reading).

    4. `$ composer require drush/drush`

        Why do we need a "per-site" Drush installation?  In short, if we want to develop Drupal 7 and Drupal 8 projects on the same development server, it's fairly necessary.  Additionally, it may be necessary for more "advanced" development needs that utilize various Drush dependencies.

        [Avoiding “Dependency Hell” with Site-Local Drush](https://pantheon.io/blog/avoiding-dependency-hell-site-local-drush)

        [Install Drush 7 and 8 Side-by-Side and Automatically Switch Versions Based on Each Project | Modules Unraveled](https://modulesunraveled.com/comment/404#comment-404)

        ### How to call a site-specific install of Drush when installing Drupal...

        [Goodbye Drush Make, Hello Composer!](https://www.lullabot.com/articles/goodbye-drush-make-hello-composer)

        -- Note that Drush is called from the `bin` directory, basically

        > The right version of Drush for Drupal 8 comes built into this package. If you have an empty database you can then install Drupal using the Drush version in the package:
        >   ```
        >   cd drupal/web
        >   ../vendor/bin/drush site-install --db-url=mysql://{username}:{password}@localhost/{database}
        >   ```
        > If you don’t do the installation with Drush you can do it manually, but the Drush installation handles all this for you. The manual process for installing Drupal 8 is:
        >
        > Copy default.settings.php to settings.php and unprotect it
        >
        > Copy default.license.yml to license.yml and unprotect it
        >
        > Create sites/files and unprotect it
        >
        > Navigate to EXAMPLE.COM/install to provide the database credentials and follow the instructions.

### If you examine your composer.json file in a text editor at this point, it should look something like the following:

```json
{
    "name": "adug/from-scratch",
    "description": "A Drupal 8 composer project 'from scratch' (without using a third-party composer project template)",
    "type": "project",
    "authors": [
        {
            "name": "Jeff Cardwell",
            "email": "jeffcardwellbusiness@gmail.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {
        "composer/installers": "^1.1",
        "drupal-composer/drupal-scaffold": "^2.0",
        "drupal/core": "^8.1",
        "drush/drush": "^8.1"
    },
    "repositories": {
        "drupal": {
            "type": "composer",
            "url": "https://packages.drupal.org/8"
        }
    }
}
```

### There are a few more items that we need to add, but we must add them manually using a text editor

5.  [Prefer Stable](https://getcomposer.org/doc/04-schema.md#prefer-stable) Setting

    `"prefer-stable" : true`

    > When this is enabled, Composer will prefer more stable packages over unstable ones when finding compatible
    stable packages is possible. If you require a dev version or only alphas are available for a package, those
    will still be selected granted that the minimum-stability allows for it.

    [Composer: required packages with differing levels of minimum-stability](http://stackoverflow.com/questions/23086204/composer-required-packages-with-differing-levels-of-minimum-stability)

    This means that Composer will always try to install a "stable" release first. Available options (in order
    of stability) are `dev`, `alpha`, `beta`, `RC`, and `stable`
    ([Minimum Stability](https://getcomposer.org/doc/04-schema.md#minimum-stability)). If there is no available
    dependency from `beta` onward (which are what Composer considers "stable"), it will use an `alpha` or a `dev`
    version.

6.  Prevent conflict between `drupal/drupal` dependencies and `drupal/core` dependencies
    ```
        "conflict": {
             "drupal/drupal": "*"
        }
    ```
    This may not be necessary at all anymore...

    [Use packages.drupal.org repository](https://github.com/drupal-composer/drupal-project/pull/159#issuecomment-232159891)

    But it definitely does not have to be `replace` instead...

    [support for cmd-line installer support of Drupal.org beta repos, instead of packagist](https://github.com/drupal-composer/drupal-project/issues/175#issuecomment-232169598)

    In cany case, it might help and it won't hurt.

7. Add calls to the `scaffold` plugin after `composer update` is invoked and after `composer install` is invoked.  This was inspired by [acquia/lightning-project](https://github.com/acquia/lightning-project/blob/8.x/composer.json) and [drupal-composer/drupal-project](https://github.com/drupal-composer/drupal-project/blob/8.x/composer.json) `.json` files.
    ```
        "scripts": {
            "post-install-cmd": [
                "DrupalComposer\\DrupalScaffold\\Plugin::scaffold"
            ],
            "post-update-cmd": [
                "DrupalComposer\\DrupalScaffold\\Plugin::scaffold"
            ]
        }
    ```

    -- Note the location of the commas when all of these code snippets are copied into your file

### Now, your composer.json file should look something like this:

```json
{
    "name": "adug/from-scratch",
    "description": "A Drupal 8 composer project 'from scratch' (without using a third-party composer project template)",
    "type": "project",
    "authors": [
        {
            "name": "Jeff Cardwell",
            "email": "jeffcardwell@gmail.com"
        }
    ],
    "minimum-stability": "dev",
    "prefer-stable" : true,
    "require": {
        "composer/installers": "^1.1",
        "drupal-composer/drupal-scaffold": "^2.0",
        "drupal/core": "^8.1",
        "drush/drush": "^8.1"
    },
    "conflict": {
      "drupal/drupal": "*"
    },
    "repositories": {
        "drupal": {
            "type": "composer",
            "url": "https://packages.drupal.org/8"
        }
    },
    "scripts": {
      "post-install-cmd": [
        "DrupalComposer\\DrupalScaffold\\Plugin::scaffold"
      ],
      "post-update-cmd": [
        "DrupalComposer\\DrupalScaffold\\Plugin::scaffold"
      ]
    }
}
```

10. Create a .gitignore file
    1. exclude vendor directory
    2. exclude core directory

11. The following files are the *only* ones that you *need* in your version control to start with.  Different scenarios will require you to add additional files.  Do *not* include your `vendor` or `core` directories.  That is why they were put in our `.gitignore` file.

    ```
    composer.json
    composer.lock
    .gitignore
    ```


## How to use this repository

1. Clone the repository and change directory

    ```
    $ git clone https://github.com/Jeff-Cardwell/adug_from_scratch_profile.git MY_NEW_PROJECT
    $ cd MY_NEW_PROJECT
    ```

2. Install the project (will install Drupal as a dependency)

    ```
    ../MY_NEW_PROJECT$ composer install
    ```

3. Install Drupal
    [Instructions from earlier](#how-to-call-a-site-specific-install-of-Drush-when-installing-Drupal)
