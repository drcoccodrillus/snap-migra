# migra

Migra is a schema diff tool for PostgreSQL, written in Python. It magically figures out all the statements required to get from A to B.

Migra supports PostgreSQL >= 9 only. Known issues exist with earlier versions.

***

## Snap building
The snap can be built using snapcraft. The snapcraft.yaml file is located in the snap directory. To build the snap, use the following command:

`snapcraft`

Remember to put yourself in the snap-migra directory before running the command.

## Snap installation
The installation process is pretty straight forward and you can install the snap in two different ways:
- From the snap store
- From a local file

The easiest way is to install the snap from the snap store. Use the following command:

`snap install migra`

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-white.svg)](https://snapcraft.io/migra)


If you prefer to install the snap from a local file, follow the instructions below.

For installing the snap in devmode from a local file, use the following command:

`snap install migra_3.0.1663481299_amd64.snap --dangerous --devmode`

For installing the snap in confined mode from a local file, use the following command:

`snap install migra_3.0.1663481299_amd64.snap --dangerous`

## Usage
The following comes from the original migra documentation.

### Basic usage

migra follows the usage pattern of other diff tools: Two arguments, `a` and `b`, or `from` and `to`. In database migration terms, they could also be thought of as current state and desired state.

For example, given two databases running locally called, `a` and `b`, the following command will generate the schema diff to transform `a`'s structure into `b`'s structure:

`migra postgresql:///a postgresql:///b`

The command will deliberately fail if any DROP... statements are contained in the diff. To stop this, and generate a diff even if it would be destructive, add the --unsafe flag:

`migra --unsafe postgresql:///a postgresql:///b`

### migra command line options
The command line version of migra includes a number of options for getting the diff output you desire. If using the API, similarly named options are available on the Migration object.

- `--unsafe`: Migra will throw an exception if drop... statements are generated, as a precaution. Adding this flag will disable the safety feature and happily generate the drop statements. Remember, always review generated statements before running them!

- `--schema [SCHEMA_NAME]`: Specify a single schema to diff.

- `--create-extensions-only` Only output create extension statements, nothing else. This is useful when you have extensions as part of a setup script for a single schema: Those extensions need to be installed, but extensions are usually not installed in a custom schema.  
You'd generate a setup script in two steps:

    - Generate the necessary create extension if not exists... statements using `--create-extensions-only`.
    - Generate the schema changes with --schema-only.

    Then combine the output of 1 and 2 into a single database sync script.

- `--with-privileges`: This tells migra to spit out permission-related change statements (grant/revoke). This is False by default: Often one is comparing databases from different environments, where the users and permissions are completely different and not something one would want to sync.

- `--force-utf8`: Some folks have reported unicode character output issues on windows command lines. This flag often fixes it!
