<< Back to [HOME](README.md)

# Windows-App-Integration-Guidelines.md

## CMS app commands

CMS windows app is a UWP (Universal Windows Platform) app. It accepts following commands:

CMS windows app using protocol to listen to the commands. This ensures that the command is delivered by the Windos OS to the CMS app even if the CMS app is not running

Below are command supported by the CMS app

|  Command	|   What does it do	|
| :-- | :-- |
| `pds://pause` | Pauses the CMS app and prevents it from coming to foreground |
| `pds://resume` | Informs a paused CMS app that it can now come back to foreground when it wants to	|

## TPA app commands

### If TPA is a UWP app
If they are a UWP app then they should use the similar protocol to as CMS app to accepts commands

Assuming that TPA app uses tpa as their schema the the commands will be as follows:

|  Command	|   What TPA app should do	|
| :-- | :-- |
| `tpa://info?deviceId=<deviceId>&timeoutInSeconds=<timeout>` | CMS app will inform the tpa app about CMS `deviceId` and the timeout to be used for implemeting the idle logic on the tpa app |
| `tpa://launch` | CMS will call this command when they want to launch the TPA app |

### If TPA is a normal windows app
If the TPA app is a normal windows app then they should use the `Process` to accept commands from the CMS UWP app:

`Process` = <a href="https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.process.start?view=netframework-4.7.2#System_Diagnostics_Process_Start" target="_blank">`system.diagnostics.process.start`</a>

