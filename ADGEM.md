# [AdGem/sail](https://github.com/AdGem/sail) fork of [laravel/sail](https://github.com/laravel/sail)

## Purpose 

The purpose of this repository is to customize the Laravel Sail project to fit our own needs.

## Customizations

We've made the following customizations to the official Sail project:

- Installed and configured xdebug.
- Added a `sail debug` command to enable xdebug when running artisan commands and phpunit tests.
- Added a `init` command for initializing the project on a new machine.
- Added a `build` command for running composer install, artisan migrate, npm install, and npm run dev in one command. 

## Why Fork?

The maintainers of the official Laravel Sail appear to be pretty set against including xdebug into their project and have turned away multiple PRs in the past. How they've come to this policy and why they are against including a debugging tool into a local development environment setup is unclear and certainly confusing.

## Using Xdebug

### Configuration

There are 2 new environment variables added to sail for configuring xdebug. These variables map directly to their respective XDEBUG_* environment vars inside the running container:

    SAIL_XDEBUG_MODE -> XDEBUG_MODE
    SAIL_XDEBUG_CONFIG -> XDEBUG_CONFIG

By default, xdebug is turned off to reduce overhead. To enable xdebug you simply need to set the appropriate mode when starting your containers:

    SAIL_XDEBUG_MODE=develop,debug
    
You can find the available modes in the official xdebug documentation: [XDEBUG_MODE Docs](https://xdebug.org/docs/step_debug#mode)

If you wish to change modes, you will need to restart your container.

The other environment variable is used to override a number of various xdebug settings. You will need to consult the official xdebug documentation for its various uses: [XDEBUG_CONFIG Docs](https://xdebug.org/docs/all_settings#mode)

#### Mac / Windows

For Windows and Mac host machines, all you should need to do to get started is to turn on xdebug:

    SAIL_XDEBUG_MODE=develop,debug

#### Linux

For Linux host machines, you will need to set the appropriate client_host because the value is not available by default within the container:

    SAIL_XDEBUG_MODE=develop,debug
    SAIL_XDEBUG_CONFIG="client_host=192.168.1.1

You can retrieve the appropriate ip address using:

    docker inspect -f {{range.NetworkSettings.Networks}}{{.Gateway}}{{end}} container-name

### Xdebug with Artisan

If you want to debug an artisan command using xdebug, simply replace "artisan" with "debug":

    # Without xdebug:
    sail artisan foo:bar

    # With xdebug:
    sail debug foo:bar

### Xdebug with PHPUnit

If you want to debug a test using xdebug, simply add "debug" before "test":

Single test:

    # Without xdebug:
    sail test test/Unit/HelloWorldTest.php

    # With xdebug:
    sail debug test test/Unit/HelloWorldTest.php

All tests:

    # Without xdebug:
    sail test

    # With xdebug:
    sail debug test

### Xdebug with web browser

If you want to debug a web session, follow the instructions provided by xdebug for initiating an xdebug session from the web browser.

https://xdebug.org/docs/step_debug#browser-extensions
