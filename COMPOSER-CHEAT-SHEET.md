# Composer command "cheat sheet"
## All commands are assumed (in this case) to be run from the root directory of your package (where your `composer.json` file resides)

- `$composer init`

    Initilizes a new Composer package

    > When you run the command it will interactively ask you to fill in the fields, while using some smart defaults.

    - [Composer Docs - init](https://getcomposer.org/doc/03-cli.md#init)

    Adding the option `--no-interaction` will just create the file and not ask the questions

    - [Composer Docs - global options](https://getcomposer.org/doc/03-cli.md#global-options)

---------------------------------------------
- `$ composer require`

    > The `require` command adds new packages to the `composer.json` file from the current directory. If no file exists one will be created on the fly.

    - [Composer Docs - require](https://getcomposer.org/doc/03-cli.md#require)

    Consider using the '--prefer-source' when requiring packages that are stored in a personal repository. This will allow you to edit them in place and have your VCS setup at hand.
    > `--prefer-source`: Install packages from source when available.

    - [Composer Docs - require](https://getcomposer.org/doc/03-cli.md#require)

---------------------------------------------
- `$ composer remove`

    > The `remove` command removes packages from the `composer.json` file from the current directory.

    - [Composer Docs - remove](https://getcomposer.org/doc/03-cli.md#remove)

---------------------------------------------
- `$ composer install`

    > The `install` command reads the `composer.json file` from the current directory, resolves the dependencies, and installs them into `vendor`.

    - [Composer Docs - install](https://getcomposer.org/doc/03-cli.md#install)

    Adding the option `--profile` "displays timing and memory usage information."  Composer can be slow, generally, and this will give you piece of mind that it's actually doing something.  Also, useful to see exactly what's taking so long. :)

    - [Composer Docs - global options](https://getcomposer.org/doc/03-cli.md#global-options)

---------------------------------------------
- `$ composer update`

    > In order to get the latest versions of the dependencies and to `update` the `composer.lock` file, you should use the update command.

    - [Composer Docs - update](https://getcomposer.org/doc/03-cli.md#update)

    Adding the option `--profile` "displays timing and memory usage information."  Composer can be slow, generally, and this will give you piece of mind that it's actually doing something.  Also, useful to see exactly what's taking so long. :)

    - [Composer Docs - global options](https://getcomposer.org/doc/03-cli.md#global-options)

---------------------------------------------
- `$ composer status`

    > If you often need to modify the code of your dependencies and they are installed from source, the `status` command allows you to check if you have local changes in any of them.

    - [Composer Docs - status](https://getcomposer.org/doc/03-cli.md#status)

    Adding the option `-v` will give you more information on what has changed. It's pretty much always useful with this command.

    - [Composer Docs - global options](https://getcomposer.org/doc/03-cli.md#global-options)

    If you've added a personal repository to your package, and you intend to edit it in place (while keeping track of the changes you made in its personal VCS repository), this is the command to help you make sure all is well. For example, if a "newer version" of a package from your personal repository is available, and the composer.json file in your newer version isn't behaving, this will alert you that there's something afoot.

---------------------------------------------
- `$ composer validate`

    > You should always run the `validate` command before you commit your composer.json file, and before you tag a release. It will check if your `composer.json` is valid.

    - [Composer Docs - validate](https://getcomposer.org/doc/03-cli.md#validate)

---------------------------------------------
- `$ composer search`

    > The `search` command allows you to search through the current project's package repositories. Usually this will be just packagist. You simply pass it the terms you want to search for.

    - [Composer Docs - search](https://getcomposer.org/doc/03-cli.md#search)

    I haven't been able to confirm that this works for custom repositories (https://packages.drupal.org/8). I think that it will be useful... if I can figure it out.

---------------------------------------------
- `$ composer show`

    > To list all of the available packages, you can use the `show` command.

    - [Composer Docs - show](https://getcomposer.org/doc/03-cli.md#show)

    The real magic from this command comes from the options that you can pass it. Pay extra attention to `--tree` and `--path`. They can help you see what's going on "behind the scenes."

    > - `--latest` (-l): List all installed packages including their latest version.
    > - `--all` (-a): List all packages available in all your repositories.
    > - `--installed` (-i): List the packages that are installed (this is enabled by default, and deprecated).
    > - `--platform` (-p): List only platform packages (php & extensions).
    > - `--self` (-s): List the root package info.
    > - `--tree` (-t): List your dependencies as a tree. If you pass a package name it will show the dependency tree for that package.
    > - `--name-only` (-N): List package names only.
    > - `--path` (-P): List package paths.
    > - `--outdated` (-o): Implies --latest, but this lists only packages that have a newer version available.
    > - `--direct` (-D): Restricts the list of packages to your direct dependencies.

    - [Composer Docs - show](https://getcomposer.org/doc/03-cli.md#show)

---------------------------------------------
- `$ composer config`

    > The `config` command allows you to edit composer config settings and repositories in either the local `composer.json` file or the global `config.json` file.

    Please see the docs for usage. Useful for adding additional packagist style repositories, and setting `prefer-stable` setting. The settings will be local unless you use the `--global` option. Use the `--global` option with extreme caution.

    - [Composer Docs - config](https://getcomposer.org/doc/03-cli.md#config)

    - [Composer Docs - modifying repositories](https://getcomposer.org/doc/03-cli.md#modifying-repositories)

    Of particular interest is the `--list` option.
    > Show the list of current config variables. With the `--global` option this lists the global configuration only.

---------------------------------------------
- `$composer clear-cache`
    >Deletes all content from Composer's cache directories.

    When updates behave strangely, a primary action is to try clearing the cache.

    - [Composer Docs - clear-cache](https://getcomposer.org/doc/03-cli.md#clear-cache)

## Most of the information here is found in the official [Composer docs](https://getcomposer.org/doc/).  They should obviously be your primary source, but I have found their layout can be inconvenient when starting out.