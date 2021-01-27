[![WTT logo](docs/wtt-logo.png)](https://www.wundermanthompson.com)

[![Gradle Status](https://gradleupdate.appspot.com/Cognifide/gradle-lighthouse-plugin/status.svg?random=123)](https://gradleupdate.appspot.com/Cognifide/gradle-lighthouse-plugin/status)
![Travis Build](https://travis-ci.org/Cognifide/gradle-lighthouse-plugin.svg?branch=master)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

# Gradle Lighthouse Plugin

Runs [Lighthouse](https://developers.google.com/web/tools/lighthouse) tests on multiple sites / web pages with checking thresholds (useful on continuous integration, constant performance checking).

[![Lighthouse Logo](docs/lighthouse-logo.png)](https://developers.google.com/web/tools/lighthouse)

## Installation

```kotlin
plugins {
    id("com.cognifide.lighthouse") version "1.0.0"
}
```

## Configuration

Plugin organizes multiple sites to be tested into suites. Any web page could be checked with different threshold and Lighthouse configuration (e.g using [performance budgets](https://developers.google.com/web/tools/lighthouse/audits/budgets)).
Suites need to be defined in file with following format:

*lighthouse/suites.json* (plugin specific format)
```json
{
  "suites": [
    {
      "name": "site.demo",
      "default": true,
      "baseUrl": "http://demo.example.com",
      "paths": [
        "/en-us.html",
        "/en-gb.html"
      ],
      "args": [
        "--config-path=lighthouse/config.json",
        "--performance=90",
        "--accessibility=90",
        "--best-practices=80",
        "--seo=60",
        "--pwa=30"
      ]
    },
    {
      "name": "site.live",
      "baseUrl": "http://example.com",
      "paths": [
        "/en-us.html"
      ],
      "args": [
        "--config-path=lighthouse/config.json",
        "--performance=90",
        "--accessibility=90",
        "--best-practices=80",
        "--seo=60",
        "--pwa=30"
      ]
    }
  ]
}
```

When using argument `--config-path` then it is also needed to have at least file:

*lighthouse/config.json* ([documentation](https://github.com/GoogleChrome/lighthouse/blob/master/docs/configuration.md#config-extension))
```json
{
  "extends": "lighthouse:default"
}
```

## Running

Underneath, Gradle Lighthouse Plugin is running [Lighthouse CI](https://www.npmjs.com/package/lighthouse-ci) multiple times in case of test suites defined (support for multiple paths under same base URL). 

Available options:

* run default suite(s): `sh gradlew lighthouseRun`,
* run only desired suite(s) by name pattern(s): `sh gradlew lighthouseRun -Plighthouse.suite=site.demo` (if suites by name not found, default suites will be used),
* run only desired suite(s) by base URL: `sh gradlew lighthouseRun -Plighthouse.baseUrl=http://example.com` (if suites by base URL not found, default suites will be used),
* run only desired suite(s) with customized base URL: `sh gradlew lighthouseRun -Plighthouse.baseUrl=http://any-host.com -Plighthouse.suite=site.live`.

Note that after running one of above commands first time, new files might be generated:

* *package.json*
* *yarn.lock*

This is indented behavior - __remember to save these files in VCS__.
If Lighthouse CI tool version need to be upgraded, just correct *package.json* file.

As a build result, reports under directory *build/lighthouse* will be generated:

![Lighthouse Build Files](docs/lighthouse-build-output.png)

Screenshot of sample report below:

![Lighthouse Report](docs/lighthouse-report.png)

## Building

1. Clone this project using command `git clone https://github.com/wttech/gradle-lighthouse-plugin.git`
2. To build plugin, simply enter cloned directory run command: `gradlew`
3. To debug built plugin:
    * Append to build command parameters `--no-daemon -Dorg.gradle.debug=true`
    * Run build, it will suspend, then connect remote at port 5005 by using IDE
    * Build will proceed and stop at previously set up breakpoint.

## Contributing

Issues reported or pull requests created will be very appreciated. 

1. Fork plugin source code using a dedicated GitHub button.
2. Do code changes on a feature branch created from *develop* branch.
3. Create a pull request with a base of *develop* branch.

## License

**Gradle Lighthouse Plugin** is licensed under the [Apache License, Version 2.0 (the "License")](https://www.apache.org/licenses/LICENSE-2.0.txt)
