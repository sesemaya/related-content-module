# Bibblio Related Content Module

Get content recommendations quickly and easily with [Bibblio](http://bibblio.org)'s pre-built Related Content Module. It comes with all the JavaScript and CSS you'll need to get content recommendations on the page.

## Signing Up

[Sign up for a free plan](https://developer.bibblio.org/signup?plan_ids=2357355848804) if you haven't already. You'll need to get started with our API to use this module.

[Check out the full docs here](http://developer.bibblio.org/docs).

## Installing the module

The module's JavaScript and CSS can be installed with [Bower](https://bower.io/#install-bower) by running:

```
bower install bibblio-related-content-module
```

Alternatively, the module's JavaScript and CSS can be installed with [npm](https://www.npmjs.com/get-npm) by running:

```
npm install bibblio-related-content-module
```

## Loading the module files

You will need to load the JavaScript and CSS files in order to use the Related Content Module on your page. If you've installed this as a project dependency and are using asset packaging pipelines then you can safely ignore this step and do it your way.

Include the files from bower by adding these tags to your page:

```html
<!-- CSS -->
<link rel="stylesheet" type="text/css" href="bower_components/bibblio-related-content-module/css/bib-related-content.css">

<!-- JavaScript -->
<script src="bower_components/underscore/underscore-min.js"></script>
<script src="bower_components/bibblio-related-content-module/js/bib-related-content.js"></script>
```
Or, if you're using the npm package:

```html
<!-- CSS -->
<link rel="stylesheet" type="text/css" href="node_modules/bibblio-related-content-module/css/bib-related-content.css">

<!-- JavaScript -->
<script src="node_modules/underscore/underscore-min.js"></script>
<script src="node_modules/bibblio-related-content-module/js/bib-related-content.js"></script>
```

The module may also be required using NodeJS's require if installed with npm:

```javascript
var Bibblio = require("bibblio-related-content-module");
```

## Using the module

The module can be added to a page by calling the JavaScript function provided:
```javascript
Bibblio.initRelatedContent(...)
```

Typically this would be done on the `pageReady` event, but could also be dropped in a script tag at the bottom of the page.

The function accepts a JavaScript object as its only parameter. This object can contain the following properties:

#### `'targetElementId'` _(required)_
The DOM id of an HTML element you'd like to initialise as a related content panel. For example, this parameter could be 'yourRelatedContentModuleDiv', provided the following element exists on the page:
```html
<div id="yourRelatedContentModuleDiv"></div>
```
You will need to drop this (empty) HTML element onto the page yourself and position it as you wish.

#### `'recommendationKey'` _(required)_
Allows you to safely connect to the Bibblio API from a page visitor's browser. The recommendation key can be obtained from [our API](http://docs.bibblio.apiary.io/#reference/authorization/recommendation-keys/list-recommendation-keys) or [your management console](https://developer.bibblio.org/admin/account) (click on the _Credentials_ page and then select _Manage my keys_).

#### `'contentItemId'` _(required unless customUniqueIdentifier is provided instead)_
The Bibblio `contentItemId` of the content item being displayed. Content recommendations will be based on this Bibblio content item. The `contentItemId` is provided when [creating a content item](http://docs.bibblio.apiary.io/#reference/storing-data/content-items/create-a-content-item), and is retrievable when [listing your content items](http://docs.bibblio.apiary.io/#reference/storing-data/content-items/list-content-items).

**NB**: You must provide either a `contentItemId` **or** a `customUniqueIdentifier`.

#### `'customUniqueIdentifier'` _(required if no contentItemId is provided)_
It is possible to use your own id to retrieve recommendations, thereby avoiding the need to store Bibblio's `contentItemId` in your database. To do this, make sure you provide a `customUniqueIdentifier` when [creating a content item](http://docs.bibblio.apiary.io/#reference/storing-data/content-items/create-a-content-item). This allows you to specify the `customUniqueIdentifier` here, when retrieving recommendations. In this case, content recommendations will be based on the Bibblio content item with the `customUniqueIdentifier` specified. If you would prefer to store Bibblio's id simply use `contentItemId` as documented above.

**NB**: You must provide either a `contentItemId` **or** a `customUniqueIdentifier`. A `customUniqueIdentifier` must be supplied if `autoIngestion` is enabled.

#### `'autoIngestion'` _(optional)_
The Related Content Module is able to ingest content automatically. It will attempt to retrieve recommendations for the specified content item. If the item does not exist and `autoIngestion` is set to `true`, the module will attempt to parse the content item from the page. Subsequent page loads will then be able to display recommendations once the item has been ingested and indexed. This saves you the trouble of integrating with Bibblio on your backend systems. There are some things to keep in mind when enabling `autoIngestion`: a `customUniqueIdentifier` must be supplied, the item will be ingested the first time it is viewed in a browser, the domain from which it originates must be [whitelisted](http://docs.bibblio.apiary.io/#reference/related-content-module/auto-ingestion-domains), and future updates to item text will not be considered. If you do require content items to be ingested and synced immediately at the time of publication and update then it would be best to [integrate with the API directly](http://docs.bibblio.apiary.io/#reference/storing-data/content-items/create-a-content-item). Please [contact us](http://www.bibblio.org/contact) if you are uncertain about the choice.


#### `'catalogueIds'` _(optional)_
The [catalogues](http://docs.bibblio.apiary.io/#reference/storing-data/catalogues) that recommendations should draw from. The `catalogueId` of [any catalogues you own](http://docs.bibblio.apiary.io/#reference/storing-data/catalogues/list-catalogues) would be valid. Default is the same catalogue as the source content item specified.

#### `'userId'` _(optional)_
Your own, unique id for the current site visitor. This allows us to apply recommendation personalization. Please do not provide any personally identifiable information for this field.

#### `'queryStringParams'` _(optional)_
Allows you to append additional query string params to the target url of recommended items. This is particularly useful for specifying analytics params such as _utm_source_. The value should be a JavaScript object. Each property will be added as a param to the url. e.g. `{ "utm_source" : "BibblioRCM", "utm_campaign" : "SiteFooter" }` would append `utm_source=BibblioRCM&utm_campaign=SiteFooter` to the url query string of all recommended items.

#### `'styleClasses'` _(optional)_
Allows you to customise the CSS styles applied to the related content module. An interactive configuration wizard is available in the Demos section of your Bibblio management console, which allows you to generate parameters for this option. If you plan to place the module on an area of your page that has a dark background color you can append 'bib--invert' to your parameters to be sure everything remains legible. If most of your content item images are portrait sized, consider appending 'bib--portrait' to your parameters so the images resize nicely in the tiles. Default is 'bib--box-6 bib--wide'.

#### `'subtitleField'` _(optional)_
Allows you to specify the content item field to use as subtitles on the recommended content panel. Any [valid content item field](http://docs.bibblio.apiary.io/#reference/storing-data/content-items/retrieve-a-content-item) can be used. Providing a value of _false_ will disable the subtitle. Default is '_headline_'.


## Tracking data

The module will automatically submit user interaction data. By default all interaction data is completely anonymous. You are, however, able to provide an optional userId when the module is initialised, which will allow us to personalize recommendations for the requested user.

Here is a sample of the tracking data submitted from within the Related Content Module:
```javascript
{ "type": "Clicked",
  "object": [["contentItemId", "123"]],
  "context": [
    ["sourceContentItemId", "012"],
    ["sourceHref", "https://example.com/the/page/url"],
    ["recommendations.contentItemId", "456"],
    ["recommendations.contentItemId", "789"],
    ["recommendations.contentItemId", "101"],
    ["recommendations.contentItemId", "123"]],
  "instrument": {
    "type": "BibblioRelatedContent",
    "version": "1.1.0",
    "config": {'styleClasses': 'bib--grd-4 bib--wide'}}
  "actor": {
    "userId": "42"}} // optional
```


## An example

The following snippet shows the initialisation of a related content module. You will need to replace `YOUR_RECOMMENDATION_KEY` and `YOUR_CONTENT_ITEM_ID` with [a recommendation key](http://docs.bibblio.apiary.io/#reference/authorization/recommendation-keys/list-recommendation-keys) and the `contentItemId` returned when [creating a content item](http://docs.bibblio.apiary.io/#reference/storing-data/content-items/create-a-content-item) or [listing your content items](http://docs.bibblio.apiary.io/#reference/storing-data/content-items/list-content-items).

```html
<head>
    <!-- * Related Content Styles -->
    <link rel="stylesheet" type="text/css" href="bower_components/bibblio-related-content-module/css/bib-related-content.css">
</head>

<!-- * Related Content HTML -->
<!-- Provide an enclosing element with an id. Position and size it as you wish. -->
<div id="bib_related-content"></div>

<!-- * Related Content Javascript -->
<script src="bower_components/underscore/underscore-min.js"></script>
<script src="bower_components/bibblio-related-content-module/js/bib-related-content.js"></script>
<script>
    // Initialise the related content plugin.
    (function() {
        Bibblio.initRelatedContent({
            targetElementId: 'bib_related-content', // Required
            recommendationKey: 'YOUR_RECOMMENDATION_KEY', // Required
            contentItemId: 'YOUR_CONTENT_ITEM_ID', // Required unless customUniqueIdentifier is provided instead
            // customUniqueIdentifier: 'YOUR_CUSTOM_UNIQUE_IDENTIFIER', // Required if no contentItemId is provided
            // autoIngestion: true, // Requires a customUniqueIdentifier. Default: false.
            // catalogueIds: ["a8365ab1-00f9-38f8-af51-4d0ff527856f"], // Default: same as content item
            // userId: "42", // Default: nil
            // queryStringParams: { "utm_source" : "BibblioRCM" }, // Appends 'utm_source=BibblioRCM' to recommended urls
            styleClasses: "bib--grd-4 bib--wide", // Default: 'bib--box-6 bib--wide'
            subtitleField: 'provider.name'  // Default: headline. passing a value of false will disable the subtitle   completely
        });
    })();
</script>
```

## Trying it out

The [example.html](example.html) file provided shows a working demo that gets these values from querystrings. You can save this file and open it in your browser in the following format:

```
example.html?recommendationKey=YOUR_RECOMMENDATION_KEY&contentItemId=YOUR_CONTENT_ITEM_ID
```
