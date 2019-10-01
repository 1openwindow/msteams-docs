---
title: Requirements for tab pages in Microsoft Teams
description: Requirements for tab pages in Microsoft Teams
keywords: teams formats links
---
# Requirements for tab pages in Microsoft Teams

All tab content, including configuration, content, and tab-removal pages must meet the following requirements:

* Pages must be hosted on a secure HTTPS endpoint. Microsoft Teams will not display insecure HTTP content.
* Your content must work in an &lt;iframe&gt;. By default, web pages can be iframed by anyone, but most existing websites prevent themselves from being embedded in an &lt;iframe&gt;. You may optionally set these headers if you wish to only allow your page to be iframed by Microsoft Teams for extra security:
  * Set header: <br/>
    `Content-Security-Policy: frame-ancestors teams.microsoft.com *.teams.microsoft.com *.skype.com` <br/>
    Most modern browsers support this.
    * For Internet Explorer 11 compatibility, set `X-Content-Security-Policy` as well.
  * Alternatively, set header `X-Frame-Options: ALLOW-FROM https://teams.microsoft.com/`. This header is deprecated but still respected by most browsers.
* Include the [Microsoft Teams JavaScript client SDK](/javascript/api/overview/msteams-client) in your page as a script source. There are multiple ways to do this; please refer to [this](/javascript/api/overview/msteams-client) for detailed instructions.
* After your page has successfully loaded, call `microsoftTeams.initialize()` to display your page. Microsoft Teams will not display your page unless you do so.
* All domains for pages you display in your tabs must be listed in the manifest's `validDomains` list. See [validDomains](~/resources/schema/manifest-schema.md#validdomains) in the manifest schema reference for more information.

> Hitting problems? See the [troubleshooting guide](~/troubleshoot/troubleshoot.md).
>
> [!TIP]
> For developers using TypeScript, Microsoft Teams provides a [definition file](https://statics.teams.microsoft.com/sdk/v1.2/types/MicrosoftTeams.d.ts) to enable IntelliSense or similar support from your code editor as well as compile-time type checking as part of your build.

## Tabs on mobile clients

> [!Note]
> Static tabs on mobile are currently in [developer preview](~/resources/dev-preview/developer-preview-intro.md). Static tabs on a mobile client with developer preview enabled will open their content URL within the Teams client.
>
>

Tabs behave differently on the iOS and Android Teams clients than on the Teams desktop or web clients.

### Desktop and web client behavior

What appears in a tab on desktop/web is defined by the value of `contentUrl`: Teams displays the contents of `contentUrl` in the tab. (See [Determine the content to display in the tab](~/concepts/tabs/tabs-configuration.md#determine-the-content-to-display-in-the-tab)). If the tab has a value for `websiteUrl`, another icon appears with the others at the upper right of the tab: ![Go to website](~/assets/images/go-to-website-icon.png). When the user clicks on this icon, `websiteUrl` is opened in a new browser tab.

### Mobile client behavior

> [!Important]
> Full support for tabs on mobile clients is coming soon. To prepare for this change you should follow the [guidance for tabs on mobile](~/resources/design/framework/tabs-mobile.md) when creating your tabs. Personal apps (static tabs) are currently available in [developer preview](~/resources/dev-preview/developer-preview-intro.md). and channel / group chat tabs are available in the `...` overflow menu for the tab.
>
> When full support for tabs is released:
>
> * All tabs will always be available on mobile
> * Your `contentUrl` **will be loaded in the mobile Teams client**.
> * For channel/group tabs, users can still open your tab in a separate browser via your `websiteUrl`, however your `contentUrl` will be loaded first.
> * If your tab uses authentication, you must upgrade your Teams JavaScript SDK to version 1.4.1 or later, or authentication will fail.

For group and channel (configurable) tabs, the mobile clients only show tabs that have a value for `websiteUrl`. If you want your tab to appear on the Teams mobile clients, you must set the value of `websiteUrl`. Personal (static) tabs are not available on mobile clients.

If user opens a configurable tab on browser using 'websiteUrl', you can use iOS *universal links* or Android *app links* to deeplink `websiteUrl` in your mobile app if you want to. The OS will also prompt the user to install your app if it is not already installed.

Information on universal links and app links:

* [iOS universal links](https://developer.apple.com/ios/universal-links/)
* [Android app links](https://developer.android.com/training/app-links/index.html)
