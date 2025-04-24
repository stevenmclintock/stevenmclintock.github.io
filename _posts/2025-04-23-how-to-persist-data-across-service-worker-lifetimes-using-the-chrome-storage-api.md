---
title: "How to Persist Data Across Service Worker Lifetimes Using the Chrome Storage API"
date: 2025-04-23 20:03:00:00 +00:00
author: steven
layout: post
image: /assets/img/2025/04/dev-tools-chrome-storage-local.png
tags: web-extensions
---

When migrating a Chrome extension to Manifest V3, one of the main issues _(and surprises)_ a developer may experience is how to persist data across the lifecycles of a service worker.

For example, if your Chrome extension relies on global variables to maintain critical data, and that data is wiped when the browser decides to kill the service worker, it may have a significant impact to the usability and/or performance of your extension.

## Adding the storage permission to the manifest file

Before we can use the various Chrome storage APIs, we first need to add the storage permission to the manifest.json file in our extension:

```
{
    "manifest_version": 3,
    "name": "Storage APIs",
    "version": "1.0.0",
    "description": "Example of storage APIs",
    "permissions": [
        "storage"
    ],
    "background": {
        "service_worker": "background.js"
    }
}
```

## chrome.storage.local

If we’d like to persist data for the lifetime of the Chrome extension _(e.g the data will continue to exist until the user uninstalls the extension)_, we can use the chrome.storage.local API.

To set data using chrome.storage.local:

```
chrome.storage.local.set({ name: "Steven McLintock" }).then(() => {
    console.log("Name was set in chrome.storage.local");
});
```

To retrieve data using chrome.storage.local:

```
chrome.storage.local.get(["name"]).then((result) => {
    console.log(`Name is set to ${result.name}`);
});
```

{%
    include image.html
    year='2025'
    month='04'
    file='dev-tools-chrome-storage-local.png'
    alt='Using the chrome.storage.local API'
    caption='Using the chrome.storage.local API'
%}

Please be aware that the data stored in chrome.storage.local _is_ easily accessible through the developer tools and may not be ideal for storing sensitive data.

## chrome.storage.session

Alternatively, for more temporary data, and perhaps sensitive data that you would _not_ like to be persisted to disk, there is the chrome.storage.session API. The data stored using chrome.storage.session is stored in memory and will be persisted across service worker lifetimes.

To set data using chrome.storage.session:

```
chrome.storage.session.set({ name: "Steven McLintock" }).then(() => {
    console.log("Name was set in chrome.storage.session");
});
```

To retrieve data using chrome.storage.session:

```
chrome.storage.session.get(["name"]).then((result) => {
    console.log(`Name is set to ${result.name}`);
});
```

{%
    include image.html
    year='2025'
    month='04'
    file='dev-tools-chrome-storage-session.png'
    alt='Using the chrome.storage.session API'
    caption='Using the chrome.storage.session API'
%}