# ADUG Basic Drupal Composer Install

1. [The how and why this repository looks like it does](#how-to-re-create-these-files-without-cloning-the-repository)
2. [How to quickly utilize this repository to get your Drupal 8 site up and running.](#how-to-use-this-repository)



## How to re-create these files without cloning the repository

###Required Software
   - Php
   - [Composer](https://getcomposer.org/doc/00-intro.md)
   -- [Composer Cheat Sheet](http://composer.json.jolicode.com/)
   - [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)



1. Create a directory where your new Composer package will live, and change to that directory

    ```shell
    $ mkdir MY_NEW_PACKAGE
    $ cd MY_NEW_PACKAGE
    ```

2. Initialize a new Composer package

    ```shell
    $ composer init
    ```

    - [Composer Docs - init](https://getcomposer.org/doc/03-cli.md#init)

    ###The following configuration wizard will be initiated
    -- Note that you can skip the configuration wizard by using ```composer init --no-interaction``` instead

    ----------------------------------
    ```shell
    Package name (<vendor>/<name>) [root/MY_NEW_PACKAGE]:
    ```
    > The name of the package. It consists of vendor name and project name, separated by /.

    [Composer Docs - name](https://getcomposer.org/doc/04-schema.md#name)

    ----------------------------------
    ```shell
    Description []:
    ```
    > A short description of the package. Usually this is just one line long.

    [Composer Docs - Description](https://getcomposer.org/doc/04-schema.md#description)

    ----------------------------------
    ```shell
    Author [FIRST_NAME LAST_NAME <EMAIL@DOMAIN.com>, n to skip]:
    ```
    > The authors of the package. This is an array of objects.

    [Composer Docs - Authors](https://getcomposer.org/doc/04-schema.md#authors)

    ----------------------------------
    ```shell
    Minimum Stability []:
    ```
    > This defines the default behavior for filtering packages by stability.

    [Composer Docs - Minimum Stability](https://getcomposer.org/doc/04-schema.md#minimum-stability)

    ----------------------------------
    ```shell
    Package Type (e.g. library, project, metapackage, composer-plugin) []:
    ```
    > The type of the package. It defaults to `library`.

    >It is recommended to omit this field and have it just default to `library`.

    [Composer Docs - Type](https://getcomposer.org/doc/04-schema.md#type)

    ----------------------------------
    ```shell
    License []:
    ```
    > The license of the package. This can be either a string or an array of strings.

    [Composer Docs - license](https://getcomposer.org/doc/04-schema.md#license)

    ----------------------------------
    ```shell
    Define your dependencies.

    Would you like to define your dependencies (require) interactively [yes]?
    ```
    > Lists packages required by this package.

    [Composer Docs - require](https://getcomposer.org/doc/04-schema.md#require)

    ----------------------------------
    ```shell
    Would you like to define your dev dependencies (require-dev) interactively [yes]?
    ```
    > Lists packages required for developing this package, or running tests, etc. The dev requirements of the root package are installed by default. Both `install` or `update` support the `--no-dev` option that prevents dev dependencies from being installed.

    [Composer Docs - require-dev](https://getcomposer.org/doc/04-schema.md#require-dev)

    The answers used for this package follow:

    ```shell
    Package name (<vendor>/<name>) [root/MY_NEW_PACKAGE]: adug/from-scratch
    Description []: A Drupal 8 composer package 'from scratch'
    Author [FIRST_NAME LAST_NAME <EMAIL@DOMAIN.com>, n to skip]:
    Minimum Stability []: dev
    Package Type (e.g. library, project, metapackage, composer-plugin) []:
    License []: GPL-2.0+

    Define your dependencies.

          Would you like to define your dependencies (require) interactively [yes]? no
          Would you like to define your dev dependencies (require-dev) interactively [yes]? no

    {
        "name": "adug/from-scratch",
        "description": "A Drupal 8 composer package 'from scratch'",
        "license": "GPL-2.0+",
        "authors": [
            {
                "name": "Your Name",
                "email": "address@domain.com"
            }
        ],
        "minimum-stability": "dev",
        "require": {}
    }

    Do you confirm generation [yes]? yes
    ```
3. Configure Composer to use the Drupal package repository (this item may be added using the command line)

    ```shell
    $ composer config repositories.drupal composer https://packages.drupal.org/8
    ```
    -- Note that this is needed because Drupal packages are not not indexed by the "official" Composer packagist repository (and most likely they never *will* be - for a variety of reasons).  D.O. has made this the alternative supported way going forward.

    - [Using Composer to install Drupal packages through Drupal.org](https://www.drupal.org/node/2718229)
    - [[meta] Path forward for Composer support](https://www.drupal.org/node/2551607)
    - [support for cmd-line installer support of Drupal.org beta repos, instead of packagist](https://github.com/drupal-composer/drupal-project/issues/175)

4.  [Prefer Stable](https://getcomposer.org/doc/04-schema.md#prefer-stable) Setting (this item may be added using the command line before we declare our dependencies)

    ```shell
    $ composer config prefer-stable true
    ```

    > When this is enabled, Composer will prefer more stable packages over unstable ones when finding compatible stable packages is possible. If you require a dev version or only alphas are available for a package, those will still be selected granted that the minimum-stability allows for it.

    - [Composer: required packages with differing levels of minimum-stability](http://stackoverflow.com/questions/23086204/composer-required-packages-with-differing-levels-of-minimum-stability)

    This means that Composer will always try to install a "stable" release first. Available options (in order of stability) are `dev`, `alpha`, `beta`, `RC`, and `stable` ([Minimum Stability](https://getcomposer.org/doc/04-schema.md#minimum-stability)). If there is no available dependency from `beta` onward (which are what Composer considers "stable"), it will use an `alpha` or a `dev` version because we have set our `minimum-stability` to dev (and `alpha` and `dev` are both "great than or equal" to that setting according to the hierarchy in the list above).

    ###Your composer.json file should look something like this:

    ```json
    {
        "name": "adug/from-scratch",
        "description": "A Drupal 8 composer package 'from scratch'",
        "license": "GPL-2.0+",
        "authors": [
            {
                "name": "Your Name",
                "email": "address@domain.com"
            }
        ],
        "minimum-stability": "dev",
        "prefer-stable": true,
        "require": {},
        "repositories": {
            "drupal": {
                "type": "composer",
                "url": "https://packages.drupal.org/8"
            }
        }
    }
    ```

5. Require the following packages (these items may be added by using the command line)

    ```shell
    $ composer require composer/installers
    $ composer require drupal-composer/drupal-scaffold
    $ composer require drupal/core
    $ composer require --dev drush/drush
    ```
    -- Note what each of these is doing...

    ----------------------------------
    ```shell
    $ composer require composer/installers
    ```

    >This is for PHP package authors to require in their composer.json. It will install their package to the correct location based on the specified package type.

    Basically this tells Composer what directory your required dependencies should be installed into.

    - [composer/installers Github project page](https://github.com/composer/installers)

    This list is of the pre-defined types of *Drupal* packages that `composer/installers` allows you to use (it can be found in the link above).  You'll be using these whenever you are creating composer.json files (for modules, themes, and whatnot) as the package `"type":`.

    - `drupal-core`
    - `drupal-module`
    - `drupal-theme`
    - `drupal-library`
    - `drupal-profile`
    - `drupal-drush`

    ----------------------------------
    ```shell
    $ composer require drupal-composer/drupal-scaffold
    ```

    >"Composer plugin for automatically downloading Drupal scaffold files (like index.php, update.php, …) when using
    drupal/core via Composer."

    - [drupal-scaffold project README.md](https://github.com/drupal-composer/drupal-scaffold/blob/master/README.md)
    - [drupal-project project README.md](https://github.com/drupal-composer/drupal-project#updating-drupal-core)

    We use this plugin because we are using `drupal/core`. This plugin allows you to update the Drupal core files *only* (without updating various scaffolding files that you may have customized).  You'll have to read the documentation at the above link to get more info.  We won't be manipulating the default actions at this time (the scaffold script, by default, runs every time you `composer update drupal/core` or `composer install` by downloading and placing new copies of the scaffolding files), but you will want investigate that depending on how your workflow is implemented.  Without writing our own scripts, this plugin is necessary when using `drupal/core` to move our scaffolding files to the appropriate places and directories (e.g. `index.php` to the web root).  Occasionally we may want to retrieve the scaffolding files anew without automatically triggering the script. We'll do that by adding special language to the `composer.json` file below (keep reading).

    ----------------------------------
    ```shell
    $ composer require drupal/core
    ```

    > This is a Git subtree split of Drupal 8's core directory which can be used to build the directory structure for a Drupal site and has the following advantages over pulling in the entire upstream Drupal repository:
    > - All the components of the Drupal site including Drupal, contributed modules and themes, as well as external libraries can be pulled in via Composer
    > - Drupal and any external libraries can be bootstrapped via Composer (i.e. without installing any modules for the external libraries)
    > - One has full control over index.php, .htaccess, robots.txt, etc. as those files will not be overridden by a Drupal core update"

    - [drupal-core project README.md](https://github.com/drupal-composer/drupal-core/blob/master/README.md)
    - [Clarify why `drupal/core` is the preferred approach](https://github.com/drupal-composer/drupal-project/issues/172)

    Using this package instead of `drupal/drupal` essentially allows you to separate updates of core from updates of scaffolding files.

    ----------------------------------
    ```shell
    $ composer require --dev drush/drush
    ```

    You may notice the new option `--dev`.  Packages added using this option are added to the `require-dev` section of the `composer.json` file.  By default, when using `composer install` or `composer update` all required packages *including* those in `require-dev` are installed.  However, you are able to exclude packages in the `require-dev` section when using `composer` commands by adding the option `--no-dev` to the command.

    - [Composer Docs - Require](https://getcomposer.org/doc/03-cli.md#require)
    - [Composer Docs - Update](https://getcomposer.org/doc/03-cli.md#update)

    Why do we need a "per-site" Drush installation?  In short, if we want to develop Drupal 7 and Drupal 8 projects on the same development server, it's fairly necessary.  Additionally, it may be necessary for more "advanced" development needs that utilize various Drush dependencies.

    - [Avoiding “Dependency Hell” with Site-Local Drush](https://pantheon.io/blog/avoiding-dependency-hell-site-local-drush)
    - [Install Drush 7 and 8 Side-by-Side and Automatically Switch Versions Based on Each Project | Modules Unraveled](https://modulesunraveled.com/comment/404#comment-404)

    ### How to call a site-specific install of Drush when installing Drupal

    - [Goodbye Drush Make, Hello Composer!](https://www.lullabot.com/articles/goodbye-drush-make-hello-composer)

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

    ### Now, your composer.json file should look something like this:

    ```json
    {
        "name": "adug/from-scratch",
        "description": "A Drupal 8 composer package 'from scratch'",
        "license": "GPL-2.0+",
        "authors": [
            {
                "name": "Your Name",
                "email": "address@domain.com"
            }
        ],
        "minimum-stability": "dev",
        "prefer-stable": true,
        "require": {
            "composer/installers": "^1.1",
            "drupal-composer/drupal-scaffold": "^2.0",
            "drupal/core": "^8.1"
        },
        "repositories": {
            "drupal": {
                "type": "composer",
                "url": "https://packages.drupal.org/8"
            }
        },
        "require-dev": {
            "drush/drush": "^8.1"
        }
    }
    ```

6.  Prevent conflict between `drupal/drupal` dependencies and `drupal/core` dependencies (this item should be added using a text editor)
    ```json
        "conflict": {
            "drupal/drupal": "*"
        }
    ```
    This may not be necessary at all anymore...

    [Use packages.drupal.org repository](https://github.com/drupal-composer/drupal-project/pull/159#issuecomment-232159891)

    But it definitely does not have to be `replace` instead...

    [support for cmd-line installer support of Drupal.org beta repos, instead of packagist](https://github.com/drupal-composer/drupal-project/issues/175#issuecomment-232169598)

    In cany case, it might help and it won't hurt.

7. Add call to the `scaffold` plugin. (this item should be added using a text editor)

    This script can be invoked by using `composer drupal-scaffold` in the root directory (where this composer.json file is found).
    ```json
        "scripts": {
            "drupal-scaffold": "DrupalComposer\\DrupalScaffold\\Plugin::scaffold"
        }
    ```

    -- Note the location of the commas when code snippets are copied into your `composer.json` file

    -- Also, add the -vvv flag to `composer drupal-scaffold` to view a very verbose output, including which scaffolding files are being downloaded

    ### Now, your composer.json file should look something like this:

    ```json
    {
        "name": "adug/from-scratch",
        "description": "A Drupal 8 composer package 'from scratch'",
        "license": "GPL-2.0+",
        "authors": [
            {
                "name": "Your Name",
                "email": "address@domain.com"
            }
        ],
        "minimum-stability": "dev",
        "prefer-stable": true,
        "require": {
            "composer/installers": "^1.1",
            "drupal-composer/drupal-scaffold": "^2.0",
            "drupal/core": "^8.1"
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
        "require-dev": {
            "drush/drush": "^8.1"
        },
        "scripts": {
            "drupal-scaffold": "DrupalComposer\\DrupalScaffold\\Plugin::scaffold"
        }
    }
    ```

8. Validate your new composer file

    ```
    $ composer validate
    ```

    >You should always run the `validate` command before you commit your `composer.json file`, and before you tag a release. It will check if your `composer.json` is valid.

    [Compser docs - validate](https://getcomposer.org/doc/03-cli.md#validate)

9. Run `composer update` to update your `composer.lock` file

10. Initialize a new project with git
    ```shell
    $ git init
    ```

11. Create a .gitignore file
    1. exclude `vendor` directory
    2. exclude `core` directory
    3. exclude `themes` directory
    4. exclude `modules` directory
    5. exclude `autoload.php` that is automatically generated by `drupal-scaffold`

    ### Your .gitignore file might look something like this:
    ```git
    # Ignore vendor files directory generated by Composer
    vendor
    # Ignore files created automatically by Composer using drupal-scaffold
    autoload.php
    # Ignore Drupal directories
    core
    sites
    themes
    modules
    # Ignore files generated by PhpStorm
    .idea
    ```

12. The following files are the only ones that you *need* in version control to start with.
    ```shell
    composer.json
    composer.lock
    .gitignore
    ```

    However...
    Different scenarios will require you to track additional files (e.g. .htaccess).  Do not include your `vendor` or `core` directories.  That is why they were put in our `.gitignore` file.  After you add further themes and/or modules using `composer require`, you will probably want to add the `themes` and `modules` directories to your `.gitignore` file as well.

    Please read the [drupal-scaffold README.md](https://github.com/drupal-composer/drupal-scaffold) for information about including scaffolding files in version control.

    -- Note the following advisory from the [drupal-scaffold README.md - limitation](https://github.com/drupal-composer/drupal-scaffold#limitation)
    >Limitation
    >
    >When using Composer to install or update the Drupal development branch, the scaffold files are always taken from the HEAD of the branch (or, more specifically, from the most recent development .tar.gz archive). This might not be what you want when using an old development version (e.g. when the version is fixed via composer.lock). To avoid problems, always commit your scaffold files to the repository any time that composer.lock is committed. Note that the correct scaffold files are retrieved when using a tagged release of drupal/core (recommended).

    Taken from the above README.md page is the following list of scaffold files that `drupal-scaffold` controls.
    ```git
    .csslintrc
    .editorconfig
    .eslintignore
    .eslintrc
    .gitattributes
    .htaccess
    index.php
    robots.txt
    sites/default/default.settings.php
    sites/default/default.services.yml
    sites/development.services.yml
    sites/example.settings.local.php
    sites/example.sites.php
    update.php
    web.config
    ```
    When it comes to which files to "keep" in version control, the following files are a good place to start.
    ```git
    .csslintrc
    .editorconfig
    .eslintignore
    .eslintrc
    .gitattributes
    .gitignore
    .htaccess
    composer.json
    composer.lock
    index.php
    robots.txt
    update.php
    web.config
    ```

    If you need to make changes to settings.php or the like that need to be kept in version control, then consider using a `local.settings.php` strategy during development.

    [Using a local.settings.php file](https://docs.acquia.com/articles/using-localsettingsphp-file)

    [Include a Local Drupal Settings file for Environment Configuration and Overrides](https://www.oliverdavies.uk/blog/include-local-drupal-settings-file-environment-configuration-and-overrides/)

    [settings.local.php for Drupal 8](http://wizzlern.nl/drupal/settingslocalphp-for-drupal-8)

13. Commit your relevant files

## How to use this repository

1. Clone the repository and change directory

    ```shell
    $ git clone git://github.com/Jeff-Cardwell/adug_from_scratch_build.git MY_NEW_PROJECT
    $ cd MY_NEW_PROJECT
    ```

2. Install the project (will install Drupal as a dependency)

    ```shell
    MY_NEW_PROJECT$ composer install
    ```

    [Composer Docs - install](https://getcomposer.org/doc/03-cli.md#install)

3. Install Drupal

    [Instructions from earlier in this document](#how-to-call-a-site-specific-install-of-drush-when-installing-drupal)
