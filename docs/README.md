# Compodoc

* goal
  * architecture
  * how does it work?

## libraries / INTERNALLY -- use to generate -- static HTML pages

### libs / NOT sync -- with -- npm

-   d3 from d3-flextree: 3.x.x
-   EventDispatcher
-   htmlparser: 2.0.0
-   innersvg: 2.x.x
-   prism: 1.29.0
-   promise
-   deep-iterator: 2.4.0

### libs / sync -- with -- npm

-   bootstrap native: 5.0.0
-   es6-shim: 0.35.1
-   svg-pan-zoom: 3.6.1
-   tablesort: 5.4.0
-   vis: 4.21.0
-   lunr: 2.3.9

## Entry files

* if you are using Compodoc
  * | module mode, `require('@compodoc/compodoc')`, -> `src/index.ts` == FIRST entry file 
  * -- via -- CLI, -> `src/index-cli.ts` == FIRST entry file 

## How does Compodoc work?

- handle CLI flags
- find files to scan -- via -- 
  - `tsconfig.json`'s include and/or exclude options, OR
  - root folder of `tsconfig.json`
- scan the files -- via -- TypeScript compiler
- generate ALL internal stuff
- emit files / EACH category (modules, components, etc)
- generation's result -- is -- echo

## How to test?

### Local unit testing

* == FROM SEVERAL files & projects, -- run -- SEVERAL documentation generation 

```shell
npm run test
```

### Local E2E testing

* -- via -- SauceLabs service

1. `npm install selenium-standalone@latest -g`
   1. [selenium-standalone](https://www.npmjs.com/package/selenium-standalone):
2. `selenium-standalone install`
   1. == configure `selenium-standalone`:
3. `selenium-standalone start`
4. `npm run test:simple-doc`
   1. == start LOCAL documentation generation | ANOTHER terminal tab
5. `npm run local-test-e2e-mocha`
   1. run local E2E testing

## How to setup development?

1. `npm i`
2. `npm run build`
3. `npm link`
   1. == 👀make `compodoc` command AVAILABLE EVERYWHERE 👀
4. `npm start`
   1. == watch process -- for -- source files & rollup build

## Node.js inspecting

* TODO:
1. Install sleep package:

    ```shell
    npm i sleep
    ```

2. Add these lines in `index-cli.ts`, after `--files` check:

    ```JavaScript
    const sleep = require('sleep');
    const isInInspectMode = /--inspect/.test(process.execArgv.join(' '));
    if (isInInspectMode) {
        // wait 10 seconds for debugger to connect in Chrome devtools
        sleep.sleep(10);
    }
    ```

3. Open one terminal and run inside `compodoc` folder:

    ```shell
    npm run start
    ```

4. Add `debugger` statement where you want to debug your code.
5. Open Chrome and this url: `chrome://inspect`.
6. Open another terminal with the source code of the [demo project](https://github.com/compodoc/compodoc-demo-todomvc-angular), and run:

    ```shell
    node --inspect ../compodoc/bin/index-cli.js -p tsconfig.json -a screenshots -n 'TodoMVC Angular documentation' --includes additional-doc --toggleMenuItems "'all'" -s
    ```

7. Compodoc will wait 10s before starting when it detects `--inspect` flag.
8. Open the debug window in Chrome, and click `inspect`.

## Release

-   gitflow start new release
-   in git release branhc, bump package.json and packe-lock.json version number
-   update changelog : npm run changelog, and copy-paste data in CHANGELOG.md
-   close release branch
-   npm publish --access public
