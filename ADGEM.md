# [AdGem/sail](https://github.com/AdGem/sail) 

A fork of [laravel/sail](https://github.com/laravel/sail)

## Purpose 

The purpose of this repository is to customize the Laravel Sail project to fit our own needs.

## Customizations

We've made the following customizations to the official Sail project:

- Installed and configured xdebug.
- Added a `sail debug` command to enable xdebug when running artisan commands and phpunit tests.

## Why Fork?

The maintainers of the official Laravel Sail appear to be pretty set against including xdebug into their project and have turned away multiple PRs in the past. How they've come to this policy and why they are against including a debugging tool into a local development environment setup is unclear and certainly confusing.

## Using Xdebug

### Configuration

There are 2 new environment variables added to sail for configuring xdebug. These variables map directly to their respective XDEBUG_* environment vars inside the running container:

    SAIL_XDEBUG_MODE -> XDEBUG_MODE
    SAIL_XDEBUG_CONFIG -> XDEBUG_CONFIG

By default, xdebug is turned off to reduce overhead. To enable xdebug you simply need to set the appropriate mode when starting your containers:

    SAIL_XDEBUG_MODE=develop,debug
    
You can find the available modes in the official xdebug documentation:

[XDEBUG_MODE Docs](https://xdebug.org/docs/step_debug#mode)

The other environment variable is used to override a number of various xdebug settings. You will need to consult the official xdebug documentation for its various uses:

[XDEBUG_CONFIG Docs](https://xdebug.org/docs/all_settings#mode)

For config changes to take effect, you need to recreate the containers.

#### Mac / Windows

For Windows and Mac host machines, you can use the built-in hostname "host.docker.internal" for configuring xdebug:

    SAIL_XDEBUG_MODE=develop,debug
    SAIL_XDEBUG_CONFIG="client_host=host.docker.internal"

#### Linux

For Linux host machines, you will need to set the appropriate client_host because the "host.docker.internal" value is not available:

    SAIL_XDEBUG_MODE=develop,debug
    SAIL_XDEBUG_CONFIG="client_host=192.168.1.1"

You can retrieve the appropriate ip address using:

    docker inspect -f {{range.NetworkSettings.Networks}}{{.Gateway}}{{end}} container-name

#### Firewall Issues

If you are running a firewall such as `ufw` then your containers may have issues connecting to your host machine. To get around this, you need to open up access to the xdebug port on your host machine.

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

[Browser Extensions](https://xdebug.org/docs/step_debug#browser-extensions)
