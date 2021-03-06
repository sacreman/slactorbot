# slactorbot

A slack bot that uses lightweights actors and dynamic module imports for plugins

Start the bot. Add, remove or edit plugins and magic happens.

# Why?

All of the Python slack bots I tried were annoying.

* Every plugin got sent every message and then filtered by regex. I just wanted slack commands to run the correct plugin.
* Working on plugins locally and then constantly restarting the bot to test them became tedious
* The basic list of commands shown by help shouldn't require manually updating any files
* Exceptions in plugins would crash everything. Why not let it live and crash the plugin.

# Usage

Copy the config.yaml.example to config.yaml and update the settings. Do a `pip install -r requirements.txt`.

Then `./run_bot.py config.yaml`

The bot responds to:

`bot_name` `plugin name` `commands`

For instance if I have my bot_name configured as `dlbot` and I've got a plugin called `example.py` I'd type:

`dlbot example text`

Into the slack channel configured in the config. This would run whatever logic is in the example.py plugin
within the elif for the command `text`.

# Installation with pip

`pip install --upgrade slactorbot`

Then run it and specify a valid config file.

`slactorbot /etc/slactorbot/config.yaml`

If you want to leave it running on a server I'd start it under supervisor or an init script.

You'll need to drop new plugins into the plugins dir printed out when slactorbot starts. This isn't configurable yet.

# Plugins

Create or modify any file with a .py extension in the slactorbot/plugins directory. slactorbot will reload
them without needing to restart.

The plugins are little actors that get dynamically imported. They each need a class called `Main` and
a `receiveMessage` function.

They receive a message which is a list of words typed after the bot_name and the plugin name in the slack room.

If in doubt have a look at the example.py plugin.

# Todo

* More slack options
* Handle restarting crashes in actors
* Make it a pip package
* Needs tests