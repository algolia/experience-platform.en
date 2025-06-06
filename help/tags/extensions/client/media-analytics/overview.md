---
title: Adobe Media Analytics for Audio and Video Extension Overview
description: Learn about the Adobe Media Analytics for Audio and Video tag extension in Adobe Experience Platform.
exl-id: 426cfd08-aead-4b35-824c-45494bca2fc8
---
# Adobe Media Analytics for Audio and Video extension overview

>[!NOTE]
>
>Adobe Experience Platform Launch has been rebranded as a suite of data collection technologies in Adobe Experience Platform. Several terminology changes have rolled out across the product documentation as a result. Please refer to the following [document](../../../term-updates.md) for a consolidated reference of the terminology changes.

Use this documentation for information on installing, configuring, and implementing the Adobe Media Analytics for Audio and Video extension (Media Analytics extension). Included are the options available when using this extension to build a rule, along with examples and links to samples.

The Media Analytics (MA) extension adds the core JavaScript Media SDK (Media 2.x SDK). This extension provides the functionality for adding the `MediaHeartbeat` tracker instance to a tag site or project. The MA extension requires two additional extensions:

* [Analytics Extension](../analytics/overview.md)
* [Experience Cloud ID Extension](../id-service/overview.md)

>[!IMPORTANT]
>
>Audio tracking requires Analytics Extension v1.6 or higher.

After you have included all three of the extensions mentioned above in your tag project, you can proceed in one of two ways:

* Use `MediaHeartbeat` APIs from your web app
* Include, or build, a player-specific extension that maps specific media player events to the APIs on the `MediaHeartbeat` tracker instance. This instance is exposed through the MA extension.

## Install and configure the MA extension

* **Install -** To install the MA extension, open your extension property, select **[!UICONTROL Extensions > Catalog]**, hover over the **[!UICONTROL Adobe Media Analytics for Audio and Video]** extension, and select **[!UICONTROL Install]**.

* **Configure -** To configure the MA extension, open the [!UICONTROL Extensions] tab, hover over the extension, and then select **[!UICONTROL Configure]**:

![MA Extension Configuration](../../../images/ext-va-config.jpg)

### Configuration options:

| Option | Description |
| :--- | :--- |
| Tracking Server | Defines the server for tracking media heartbeats (this is not the same server as your analytics tracking server) |
| Application Version | The version of the media player app/SDK |
| Player Name | Name of the media player in use (e.g., "AVPlayer", "HTML5 Player", "My Custom VideoPlayer") |
| Channel | Channel name property |
| Online Video Provider | Name of the online video platform through which content gets distributed |
| Debug Logging | Enable or Disable logging |
| Enable SSL | Enable or Disable sending pings over HTTPS |
| Export APIs to Window Object | Enable or Disable exporting Media Analytics APIs to global scope |
| Variable Name | A variable you use to export Media Analytics APIs under the `window` object |

**Reminder:** The MA extension requires the [Analytics](../analytics/overview.md) and [Experience Cloud ID](../id-service/overview.md) extensions. You must also add these extensions to your extension property and configure them.

## Using the MA extension

### Using from a webpage/JS-app

The MA extension exports the MediaHeartbeat APIs in the global window object by enabling the "Export APIs to Window Object" setting in the [!UICONTROL Configuration] page. It exports the APIs under the configured variable name. For example, if the variable name is configured to be `ADB` then MediaHeartbeat can be accessed by `window.ADB.MediaHeartbeat`.

>[!IMPORTANT]
>
>The MA extension exports the APIs only when `window["CONFIGURED_VARIABLE_NAME"]` is undefined and does not override existing variables.

