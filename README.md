# RFS [![npm][npm-image]][npm-url]

RFS (simply the abbreviation for Responsive Font Size) is an font size engine which **automatically calculates the appropriate font size** based on the dimensions of the browser viewport. It's available in one of your favourite preprocessors or postprocessor: [Sass](https://sass-lang.com/), [Less](http://lesscss.org/), [Stylus](http://stylus-lang.com/) or [PostCSS](https://postcss.org/).


## Advantages

- Font sizes will **rescale for every screen or device**, this prevents long words from being chopped off the viewport on small devices
- RFS will prevent the font size from rescaling too small so readability can be assured
- **Super easy** to use, just use the `font-size` mixin (or `responsive-font-size` property for PostCSS) instead of the `font-size` property 
- The font sizes of all text elements will always remain in relation with each other


![RFS](http://i.imgur.com/gJH6m6g.gif)


## Installation

RFS can be installed using a package manager (recommended):

- npm: `npm install rfs`
- yarn: `yarn add rfs`
- bower (deprecated): `bower install rfs --save`

**Copy/paste (not recommended):**

The source files can also be downloaded manually and used in a project. This method is not recommended because you
lose the ability to easily and quickly manage and update RFS as a dependency.


## Usage

In the following examples, this folder structure is assumed (you will probably just use one pre/postprocessor):

```text
project/
├── postccss/
│   └── main.css
├── less/
│   └── main.less
├── node_modules/
│   └── rfs
│        └── ...
├── sass/
│   └── main.sass
├── scss/
│   └── main.scss
└── stylus/
    └── main.styl
```


### Sass

```scss
// .scss syntax:
// scss/main.scss

@import "../node_modules/rfs/sass/rfs";

.title {
  @include responsive-font-size(4rem); // OR @include responsive-font-size(64px); OR @include rfs(64);
}
```
```sass
// .sass syntax:
// scss/main.scss

@import "../node_modules/rfs/sass/rfs";

.title
  +responsive-font-size(4rem) // OR +responsive-font-size(64px) OR +rfs(64)
```


### PostCSS

```postcss
// postcss/main.css

.title {
  responsive-font-size: 4rem; // OR responsive-font-size: 64px; OR rfs: 64;
}

// Handle postcss afterwards (see examples folder for PostCSS example)
```


### Less

```less
// less/main.less

@import "../node_modules/rfs/less/rfs";

.title {
  .responsive-font-size(4rem); // OR .responsive-font-size(64px); OR .rfs(64);
}
```


### Stylus

```stylus
// stylus/main.styl

@import "../node_modules/rfs/stylus/rfs";

.title
  responsive-font-size(4rem) // OR responsive-font-size(64px) OR rfs(64)
```


### Generated css

```css
.title {
  font-size: 4rem;
}

@media (max-width: 1200px) {
  .title {
    font-size: calc(1.525rem + 3.3vw);
  }
}
```


## Visualisation

If you wonder how the font sizes are rescaled, wonder no more and stare at this graph which might clarify things a bit. The font sizes used on the graph are in `px`, but in reality RFS renders them in `rem` by default:

![RFS visualisation](https://i.imgur.com/P2ixaKH.png)


## Configuration

RFS works out of the box without any configuration tweaks, but if you feel the urge to go loco and fine tune the way font sizes are rescaled, you can:


### Base font size <sub><sup>(unit in `px` or `rem`)</sup></sub>

* SCSS, Sass & Stylus: `$rfs-base-font-size`
* Less: `@rfs-base-font-size`
* PostCSS: `baseFontSize`

The option will prevent the font size from becoming too small on smaller screens. If the font size which is passed to RFS is smaller than this base font size, no fluid font rescaling will take place.

*Default value: `1.25rem`*


### Font size unit <sub><sup>(`px` or `rem`)</sup></sub>

* SCSS, Sass & Stylus: `$rfs-font-size-unit`
* Less: `@rfs-font-size-unit`
* PostCSS: `fontSizeUnit`

The output font size will be rendered in this unit. Setting it in `px` will disable the ability for users to change the the font size in their browser.

*Default value: `rem`*


### Breakpoint <sub><sup>(in `px`, `em` or `rem`)</sup></sub>

* SCSS, Sass & Stylus: `$rfs-breakpoint`
* Less: `@rfs-breakpoint`
* PostCSS: `breakpoint`

Above this breakpoint, the font size will be equal to the font size you passed to RFS; below the breakpoint, the font size will dynamically scale.

*Default value: `1200px`*


### Breakpoint unit <sub><sup>(`px`, `em` or `rem`)</sup></sub>

* SCSS, Sass & Stylus: `$rfs-breakpoint-unit`
* Less: `@rfs-breakpoint-unit`
* PostCSS: `breakpointUnit`

The width of the max width in the media query will be rendered in this unit.

*Default value: `px`*


### Factor <sub><sup>(number)</sup></sub>

* SCSS, Sass & Stylus: `$rfs-factor`
* Less: `@rfs-factor`
* PostCSS: `factor`

This value determines the strength of font size resizing. The higher the factor, the less difference there is between font sizes on small screens. The lower the factor, the less influence RFS has, which results in bigger font sizes for small screens. The factor must me greater than 1.

*Default value: `10`*


### Two dimensional <sub><sup>(boolean)</sup></sub>

* SCSS, Sass & Stylus: `$rfs-two-dimensional`
* Less: `@rfs-two-dimensional`
* PostCSS: `twoDimensional`

Enabling the two dimensional media queries will determine the font size based on the smallest side of the screen with `vmin`. This prevents the font size from changing if the device toggles between portrait and landscape mode.

*Default value: `false`*


### Class <sub><sup>(boolean)</sup></sub>

* SCSS, Sass & Stylus: `$rfs-class`
* Less: `@rfs-class`
* PostCSS: `class`

RFS can be enabled or disabled with a class. There are 3 options:

- `false`
  No extra classes are generated.
- `disable`
  When the the disable classes are generated you can add the `.disable-responsive-font-size` class to an element to disable responsive font sizes for the element and its child elements.
- `enable`
  RFS is disabled by default in this case. The `.enable-responsive-font-size` class can be added to an element to enable responsive font sizes for the element and its child elements.

*Default value: `false`*


### Safari iframe resize bug fix <sub><sup>(boolean)</sup></sub>

* SCSS, Sass & Stylus: `$rfs-safari-iframe-resize-bug-fix`
* Less: `@rfs-safari-iframe-resize-bug-fix`
* PostCSS: `safariIframeResizeBugFix`

Safari doesn't resize its font size in an iframe if the iframe is resized. To fix this `min-width: 0vw` can be added and that's what happens if this option is enabled. See [#14](https://github.com/twbs/rfs/issues/14).

*Default value: `false`*


## !important

By setting a second parameter to true, `!important` is added after the font-size value. (Example is in `scss`)

```scss
.label {
  @include responsive-font-size(2.5rem, true);
}
```

CSS:

```css
.label {
  font-size: 2.5rem !important;
}

@media (max-width: 1200px) {
  .label {
    font-size: calc(1.375rem + 1.5vw) !important;
  }
}
```


## Best practices

- Don't set RFS on the `html` element, because this influences the value of `rem` and could lead to unexpected results.
- Always set your line-heights relative (in `em` or unitless) to prevent interline issues.


## Demos

- [Simple Codepen Demo](https://codepen.io/MartijnCuppens/pen/ZBjdMy)
- [RFS in bootstrap demo](https://martijncuppens.github.io/rfs)


## Creator

**Martijn Cuppens**

* <https://twitter.com/Martijn_Cuppens>
* <https://github.com/MartijnCuppens>


## Copyright and license

Code released under [the MIT license](https://github.com/twbs/rfs/blob/master/LICENSE).


[npm-image]: https://img.shields.io/npm/v/rfs.svg
[npm-url]: https://npmjs.org/package/rfs
