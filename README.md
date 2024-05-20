## About

🌐 cample-html is a small library for working with server-side html. It is based on requests sent to the server via XMLHttpRequest and processed into ready-made HTML.

### Example

<b>HTML before</b>

```html
<div>
  <template data-cample data-src="/api/test" data-method="get"></template>
</div>

<script src="https://unpkg.com/cample-html@0.0.3"></script>
```

<b>Server route - /api/test</b>

```html
<div>123</div>
```

<b>HTML after</b>

```html
<div>
  <div>123</div>
</div>

<script src="https://unpkg.com/cample-html@0.0.3"></script>
```

### Why cample-html?

cample-html is easy to use and effective in practice. You can literally download reusable HTML from the server in just a couple of clicks, which will reduce a huge amount of code and also simplify the creation of the user interface. Also, this product is open-source under the [MIT license](https://github.com/cample-html/cample-html/blob/master/LICENSE), which allows it to be used for commercial purposes.

Here are a few small advantages that the module has:

- Light weight
- Ability to work with template mounting directly via js
- Request Status Update
- Fairly safe HTML processing without outerHTML and similar functions, which minimizes the likelihood of errors
- Fully documented

And other advantages that will be visible when working with the module.

### About server-side rendering

Although the markup is generated on the server, the module <b>does not provide</b> functionality for displaying content to search robots. This is not expected in the future because this module is intended for other purposes.

### Discussion and development of an open-source project

This product has a discussion platform on [github](https://github.com/cample-html/cample-html/discussions). You can write your reviews or wishes about the project there. The project developer will try to answer you as soon as possible.

In the future, it is planned to maintain social networks, but for now the entire focus is only on the product.

## More examples

### Example 1

(Future versions will implement get and set functionality)

```javascript
const templateFn = CampleHTML.createTemplate(
  `<template data-cample data-src="/api/test" data-method="get"></template>`
);

const elementObj = templateFn();

setTimeout(() => {
  if (elementObj.element) {
    console.log(elementObj.element);
  } else {
    console.log("The data hasn't arrived yet");
  }
}, 1000);
```

### Example 2

```html
<div>
  <button class="getText">Get text</button>
  <p>
    <template
      data-cample
      data-src="/api/text"
      data-method="get"
      data-after="click:.getText"
      data-mode="one"
    >
    </template>
  </p>
</div>
<script src="https://unpkg.com/cample-html@0.0.3"></script>
```

## Installation

Cample-html can be installed in several ways, which are described in this article. This tool is a simple javascript file that is connected in the usual way through a `script`, or using the `import` construct in an environment that supports this (webpack build, parcel build etc.). The first and easiest way is to install using a CDN.

### Package Manager

This method involves downloading through npm or other package managers.

```bash
npm i cample-html
```

> [Node.js](https://nodejs.org) is required for npm.

Along the path node-modules/cample-html/dist you can find two files that contain a regular js file and a minified one.

### Manual download

You can install the package by simply [downloading](https://unpkg.com/cample-html@0.0.3/dist/cample-html.min.js) it as a file and moving it to the project folder.

```html
<script src="./cample-html.min.js"></script>
```

If, for some reason, you do not need the minified file, then you can download the full file from this [link](https://unpkg.com/cample-html@0.0.3/dist/cample-html.js).

```html
<script src="./cample-html.js"></script>
```

The non-minified file is larger in size, but it is there as it is with all the formatting.

### CDN

This method involves connecting the file through a third-party resource, which provides the ability to obtain a javascript file from npm via a link.

```html
<script
  src="https://unpkg.com/cample-html@0.0.3"
  integrity="sha384-8mZtz6xTSNyJniv3xBUE6nBw3Sc+4HEyMa0XXfxx4KAKygzKiZizaNMBd0ntLpmL"
  crossorigin="anonymous"
></script>
```

This resource could be unpkg, skypack or other resources. The examples include unpkg simply because it is one of the most popular and its url by characters is not so long.

## Getting started

After installation using any convenient method described in [Installation](/?id=installation), you can start working with the server in the following way:

```html
<div>
  <template data-cample data-src="/api/test" data-method="get"></template>
</div>
```

Or, if the html method is not suitable, then in cample-html there is a `CampleHTML` object that provides a list of functions and methods that allow you to conveniently work with the server. Usage example:

```javascript
const templateFn = CampleHTML.createTemplate(
  `<template data-cample data-src="/api/test" data-method="get"></template>`
);

// (After the response arrives from the server) { element = template (HTMLTemplateElement type), status = 200 }
const elementObj = templateFn({ withCredentials: false, timeout: 0 });
```

These will be the two main ways to interact with the server. In future versions, the functionality will be expanded, but the methods themselves will not change.

## Template

The main tag when working with cample-html is `template`. This tag allows you to post content almost anywhere. This makes it possible to work with a table and other 'specific' tags.

> When working with template, all `script` tags are removed by the module.

To work with template, special attributes are defined that allow the code to access the HTML markup.

Requests to the server are sent as soon as the `document` is loaded.

### data-cample

This attribute is necessary for the module to work. It specifies exactly what `template` will be processed through the module. This allows you to differentiate between a regular `template` tag with similar attributes (`data-src`, for example) and a tag with a modular attribute.

```html
<template data-cample></template>
```

The value for this attribute is usually empty, but in any case, the attribute can take any string as a value.

### data-src

This attribute specifies the url to which the request will be sent.

```html
<template data-src="/api/test"></template>
```

```html
<template data-src="http://localhost:5000/api/test"></template>
```

It is worth considering that if there is no hostname (protocol etc.) in the url, the hostname (protocol etc.) of the address from which the request is sent will be substituted.

### data-method

This attribute specifies the request method that is sent to the server. The default value is the `get` method.

```html
<template data-method="get"></template>
```

Only `post` and `get` methods are currently supported. Other methods will be added in future versions.

### data-after

The data-after property specifies after which event the request will be sent to the server. The value of the property is the string of the following construction `${event}:${selectors}`, where event is the event after which the request will be sent. and selectors are the targets to which event handlers will be assigned

```html
<template data-after="click:.target"></template>
```

The HTML that comes from the server will change to a new one each time in the DOM if events are triggered.

### data-mode

This property specifies the mode of sending requests to the server and replacing HTML in the real DOM. Takes two values `one` and `all`. The first value makes it so that the request to the server will be sent once, and after that all event handlers that were assigned by `selectors` will be removed and the second value, by default, sends a request every time the event is triggered. At the same time, HTML also changes once and changes constantly.

```html
<template data-mode="one"></template>
```

### data-status

The attribute is not set according to the logic of use. It is automatically updated depending on whether a response is received from the server or not.

```html
<template data-status="loading"></template>
```

When a request comes with an error or loading response, the `data-status` attribute is removed from the template, and only then changed to the HTML server template.

## CampleHTML

The main object of the entire module. It includes all the properties and methods that help you work with the server.

This object does not need to be imported. It is assigned to the entire document, so you can immediately use it in your code.

### createTemplate

This function receives a string that contains a template with modular attributes.

```javascript
const templateFn = CampleHTML.createTemplate(
  `<template data-cample data-src="/api/test" data-method="get"></template>`
);
```

This function returns a function that already creates an object with values for template. Function also receives an options object. It is optional.

```javascript
const elementObj = templateFn({
  withCredentials: false,
  timeout: 0,
  requestBody: {},
  headers: {},
});
```

Options object type:

```typescript
{
  requestBody?: Document | XMLHttpRequestBodyInit | null;
  withCredentials?: boolean;
  headers?:{
    [key: string]: string;
  }
  timeout?: number;
}
```

The function returns an object with two values - `element` and `status`:

```javascript
const { status, element } = elementObj;
```

Object type:

```typescript
{
  status: number,
  element: undefined | HTMLTemplateElement
}
```

Values are dynamically assigned to the object depending on the server response.

> The status changes depending on the server response. But, the most important thing is that it is not assigned several times if it is the same. When working with `Proxy` or `Object.defineProperty` or something like that, this will not give the object unnecessary updates!
