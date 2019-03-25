[<< Back to HOME](README.md)

# Android App Integration Guidelines

## CMS app commands

CMS app needs to implement a [`BroadcastReceiver`](https://developer.android.com/reference/android/content/BroadcastReceiver) with following `actions` and read `extras`.

This `BroadcastReceiver` should listent to these `actions` as mentioned in table below

|  `val action = intent.action`	(commands) |   What does it do	|
| :-- | :-- |
| `com.panasonic.digital.signage.pause` | Pauses the CMS app and prevents it from coming to foreground. TP app should provide `tpappId` as a `Long` `extra` in then intent. Eg: `intent.putExtra(EXTRA_TPAPP_ID, 14L)` |
| `com.panasonic.digital.signage.resume` | Informs a paused CMS app that it can now come back to foreground when it wants to. |
| `com.panasonic.digital.signage.request` | Request the info from the PDS app. TP app should provide `tpappId` as a `Long` `extra` in then intent. TP app should also provide `launch` as a `boolean` extra. Depending upon the value of `launch` = `true` or `false` PDS app will broadcast the `launch` or `info` command. |
| `com.panasonic.digital.signage.info` | TP app should provide `tpappId` as a `Long` `extra` in then intent. TP app should also provide `appVersion` as a `String` extra |


Smaple `BroadcastReceiver` code in CMS (Digital Signage Android App):
```kotlin
import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.util.Log

const val PAUSE = "com.panasonic.digital.signage.pause"
const val RESUME = "com.panasonic.digital.signage.resume"
const val REQUEST = "com.panasonic.digital.signage.request"
const val INFO = "com.panasonic.digital.signage.info"

const val EXTRA_TPAPP_ID = "EXTRA_TPAPP_ID"
const val EXTRA_LAUNCH = "EXTRA_LAUNCH"
const val EXTRA_APP_VERSION = "EXTRA_APP_VERSION"

class CommandsReceiver : BroadcastReceiver() {
    private val TAG = CommandsReceiver::class.java.canonicalName

    override fun onReceive(context: Context?, intent: Intent?) {
        if (intent != null) {
            val action: String? = intent.action
            val tpAppId: Long? = if (intent.hasExtra(EXTRA_TPAPP_ID)) {
                intent.getLongExtra(EXTRA_TPAPP_ID, -1L)
            } else {
                null
            }
            val launch: Boolean? = if (intent.hasExtra(EXTRA_LAUNCH)) {
                intent.getBooleanExtra(EXTRA_LAUNCH, false)
            } else {
                null
            }
            val appVersion: String? = if (intent.hasExtra(EXTRA_APP_VERSION)) {
                intent.getBooleanExtra(EXTRA_APP_VERSION, null)
            } else {
                null
            }
            when (action) {
                PAUSE -> pauseApp(tpAppId)
                RESUME -> resumeApp()
                REQUEST -> handleRequest(tpAppId, launch)
                INFO -> handleInfo(tpAppId, appVersion)
            }
        }
    }

    fun pauseApp(tpAppId: Long?) {
        if (tpAppId == null) {
            Log.e(TAG, "tpAppId should not be null")
            return
        }
        TODO()
    }

    fun resumeApp() {
        TODO()
    }

    fun handleRequest(tpAppId: Long?, launch: Boolean?) {
        if (tpAppId == null) {
            Log.e(TAG, "tpAppId should not be null")
            return
        }
        if (launch == null) {
            Log.e(TAG, "launch should not be null")
            return
        }
        TODO()
    }
    fun handleInfo(tpAppId: Long?, appVersion: String?) {
        if (tpAppId == null) {
            Log.e(TAG, "tpAppId should not be null")
            return
        }
        if (appVersion == null) {
            Log.e(TAG, "appVersion should not be null")
            return
        }
        TODO()
    }
}
```

## TPA app commands

TPA app needs to implement a [`BroadcastReceiver`](https://developer.android.com/reference/android/content/BroadcastReceiver) with following `actions` and read `extras`.

This `BroadcastReceiver` should listen to these `actions` as mentioned in table below

| `val action = intent.action` (commands) |  What TPA app should do	|
| :-- | :-- |
| `com.panasonic.digital.signage.info` | CMS app will inform the tpa app about CMS `deviceId` as a `Long` `extra` in the intent. And  also the `timeoutInSeconds` as a `Long` `extra` in the intent. Please note this is not a launch command but it is just a info command so you need to accept the info but not open. |
| `com.panasonic.digital.signage.launch` | CMS app will inform the tpa app about CMS `deviceId` as a `Long` `extra` in the intent. And  also the `timeoutInSeconds` as a `Long` `extra` in the intent. CMS will call this command when they want to launch the TPA app |
| `com.panasonic.digital.signage.request` | CMS app will send this action in a broadcast to TP app to their `appVersion` as a `String` extra to tpapp |

Smaple `BroadcastReceiver` code in TPA app:
```kotlin
import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.util.Log


const val INFO = "com.panasonic.digital.signage.info"
const val LAUNCH = "com.panasonic.digital.signage.launch"
const val REQUEST = "com.panasonic.digital.signage.request"
const val INFO = "com.panasonic.digital.signage.info"

const val EXTRA_DEVICE_ID = "EXTRA_DEVICE_ID"
const val EXTRA_TIMEOUT_IN_SECONDS = "EXTRA_TIMEOUT_IN_SECONDS"
const val EXTRA_APP_VERSION = "EXTRA_APP_VERSION"

class CommandsReceiver : BroadcastReceiver() {
    private val TAG = CommandsReceiver::class.java.canonicalName

    override fun onReceive(context: Context?, intent: Intent?) {
        if (intent != null) {
            val action: String? = intent.action
            val deviceId: Long? = if (intent.hasExtra(EXTRA_DEVICE_ID)) {
                intent.getLongExtra(EXTRA_DEVICE_ID, -1L)
            } else {
                null
            }
            val timeoutInSeconds: Long? = if (intent.hasExtra(EXTRA_TIMEOUT_IN_SECONDS)) {
                intent.getLongExtra(EXTRA_TIMEOUT_IN_SECONDS, -1L)
            } else {
                null
            }
            when (action) {
                INFO -> saveInfo(deviceId, timeoutInSeconds)
                LAUNCH -> launchMyApp(deviceId, timeoutInSeconds)
                REQUEST -> sendInfoToDSApp()
            }
        }
    }

    fun saveInfo(deviceId: Long?, timeoutInSeconds: Long?) {
        if (deviceId == null) {
            Log.e(TAG, "deviceId should not be null")
            return
        }
        if (timeoutInSeconds == null) {
            Log.e(TAG, "timeoutInSeconds should not be null")
            return
        }
        TODO()
    }

    fun launchMyApp(deviceId: Long?, timeoutInSeconds: Long?) {
        if (deviceId == null) {
            Log.e(TAG, "deviceId should not be null")
            return
        }
        if (timeoutInSeconds == null) {
            Log.e(TAG, "timeoutInSeconds should not be null")
            return
        }
        TODO()
    }

    fun sendInfoToDSApp() {
        val appVersion = BuildConfig.VERSION_NAME
        val intent = Intent()
        intent.setPackage("DS app applicationId") // DS team will provide this to you
        intent.action = INFO
        intent.putExtra(EXTRA_APP_VERSION, appVersion)
        intent.addFlags(Intent.FLAG_INCLUDE_STOPPED_PACKAGES)
        context.sendBroadcast(intent)
    }
}
```

## Guidelines for sending broadcast:

Make sure you set the applicationId of Digital Signage app correctly. CMS team will provide the application ID to you.

Sample code:
```kotlin
val applicationId = "com.exmaple.myapp"
val otherAppBroadcastReceiverClassName = "com.example.myapp.CommmandsReceiver"
val INFO = "com.panasonic.digital.signage.info"

fun sendBroadCast(context: Context) {
    val intent = Intent()
    intent.setPackage(applicationId)
    intent.action = INFO
    intent.putExtra(EXTRA_DEVICE_ID, 14L)
    intent.putExtra(EXTRA_TIMEOUT_IN_SECONDS, 30L)
    intent.addFlags(Intent.FLAG_INCLUDE_STOPPED_PACKAGES)
    context.sendBroadcast(intent)
}
```
