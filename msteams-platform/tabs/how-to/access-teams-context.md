---
title: Get context for your tab
description: Describes how to get user context to your tabs
keywords: teams tabs user context
---

# Get context for your MIcrosoft Teams tab

Your tab needs contextual information to display relevant content:

* Basic information about the user, team, or company.
* Locale and theme information.
* Read the `entityId` or `subEntityId` that identifies what is in this tab.

## User context

Context about the user, team, or company can be especially useful when:

* You create or associate resources in your app with the specified user or team.
* You initiate an authentication flow against Azure Active Directory or other identity provider, and you don't want the user to re-enter their username. (For more information on authenticating within your Microsoft Teams tab, see [Authenticate a user in your Microsoft Teams tab](~/concepts/authentication/authentication.md).

> [!IMPORTANT]
> While this user information can help ensure a smooth user experience, you should not use it as proof of identity. For example, an attacker could load your page in a 'bad browser' and generate malicious information or requests.

## Access context

You can access context information in two ways:

* Insert URL placeholder values.
* Use the [Microsoft Teams JavaScript client SDK](/javascript/api/overview/msteams-client).

### Get context by inserting URL placeholder values

Use placeholders in your configuration or content URLs. Teams replaces the placeholders with the relevant values when determining the actual configuration or content URL. The available placeholders include all fields on the [Context](/javascript/api/@microsoft/teams-js/microsoftteams.context?view=msteams-client-js-latest&preserve-view=true) object. Common placeholders include the following:

* {entityId}: The ID you supplied for the item in this tab when first [configuring the tab](~/tabs/how-to/create-tab-pages/configuration-page.md).
* {subEntityId}: The ID you supplied when generating a [deep link](~/concepts/build-and-test/deep-links.md) for a specific item _within_ this tab. This must be used to restore to a specific state within an entity. for example, scrolling to or activating a specific piece of content.
* {loginHint}: A value suitable as a login hint for Azure AD. This is usually the login name of the current user in their home tenant.
* {userPrincipalName}: The User Principal Name of the current user in the current tenant.
* {userObjectId}: The Azure AD object ID of the current user in the current tenant.
* {theme}: The current UI theme such as `default`, `dark`, or `contrast`.
* {groupId}: The ID of the Office 365 Group in which the tab resides.
* {tid}: The Azure AD tenant ID of the current user.
* {locale}: The current locale of the user formatted as languageId-countryId (for example, en-us).

>[!NOTE]
>The previous `{upn}` placeholder is now deprecated. For backward compatibility, it is currently a synonym for `{loginHint}`.

For example, suppose in your tab manifest you set the `configURL` attribute to

`"https://www.contoso.com/config?name={loginHint}&tenant={tid}&group={groupId}&theme={theme}"`

And the signed-in user has the following attributes:

* Their username is 'user@example.com'.
* Their company tenant ID is 'e2653c-etc'.
* They are a member of the Office 365 group with id '00209384-etc'.
* The user has set their Teams theme to 'dark'.

When they configure your tab, Teams calls this URL:

`https://www.contoso.com/config?name=user@example.com&tenant=e2653c-etc&group=00209384-etc&theme=dark`

### Get context by using the Teams JavaScript library

You can also retrieve the information listed above using the [Microsoft Teams JavaScript client SDK](/javascript/api/overview/msteams-client) by calling `microsoftTeams.getContext(function(context) { /* ... */ })`.

The following example shows context variables:

```json
{
    "teamId": "The Microsoft Teams ID in the format 19:[id]@thread.skype",
    "teamName": "The name of the current team",
    "channelId": "The channel ID in the format 19:[id]@thread.skype",
    "channelName": "The name of the current channel",
    "chatId": "The chat ID in the format 19:[id]@thread.skype",
    "locale": "The current locale of the user formatted as languageId-countryId (for example, en-us)",
    "entityId": "The developer-defined unique ID for the entity this content points to",
    "subEntityId": "The developer-defined unique ID for the sub-entity this content points to",
    "loginHint": "A value suitable as a login hint for Azure AD. This is usually the login name of the current user, in their home tenant",
    "userPrincipalName": "The principal name of the current user, in the current tenant",
    "userObjectId": "The Azure AD object id of the current user, in the current tenant",
    "tid": "The Azure AD tenant ID of the current user",
    "groupId": "Guid identifying the current O365 Group ID",
    "theme": "The current UI theme: default | dark | contrast",
    "isFullScreen": "Indicates if the tab is in full-screen",
    "teamType": "The type of team",
    "teamSiteUrl": "The root SharePoint site associated with the team",
    "teamSiteDomain": "The domain of the root SharePoint site associated with the team",
    "teamSitePath": "The relative path to the SharePoint site associated with the team",
    "channelRelativeUrl": "The relative path to the SharePoint folder associated with the channel",
    "sessionId": "The unique ID for the current Teams session for use in correlating telemetry data",
    "userTeamRole": "The user's role in the team",
    "isTeamArchived": "Indicates if team is archived",
    "hostClientType": "The type of host client. Possible values are android, ios, web, desktop, rigel",
    "frameContext": "The context where tab URL is loaded (for example, content, task, setting, remove, sidePanel)",
    "sharepoint": "The SharePoint context is available only when hosted in SharePoint",
    "tenantSKU": "The license type for the current user tenant",
    "userLicenseType": "The license type for the current user",
    "parentMessageId": "The parent message ID from which this task module is launched",
    "ringId": "The current ring ID",
    "appSessionId": "The unique ID for the current session used for correlating telemetry data",
    "isCallingAllowed": "Indicates if calling is allowed for the current logged in user",
    "isPSTNCallingAllowed": "Indicates if PSTN calling is allowed for the current logged in user",
    "meetingId": "The meeting ID used by tab when running in meeting context",
    "defaultOneNoteSectionId": "The OneNote section ID that is linked to the channel"
}
```

## Retrieve context in private channels

> [!Note]
> Private channels are currently in private developer preview.

When your content page is loaded in a private channel, the data you receive from the `getContext` call is obfuscated to protect the privacy of the channel. The following fields changes when your content page is on a private channel. If your page uses any of the following values, you need to check the `channelType` field to see if your page is loaded in a private channel, and respond accordingly.

* `groupId` - Undefined for private channels.
* `teamId` - Set to the threadId of the private channel.
* `teamName` - Set to the name of the private channel.
* `teamSiteUrl` - Set to the URL of a distinct, unique SharePoint site for the private channel.
* `teamSitePath` - Set to the path of a distinct, unique SharePoint site for the private channel.
* `teamSiteDomain` - Set to the domain of a distinct, unique SharePoint site domain for the private channel.

> [!Note]
>  teamSiteUrl works well for standard channels also.

## Theme change handling

You can register your app to be told if the theme changes by calling `microsoftTeams.registerOnThemeChangeHandler(function(theme) { /* ... */ })`.

The `theme` argument in the function will be a string with a value of `default`, `dark`, or `contrast`.
