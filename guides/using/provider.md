---
title: How to use an anyFetch provider?
layout: doc
---

Unlike [hydraters](/guides/using/hydrater.html), you'll probably never need to use a provider on its own -- you'll always let anyFetch handle all communication.

Still, here's a quick overview of the basic workflow for a provider.

# Provider workflow
## Init
The first endpoint will be `/init/connect`. This call should include a `?code` parameter, which is an `authorization_grant` to be used later on the OAuth flow for authentication on anyFetch servers.
This endpoint will (often) display some setup page or redirect to another provider page to get a grant.

The Dropbox provider, for instance, will create a token and redirect the user to the Dropbox consentment page.
The local provider will display a HTML form asking you for informations about the data you wish to provide.

Once setup is completed (authorization has been granted, ...), the provider will then redirect the user to `/init/callback`. Most often, the `?code` parameter will still be there (sometimes the provider will store this in a cookie, although this is not a good REST practice).

From there, the `code` will be traded for an actual anyFetch OAuth token.

The provider is now ready for use.

## Update (pull new documents)
When a call to `/update` is made (with a POST parameter specifying the `access_token` to update), the provider will retrieve a list of documents to send.

An http status code of `202 Accepted` will be immediately returned, while the provider gathers a list of resource to send.
If an update of the `access_token` is already pending, the status will be `204 No Content`.

The provider will then asynchronously send documents to anyFetch.

## Sample providers
anyFetch ships with some default providers you may want to check:

* `dropbox.anyfetch.com`: connect a Dropbox account. [Source](https://github.com/AnyFetch/dropbox-provider.anyfetch.com)
* `gmail.anyfetch.com`: connect a GMail account. [Source](https://github.com/AnyFetch/gmail-provider.anyfetch.com)
* `gdrive.anyfetch.com`: connect a Google Drive account. [Source](https://github.com/AnyFetch/gdrive-provider.anyfetch.com)
* `local.anyfetch.com`: sync a local directory with anyFetch. [Source](https://github.com/AnyFetch/local-provider.anyfetch.com)
* `salesforce.anyfetch.com`: connect a Salesfetch account. [Source](https://github.com/AnyFetch/salesforce-provider.anyfetch.com)
* `evernote.anyfetch.com`: connect an Evernote account. [Source](https://github.com/AnyFetch/evernote-provider.anyfetch.com)
* `box.anyfetch.com`: connect a Box account. [Source](https://github.com/AnyFetch/box-provider.anyfetch.com)
