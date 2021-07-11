---
id: upgrading_opendialog
title: Upgrading OpenDialog
sidebar_label: Upgrading OpenDialog
---


## Introduction

Upgrading OpenDialog should generally be a similar process to upgrading any Laravel application. We aim to provide detailed upgrade instructions here once OpenDialog is on a more stable release schedule, especially where there may be breaking changes in the graph schema or the interfaces for the core OpenDialog conversational framework. 

In the meantime, we offer some general pointers that we use for guiding the upgrade process.

## Backup all data 

Make sure all data is saved from the SQL and Dgraph databases. Depending on the state of your own application and any upgrades to scehmes there may be information that will not smoothly migrate to further versions. 

If you are not concerned about any application specific data other that the conversation templates and messages templates you can export just those through an Artisan command. 

`php artisan conversations:export`

Webchat settings cannot, currently, be exported. Make sure you manually capture any settings in the [SetWebchatSettings](https://github.com/opendialogai/opendialog/blob/0.7.x/app/Console/Commands/SetWebchatSettings.php) command so that you can import them back into your application as required. 


## Upgrade the OpenDialog application

Get the latest version of the OpenDialog application from the [repository](https://github.com/opendialogai/opendialog) and merge it into your own application. 

If you haven't changed any core OpenDialog elements you are only likely to see merge conflicts in areas such as your `composer.json`. 

Ensure that you are adhering to all the version requirements for the OpenDialog app, resolve conflicts and do a `composer update`. This will pull in the latest versions of OpenDialog Core and OpenDialog webchat.

Then make sure to perform a migration:

`php artisan migrate` 

for any database tables and data migrations.

## Follow any Laravel framework upgrade instructions

Depending on what Laravel framework upgrade has taken place through the OpenDialog update check out the [Laravel upgrade](https://laravel.com/docs/6.x/upgrade) guide to apply any relevant changes.

## Upgrade your bot

Your own bot may be using custom Attributes, Actions, Interpreters and Contexts - make sure you check for any interface updates to how those elements are defined and match those changes in your domain-specific implementations. 


## Test your conversations

Finally, test your conversations to ensure that the bot is behaving as expected! 