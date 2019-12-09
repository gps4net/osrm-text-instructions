[<img src="https://travis-ci.org/Project-OSRM/osrm-text-instructions.svg?branch=master" align="right" alt="Build status">](https://travis-ci.org/Project-OSRM/osrm-text-instructions)

# OSRM Text Instructions

OSRM Text Instructions is a Node.js library that transforms route data generated by [OSRM](http://www.project-osrm.org/) into localized turn instructions to be displayed visually or read aloud by a text-to-speech engine. OSRM Text Instructions is the basis of guidance instructions in [osrm-frontend](https://github.com/Project-OSRM/osrm-frontend/), the [Mapbox Directions API](https://www.mapbox.com/api-documentation/#directions), and the [Mapbox Navigation SDK](https://www.mapbox.com/navigation-sdk/).

* **Global**: Text instructions are available in over a dozen languages via [Transifex](https://www.transifex.com/project-osrm/osrm-text-instructions/). [Abbreviations](languages/abbreviations/README.md) and [advanced grammatical transformations](languages/grammar/README.md) are available in some languages.
* **Customizable**: Flexible options allow you to format and tweak the results to your liking.
* **Cross-platform**: A data-driven approach facilitates implementations in other programming languages. OSRM Text Instructions is also available [in Swift and Objective-C](https://github.com/Project-OSRM/osrm-text-instructions.swift/) (for iOS, macOS, tvOS, and watchOS) and [in Java](https://github.com/Project-OSRM/osrm-text-instructions.java/) (for Android and Java SE).
* **Well-tested**: A data-driven test suite ensures compatibility across languages and platforms.

## Usage

[![NPM](https://nodei.co/npm/osrm-text-instructions.png)](https://npmjs.org/package/osrm-text-instructions/)

```js
var version = 'v5';
var osrmTextInstructions = require('osrm-text-instructions')(version);

response.legs.forEach(function(leg) {
  leg.steps.forEach(function(step) {
    instruction = osrmTextInstructions.compile('en', step, options)
  });
});
```

If you are unsure if the user's locale is supported by osrm-text-inustrctions, use [@mapbox/locale-utils](https://github.com/mapbox/locale-utils) for finding the best fitting language.

#### Parameters `require('osrm-text-instructions')(version)`

parameter | required? | values | description
---|----|----|---
`version` | required | `v5` | Major OSRM version

#### Parameters `compile(language, step, options)`

parameter | required? | values | description
---|----|----|---
`language` | required | `en` `de` `zh-Hans` `fr` `nl` `ru` [and more](https://github.com/Project-OSRM/osrm-text-instructions/tree/master/languages/translations/) | Compiling instructions for the selected language code.
`step` | required | [OSRM route step object](https://github.com/Project-OSRM/osrm-backend/blob/master/docs/http.md#routestep-object) | The RouteStep as it comes out of OSRM
`options` | optional | Object | See [below](#options)

##### Options

key | type | description
----|----|----
`legCount` | integer | Number of legs in the route
`legIndex` | integer | Zero-based index of the leg containing the step; together with `legCount`, this option determines which waypoint the user has arrived at
`formatToken` | function | Function that formats the given token value after grammaticalization and capitalization but before the value is inserted into the instruction string; useful for wrapping tokens in markup
`waypointName` | string | Optional custom name for the leg's destination, replaces `"your {nth} destination"`

`formatToken` takes two parameters:

* `token`: A string that indicates the kind of token, such as `way_name` or `direction`
* `value`: A grammatical string for this token, capitalized if the token appears at the beginning of the instruction

and returns a string.

## Architecture

* index.js contains the main transformation logic in JavaScript.
* languages/ contains the localization files, including raw format strings, abbreviation files, and grammar rules.
* languages.js loads the localizations and contains some localization-related helper functions.
* test/ contains data-driven integration tests and test fixtures for all supported languages.

## Contributing

We welcome feedback, code contributions, and translations! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

[![Transifex](https://www.transifex.com/projects/p/osrm-text-instructions/resource/enjson/chart/image_png)](https://www.transifex.com/project-osrm/osrm-text-instructions/)
