# Database migration for Mendix

This module allows data migration which is not supported by Mendix. The migration should be executed when application starts, but it's possible to do it anytime. Inspired by Liquibase, simplified for Mendix.

## Use case
If you make some changes in your model (creation of new entities, or re-organization of existing entities for example), it's usually necessary to migrate the data : a new table should be filled with data coming from an older table. This module let you create migration microflow which are executed once per database.

## Usage

Add microflow "Act_Migrate" at the beginning of your after-startup microflow. Define:
* the module where your migration microflows are set (or leave it empty if you want the migration module to scan your whole project)
* the prefix of your migration microflows ("Mig_" could be a good choice)
The migration will occur in alphabetical order. The execution of a migration microflow is stored in the database, so they're executed only one per database.
It's generally recommended to keep the migration microflow in your project, so it's possible to setup a new database from scratch, but you can also delete the old migration microflows once all your databases have been migrated.

The table "Script" contains the timestamp of all script executions, and their order.

## Update of migration microflow name or module

In case you want to change the name of a migration microflow or the module where you store them, you'll have to update the "Script" table, and update the parameters sent to "Act_Migrate". It's indeed a good example of a scenario where we would need a migration script :)

## Errors during migration

If a migration microflow fails to execute, the migration process stop there (the app won't start if you added "Act_Migrate" in your after-startup microflow). It will let you fix the issue either in the database or in the migration microflow itself. When you'll restart the app, the process restarts from the last "success" execution.

## Force the re-execution of a migration microflow

If you want to re-execute a migration microflow, you'll have to remove its execution in the database, and restart the migration process.

## Dependencies
* Community-Commons

## Version
Mendix 9

## Feedback, PR & bugs
Please submit your bugs / PR (here)[https://github.com/lordlothar99/mx-migration], and contact me at "francois at kohomai.com"