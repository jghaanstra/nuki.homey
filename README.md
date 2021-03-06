# Nuki Direct
This is an alternative Homey app for the Nuki Smart Lock and Nuki Opener. Differently from The [Nuki app by Athom](https://apps.athom.com/app/io.nuki) that relies on the Nuki cloud service for controlling your devices, Nuki Direct uses nothing but your local network for communications between your Homey and your Nuki devices (internet access is optionally used for simplifying the initial pairing of a device). When a Nuki device changes its state, it will notify the new state directly to Homey. When Homey issues a command to a Smart Lock or to an Opener, it will send the request directly to the device.  
So, benefits of this approach over the cloud approach are:
* No internet connection needed for communication between Homey and Nuki.
* Improved reliability: it does work even if the internet is down or the Nuki cloud service is temporarily unavailable.
* Quick responsiveness thanks to the direct communication: the faster Homey knows about a state change, the better Homey can serve us.
* Simpler code, smaller memory footprint.

Nuki Direct can also manage the extra features provided by Nuki API in addition to the standard commands, settings, capabilities and flow cards supplied by Homey. With Nuki Direct you can fine-tune the settings and control the specific lock states and events of your Nuki devices.

Since Nuki Direct and Nuki app by Athom have been developed with distinct frameworks, they can happily run side by side on the same Homey without interfering.

## Adding your devices
Follow these steps to add your Nuki devices to Homey.
* First, make sure the Bridge HTTP API (https://developer.nuki.io/page/nuki-bridge-http-api-1-11/4/) in your Nuki Bridge is enabled. You can do this within the Nuki smartphone app. Go to "Manage my devices" and select your Nuki Bridge. There you can enable the HTTP API.
* Once enabled, go to the Homey app and add a Nuki device by selecting the "Nuki Direct" icon and then "Nuki Smart Lock" or "Nuki Opener".
* Tap the "Connect" button and wait for the searching process to start.
* Press the button of your Nuki Bridge during the searching process.
* Select the Nuki device(s) you wish to add to Homey and confirm.

Your Nuki device(s) have now been added to Homey.

## Release Notes for the latest version (release notes of previous versions are available [here](https://github.com/pfreguia/nuki.homey/releases))
### v3.0.6- 2020-11-03
* Complete German translation of the app UI (thanks to Dirk).
* New condition flow card "The contact alarm changed {less|more} than n seconds ago". For more information see [here](https://github.com/pfreguia/nuki.homey/wiki/Nuki-Smart-Lock-and-Homey-presence). 
* Open (unlatch) a Smart Lock or an Opener by app UI or by action flow card: added a "safety" timer that restores the correct state in case the final event from the device is not received.
* Pairing procedure has been rewritten. The new pairing procedure asks automatically the Nuki Servers for the list of registered Bridges (each Nuki Bridge is always registered at the Nuki Servers infrastructure).
The Nuki Servers's answer can be one of the following:
  1. More than one Bridge is registered. The user must select a Bridge from the list of Bridges.
  2. A single Bridge is registered (or the user selected a Bridge in the previous step). The new pairing procedure tries to authenticate against the Bridge via HTTP API. If API Quick Discovery is enabled, Authentication requires the user to press the button on the Bridge. Once authenticated, the procedure asks the Bridge for the list of its devices (Smart Locks and Openers). 
  3. Other (unexpected answer, Nuki Servers are unreachable, ...). Go to manual pairing.

  The Nuki Bridge's answer can be one of the following:
  1. HTTP API is not enabled on the Bridge. Pairing failure.
  2. API Quick Discovery is not enabled on the Bridge. Go to manual paring.
  3. API Quick Discovery is enabled but the user did not press the button on the Nuki Bridge within 30 seconds. Go to manual paring.
  4. The Bridge returned a valid device list. The user can select the device they want to add to Homey and the pairing is successful.
  5. Other (answer not received, unexpected answer, ...). Pairing failure.

  The manual pairing procedure prompts the user to enter IP address, port and API Token of the Bridge. If the entered data are correct, the pairing procedure retrieves the device list from the Bridge, the user can select the device they want to add to Homey and the pairing is successful.   
  Thus, in the most common case (a single Bridge, HTTP API and API Quick Discovery already enabled in the Bridge) the paring procedure only requires the user to press the button on the Bridge.
* Previous versions of Nuki Direct lose all devices and flows if the local IP address Homey changes. This version loses neither devices nor flows.
* Some users have reported Nuki Direct's slowness in updating the status of the devices in the first few minutes after a restart. Indeed, previous versions of Nuki Direct may take up to 5 minutes after a restart for the device status to update. This version updates the devices status immediately after the initialization.
* Nuki Direct v3.0.0 introduced the ability to mark as unavailable all the devices of an unreachable Bridge. This version adds the ability to mark as unavailable a device that is unreachable by the Bridge as well.
* Bug: the condition flow card "Doorbell rang less than n seconds ago" introduced in version v.3.0.3 was always evaluated as false.
