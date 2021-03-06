# vue-fontawesome

[![npm](https://img.shields.io/npm/v/@fortawesome/vue-fontawesome.svg?style=flat-square)](https://www.npmjs.com/package/@fortawesome/vue-fontawesome)
[![Travis-CI](https://img.shields.io/travis/FortAwesome/vue-fontawesome/master.svg)](https://travis-ci.org/FortAwesome/vue-fontawesome)

> Font Awesome 5 Vue component using SVG with JS

<!-- toc -->

- [Introduction](#introduction)
    + [Upgrading Font Awesome?](#upgrading-font-awesome)
    + [Get started](#get-started)
    + [Learn about our new SVG implementation](#learn-about-our-new-svg-implementation)
    + [Going from 0.0.x to 0.1.0](#going-from-00x-to-010)
- [Installation](#installation)
- [Add more styles or Pro icons](#add-more-styles-or-pro-icons)
- [Usage](#usage)
  * [Recommended](#recommended)
    + [Quick warning about self-closing tags](#quick-warning-about-self-closing-tags)
    + [Processing i tags into svg using Font Awesome](#processing-i-tags-into-svg-using-font-awesome)
  * [The icon property](#the-icon-property)
    + [Shorthand that assumes a prefix of "fas"](#shorthand-that-assumes-a-prefix-of-fas)
    + [Explicit prefix (note the Vue bind shorthand because this uses an array)](#explicit-prefix-note-the-vue-bind-shorthand-because-this-uses-an-array)
    + [Explicit icon definition through something like a computed property](#explicit-icon-definition-through-something-like-a-computed-property)
  * [Alternative using component property](#alternative-using-component-property)
  * [Why use the concept of a library?](#why-use-the-concept-of-a-library)
    + [Import the specific icons that you need](#import-the-specific-icons-that-you-need)
    + [Import entire styles](#import-entire-styles)
  * [Tree shaking alternative](#tree-shaking-alternative)
- [Features](#features)
  * [Register your components first](#register-your-components-first)
  * [Basic](#basic)
  * [Advanced](#advanced)

<!-- tocstop -->

## Introduction

Hey there! We're glad you're here...

#### Upgrading Font Awesome?

If you've used Font Awesome in the past (version 4 or older) there are some
things that you should learn before you dive in.

> https://fontawesome.com/how-to-use/upgrading-from-4

#### Get started

This package is for integrating with Vue.js. If you aren't using Vue then it's
not going to help you. Head over to our "Get Started" page for some guidance.

> https://fontawesome.com/get-started

#### Learn about our new SVG implementation

This package, under the hood, uses SVG with JS and the `@fortawesome/fontawesome-svg-core` library. This implementation differs drastically from
the web fonts implementation that was used in version 4 and older of Font Awesome. You might head over there to learn about how it works.

> https://fontawesome.com/how-to-use/svg-with-js

#### Going from 0.0.x to 0.1.0

See [UPGRADING.md](./UPGRADING.md).

You might also be interested in the larger umbrella project [UPGRADING.md](https://github.com/FortAwesome/Font-Awesome/blob/master/UPGRADING.md)

## Installation

```
$ npm i --save @fortawesome/fontawesome-svg-core@prerelease
$ npm i --save @fortawesome/free-solid-svg-icons@prerelease
$ npm i --save @fortawesome/vue-fontawesome@prerelease
```

## Add more styles or Pro icons

Brands are separated into their own style and for customers upgrading from
version 4 to 5 we have a limited number of Regular icons available.

**Visit [fontawesome.com/icons](https://fontawesome.com/icons) to search for free and Pro icons**

```
$ npm i --save @fortawesome/free-brands-svg-icons@prerelease
$ npm i --save @fortawesome/free-regular-svg-icons@prerelease
```

If you are a [Font Awesome Pro](https://fontawesome.com/pro) subscriber you can install Pro packages.

```
$ npm i --save @fortawesome/pro-solid-svg-icons@prerelease
$ npm i --save @fortawesome/pro-regular-svg-icons@prerelease
$ npm i --save @fortawesome/pro-light-svg-icons@prerelease
```

Using the Pro packages requires [additional configuration](https://fontawesome.com/how-to-use/js-component-packages).

Or with Yarn:

```
$ yarn add @fortawesome/fontawesome-svg-core@prerelease
$ yarn add @fortawesome/free-solid-svg-icons@prerelease
$ yarn add @fortawesome/vue-fontawesome@prerelease
```

## Usage

### Recommended

The following examples are based on a project configured with [vue-cli](https://github.com/vuejs/vue-cli).

`src/main.js`

```javascript
import Vue from 'vue'
import App from './App'
import { library } from '@fortawesome/fontawesome-svg-core'
import { faCoffee } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faCoffee)

Vue.component('font-awesome-icon', FontAwesomeIcon)

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  components: { App },
  template: '<App/>'
})
```

`src/App.js`

```javascript
<template>
  <div id="app">
    <font-awesome-icon icon="coffee" />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>
```

#### Quick warning about self-closing tags

If you are using inline templates or HTML as templates you need to be careful to avoid self-closing tags.

See [this issue on the Vue.js project](https://github.com/vuejs/vue/issues/1036)

If you are writing these types of templates make sure and use valid HTML syntax:

```html
<font-awesome-icon icon="coffee"></font-awesome-icon>
```

#### Processing i tags into svg using Font Awesome

A basic installation of [Font Awesome](https://fontawesome.com/get-started) has
the ability to automatically transform `<i class="fas fa-coffee"></i>` into
`<svg class="...">...</svg>` icons. This technology works with the browser's
DOM, [`requestAnimationFrame`][raf], and [`MutationObserver`][mo].

When using the `@fortawesome/fontawesome-svg-core` package this **behavior is
disabled by default**. This project uses that package so you will have to
explicitly enable it like this:

```javascript
import { dom } from '@fortawesome/fontawesome-svg-core'

dom.watch() // This will kick of the initial replacement of i to svg tags and configure a MutationObserver
```

[raf]: https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame
[mo]: https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver

### The icon property

The `icon` property of the `FontAwesomeIcon` component can be used in the following way:

#### Shorthand that assumes a prefix of "fas"

```javascript
<font-awesome-icon icon="spinner" />
<font-awesome-icon :icon="['fas', 'spinner']" /> # Same as above
```

For the above to work you must add the `spinner` icon to the library.

```javascript
import { library } from '@fortawesome/fontawesome-svg-core'
import { faSpinner } from '@fortawesome/free-solid-svg-icons'

library.add(faSpinner)
```

#### Explicit prefix (note the Vue bind shorthand because this uses an array)

```javascript
<font-awesome-icon :icon="['far', 'spinner']" />
```

For the above to work you must add the regular `spinner` icon (Pro only) to the library.

```javascript
import { library } from '@fortawesome/fontawesome-svg-core'
import { faSpinner } from '@fortawesome/pro-regular-svg-icons'

library.add(faSpinner)
```

#### Explicit icon definition through something like a computed property

```javascript
<template>
  <div id="app">
    <font-awesome-icon icon="appIcon" />
  </div>
</template>

<script>
import { faChessQueen } from '@fortawesome/free-solid-svg-icons'

export default {
  name: 'App',

  computed: {
    appIcon () {
      return faChessQueen
    }
  }
}
</script>
```

### Alternative using component property

With Vue you can tell your component to resolve other component explicitly.

```javascript
<template>
  <div>
    <font-awesome-icon :icon="myIcon" />
  </div>
</template>

<script>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faSpinner } from '@fortawesome/free-solid-svg-icons'

export default {
  name: 'MyComponent',

  data () {
    return {
      myIcon: faSpinner
    }
  },

  components: {
    FontAwesomeIcon
  }
}
</script>
```

### Why use the concept of a library?

Explicitly selecting icons offer the advantage of only bundling the icons that you
use in your final bundled file. This allows us to subset Font Awesome's
thousands of icons to just the small number that are normally used.

#### Import the specific icons that you need

```javascript
import Vue from 'vue'
import Main from './Main.vue'
import fontawesome from '@fortawesome/fontawesome'
import brands from '@fortawesome/fontawesome-free-brands'
import faSpinner from '@fortawesome/fontawesome-free-solid/faSpinner'

import { library } from '@fortawesome/fontawesome-svg-core'
import { faCoffee } from '@fortawesome/free-solid-svg-icons'
import { faSpinner } from '@fortawesome/pro-light-svg-icons'

library.add(faCoffee, fab, faSpinner)
```

#### Import entire styles

```javascript
import { fab } from '@fortawesome/free-brands-svg-icons'

<script>
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
export default {
  name: 'FAExample',

library.add(fab)
```

This will add the _entire brands style to your library_. Be careful with this
approach as it may be convenient in the beginning but your bundle size will be
large. We **highly** recommend that you take advantage of subsetting through tree shaking.

### Tree shaking alternative

Keeping the bundles small when using `import { faCoffee }` relies on
[tree-shaking](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking).
If you are not using a tool that supports tree shaking **you may end up bundling more
icons than you intend**. Here are some alternative import syntaxes:

```javascript
import { library } from '@fortawesome/fontawesome-svg-core'
import faCoffee from '@fortawesome/free-solid-svg-icons/faCoffee'
import faSpinner from '@fortawesome/pro-light-svg-icons/faSpinner'

library.add(faCoffee, faSpinner)
```

How does this work? We have individual icon files like
`node_modules/@fortawesome/free-solid-svg-icons/faCoffee.js` that contain just
that specific icon.

## Features

The following features are available as [part of Font Awesome](https://fontawesome.com/how-to-use/svg-with-js).

### Register your components first

To use the following examples you must first register your component so Vue is aware of it.

A good place to do this is in `main.js` or in the module you are calling `new
Vue()`. **Make sure you register the component** and **have added icons to your
library** before you bootstrap your Vue application.

```
import Vue from 'vue'
import { FontAwesomeIcon, FontAwesomeLayers, FontAwesomeLayersText } from 'vue-fontawesome'

Vue.component('font-awesome-icon', FontAwesomeIcon)
Vue.component('font-awesome-layers', FontAwesomeLayers)
Vue.component('font-awesome-layers-text', FontAwesomeLayersText)
```

### Basic

Spin and pulse animation:

```html
<font-awesome-icon icon="spinner" spin />
<font-awesome-icon icon="spinner" pulse />
```

Fixed width:

```html
<font-awesome-icon icon="spinner" fixed-width />
```

Border:

```html
<font-awesome-icon icon="spinner" border />
```

Flip horizontally, vertically, or both:

```html
<font-awesome-icon icon="spinner" flip="horizontal" />
<font-awesome-icon icon="spinner" flip="vertical" />
<font-awesome-icon icon="spinner" flip="both" />
```

Size:

```html
<font-awesome-icon icon="spinner" size="xs" />
<font-awesome-icon icon="spinner" size="lg" />
<font-awesome-icon icon="spinner" size="6x" />
```

Rotate:

```html
<font-awesome-icon icon="spinner" rotation="90" />
<font-awesome-icon icon="spinner" rotation="180" />
<font-awesome-icon icon="spinner" rotation="270" />
```

Pull left or right:

```html
<font-awesome-icon icon="spinner" pull="left" />
<font-awesome-icon icon="spinner" pull="right" />
```

### Advanced

Power Transforms:

```html
<font-awesome-icon icon="spinner" transform="shrink-6 left-4" />
<font-awesome-icon icon="spinner" :transform="{ rotate: 42 }" />
```

Masking:

```html
<font-awesome-icon icon="coffee" :mask="['far', 'circle']" />
```

Symbols:

```html
<font-awesome-icon icon="edit" symbol />
<font-awesome-icon icon="edit" symbol="edit-icon" />
```

Layers:

```html
<font-awesome-layers class="fa-lg">
  <font-awesome-icon icon="circle" />
  <font-awesome-icon icon="check" transform="shrink-6" style="color: white;" />
</font-awesome-layers>
```

Layers text:

```html
<font-awesome-layers full-width class="fa-4x">
  <font-awesome-icon icon="queen"/>
  <font-awesome-layers-text class="gray8" transform="down-2 shrink-8" value="Q" />
</font-awesome-layers>
```