1. **Create MediaHeartbeat Instance:**&nbsp;`window["CONFIGURED_VARIABLE_NAME"].MediaHeartbeat.getInstance`

    **Params:** A valid delegate object exposing these functions.
 
    | Method | &nbsp;Description&nbsp;&nbsp; |
    | :--- | :--- |
    | `getQoSObject()` | Returns `theMediaObject` instance that contains current QoS information. This method will be called multiple times during a playback session. Player implementation must always return the most recently available QoS data. |
    | `getCurrentPlaybackTime()` | Returns the current position of the playhead. For VOD tracking, the value is specified in seconds from the beginning of the media item. For LIVE/LIVE tracking, the value is specified in seconds from the beginning of the program. |
 
    **Return Value:** A promise which either resolves with a `MediaHeartbeat` instance or rejects with an error message.

1. **Access MediaHeartbeat Constants:**&nbsp;`window["CONFIGURED_VARIABLE_NAME"].MediaHeartbeat`

    This exposes all of the constants and static methods from the [`MediaHeartbeat`](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript/MediaHeartbeat.html) class.
 
    You can obtain the sample player here: [MA Sample Player](https://github.com/Adobe-Marketing-Cloud/media-sdks/tree/master/samples/launch/js/2.x). The sample player acts as a reference to showcase how to use the MA extension to support Media Analytics directly from a webapp.

1. Create the MediaHeartbeat tracker instance as follows:

    ```javascript
    var MediaHeartbeat = window["CONFIGURED_VARIABLE_NAME"].MediaHeartbeat;

    var delegate = {
        getCurrentPlaybackTime: this._getCurrentPlaybackTime.bind(this),
        getQoSObject: this._getQoSObject.bind(this),
    };
    
    var config = {
        playerName: "Custom Player",
        ovp: "Custom OVP",
        channel: "Custom Channel"
    };
    
    var self = this;
    MediaHeartbeat.getInstance(delegate, config).then(function(instance) {
        self._mediaHeartbeat = instance;
        // Do Tracking using the MediaHeartbeat instance.
    }).catch(function(err){
        // Getting MediaHeartbeat instance failed.
    });
    ```

### Using from other extensions

The MA extension exposes the `get-instance` and `media-heartbeat` shared modules to other extensions. (For additional information on Shared Modules, see [Shared Modules documentation](../../../extension-dev/web/shared.md).)

>[!IMPORTANT]
>
>Shared Modules can only be accessed from other extensions. That is, a webpage/JS app cannot access the shared modules, or use `turbine` (see code sample below) outside of an extension.

1. **Create MediaHeartbeat Instance:**&nbsp;`get-instance` Shared Module

    **Params:**
 
    * A valid delegate object exposing these functions:
 
       | Method | &nbsp;Description&nbsp;&nbsp; |
       | :--- | :--- |
       | `getQoSObject()` | Returns the `MediaObject` instance that contains the current QoS information. This method will be called multiple times during a playback session. The player implementation must always return the most recently available QoS data. |
       | `getCurrentPlaybackTime()` | Returns the current position of the playhead. For VOD tracking, the value is specified in seconds from the beginning of the media item. For LIVE/LIVE tracking, the value is specified in seconds from the beginning of the program. |
 
    * An optional config object exposing these properties:
 
       | Property | Description | Required |
       | :--- | :--- | :--- |
       | Online Video Provider | Name of the online video platform through which content is distributed. | No. If present, overrides the value defined during extension configuration. |
       | Player Name | Name of the media player in use (e.g., "AVPlayer", "HTML5 Player", "My Custom VideoPlayer") | No. If present, overrides the value defined during extension configuration. |
       | Channel | Channel name property | No. If present, overrides the value defined during extension configuration. |
 
    **Return Value:** A promise which either resolves with a `MediaHeartbeat` instance or rejects with an error message.

1. **Access MediaHeartbeat Constants:**&nbsp;`media-heartbeat` Shared Module

    This module exposes all of the constants and static methods from this class: [https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript/MediaHeartbeat.html](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript/MediaHeartbeat.html).

1. Create the MediaHeartbeat tracker instance as follows:

    ```javascript
    var getMediaHeartbeatInstance =
      turbine.getSharedModule('adobe-video-analytics', 'get-instance');

    var MediaHeartbeat =
      turbine.getSharedModule('adobe-video-analytics', 'media-heartbeat');
      ...

    var delegate = {
        getCurrentPlaybackTime: this._getCurrentPlaybackTime.bind(this),
        getQoSObject: this._getQoSObject.bind(this),
    }

    var config = {
        playerName: "Custom Player",
        ovp: "Custom OVP",
        channel: "Custom Channel"
    }
    ...

    var self = this;
    getMediaHeartbeatInstance(delegate, config).then(function(instance) {
        self._mediaHeartbeat = instance;
        ...
        // Do Tracking using the MediaHeartbeat instance.
    }).catch(function(err){
        // Getting MediaHeartbeat instance failed.
    });

    ...
    ```

1. Using the Media Heartbeat instance, follow the [Media SDK JS documentation](https://experienceleague.adobe.com/docs/media-analytics/using/legacy-implementations/legacy-media-sdks/setup-javascript/set-up-js-2.html) and [JS API documentation](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript/index.html) to implement media tracking.

>[!NOTE]
>
>**Testing:** For this release, to test your extension you must upload it to [Experience Platform](../../../extension-dev/submit/upload-and-test.md), where you have access to all dependent extensions.


<!--
## Leveraging the sample HTML5 player

You can obtain the MA extension sample HTML5 player here: [HTML5 Sample Player](https://github.com/adobe/reactor-adobe-va-sample-player). The sample player acts as a reference to create video player extensions and to showcase using the MA extension to support media tracking.

### Sample player extension action types

This section describes the action types available in the Sample Player extension.

#### Open Video

The _Open Video_ action provides various configurations for creating and customizing an HTML5 player, providing a video to play and enabling/disabling Adobe Video Analytics tracking.

**Action Configuration / Player Settings:** Note the CSS Selector setting which specifics the `<div>` in the web page where the player is added. Note also that the _Enable Adobe Analytics_ checkbox is checked in the Analytics Settings pane.

![Player Extension Action Configuration](../../../images/ext-va-sp-action.png)

![Player Extension Action Configuration](../../../images/ext-va-sp-action1.png)

* [\[...\]/openVideo/openVideo.jsx](https://github.com/adobe/reactor-adobe-va-sample-player/blob/master/src/view/actions/openVideo/openVideo.jsx) -

  UI Code to configure the Action is defined here.

* [\[...\]/actions/openVideo.js](https://github.com/adobe/reactor-adobe-va-sample-player/blob/master/src/lib/actions/openVideo.js) - This file exports a function that gets executed when the Action is triggered as part of the tag rule.

  This is a code snippet from `openVideo.js` where the `openVideo` Action is executed:

  ```javascript
    function openVideo(settings) {
        let player;
        try {
            Logger.info(LOG_TAG, `Executing action with ${JSON.stringify(settings)}`);

            player = new PlayerFacade(settings);
            PlayerStore.registerPlayer(player);
            player.load(settings.media);
        } catch (ex) {
            Logger.error(LOG_TAG, `Creating player for action openVideo failed with error ${ex.message}`);

            // Cleanup from DOM.
            if (player) {
                player.destroy();
            }
        }
    }
    ...
  ```

* [\[...\]/analytics/adobeAnalyticsProvider.js](https://github.com/adobe/reactor-adobe-va-sample-player/blob/master/src/lib/helpers/analytics/adobeAnalyticsProvider.js) - This file implements Video Analytics tracking by using Shared Modules exposed by the VA extension.

### Sample player extension basic deployment

Once the Sample Player extension is installed, you'll need to create at least one rule to properly deploy it. The Image below shows a sample rule that opens the specified video when the core extension fires the `DOMLoaded` event.

![Player Extension Sample Rule](../../../images/ext-va-sp-rule.png)

After you have saved this rule, you will need to add it to a Library, and then build and deploy so that you can test the behavior.

-->
