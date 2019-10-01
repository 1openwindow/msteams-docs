---
title: Create a content page for your tab
description: Describes how to create content pages for your tabs
keywords: teams tabs pages content
---

# Create a content page for your Microsoft Teams tab

> [!Important]
> Full support for tabs on mobile clients is coming soon. To prepare for this change you should follow the [guidance for tabs on mobile](~/resources/design/framework/tabs-mobile.md) when creating your tabs. Personal apps (static tabs) are currently available in [developer preview](~/resources/dev-preview/developer-preview-intro.md). and channel / group chat tabs are available in the `...` overflow menu for the tab.
>
> When full support for tabs is released:
>
> * All tabs will always be available on mobile
> * Your `contentUrl` **will be loaded in the mobile Teams client**.
> * For channel/group tabs, users can still open your tab in a separate browser via your `websiteUrl`, however your `contentUrl` will be loaded first.
> * If your tab uses authentication, you **must** upgrade your Teams JavaScript SDK to version 1.4.1 or later, or authentication will fail.

The content page is an HTML page that you host. When the user visits your tab, Microsoft Teams will load the `contentUrl` (that you [provided earlier](~/concepts/tabs/tabs-configuration.md)) within an iframe inside the main tab canvas area.

In this page, you present the main function of your tab, following our [design recommendations](~/get-started/design.md#designing-a-great-tab). You might also need to use the [supplied context](~/concepts/tabs/tabs-context.md) to help display the correct content.

## Prerequisites for content displayed in your tab

For your content to display within a Microsoft Teams tab, make sure it meets the [requirements for tab pages](~/resources/general/requirements.md).

>In summary: You must host your page on a secure HTTPS endpoint, ensure that your page permits itself to be iframed, include the [Microsoft Teams JavaScript client SDK](/javascript/api/overview/msteams-client), and call `microsoftTeams.initialize()`.

## Deep links to items within your content page

You can enable team members to create and share links to items within your tab, such as an individual task within a tab that contains a task list. When chosen, the link navigates to your tab, which focuses on the specific item. See [Create deep links](~/concepts/deep-links.md) to learn how.

## Samples

Check out our [sample tabs](~/samples/code-samples.md) on GitHub.
