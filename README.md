
# ember-cli-sass-variables-export


[![GitHub license](https://img.shields.io/github/license/minusfive/ember-cli-sass-variables-export.svg)](https://github.com/minusfive/ember-cli-sass-variables-export/blob/master/LICENSE.md)
[![Ember Observer Score](https://emberobserver.com/badges/ember-cli-sass-variables-export.svg)](https://emberobserver.com/addons/ember-cli-sass-variables-export)
[![npm version](https://badge.fury.io/js/ember-cli-sass-variables-export.svg)](https://badge.fury.io/js/ember-cli-sass-variables-export)
[![Build Status](https://travis-ci.org/minusfive/ember-cli-sass-variables-export.svg?branch=master)](https://travis-ci.org/minusfive/ember-cli-sass-variables-export)

Export Sass `$variable` values as JSON data to [Ember Utilities](https://guides.emberjs.com/v3.1.0/tutorial/service/#toc_accessing-the-google-maps-api-with-a-utility), so they can be consumed from the rest of your app. The idea is to help you step closer to an SSoT for style-related data/documentation (e.g. populating styling documentation with [Ember Freestyle](http://ember-freestyle.com/)).

This addon is meant to be used with ember-cli-sass. Works with both dart sass and node-sass implementations. Check [ember-cli-sass docs](https://github.com/aexmachina/ember-cli-sass) for more information on how to provide your preferred implementation.

## Installation
`ember i ember-cli-sass-variables-export`

## Usage

### 1. Use The `export` Sass Function

This addon makes an `export` Sass function available in your app/addon. Use it to export the value of a `$variable` to an Ember Utility file (as JSON), like so:

```scss
/**
@function export
  @param  {string}  fileName  - Ember Utility file name for output
  @param  {*any*}   value     - $variable value to be exported
  @returns value              - Returns the untouched value you passed
*/
$my-sass-variable: export('util-file-name', $value);
```

The **first argument** is a `string`, used as the _name for the utility file_ your variable value will be exported to.

The **second argument** is the _value_ for the `$variable`. All [Sass Data Types](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#data_types) are [should be?] supported, (i.e. **lists**, [nested] **maps**, **colors**, etc.), since node-sass' native functions are used to parse the data.

**IMPORTANT:** _Make sure all your **map keys** are **`'quoted'`**. This is just good practice and will save you a ton of headaches._

### 2. Import Generated Ember Utility

Import the generated utility from your components, controllers, etc.

```js
import mySassVar from 'ember-cli-sass-variables-export/utils/util-file-name';
```

### 3. Profit!

## Examples

Simple example showing how to export color palettes defined in your Sass:

```scss
// app/styles/settings/colors.scss
$color-palette: (
  'primary': (
    'base': $color-brand--primary,
    'light': mix($color-white, $color-brand--primary, 33%),
    'dark': mix($color-black, $color-brand--primary, 33%),
    'accent': $color-brand--primary--accent,
    'trans': transparentize($ivb-color-brand--primary, 0.50)
  ),
  'secondary': (
    'base': $color-brand--secondary,
    'light': mix($color-white, $color-brand--secondary, 33%),
    'dark': mix($color-black, $color-brand--secondary, 33%),
    'accent': $color-brand--secondary--accent
  )
);
$color-palette-export: export('color-palette', $color-palette);
```

```js
// /app/utils/color-palette.js
// Generated Ember Utility output
exports default = {
  "primary": {
    "base": "rgb(161, 148, 213)",
    "light": "rgb(191, 182, 226)",
    "dark": "rgb(113, 104, 148)",
    "accent": "rgb(161, 148, 213)",
    "trans": "rgba(161, 148, 213, 0.50)"
  },
  "secondary": {
    "base": "rgb(242, 135, 97)",
    "light": "rgb(245, 174, 148)",
    "dark": "rgb(168, 96, 70)",
    "accent": "rgb(242, 135, 97)"
  }
};
```

```js
// app/components/my-component.js
import Component from '@ember/component';
import colorPalette from 'ember-cli-sass-variables-export/utils/color-palette';

export default Component.extend({
  colorPalette
});
```

[//]: # ({% raw %})
```hbs
<!-- app/templates/components/my-component.js  -->
{{#each-in colorPalette as |color shade|}}
  {{#each-in shade as |shadeKey shadeColor|}}
    <div style="background-color: {{shadeColor}}">
      {{color}}--{{shade}}
      ({{shadeColor}})
    </div>
  {{/each-in}}
{{/each-in}}
```
[//]: # ({% endraw %})

## Bugs

Please report any bugs [through issues](https://github.com/minusfive/ember-cli-sass-variables-export/issues).

### Known Issues

* Double build initially triggered, believe caused by initial JS variables files generation after initial Sass parsing/project build, which makes the watch process trigger a new build. This hasn't caused a problem yet, but open to PRs to correct it.

## Contributing

### Installation

* `git clone <repository-url>`
* `cd ember-cli-sass-variables-export`
* `yarn install`

### Linting

* `yarn lint:js`
* `yarn lint:js --fix`

### Running tests

* `yarn test` – Runs the test suite on the current Ember version
* `yarn test:all` – Runs the test suite against multiple Ember versions

## License

This project is licensed under the [MIT License](LICENSE.md).

## Credits

Forked from https://github.com/mikemunsie/ember-export-sass-variables (no longer maintained).

#### Original Project Credits:

- https://github.com/Punk-UnDeaD/node-sass-export
- https://github.com/davidpett/ember-cli-sass-variables
