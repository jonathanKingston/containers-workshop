# Making your own extension - Containers Workshop *by* [@KingstonTime](https://twitter.com/KingstonTime)

[< Back to the workshop](README.md)

## Making your own extension

To make an extension:

- Create a directory for the extension.
- Create a [`manifest.json` file](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json)

A basic example looks like this:

<code style="white-space: pre; display: block;">
{
  "manifest_version": 2,
  "name": "My Extension",
  "version": "1.0",

  "<a href="https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json/background">background</a>": {
    "scripts": ["jquery.js", "my-background.js"]
  },

  "browser_action": {
    "default_icon": {
      "19": "button/icon-19.png",
      "38": "button/icon-38.png"
    },
    "default_title": "I am a popup",
    "default_popup": "popup/index.html"
  },

  "<a href="https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json/permissions">permissions</a>": [
    // Put permissions here
    "webRequest",
    "*://developer.mozilla.org/*",
    "tabs"
  ],

}
</code>

- [Tutorial on your first extension](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Your_first_WebExtension)
- [A big list of example extensions](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Examples)

### Browser actions

Are an icon that represents the extension in the tool bar, clicking the icon opens a HTML panel which the extension controls.

![](images/extensions/browser-action.png)

- [Web extension example that makes a browser action to manage containers](https://github.com/mdn/webextensions-examples/tree/master/contextual-identities)

### Content scripts

Can inject into any page to read from the DOM or manipulate the page.

Add the following to your extensions manifest:
```
  "content_scripts": [{
     "matches": ["<all_urls>"],
     "js": ["content-script.js"]
  }]
```

`content-script.js` will then be loaded into all web pages, with access to read and write to the DOM.

- [Add copy functionality to pages](https://github.com/mdn/webextensions-examples/tree/master/selection-to-clipboard)
- [Page to content messaging](https://github.com/mdn/webextensions-examples/tree/master/page-to-extension-messaging) - this example shows how an event listener injected into the page can fire an event back into the content script. The same technique is used for messages between content and background scripts.


### Background scripts

Background scripts are script that is loaded into it's own "background page", it has the ability to behave like a browser process with functionality such as: watch requests, manipulate cookies and communicate with other parts of the extension.

## Javascript APIs

Above are some of the individual behaviours extensions can do, however along with standard JavaScript and web features extensions open up APIs ([Full list of APIs](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API)).

Most of these new APIs will require a new "permission" in your manifest file.

### Tabs

Lots of the extension APIs link to the tab that they are using [webRequest](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/webRequest), [webNavigation](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/webNavigation) etc.

To use the [tabs API](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/tabs) you will need to have in your manifest:
- "tabs" permission

- "activeTab" permission
or
- A url to match on or alternatively "&lt;all_urls&gt;"

We can query for the container on the tab when both of the following are true:
- "cookies" permission
- `privacy.userContext.enabled` is set to true. (Check in about:config)

Once you have the right permissions you can start checking on the fly the [cookieStoreId](glossary.md#cookie-store-id) you have set for a tab:
```
browser.tabs.onActivated.addListener((activeInfo) => {
  browser.tabs.get(activeInfo.tabId).then((tab) => {
    console.log(`The current tab has the following cookie store: ${tab.cookieStoreId}`);
  });
});
```

The console log will be printed into the extension debugger:
```
The current tab has the following cookie store: firefox-container-1
```

## Make it bleeding

Another nice feature of extensions is having the latest browser technology without worry about transpilation.

So features such as:
- JavaScript
  - [Maps](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) and [Sets](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
  - [Template strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
  - [Class syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/class)
  - [array.includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)
  - [Symbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
- CSS
  - [Flexbox](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox)
  - [Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)

All of these tools make extension writing much simpler and easier to get going with.

[< Back to the workshop](README.md)