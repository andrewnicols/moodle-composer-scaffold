# Moodle Composer Scaffolding system

The Moodle Composer Scaffolder is a Composer Plugin intended to simplify and easy the installation of Moodle as a Composer project.

This tooling will:

- Generate a Moodle configuration file
- Support the generation of Configuration for Moodle
- Assist with the installation of Moodle

## Files generated

### The Moodle configuration

This tooling will guide you through the creation of a configuration file for Moodle which sits alongside your `composer.json`.

#### Placeholders

The following placeholders may be provided either in response to interactive prompts, or via the environment variable configuration:

| Placeholder | Meaning | Default |
| ----------- | ------- | ------- |
| `[NAME]`    | The name of the site | The _base name_ of the folder that the project is in |

#### Configuration

You can provide a `.env` file in any of the following locations:

- `.env` in your project's root directory
- `.env.local` in your project's root directory
- `.env` in the parent directory of you project
- `.env.local` in the parent directory of you project

The following parameters are accepted:

| Environment key           | Default   | Description | Placeholders | Example |
| ---------------           | -------   | ----------- | ------------ | ------- |
| `MOODLE_AGREE_LICENSE`    | `N`       | Whether you agree to the Moodle GPL License agreement | - | `Y` |
| `MOODLE_ADMIN_PASSWORD`   | -         | The admin password to use when installing your site | - | `password` |
| `MOODLE_ADMIN_EMAIL`      | -         | The email address of the admin user | - | `admin@example.com` |
| `MOODLE_DB_USERNAME`      | -         | The username to use for the database server | - | `webdev` |
| `MOODLE_DB_PASSWORD`      | -         | The password to use for the database server | - | `moodle` |
| `MOODLE_DB_HOST`          | -         | The hostname to use for the database server | - | `localhost` |
| `MOODLE_DB_NAME`          | `[NAME]`  | The name of the database to use | `[NAME]` | `[NAME]` |
| `MOODLE_DB_PREFIX`        | `mdl_`    | The database prefix to use | - | `mdl_` |
| `MOODLE_DB_DRIVER`        | -         | The database driver to use | - | `pgsql` |
| `MOODLE_WWWROOT`          | -         | The URL of the Moodl esite | `[NAME]` | `https://[NAME].example` |

### Generated files

#### Moodle Configuration - `/config.php`

The root `/config.php` file stores all of the standard Moodle configuration properties.

This is functionally equivalent to the `config.php` in the standard installation and the values and documentation in `config-dist.php` apply here.

Note: There is no need to manually include the `setup.php` file as in the non-Composer installation method.

This file will only be generated if it does not already exist, but will be attempted on every Composer operation, or whenever the `composer scaffold` or `composer configure` commands are run.

#### Moodle Configuration Shim - `/moodle/config.php`

The scaffolder will create a `config.php` within the Moodle installation directory.

The purpose of this is to act as a shim for parts of Moodle which manually load the configuration file.

It will be generated on every Composer operation, or whenever the `composer scaffold` command is run.

#### Moodle Composer Autoload Shim - `/moodle/vendor/autoload.php`

The scaffolder will create a `/moodle/vendor/autoload.php` file which will load the Composer Autoload file from the project root.

This is required because in instances where Moodle is symlinked to the target directory (for example when using a Local Composer repository) the symlink source cannot be determined automatically from Moodle.

It will be generated on every Composer operation, or whenever the `composer scaffold` command is run.

#### Moodle Binary Helper - `/msh`

For versions of Moodle which support Console commands, a helper is created at `/msh` in the project root to link to the `bin/moodle` within the project root.

It will be generated on every Composer operation, or whenever the `composer scaffold` command is run.

## Commands

The scaffolder includes several commands which can be manually run if required.

### `composer scaffold`

This command will manually run the scaffold system.

Note: This is automatically called as a post-installation step any time that a package is installed or updated.

### `composer configure`

This command will run the configuration wizard to create an initial manual configuration file.

If a configuration file already exists, it has no effect.

Note: This is automatically called as a post-installation step any time that a package is installed or updated.

## References

This plugin was inspired by the Drupal Composer Scaffolder.
