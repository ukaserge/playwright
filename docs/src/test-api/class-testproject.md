# class: TestProject
* since: v1.10
* langs: js

Playwright Test supports running multiple test projects at the same time. This is useful for running tests in multiple configurations. For example, consider running tests against multiple browsers.

`TestProject` encapsulates configuration specific to a single project. Projects are configured in [`property: TestConfig.projects`] specified in the [configuration file](../test-configuration.md). Note that all properties of [TestProject] are available in the top-level [TestConfig], in which case they are shared between all projects.

Here is an example configuration that runs every test in Chromium, Firefox and WebKit, both Desktop and Mobile versions.

```js tab=js-js
// playwright.config.js
// @ts-check
const { devices } = require('@playwright/test');

/** @type {import('@playwright/test').PlaywrightTestConfig} */
const config = {
  // Options shared for all projects.
  timeout: 30000,
  use: {
    ignoreHTTPSErrors: true,
  },

  // Options specific to each project.
  projects: [
    {
      name: 'Desktop Chromium',
      use: {
        browserName: 'chromium',
        viewport: { width: 1280, height: 720 },
      },
    },
    {
      name: 'Desktop Safari',
      use: {
        browserName: 'webkit',
        viewport: { width: 1280, height: 720 },
      }
    },
    {
      name: 'Desktop Firefox',
      use: {
        browserName: 'firefox',
        viewport: { width: 1280, height: 720 },
      }
    },
    {
      name: 'Mobile Chrome',
      use: devices['Pixel 5'],
    },
    {
      name: 'Mobile Safari',
      use: devices['iPhone 12'],
    },
  ],
};

module.exports = config;
```

```js tab=js-ts
// playwright.config.ts
import { type PlaywrightTestConfig, devices } from '@playwright/test';

const config: PlaywrightTestConfig = {
  // Options shared for all projects.
  timeout: 30000,
  use: {
    ignoreHTTPSErrors: true,
  },

  // Options specific to each project.
  projects: [
    {
      name: 'Desktop Chromium',
      use: {
        browserName: 'chromium',
        viewport: { width: 1280, height: 720 },
      },
    },
    {
      name: 'Desktop Safari',
      use: {
        browserName: 'webkit',
        viewport: { width: 1280, height: 720 },
      }
    },
    {
      name: 'Desktop Firefox',
      use: {
        browserName: 'firefox',
        viewport: { width: 1280, height: 720 },
      }
    },
    {
      name: 'Mobile Chrome',
      use: devices['Pixel 5'],
    },
    {
      name: 'Mobile Safari',
      use: devices['iPhone 12'],
    },
  ],
};
export default config;
```

