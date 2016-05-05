![logo](tokbox-logo.png)
# OpenTok Tele Health Solution <br/>Version 0.1

The Tele Health Solution widget makes it easy to embed one-to-one communications in a website:
* Video
* Audio
* Text
* Screen sharing


## Requirements

Review the requirements for [OpenTok](https://tokbox.com/developer/requirements/).

The widget supports the following browsers:
* Chrome 47 or later
* Firefox 43 or later
* Internet Explorer 11 (See [Known Issues](#known-issues))


## Known issues

Number  | Description
:------ | :------
WMS-161 | Internet Explorer 11 clients cannot share their screens.


## About the contents of the widget ZIP file

Since you are reading this, we assume you have succeeded in obtaining and extracting the widget ZIP file. If you do not have the package and would like it, feel free to [reach out to us](https://tokbox.com/contact/sales). This section discusses the contents of the package.

* **sample-app**: Contains a sample implementation of the widget.

* **images**: Contains image files that the widget needs for its user interface.

* **templates**: Contains a base.html file. The widget uses this file for various prompts to the end user.

* **css**: Contains a ths-min.css and a themes.css file. See [Customizing the widget styles](#customizing-the-widget-styles) for more information.

* **LICENSE**: Contains important legal information describing the terms under which you can use and/or redistribute the widget.

* **demo.html**: Use this to [get started very quickly](#quick-start) or just to view the minimum HTML that the widget needs.


* **ths-min.js**: A minified file containing the widget and the libraries that it depends on such as jQuery and Underscore.


## Prerequisites

Before continuing further, make sure you have the following:

* **A secure web server**: The page containing the widget must be served over HTTPS to allow screen sharing and support clients running [Chrome 47 or later](https://groups.google.com/forum/#!topic/discuss-webrtc/sq5CVmY69sc). Browser security limitations prevent you from publishing video using a `file://` path, as discussed in the [OpenTok.js Release Notes](https://www.tokbox.com/developer/sdks/js/release-notes.html#knownIssues). If you do not already have a secure web server, you can use [MAMP](https://www.mamp.info/), [XAMPP](https://www.apachefriends.org/index.html), or a free cloud service like [Heroku](https://www.heroku.com/).

* **API Key**: You can get this by [signing up for an OpenTok developer account](https://dashboard.tokbox.com/users/sign_up). It's free for thirty days, no credit card required.

* **Session ID** & **token**: During the test and development phases, you can generate these manually from the OpenTok [Developer Dashboard](https://dashboard.tokbox.com/). Before you go into production, you must deploy one of the [OpenTok server SDKs](https://tokbox.com/developer/sdks/server/) to generate these values dynamically. If you do not want to use one of the server SDKs that we provide, you can make calls directly against the [OpenTok REST API](https://tokbox.com/developer/rest/).


## Deploying the widget

Take the following from the ZIP package and upload them to your web server:
* ths-min.js file
* ths-min.css file
* templates directory (contains the file base.html)
* images directory


## Quick start

This procedure will get you a working widget very quickly but you do need Firefox. Before you begin, make sure you have completed the steps in [Deploying the widget](#deploying-the-widget).  

1. Open the demo.html file using your choice of editor.

2. Modify the file as indicated in the comments to include the following values:
    * Path to the ths-min.css file
    * Path to the ths-min.js file
    * API key
    * Session ID
    * Token

    **NOTE**: See the [Prerequisites](#prerequisites) section for more information on these values.

3. Leave `extensionID` and `extensionPathFF` blank.

4. Save the file and upload it to your web server.

4. Open a Firefox browser.

5. Type **about:config** in the address box.

6. Click past the warning.

7. Find the **media.getusermedia.screensharing.allowed_domains** key.

8. Add the domain that your widget is hosted on.

    **PRO TIP**: Prefatory wildcards are supported, e.g., *.tokbox.com

9. Now visit your hosted demo.html file to see the widget in action!


## Enabling production screen sharing

To enable actual users to share screens using the widget, follow the steps discussed in the [OpenTok Developer Center](https://tokbox.com/developer/guides/screen-sharing/js/).


## Adding the widget to an existing web page

1. Add a `script` tag to load the ths-min.js file.
    ```html
    <script src="path/ths-min.js"></script>
    ```

2. Add a `link` tag to load the ths-min.css file.
    ```html
    <link rel="stylesheet" href="path/ths-min.css">
    ````

3. Define a container for the widget.
    ```html
    <div class="container">
    <div id="widget_container" style="width:380px; margin:50px auto 0;"></div>
    </div>
    ```

## Initializing the widget

Before you can use the widget, you must initialize it. The widget's `init` function accepts a single argument: an `options` object. The `options` object must contain the following name-value pairs.

Name  | Required? | Value(s)
:-----| :-------- | :-------
`user` | Only the `id` value is required | A nested object containing the following name-value pairs: `name:"string",id:NNNN,role:"client"|"advisor"`. The `name` value is used as the alias for text chat and is optional but highly recommended. The `id` value should be a unique identifier (string value) and is required. The `role` value is optional.
`apiKey` | Yes | Your OpenTok API key (see [Prerequisites](#prerequisites))
`sessionId` | Yes | The session ID (see [Prerequisites](#prerequisites))
`token` | Yes | A token value (see [Prerequisites](#prerequisites))
`onTHSStarted` | No | Define the function to call after a successful initialization of the widget and start of the communications session.
`onTHSEnded` | No | Define the function to call after the communications session ends.
`onTHSError` | No | Define the function to call if there is an error loading the widget or starting the communications session.
`el` | Yes | The widget container element. You can use jQuery for this `$("#widget_container")`, where `widget_container` is the id of the container element.
`extensionID` | No | To allow a Chrome client to share their screen, you must use this parameter to pass the ID of your Chrome extension. (See [Enabling production screen sharing](#enabling-production-screen-sharing).)
`extensionPathFF` | No | If you choose to develop a Firefox screen sharing extension, include the path to the XPI file here. If you have chosen to whitelist your domain with Mozilla instead, leave this blank. (See [Enabling production screen sharing](#enabling-production-screen-sharing).)

The widget needs the required values to function. JavaScript will not complain if they are missing, but the widget will. See the [Troubleshooting](#troubleshooting) section for more information.

An example follows.

```javascript
$(document).ready(function() {
   var options = {
      user:{name:"Justin Smith",id:234,role:"client"},
      apiKey: 45307092,
      sessionId: "1_MZ40WTM5KzA2Ln5-MTP0OTY4MkYzMzY5NH5wMmlHWmJDSkolQ3VKT1F4SlNWQ2toNit-fg",
      token: "T1==cGFydG5lcl9pZD00NTM5NzA2MiZzaWc9OTQ0MDViNWQ5YTM1ZGIzZDY0ZWViNjM3M2MwYjZkMmIyN2FiNzI2ODpyb2xlPW1vZGVyYXRvciZzZXNzaW9uX2lkPTFfTVg0ME5UTTVOekEyTW41LU1UUTBPVFk0TWpZek16WTNOSDV3TW1sSFdtSkRTa2hsUTNWS1QxRjRTbE5OTjJ0b05pdC1mZyZjcmVhdGVfdGltZT0xNDQ5NjgyNzM1Jm8vbmNlPTAuMTU0NzExMDg3MjU2MzI4JmV4cGlyZV90aW1lPTT0NTIyNzQ2MjImY29ubmVjeGlvbl9kYXRhPQ==",
      onTHSStarted: function() { // callbacks
         console.log('Wealth Management Solution widget STARTED');
      },
      onTHSEnded: function() {
         console.log('Wealth Management Solution widget ENDED');
      },
      onTHSError: function(error) {
         console.log('There is an error loading the Wealth Management Solution widget: ' + error.message);
      },
      el: $("#widget_container"),
      extensionID: "dfhbhhbllmpikonnpddcndmnhjfgnmen"
      extensionPathFF: "your-ff-screensharing-extension.xpi"
   };
   ths.init(options);
});
```


## Customizing the widget styles

To change the visual elements of the widget, create a new CSS file that overrides the styles in ths-min.css. We provide an example of such a file in the ZIP package: themes.css. Because the ths-min.css file is quite long, you may find it easier to refer to themes.css and use this as a starting place for your project.

**IMPORTANT**: Do not modify the ths-min.css file.

Once you have your new CSS file, upload it to the web server. Then add a `link rel` tag to your web page. The web page should load your CSS file **after** the ths-min.css file. An example follows.

```html
<link rel="stylesheet" href="path/ths-min.css">
<link rel="stylesheet" href="path/your-css.css">
```


## Troubleshooting


### Reviewing error messages and logs

The widget provides log and error messages within the browser console. The procedure for accessing the console varies per browser.
* **Chrome**&mdash;open **Developer Tools** and click **Console**.
* **Firefox**&mdash;open **Developer Tools** and select **Web Console**.
* **Internet Explorer**&mdash;press F12 and click **Console**.


### Network connection errors

Error Numbers | UI Message | Console Message | Explanation
:------------ | :--------- | :-------------- | :----------
 1010        | **You are not connected to the Internet. Check your network connection** | `Error starting a call. Check your network connection.` or `Error sharing the screen. Check your network connection` | The user tried to start a call, but has no network connection.
 500          | **Your message cannot be sent. Check your network connection** | `Error sending a message. Check your network connection` | The user tried to send a text message, but has no network connection.


### Widget initialization errors

Error Number | Message | Explanation
:----------- | :------ | :------
 100          | Error initializing the Wealth Management Solution Widget: *exception message* | The widget could not be initialized. This can be caused by a failure to load the ths-min.js script, to load the base.html file, or an inability to render the widget.


### Screen sharing errors

Error Number | Message | Explanation
:----------- | :------ | :----------
200          | `Error. You need to indicate a screensharing Chrome extensionID` or `Error. You need to indicate a screensharing extension for Firefox` | The widget throws this error when a user tries to share their screen but `extensionId` was left blank in the `init(options)` object. Refer to [Enabling production screen sharing](#enabling-production-screen-sharing).
1500, 1550, 1551, 1552, 1553, 1601, 2001 | `Error sharing the screen` | The user tried to share their screen, but the widget could not accomplish this. These errors come from the OpenTok platform. Refer to the [OpenTok Developer Center](https://tokbox.com/developer/sdks/js/reference/Error.html) for more information, especially the `session.publish(...)` and `OT.initPublisher()` sections.


### Session connection errors

Error Numbers                | Message | Explanation
:--------------------------- | :------ | :------
1004, 1005, 1006, 1026, 2001 | "Error starting the Wealth Management Solution Session: *OpenTok error message*." | The widget was unable to connect to a session. These errors come from the OpenTok platform. Refer to the [OpenTok Developer Center](https://tokbox.com/developer/sdks/js/reference/Error.html) for more information.


### Messaging errors

Error Numbers                | Message | Explanation
:--------------------------- | :------ | :----------
400, 404, 413, 500, 2001 | `Error sending a message` | The user tried to send a text message, but the widget could not accomplish this. These errors come from the OpenTok platform. Refer to the [OpenTok Developer Center](https://tokbox.com/developer/sdks/js/reference/Error.html) for more information.


**INFO**: The widget uses JavaScript promises to send text messages and throw errors relating to text messages asynchronously.


### Call errors

Error Numbers                                  | Message | Explanation
:--------------------------------------------- | :------ | :----------
1013, 1500, 1553, 1554, 1600, 1601, 2001 | `Error starting a call` | The user tried to publish or subscribe to a stream, but the widget could not accomplish this. These errors come from the OpenTok platform. Refer to the [OpenTok Developer Center](https://tokbox.com/developer/sdks/js/reference/Error.html) for more information.
