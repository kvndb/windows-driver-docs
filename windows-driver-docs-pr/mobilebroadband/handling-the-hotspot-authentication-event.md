---
title: Handling the hotspot authentication event
description: Handling the hotspot authentication event
ms.assetid: e293757e-de4b-4669-a6c4-a57fff157cf4
ms.date: 04/20/2017
ms.localizationpriority: medium
---

# Handling the hotspot authentication event


Windows 8, Windows 8.1, and Windows 10 trigger the hotspot authentication event when it detects a captive portal that supports Wireless Internet Service Provider roaming (WISPr).

When the event occurs, the receiving app must immediately call the [**Windows.Networking.NetworkOperator.HotspotAuthentication.TryGetAuthenticationContext**](https://msdn.microsoft.com/library/windows/apps/hh758381) function by using the event token that is provided as an argument to the event handler. This function returns an object that manages the hotspot authentication attempt. In the event that the function fails, the event handler must exit without performing any additional actions.

Properties on the object allow your app to retrieve the following items:

-   The SSID of the wireless network.

-   Details about the network adapter that is connected to the hotspot.

-   The URL that contains the WISPr message.

-   The XML payload of the WISPr message.

-   The authentication URL to which credentials are supplied.

Other APIs exist to retrieve the following items:

-   The BSSID of the wireless network (see [**Windows.Networking.Connectivity namespace**](https://msdn.microsoft.com/library/windows/apps/br207308)).

-   DHCP parameters of the network (see [Win32 and COM for UWP apps](https://msdn.microsoft.com/library/windows/apps/br205757)).

By using this information and any other information that your app needs to obtain from the local system or the network, credentials can be generated. The object also contains methods that permit the app to continue or complete the hotspot authentication.

The following sections are available in this topic:

-   [Issue credentials](#issuecred)

-   [Abort authentication](#abortauth)

-   [Use alternate authentication methods](#altauth)

-   [Interact with the user](#userint)

## <span id="issuecred"></span><span id="ISSUECRED"></span>Issue credentials


In the simplest case, the app generates credentials based on information it has or can retrieve. For example, a username and password are generated by using information in the WISPr payload and information about the network adapter.

After performing any actions necessary to generate or obtain credentials, the app calls the [**IssueCredentials**](https://msdn.microsoft.com/library/windows/apps/hh758375) method on the [**HotspotAuthenticationContext**](https://msdn.microsoft.com/library/windows/apps/hh758372) object. This method permits the app to supply the following:

-   The WISPr *UserName* parameter

-   The WISPr *Password* parameter

-   Arbitrary non-standard parameters to include in the WISPr response

-   Behavior on failure

If the server rejects the credentials supplied by the app, Windows disconnects from the network and does not retry a connection in the current user session. The final flag allows the application to indicate that if the credentials are unsuccessful, Windows should never automatically retry a connection by using this profile.

There are two variations of this API. The [**IssueCredentials**](https://msdn.microsoft.com/library/windows/apps/hh758375) method passes the parameters to Windows and then returns instantly. This API does not provide the outcome of the authentication attempt. The [**IssueCredentialsAsync**](https://msdn.microsoft.com/library/windows/apps/dn266072) method, introduced in Windows 8.1, is an asynchronous version that enables applications to retrieve the result of the authentication attempt.

## <span id="abortauth"></span><span id="ABORTAUTH"></span>Abort authentication


If the app discovers that it cannot generate credentials for the current network (because roaming agreements have changed, information is unavailable, or for another reason), it must call the [**AbortAuthentication**](https://msdn.microsoft.com/library/windows/apps/hh758373) method on the [**HotspotAuthenticationContext**](https://msdn.microsoft.com/library/windows/apps/hh758372) object.

Windows disconnects from the network and does not reattempt a connection in the current user session. This function accepts a flag that indicates Windows should never automatically retry a connection using this profile.

**Note**  
This method does not remove the profile from the system, and the app can be asked for credentials again if the user manually attempts to connect to the network. If the profile is completely removed, the app must supply a new provisioning file that removes the associated profile.

 

## <span id="altauth"></span><span id="ALTAUTH"></span>Use alternate authentication methods


If the app can authenticate by using a method other than WISPr, it may do so. After successfully authenticating to the network by using an alternate method, it must complete the connection by calling the [**SkipAuthentication**](https://msdn.microsoft.com/library/windows/apps/hh758379) method on the [**HotspotAuthenticationContext**](https://msdn.microsoft.com/library/windows/apps/hh758372) object. When this method is called, Windows re-detects connectivity to the Internet, thereby helping the user interface (UI) to correctly reflect the authenticated state.

**Note**  
The HotspotAuthentication event is not invoked for hotspots that do not advertise support for the WISPr protocol. However, this permits an app to choose a different protocol to use in response, or to use a custom version of WISPr if required.

 

## <span id="userint"></span><span id="USERINT"></span>Interact with the user


If user interaction is required before authentication can proceed, the app must call the [**TriggerAttentionRequired**](https://msdn.microsoft.com/library/windows/apps/hh758380) method on the [**HotspotAuthenticationContext**](https://msdn.microsoft.com/library/windows/apps/hh758372) object. This method is useful in the following circumstances:

-   The user has elected not to store credentials in the app and must sign in.

-   Completing the connection will charge the user’s credit card or other account; therefore, consent is required before proceeding.

-   No credit is available on the user’s account, and a new purchase is required.

This method does not complete the authentication. When this method is invoked, the app requests to be opened in the foreground by using the specified arguments. The event token should be passed to the foreground application so that it can retrieve the [**HotspotAuthenticationContext**](https://msdn.microsoft.com/library/windows/apps/hh758372) object again and complete the authentication by using one of the other three methods.

The app’s request to open in the foreground is not guaranteed to succeed. The HotspotAuthentication event can occur due to an automatic connection while the computer is in Connected Standby. The app is launched only when the computer is no longer in Connected Standby, has been unlocked, and is still connected to the wireless network. Because this delays Internet access until the user unlocks the computer, this state should be avoided whenever credentials can be generated automatically.

## <span id="related_topics"></span>Related topics


[WISPr authentication](wispr-authentication.md)

 

 






