# JSON File Storage

CFConfig can represent the settings for any server in a generic JSON format.  Such CFConfig JSON files can be used to hold the CFConfig `export` of settings from a server, or used to `import` settings into a server. They can also be used for comparison (`diff`) of a server to such a json file. Commandbox can also configure a server using such CFConfig JSON files, and Lucee 6 and above now store their settings using such CFConfig json files.

## Creating CFConfig JSON files

There are multiple ways CFConfig JSON files can be created, such as: 
- The easiest may be the CFConfig [`export`](/using-the-cli/command-overview/export-settings) command, which can be used to create or append to an existing json file
- Starting with Lucee 6, it stores its admin configuration settings by default in a CFCondfig json-formatted file, specifically a `.CFConfig.json` file stored at `<installation>/lucee-server/context/` (for Server settings) or `<web-context>/WEB-INF/lucee/.CFConfig.json` (for Web context settings, though only in Lucee 6 as Lucee 7 has dropped such "multi-mode" support)
- Of course, you can create them by hand or some other automation (as long as you maintain the format reflected in the above files)

Note that although some settings may be specific to Lucee, ColdFusion, or BoxLang, note that the JSON format itself is generic and can be used on any engine. Any settings that don't apply to a given engine will simply be ignored. For example, caches or mail servers past the first one are ignored when importing to Adobe ColdFusion.

## Automatically Deploying Saved Configurations

CommandBox supports use of CFConfig json files to configure settings automatically: if it finds a a file called `.cfconfig.json` in the webroot, that will cause them to automatically get loaded in on every server start. This is the easiest way to share configuration with your coworkers. 

Beware, though: one issue with this implicit behavior (storing the JSON file in your web root) is that if the file is moved to a production server and now web accessible. While web servers such as Apache will not serve files starting with `.` by default, other web servers (including IIS) will happily share such "intended-to-be-hidden" files. Beyond solving this via posible web server configuration features, you can also work around this by placing the JSON file *outside* the web root and using Commandbox's `cfconfigFile` property in your `server.json` to point to the file. You can use relative paths like `../build/mySettings.json`.

## Separate Lucee/Railo Server/Web Context Config

If you are using Lucee 6 or earlier, or Railo, and wish to have separate config for your server and web contexts, you can use the following file names which will be found by convention if there are no other settings (env vars or `server.json` keys specifying JSON file locations).

* `.cfconfig-server.json`
* `.cfconfig-web.json`

If these files exist for an Adobe server, they will still be used.

### Dynamic Values and System Settings

Another potential issue with the JSON file is that you may have secrets in your config such as passwords that you don't want to commit to your repo or that are simply different on some servers. You can manage this by using CommandBox's "system settings" which will expand any environment variables in your CFConfig JSON file. System settings are in the format `${mySetting}` and would look like so in your JSON file:

```json
{
  "adminPassword": "${LOCAL_DEV_CF_PASS}"
}
```

That JSON above would automatically replace the `adminPassword` setting with the value of an environment variable called `LOCAL_DEV_CF_PASS`.

You can take this a step further and provide a default value so the env var is just an override. The format is `${name:default}`.

```json
{
  "requestTimeout": "${LOCAL_DEV_TIMEOUT:0,0,1,0}"
}
```

That JSON above would set the request timeout to 1 minute unless it was overridden by an env var called `LOCAL_DEV_TIMEOUT`.

### Handling Scheduled Tasks

Since scheduled tasks are also managed by CFConfig, any running tasks on the source server will be running on the destination server which might not be desirable. Use these techniques to pause tasks:
- in conjunction with the `cfconfigFile` in `server.json`, set the `CFConfigPauseTasks` setting to true with `server set CFConfigPauseTasks=true`
- when using CFConfig to import, export, or transfer settings, use the `--pauseTasks` flag, i.e., `cfconfig import myConfig.json --pauseTasks`