## property: TestProject.expect
* since: v1.10
- type: ?<[Object]>
  - `timeout` ?<[int]> Default timeout for async expect matchers in milliseconds, defaults to 5000ms.
  - `toHaveScreenshot` ?<[Object]> Configuration for the [`method: PageAssertions.toHaveScreenshot#1`] method.
    - `threshold` ?<[float]> an acceptable perceived color difference between the same pixel in compared images, ranging from `0` (strict) and `1` (lax). `"pixelmatch"` comparator computes color difference in [YIQ color space](https://en.wikipedia.org/wiki/YIQ) and defaults `threshold` value to `0.2`.
    - `maxDiffPixels` ?<[int]> an acceptable amount of pixels that could be different, unset by default.
    - `maxDiffPixelRatio` ?<[float]> an acceptable ratio of pixels that are different to the total amount of pixels, between `0` and `1` , unset by default.
    - `animations` ?<[ScreenshotAnimations]<"allow"|"disabled">> See [`option: animations`] in [`method: Page.screenshot`]. Defaults to `"disabled"`.
    - `caret` ?<[ScreenshotCaret]<"hide"|"initial">> See [`option: caret`] in [`method: Page.screenshot`]. Defaults to `"hide"`.
    - `scale` ?<[ScreenshotScale]<"css"|"device">> See [`option: scale`] in [`method: Page.screenshot`]. Defaults to `"css"`.
  - `toMatchSnapshot` ?<[Object]> Configuration for the [`method: SnapshotAssertions.toMatchSnapshot#1`] method.
    - `threshold` ?<[float]> an acceptable perceived color difference between the same pixel in compared images, ranging from `0` (strict) and `1` (lax). `"pixelmatch"` comparator computes color difference in [YIQ color space](https://en.wikipedia.org/wiki/YIQ) and defaults `threshold` value to `0.2`.
    - `maxDiffPixels` ?<[int]> an acceptable amount of pixels that could be different, unset by default.
    - `maxDiffPixelRatio` ?<[float]> an acceptable ratio of pixels that are different to the total amount of pixels, between `0` and `1` , unset by default.

Configuration for the `expect` assertion library.

Use [`property: TestConfig.expect`] to change this option for all projects.

## property: TestProject.fullyParallel
* since: v1.10
- type: ?<[boolean]>

Playwright Test runs tests in parallel. In order to achieve that, it runs several worker processes that run at the same time.
By default, **test files** are run in parallel. Tests in a single file are run in order, in the same worker process.

You can configure entire test project to concurrently run all tests in all files using this option.

## property: TestProject.grep
* since: v1.10
- type: ?<[RegExp]|[Array]<[RegExp]>>

Filter to only run tests with a title matching one of the patterns. For example, passing `grep: /cart/` should only run tests with "cart" in the title. Also available globally and in the [command line](../test-cli.md) with the `-g` option. The regular expression will be tested against the string that consists of the test file name, `test.describe` name (if any) and the test name divided by spaces, e.g. `my-test.spec.ts my suite my test`.

`grep` option is also useful for [tagging tests](../test-annotations.md#tag-tests).

## property: TestProject.grepInvert
* since: v1.10
- type: ?<[RegExp]|[Array]<[RegExp]>>

Filter to only run tests with a title **not** matching one of the patterns. This is the opposite of [`property: TestProject.grep`]. Also available globally and in the [command line](../test-cli.md) with the `--grep-invert` option. This filter and its command line counterpart also applies to the setup files. If all [`property: TestProject.setupMatch`] tests match the filter Playwright **will** run all setup files before running the matching tests.

`grepInvert` option is also useful for [tagging tests](../test-annotations.md#tag-tests).

## property: TestProject.metadata
* since: v1.10
- type: ?<[Metadata]>

Metadata that will be put directly to the test report serialized as JSON.

## property: TestProject.name
* since: v1.10
- type: ?<[string]>

Project name is visible in the report and during test execution.

## property: TestProject.setupMatch
* since: v1.29
- type: ?<[string]|[RegExp]|[Array]<[string]|[RegExp]>>

Project setup files that will be executed before all tests in the project.

**Details**

If project setup fails the tests in this project will be skipped. All project setup files will run in every shard if the project is sharded. [`property: TestProject.grep`] and [`property: TestProject.grepInvert`] and their command line counterparts also apply to the setup files. If such filters match only tests in the project, Playwright will run **all** setup files before running the matching tests.

If there is a file that matches both [`property: TestProject.setupMatch`] and [`property: TestProject.testMatch`] filters an error will be thrown.

## property: TestProject.snapshotDir
* since: v1.10
- type: ?<[string]>

The base directory, relative to the config file, for snapshot files created with `toMatchSnapshot`. Defaults to [`property: TestProject.testDir`].

The directory for each test can be accessed by [`property: TestInfo.snapshotDir`] and [`method: TestInfo.snapshotPath`].

This path will serve as the base directory for each test file snapshot directory. Setting `snapshotDir` to `'snapshots'`, the [`property: TestInfo.snapshotDir`] would resolve to `snapshots/a.spec.js-snapshots`.

## property: TestProject.snapshotPathTemplate = %%-test-config-snapshot-path-template-%%
* since: v1.28

## property: TestProject.outputDir
* since: v1.10
- type: ?<[string]>

The output directory for files created during test execution. Defaults to `<package.json-directory>/test-results`.

This directory is cleaned at the start. When running a test, a unique subdirectory inside the [`property: TestProject.outputDir`] is created, guaranteeing that test running in parallel do not conflict. This directory can be accessed by [`property: TestInfo.outputDir`] and [`method: TestInfo.outputPath`].

Here is an example that uses [`method: TestInfo.outputPath`] to create a temporary file.

```js tab=js-js
const { test, expect } = require('@playwright/test');
const fs = require('fs');

test('example test', async ({}, testInfo) => {
  const file = testInfo.outputPath('temporary-file.txt');
  await fs.promises.writeFile(file, 'Put some data to the file', 'utf8');
});
```

```js tab=js-ts
import { test, expect } from '@playwright/test';
import fs from 'fs';

test('example test', async ({}, testInfo) => {
  const file = testInfo.outputPath('temporary-file.txt');
  await fs.promises.writeFile(file, 'Put some data to the file', 'utf8');
});
```

Use [`property: TestConfig.outputDir`] to change this option for all projects.

## property: TestProject.repeatEach
* since: v1.10
- type: ?<[int]>

The number of times to repeat each test, useful for debugging flaky tests.

Use [`property: TestConfig.repeatEach`] to change this option for all projects.

## property: TestProject.retries
* since: v1.10
- type: ?<[int]>

The maximum number of retry attempts given to failed tests. Learn more about [test retries](../test-retries.md#retries).

Use [`method: Test.describe.configure`] to change the number of retries for a specific file or a group of tests.

Use [`property: TestConfig.retries`] to change this option for all projects.

## property: TestProject.testDir
* since: v1.10
- type: ?<[string]>

Directory that will be recursively scanned for test files. Defaults to the directory of the configuration file.

Each project can use a different directory. Here is an example that runs smoke tests in three browsers and all other tests in stable Chrome browser.

```js tab=js-js
// playwright.config.js
// @ts-check

/** @type {import('@playwright/test').PlaywrightTestConfig} */
const config = {
  projects: [
    {
      name: 'Smoke Chromium',
      testDir: './smoke-tests',
      use: {
        browserName: 'chromium',
      }
    },
    {
      name: 'Smoke WebKit',
      testDir: './smoke-tests',
      use: {
        browserName: 'webkit',
      }
    },
    {
      name: 'Smoke Firefox',
      testDir: './smoke-tests',
      use: {
        browserName: 'firefox',
      }
    },
    {
      name: 'Chrome Stable',
      testDir: './',
      use: {
        browserName: 'chromium',
        channel: 'chrome',
      }
    },
  ],
};

module.exports = config;
```

```js tab=js-ts
// playwright.config.ts
import type { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  projects: [
    {
      name: 'Smoke Chromium',
      testDir: './smoke-tests',
      use: {
        browserName: 'chromium',
      }
    },
    {
      name: 'Smoke WebKit',
      testDir: './smoke-tests',
      use: {
        browserName: 'webkit',
      }
    },
    {
      name: 'Smoke Firefox',
      testDir: './smoke-tests',
      use: {
        browserName: 'firefox',
      }
    },
    {
      name: 'Chrome Stable',
      testDir: './',
      use: {
        browserName: 'chromium',
        channel: 'chrome',
      }
    },
  ],
};
export default config;
```

Use [`property: TestConfig.testDir`] to change this option for all projects.

## property: TestProject.testIgnore
* since: v1.10
- type: ?<[string]|[RegExp]|[Array]<[string]|[RegExp]>>

Files matching one of these patterns are not executed as test files. Matching is performed against the absolute file path. Strings are treated as glob patterns.

For example, `'**/test-assets/**'` will ignore any files in the `test-assets` directory.

Use [`property: TestConfig.testIgnore`] to change this option for all projects.

## property: TestProject.testMatch
* since: v1.10
- type: ?<[string]|[RegExp]|[Array]<[string]|[RegExp]>>

Only the files matching one of these patterns are executed as test files. Matching is performed against the absolute file path. Strings are treated as glob patterns.

By default, Playwright Test looks for files matching `.*(test|spec)\.(js|ts|mjs)`.

Use [`property: TestConfig.testMatch`] to change this option for all projects.

## property: TestProject.timeout
* since: v1.10
- type: ?<[int]>

Timeout for each test in milliseconds. Defaults to 30 seconds.

This is a base timeout for all tests. Each test can configure its own timeout with [`method: Test.setTimeout`]. Each file or a group of tests can configure the timeout with [`method: Test.describe.configure`].

Use [`property: TestConfig.timeout`] to change this option for all projects.

## property: TestProject.use
* since: v1.10
- type: <[Fixtures]>

Options for all tests in this project, for example [`property: TestOptions.browserName`]. Learn more about [configuration](../test-configuration.md) and see [available options][TestOptions].

```js tab=js-js
// playwright.config.js
// @ts-check

/** @type {import('@playwright/test').PlaywrightTestConfig} */
const config = {
  projects: [
    {
      name: 'Chromium',
      use: {
        browserName: 'chromium',
      },
    },
  ],
};

module.exports = config;
```

```js tab=js-ts
// playwright.config.ts
import type { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  projects: [
    {
      name: 'Chromium',
      use: {
        browserName: 'chromium',
      },
    },
  ],
};
export default config;
```

Use [`property: TestConfig.use`] to change this option for all projects.
