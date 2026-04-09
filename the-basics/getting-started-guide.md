# Getting Started Guide

To use CFConfig, you can install it into CommandBox easily like so.

```text
CommandBox> install commandbox-cfconfig
```

See [installation](using-the-cli/installation) for more detail on installing CFConfig or CommandBox itself.

Now that you've got the tool installed you can dig into the command help to see where to go from there. Here's a quick overview of some of the commands. Run the built-in command help for more info on each one.

## View help

```text
CommandBox> cfconfig help
CommandBox> cfconfig help export
CommandBox> cfconfig help datasource
```

The following commands can work on any CF engine. Whether (and how) you need to name arguments "depends". For most commands, CFConfig needs to know the location "from which" you want to obtain values (such as for `show`), or "to which" you want to set them (such as `set`). Some commands need both locations (such as `import`, `export`, `transfer`, and `diff`).

And the location can either be:
- a commandbox server name (which need not even be named when run from the root of a Commandbox server)
- the [server home](/using-the-cli/usage#specifying-a-server-home) directory
- a [CFConfig json file](/using-the-cli/json-file-storage)

The first section below will elaborate on using such different locations, but of course all commands can specify such locations when necessary.

The several CFConfig commands can be grouped with regard to the kind of settings they work with and the operations they perform, as follows.

## Commands to manage individual (or all) settings

### View a setting

The simplest demonstration of using CFConfig may be to view the value of a given setting in a given server (or config file). Again there are different ways you can view that setting via CFCONFIG, depending on how the server (or config file) is implemented. 

The first example here can be used from the webroot of a Commandbox server (no location needs to be specified). The other variations show first naming another commandbox server (i.e., MyCmdboxServer), then naming different locations for CF, Lucee, and Boxlang [server home](/using-the-cli/usage#specifying-a-server-home) locations, and finally naming a CFConfig json file.

(The examples show using `sessiontimeout` as that is a setting common to all the supported engines, Adobe CF, Lucee, and Boxlang.)

```text
CommandBox> cfconfig show sessionTimeout

CommandBox> cfconfig show sessionTimeout MyCmdboxServer

CommandBox> cfconfig show sessionTimeout C:\ColdFusion2025\cfusion\
CommandBox> cfconfig show sessionTimeout C:\lucee\tomcat\lucee-server\
CommandBox> cfconfig show sessionTimeout C:\lucee\tomcat\webapps\ROOT\WEB-INF\lucee\
CommandBox> cfconfig show sessionTimeout /opt/lucee/lib/lucee-server/
CommandBox> cfconfig show sessionTimeout C:\BoxLang\config/
CommandBox> cfconfig show sessionTimeout C:\temp\myconfig.json
```

Notice that when a CFConfig command takes such a single `from` or `to` argument, as in each of the above, the actual argument name (`from=` or `to=`) can be left off. If it's specified (for any reason) in a command that takes additional arguments, then all arguments to the command must be named, for example:

```text
CommandBox> cfconfig show property=sessionTimeout from=C:\ColdFusion2025\cfusion\
```

The remaining command examples will forego showing all these location varations. Add them as may be appropriate to your situation. The available help for each command will clarify the available arguments and their values.

### Set a setting

```text
CommandBox> cfconfig set sessionTimeout=0,0,10,0
```

### View all settings for a server

```text
CommandBox> cfconfig show
```

## Commands to manage groups of settings

Because some settings can define one or more of that kind of setting (such as datasources or CF mappings)

### Add, list and delete a datasource

```text
CommandBox> cfconfig datasource save name=myDSN dbdriver=mysql host=localhost port=3306 database=myDB username=brad password=foobar
CommandBox> cfconfig datasource list
CommandBox> cfconfig datasource delete myDSN
```

### Add, list, and delete a CF Mapping

```text
CommandBox> cfconfig cfmapping save virtual=/foo physical=C:/bar
CommandBox> cfconfig cfmapping list
CommandBox> cfconfig cfmapping delete /foo
```

## Commands to export/import/transfrer/compare settings among servers 

### Export all settings from a server

```text
CommandBox> cfconfig export .CFConfig.json
```

### Import settings into a server

```text
CommandBox> cfconfig import .CFConfig.json
```

### Transfer settings from one server to another

```text
CommandBox> cfconfig transfer from=oneServer to=anotherServer
```

### Diff all the settings between two servers

```text
CommandBox> cfconfig diff from=oneServer to=anotherServer
CommandBox> cfconfig diff to=anotherServerName
CommandBox> cfconfig diff to=anotherServerName --fromOnly
CommandBox> cfconfig diff to=anotherServerName --toOnly
CommandBox> cfconfig diff to=anotherServerName --valuesDiffer
```

