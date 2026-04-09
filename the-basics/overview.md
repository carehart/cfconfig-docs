# Overview

```text
   ____ _____ ____             __ _       
  / ___|  ___/ ___|___  _ __  / _(_) __ _ 
 | |   | |_ | |   / _ \| '_ \| |_| |/ _` |
 | |___|  _|| |__| (_) | | | |  _| | (_| |
  \____|_|   \____\___/|_| |_|_| |_|\__, |
                                    |___/
```

## Overview

CFConfig gives you the ability to manage almost every setting that shows up in the web administrator, but instead of logging into a web interface, you can manage the settings from the command line by hand or as part of a scripted server setup. You can seamlessly transfer config for all the following:

* CF Mappings
* Datasources
* Mail servers
* Request, session, or application timeouts
* Licensing information (for Adobe)
* Passwords
* Template caching settings
* And much more: basically any settings in the web-based administrator

### Use on any server

CFConfig will work on any CF, Lucee, or BoxLang server regardless of how it was installed. Since it interacts directly with the config files, the server doesn't need to be running. Heck, the server doesn't even need to be installed yet! CFConfig can be used to update config files before you even start a server for the first time.

And CFConfig is not just for use with CommandBox servers. All you need is the folder path to the [server home](/using-the-cli/usage#specifying-a-server-home) for your instance in order for CFConfig to manage it. 

This flexibility means CFConfig can be used for many purposes: configuring new or existing servers (including standing up docker containers or provisioning VMs), exporting or importing settings for a server, comparing or syncing config across existing servers, detecting changes to the config of a server, and much more.

### How does it work

CFConfig interfaces directly with the XML or other property files used by your CF, Lucee, or Boxlang engine to store its configuration. It takes care of translating the config properly sothe same commands can be used regardless engine. You can tell the tool the engine and version, or it can try to figure it out. When making changes to a running instance, note that while the CFML engine may detect and reflect the change without restart, for others you may need to restart the server instance for it to pick up changes.

CFConfig exists in two parts:

* A service layer for reading, writing, and storing configuration for all CF engines.
* A set of scriptable commands built on top of CommandBox CLI.

While most of your use of (and the documentation for) CFConfig may center on the CLI, the underlying CFConfig Service layer operates mostly under the covers providing the actual operation of CFConfig. Let's look at each more closely.

### CFConfig Service Layer

The heart of CFConfig is a standalone module that provides a set of models and services for interacting with configuration files for all CF engines. This library allows for reading, writing, storing, and diffing configuration. This is an underlying service layer meant to have other tools built on top of it.

The CFConfig Services do not require CommandBox but can be used on their own and provide a nice, fluent API for managing configs. They do not use RDS (a configuration protocol available for Adobe servers) and again CFConfig doesn't need the server to be running. The services just need access to the server home folder and its config files.

#### Features at a Glance

* Generic JSON storage of any CF engine's settings (some or all)
* Engine-specific mappings for all major engines to convert their config to and from the generic JSON format
* Export config from a server as a backup
* Import config to a server to speed/automate setup
* Copy config from one server to another. Servers could be different engines–i.e., copy config from Adobe CF2025 to Lucee 7
* Compare config between servers, or between servers and exprted settings
* Merge config from multiple servers together. Ex: combine several Lucee web contexts into a single config (mappings, datasources, etc.)

### CFConfig CLI

The CLI portion of CFConfig wraps up the services layer into a CommandBox module that provides command line access to all the features above, whether from your native OS shell, bash scripts, automations, or Docker/Vagrant/Heroku provisioners.

The CFConfig CLI also has a deep integration with CommandBox servers, making it very easy to manage their configuration.

#### Features at a Glance

* Provides a native CLI tool for managing server configuration
* Scriptable for automated server setup
* Provides complete command help built-in
* Tight integration with CommandBox servers
