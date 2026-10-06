---
layout: default
title: Documentation
description: Documentation on how to get started with Illusion and an overview of available functions, mixins and styling.
---

{% include layout/splitter.md %}

## Getting started

### Step 1

{% highlight bash %}
$ npm install --save-dev github:marsvaardig/illusion normalize.css
{% endhighlight %}

Or with Yarn: `yarn add marsvaardig/illusion normalize.css --dev`. Add a tag to pin a version, for example `github:marsvaardig/illusion#v8.1.3`. Illusion installs from GitHub: the `illusion` package on the npm registry is a different project.

### Step 2

Include [Modernizr](https://modernizr.com/) and at least add `JS - No JS detection` and `flexbox detection` if you're gonna use flexbox.

### Step 3

Create a SCSS file, something like:

{% highlight css %}
// Include Normalize before anything else
@use "node_modules/normalize.css/normalize";

// Load Illusion and pass in your settings
@use "node_modules/illusion/scss/illusion" as * with (
  $illusion-extendalize: true
);

// Start writing code
.foo {
  @include gallery(4);
}
{% endhighlight %}

Still using `@import`? Declare your settings as variables before you import `node_modules/illusion/scss/illusion`.

---

## Recommendations

### Clean CSS

Use [grunt-contrib-cssmin](https://github.com/gruntjs/grunt-contrib-cssmin) to eliminate duplicate code. Duplicate code is generated because Illusion doesn't know whether you already used a specific mixin in an element when you use it again.

### Autoprefixer

Use [autoprefixer](https://github.com/nDmitry/grunt-autoprefixer) to add the browser prefixes you need for your project.

---

## Settings

All settings are `!default` variables. Pass them in the `with (...)` map when you load Illusion with `@use`, or declare them before `@import`. The examples on this page write them as plain variable declarations.

Most mixin arguments take their default from an `$illusion-` variable named after the mixin and the argument, for example `$illusion-span-multiplier` for the `multiplier` of `@mixin span`. [All variables](https://github.com/marsvaardig/illusion/tree/master/scss/tools/variables).

### Grid and breakpoints

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `$alfa`, `$alfa--plus`, `$bravo`, `$charlie`, `$delta`, `$echo` | `320px`, `440px`, `560px`, `768px`, `1024px`, `1112px` | Breakpoints. All except `$alfa` also have a `-min` version that is 1px smaller for max-width media queries, for example `$bravo-min` |
| `$illusion-grid-container` | `12` | Amount of columns |
| `$illusion-grid-maxwidth` | `$echo` | Max width of the content in `@mixin container`. The largest gutter is added on both sides |
| `$illusion-grid-breakpoints` | `default`: `0` / `16px`, `bravo`: `$bravo` / `24px`, `delta`: `$delta` / `32px` | Widths and gutters used by the grid and spacing mixins and functions. Set the width and gutter variables, like `$illusion-grid-bravo-gutter`, or replace the whole map |
| `$illusion-grid-type` | `float` | `float` or `flex`. Default for `$illusion-grid-container-type` and `$illusion-grid-row-type`. `@mixin span` only adds floats and clears when this is `float` |
| `$illusion-breakpoint-width-max` | `false` | Default max-width of `@mixin breakpoint` |
| `$illusion-breakpoint-type` | `inherit` | Unit for `@mixin breakpoint`. `inherit` keeps the unit of the width; `em` or `rem` converts px widths (16px per `em`) |

### Type, spacing and color

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `$font-family` | System font stack | Used by the body styling |
| `$font-size` | `16px` | Used by the body styling and `@mixin fluid-property` |
| `$line-height` | `24px` | Used by the body styling, `@mixin content-block` and the form styling |
| `$root-site-font-size` | `18` | Used by `px()` and `fluid()` to convert design pixels to `rem` |
| `$weight-light`, `$weight-normal`, `$weight-bold` | `300`, `400`, `700` | Font weights |
| `$spacing-xs`, `$spacing-s`, `$spacing-m`, `$spacing-l`, `$spacing-xl` | `4px`, `8px`, `16px`, `24px`, `32px` | Spacing that doesn't grow with the responsive gutters |
| `$color-type`, `$color-error`, `$color-success`, `$color-warning`, `$color-focus`, `$color-form-input` | `#0b0c0c`, `#e74c3c`, `#27ae60`, `#e67e22`, `#febd22`, `#ffffff` | Colors. The base styling and forms use `$color-type`, `$color-focus` and `$color-form-input` |

### Fluid values

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `$illusion-fluid-from` | `$alfa` | Viewport width where `fluid()` starts scaling |
| `$illusion-fluid-to` | `$illusion-grid-maxwidth` + 2 × the largest gutter (`1176px`) | Viewport width where `fluid()` stops scaling: where `@mixin container` stops growing |
| `$illusion-fluid-property-min-value`, `$illusion-fluid-property-max-value`, `$illusion-fluid-property-min-screen`, `$illusion-fluid-property-max-screen` | `$font-size`, `20px`, `$bravo`, `$delta` | Defaults of `@mixin fluid-property` |
| `$illusion-fluid-property-clamp` | `false` | Set to `true` to let `@mixin fluid-property` use `clamp()` |

### Logical properties

{% highlight css %}
$illusion-logical-properties: true;
{% endhighlight %}

Makes `@mixin property` write logical properties, for example `margin-inline-start` instead of `margin-left`. The mixins and base styling that use `@mixin property` follow automatically. It also makes `@mixin cluster` use `gap`, `@mixin fluid-property` output only a `clamp()` and `@mixin ms` output only the custom property version. Which properties are converted is set in [the logical properties maps](https://github.com/marsvaardig/illusion/blob/master/scss/tools/variables/_logical-properties.scss).

### Other settings

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `$illusion-layout-flex-gap` | `false` | Set to `true` to let `@mixin cluster` use `gap` |
| `$illusion-css-variables-prefix` | `''` | Prefix for the `--spacing` custom property that `@mixin ms` uses. For example `il-` gives `--il-spacing` |
| `$illusion-fallback` | `false` | Set to `true` to leave out the custom property version of `@mixin ms`, to test the fallback for older browsers |
| `$illusion-fallback-amount` | `1.5` | Multiplier for the `rem` fallback of `@mixin ms` |
| `$illusion-ms-ratio` | `1.5` | Ratio of the modular scale. The steps `-4` to `4` are stored in `$illusion-ms-sizes`, starting from `$illusion-ms0` (`1`) |
| `$illusion-flexbox` | `true` | Set to `false` to leave out everything inside `@mixin flexbox`, to test the fallback |
| `$illusion-display-warnings` | `true` | Set to `false` to hide the warnings Illusion shows |

---

## Base styling

To stop all the copy pasting in this world we added some base styling in two levels.

### Level 1: Extendalize

The first level we call "Extendalize" and it basically [extends Normalize](https://github.com/marsvaardig/illusion/tree/master/scss/atoms) styling the way we think it should be.

By default the extendalize styling is set to false and can be [configured using variables](https://github.com/marsvaardig/illusion/blob/master/scss/tools/variables/_extendalize.scss).

#### Enable all extendalize features:

{% highlight css %}
$illusion-extendalize: true;
{% endhighlight %}

#### Enable individual extendalize features:

{% highlight css %}
$illusion-extendalize: false;
$illusion-extendalize-box-sizing: true;
$illusion-extendalize-svg: true;
{% endhighlight %}

#### Disable individual extendalize features:

{% highlight css %}
$illusion-extendalize: true;
$illusion-extendalize-image: false;
$illusion-extendalize-paragraph: false;
{% endhighlight %}

#### Extendalize features

Each feature defaults to `default`, which follows `$illusion-extendalize`. `true` turns a feature on even when `$illusion-extendalize` is `false`, and `false` turns it off.

| Variable | Styles |
| -------- | ------ |
| `$illusion-extendalize-box-sizing` | `box-sizing: border-box` on `html`, inherited by all elements |
| `$illusion-extendalize-anchor` | `a`: inherits the color, no underline on hover and focus |
| `$illusion-extendalize-address` | `address`: margins and normal font style |
| `$illusion-extendalize-body` | `body`: margin, font family, size, weight, line height and color |
| `$illusion-extendalize-fieldset` | `fieldset`: full width without border, padding and margin. `legend`: bottom margin |
| `$illusion-extendalize-figure` | `figure`: margins |
| `$illusion-extendalize-heading` | `h1` to `h6`: margins, font size and font weight |
| `$illusion-extendalize-image` | `img`: max width, auto height and vertical alignment |
| `$illusion-extendalize-list` | `ul` and `ol`: margins |
| `$illusion-extendalize-paragraph` | `p`: margins |
| `$illusion-extendalize-picture` | `picture img`: vertical alignment |
| `$illusion-extendalize-svg` | `svg`: a fill of `$illusion-extendalize-svg-color` (`currentColor`, or set a color) and a width and height of `$illusion-extendalize-svg-width` and `-height` (`1em`) when the SVG has no width or height attribute. Set any of these to `false` to leave it out. Also hides `.svg-sprite` |

Every element with base styling gets its margins from its own settings, for example `$illusion-extendalize-paragraph-margin-bottom`. Two settings control the margin at the edge of the parent, so the first or last element adds no space there:

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `$illusion-extendalize-last-children` | `default` | Set to `false` to keep the edge margins. `default` and `true` remove them |
| `$illusion-extendalize-last-children-direction` | `bottom` | `bottom` removes the bottom margin of the last child, `top` the top margin of the first child. Applies to addresses, figures, headings, lists and paragraphs. `legend`, `.form__group` and `.multiple-choice` only have a bottom margin, so they always reset the last child |

### Level 2: Element styling

The second level adds default styling for certain elements. The following elements are available:

#### Body fallback

For browsers that do not support `CSS calc()` there's an option to set the body width to make it look better on those older browsers.

Enable the body fallback styling by setting the following variable:

{% highlight css %}
$illusion-body-fallback: true;
{% endhighlight %}

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `$illusion-body-fallback-align` | `center` | Align the body `left`, `center` or `right` |
| `$illusion-body-fallback-width` | `$bravo` | Width of the body |

[All options](https://github.com/marsvaardig/illusion/blob/master/scss/tools/variables/_body-fallback.scss).

#### Forms

Default [form styling](/examples/#form) is available.

{% highlight css %}
$illusion-form: true;
{% endhighlight %}

This styles `.form`, `.form__group`, `.form__label` and `.form__input`. It also sets a height on all `select` elements and adds button styling to `[type=submit]`. Change the class names with `$illusion-form-selector`, `$illusion-form-group-selector`, `$illusion-form-label-selector` and `$illusion-form-input-selector`.

The custom select and multiple choice styling below are turned on together with `$illusion-form`. Set their variable to `false` to leave them out, or to `true` to use them without the rest of the form styling.

{% highlight css %}
$illusion-form: true;
$illusion-custom-select: false;
$illusion-multiple-choice: false;
{% endhighlight %}

[All options](https://github.com/marsvaardig/illusion/blob/master/scss/tools/variables/_form.scss).

#### Custom select

Styles a native `select` inside a `.custom-select` element and adds a custom arrow. Set `$illusion-custom-select-theme` to `false` to leave out the border, font and focus styling. Change the class name with `$illusion-custom-select-selector`.

#### Multiple choice

Styles radio buttons and checkboxes inside a `.multiple-choice` element with a custom box, dot and checkmark. The `input` needs to be directly followed by its `label`. Change the class name with `$illusion-multiple-choice-selector`.

---

## Mixins

All settings are controlled with variables, see [Settings](#settings).

Illusion comes with a great mixin and function library. All the parameters inside mixins are overwriteable.

{% include documentation/mixins/breakpoint.html %}
{% include documentation/mixins/button.html %}
{% include documentation/mixins/clearfix.html %}
{% include documentation/mixins/cluster.html %}
{% include documentation/mixins/collapse.html %}
{% include documentation/mixins/container.html %}
{% include documentation/mixins/content-block.html %}
{% include documentation/mixins/coverall.html %}
{% include documentation/mixins/flexbox.html %}
{% include documentation/mixins/fluid-property.html %}
{% include documentation/mixins/font-smoothing.html %}
{% include documentation/mixins/gallery.html %}
{% include documentation/mixins/hover.html %}
{% include documentation/mixins/if-breakpoint.html %}
{% include documentation/mixins/js-disabled.html %}
{% include documentation/mixins/js-enabled.html %}
{% include documentation/mixins/modernizr.html %}
{% include documentation/mixins/ms.html %}
{% include documentation/mixins/no-flexbox.html %}
{% include documentation/mixins/property.html %}
{% include documentation/mixins/pseudo.html %}
{% include documentation/mixins/ratio-block.html %}
{% include documentation/mixins/reset.html %}
{% include documentation/mixins/row.html %}
{% include documentation/mixins/selectors.html %}
{% include documentation/mixins/shift.html %}
{% include documentation/mixins/spacing.html %}
{% include documentation/mixins/span.html %}
{% include documentation/mixins/stack.html %}
{% include documentation/mixins/svg-background.html %}
{% include documentation/mixins/svg-mask.html %}
{% include documentation/mixins/transition.html %}
{% include documentation/mixins/triangle.html %}
{% include documentation/mixins/visually-hidden.html %}
{% include documentation/mixins/visually-shown.html %}

---

## Functions

{% include documentation/functions/calcInterpolation.html %}
{% include documentation/functions/calculateRatio.html %}
{% include documentation/functions/fluid.html %}
{% include documentation/functions/getMargin.html %}
{% include documentation/functions/getWidth.html %}
{% include documentation/functions/mapCollect.html %}
{% include documentation/functions/mapDeepGet.html %}
{% include documentation/functions/mapGetNext.html %}
{% include documentation/functions/ms.html %}
{% include documentation/functions/nonDestructiveMapMerge.html %}
{% include documentation/functions/px.html %}
{% include documentation/functions/spacing.html %}
{% include documentation/functions/stripUnit.html %}
{% include documentation/functions/svgUrl.html %}
