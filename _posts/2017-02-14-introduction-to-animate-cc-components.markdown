---
published: true
title: Quick dive into Animate CC HTML5 Components
layout: post
tags: [adobe, animate-cc]
---

I've been working on Adobe Animate CC quite a bit lately and wanted to create some custom Animate CC components. There hasn't been too much documentation on this topic, so here's what I gathered. I learnt a lot by looking at Adobe Animate CC's default set of components which can be found in:

Windows

* Defaults: C:\Program Files\Adobe\Adobe Animate CC 2017\Common\Configuration\HTML5Components
* Custom: C:\Users\KahWee\AppData\Local\Adobe\Animate CC 2017\en_US\Configuration\HTML5Components

The rest of this is a guide to how Animate CC locates the contents in the directory and build up the Components panel you now see.

The slashes may get confusing, the convention of Adobe is to use the UNIX-style slashes `/` instead of Windows `\` as the directory separator so be sure to stick with it.

## Diving into the main directories

Let's look at the main dirctories in the defaults directory.

```
C:.
├───jqueryui
│   ├───assets
│   ├───jquery-ui-1.1
│   │   └───images
│   ├───locale
│   │   ├───...
│   │   └───zh_TW
│   └───src
├───lib
├───META-INF
├───sdk
├───ui
│   ├───assets
│   ├───locale
│   │   ├───...
│   │   └───zh_TW
│   └───src
└───video
    ├───assets
    ├───locale
    │   ├───...
    │   └───zh_TW
    └───src
```

You see `jqueryui`, `lib`, `META-INF`, `sdk`, `ui` and `video`, of which `jqueryui`, `ui` and `video` are shown in the Animate CC Components Panel here:

![Animate CC Components Panel](/public/images/animate-cc-components-panel.png)

While I do not know why are Animate CC HTML5 Components ordered the way it is, a set of components qualifies as category set when it contains `components.js`.

## Learning from Video

If there's a component to start learning from, I'll humbly suggest "Video" as it is relatively trivial in terms of definition. Let's start by dragging a "Video" from the Animate CC Components Panel.

![Animate CC Components Panel](/public/images/animate-cc-canvas-with-component.png)

The component is available as it picks up contents from `.\HTML5Components\video\components.js` which is a JSON file.

```json
{
  "category": "CATEGORY_VIDEO",
  "components": [{
    "className": "an.Video",
    "displayName": "DISP_NAME_VIDEO",
    "version": "1.0",
    "source": "src/video.js",
    "icon": "assets/SP_FLVPlayback_Sm",
    "dimensions": [400, 300],
    "dependencies": [
      {"src": "../lib/jquery-2.2.4.min.js", "cdn": "https://code.jquery.com/jquery-2.2.4.min.js"},
      {"src": "../sdk/anwidget.js"}
    ],
    "properties": [
      {"name": "PROP_SOURCE", "variable": "src", "type": "Video Content Path", "default": ""},
      {"name": "PROP_AUTOPLAY", "variable": "autoplay", "type": "Boolean", "default": "true"},
      {"name": "PROP_CONTROLS", "variable": "controls", "type": "Boolean", "default": "true"},
      {"name": "PROP_MUTED", "variable": "muted", "type": "Boolean", "default": "false"},
      {"name": "PROP_LOOP", "variable": "loop", "type": "Boolean", "default": "true"},
      {"name": "PROP_POSTER", "variable": "poster", "type": "Image Path", "default": ""},
      {"name": "PROP_PRElOAD", "variable": "preload", "type": "Boolean", "default": "true"},
      {"name": "PROP_CLASS", "variable": "class", "type": "String", "default": "video"}
    ]
  }]
}
```

In the above JSON file -- oddly named `components.json`, you see `components` key holding an array. In the case of the "Video" category, there is only one component -- "Video". A good way to learn how to define the component is to modify and reload the Components Panel in ACC.

## Defing properties in Video

There are 8 properties and they all correspond to what is in the Property Panel in order.

![Animate CC Property Panel for Video](/public/images/animate-cc-component-property.png)

The `properties[].name` placeholders can be defined in `.\HTML5Components\video\locale\strings.json`. Here's the contents for the file:

```json
{
  "locale": "en_US",
  "language": "en",
  "CATEGORY_VIDEO": "Video",
  "DISP_NAME_VIDEO": "Video",	
  "PROP_SOURCE": "source",
  "PROP_AUTOPLAY": "autoplay",
  "PROP_CONTROLS": "controls",
  "PROP_MUTED": "muted",
  "PROP_LOOP": "loop",
  "PROP_POSTER": "poster image",
  "PROP_PRElOAD": "preload",
  "PROP_CLASS": "class"
}
```

`strings.json` lets you assign language set to the placeholders.

### Types of properties

* `Video Content Path`: Lets you select a video file. 
* `Boolean`: Reveal a checkbox to check and uncheck. The `default` includes `"true"` and `"false"` (note that they are strings instead of booleans).
* `Image Path`: Lets you select an image file. Once you chosen the image, it gets copied into your `images` directory.
* `String`: Lets you set a text field.
* `Collection` (Not shown)

## Example of a property -- class

`class` is pretty straightfoward -- let's start by looking at how to define this.

```json
{
  "name": "PROP_CLASS", 
  "variable": "class", 
  "type": "String", 
  "default": "video"
}
```

Keys:

* `name`: What we have here is to define that the name of this property is using the `PROP_CLASS` placeholder, of which you can locate in `.\HTML5Components\video\locale\strings.json` file.
* `variable`: This is how to access the variable later in your code. It's going to look like this -- `this._options['class']`, we will focus on this in a future post.
* `type`: This determines a text field to be used in the "Property Panel"
* `default`: This sets the default value of the text field to "video"

In this case, `class` is used in defining the class for the "Video" component which can later be used for styling and more.

## End

This post gets a little longer than I hope for. I'll end here and guide you through the source code of the "Video" component in the next post. We'll dive into `components[].source`, located in `.\HTML5Components\video\src\video.js`.
