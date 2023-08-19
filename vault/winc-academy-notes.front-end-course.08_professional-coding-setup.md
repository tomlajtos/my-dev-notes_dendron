---
id: 4q0k7la6wwzi5krzdtuxi5f
title: FE Module 08 - Professional Coding Setup
desc: "Winc FE Module 08"
updated: 1692450028466
created: 1692448192441
---

## 01. CLI (command line interface)

- ? > where am I - `pwd` (print working directory)
- ? > list - `ls/ ls -l/ -ls -la`
- ? > change directory - cd 'dirname' / cd ..
- ? > create directory - mkdir
- ? > delete directory - rmdir (only if it is empty) / rm -rf dir :warning: permanent action
- ? > create a file - touch 'filename'
- ? > remove a file - rm 'filename'
- ? >
- ? >

## 02. -

## 03. -

## 04. -

## 05. -

## 06. Node/npm

- install node/npm or NVM
- install nodemon (node monitor - live refresh on save in terminal)

## 07. Installing global npm packages

- ? > `npm install -g package_name`
- ? > run a package without installing it ==> use 'npx' i.e.: `npx package_name`
  (download **&rArr;** run **&rArr;** remove)

## 08. JavaScript projects

### 08.01. Initialize a project with npm

- in project root **&rArr;** `npm init` **&rArr;** answer qs **&rArr;** creates package.json (editable)

### 08.02. Add script for nodemon

```javascript
{
"name": "zodiac_esm_solution",
"version": "1.0.0",
"description": "",
"main": "index.js",
"type": "commonjs", // or "module" if ES Module (ESM)
"scripts": { // define script here // ? >
"start": "nodemon --quiet"
},
"keywords": [],
"author": "",
"license": "ISC"
}
```

> ? > run the script: ./npm run start (runs nodemon in quiet mode)

### 08.03 Splitting up a JavaScript project into modules

#### 08.03.01. CommonJS modules

- ? > export functions or variables with commonjs -- `modules.exports = { item1, item2, item2 };`
- ? > import from commonjs module -- `const {item1, item2, item3} = require("./moduleName");`

#### 08.03.02. ES Modules (ESM)

- ? > export functions or variables with ESM -- `export { item1, item2, item3 };`
- ? > import from ES module -- `import { item1, item2, item3 } from "./moduleName.js";`

#### 08.03.03. Using ESM and CommonJS in one project (ESM only is advised)

:warning: ONLY imports CommonJS modules into ES modules (doesn't work the other way)

- in package.json
  `"type": "module",`
- CommonJS module file extension is: fileName.cjs
- import statement in ESM needs to point to the full path of the module.cjs, :warning: extension included
- tools to help with managing this: i.e. Babel

### 08.04. Package versions and scoped packages

#### 08.04.01. Semantic versioning:

> ? > Semantic version numbers look like this: 4.7.1 or 3.12.1 or 27.1.0.

- The 1st number indicates the MAJOR version.
- The 2nd number indicates the MINOR version.
- The 3rd number indicates the PATCH version.

#### 08.04.02. npm version calculator // [link](https://semver.npmjs.com/)

Query examples:

- '^2.1.3' **&rArr;** any version with the same main version (1st no.) and minor and patch versions are equal or higher
- '~3.9.1' **&rArr;** any verion within 3.9... patch number is equal or higher
- '>=4.1.0' **&rArr;** any version that are equal or higher incl. major/minor/patch
- also based on the logic the following operators can be used too: >, <, <=
- 1.0.0 - 3.0.0 **&rArr;** exact range, icluding the lover and upper limits

#### 08.04.03. Packages with a scope

@scope_name/package_name

- sometimes used by organizations - "@ibm/motion"
- or to group packages - i.e. "@storybook/"
  Advantage: easier naming of npm packages, any name can be used within a scope

### 08.05. Third Party Dependencies

#### 08.05.01. Direct and Indirect Dependencies

- direct: where the project's code directly imports from / installed
- indirect (aka "transitive dependency"): package that is needed by another
  dependency **&rArr;** dep. relations can be represented by a dep. tree.
- ? > TO SHOW DEPENDENCY TREE: "npm list -all"

#### 08.05.02. Development dependnecies ("dev dependencies")

- It is a dependency that is only needed during development. i.e.: formatter
- ? > Install a dev. dep. : `npm install -save-dev{package_name}` **&rArr;** this will
  list the dep under the devDependencies in package.json

#### 08.05.03. Role of package.json and package-lock.json and differences

- ? > package.json: contains project metadata, configs and automation
- ? > package-lock.json: describe indeirect and direct dependencies down to the
  exact version number **&rArr;** allows acurate rebuild without braking any code

#### 08.05.04. npm ci (clean install)

- to install a freshly downloaded project's dependencies
- best way with the command `npm ci` **&rArr;** install dependencies from package-lock.json
  with the right version. [link](https://docs.npmjs.com/cli/v8/commands/npm-ci)

### 08.06. Evaluating 3rd party code licensing

conditions of oss/os project use is described in the project license (MIT etc.)

### 08.07. Evaluating 3rd party code security

(aka "Software Supply Chain Security")
:warning:**&rArr;** npm packages have the potential risk to contain malwares, delete files or worse.

- npm install reports vulnerabilities
- use reliable source
- :warning: wait with installig updates: couple days to see if there are problems
  EXCEPT: security updates
- npm audit: install package into an empty project and run 'npm audit' **&rArr;** does a security scan [link](https://docs.npmjs.com/cli/v8/commands/npm-audit)
- use 3rd party security tools (i.e. Synk)

### 08.08. Updating dependnecies

- on Github, use 'Dependabot' to track dependnecy updates automatically

## 09. Bundlers, buildtools and front-end toolchains

### 09.01. Bundling code

- bundler will: analyze **&rArr;** order **&rArr;** sort **&rArr;** combine the code **&rArr;** send to browser
- Bundlers can also bundle non-JavaScript code like CSS
  - [Esbuild](https://esbuild.github.io/)
  - [Rollup](https://rollupjs.org/guide/en/)
  - [Webpack](https://webpack.js.org/)

### 09.02. Compilatoin

- converting code into output files (Sass >> CSS, TypeScript >> JS)

### 09.03. Transpilation (source-to-source transpilation)

- rocess of taking code written using certain language constructs and converting it
  into code that does the same but uses different language construct
  i.e. ESM to CommonJS, or ES6 >> old JS for old browsers, Node >> browser

### 09.04. Polyfilling (and code instead of transforming /like transpilation/)

- makes functionality available by adding code
  i.e. [js-temporal_polyfill](https://www.npmjs.com/package/@js-temporal/polyfill)

- XTRA-info: [link](https://javascript.info/polyfills)
- TOOLS for compilation and transpilation:
  - [Sass compiler](https://www.npmjs.com/package/sass)
  - [TypeScript compiler](https://www.npmjs.com/package/typescript)
  - [Babel](https://babeljs.io/)
  - [Webpack](https://webpack.js.org/)

### 09.05. Image processing/optimization

- for website optimization (resize, crop) images need to be as light as possible
- Tools:
  - [imagemin package](https://github.com/imagemin/imagemin)

### 09.06. Serving during development (dev server)

- serv website and other project locally
- some kind of live server application with HMR (hot module realoading)

### 09.07. Toolchains

- combination of tools configured to work together
- e.i. by popularity:
  - [Vite](https://vitejs.dev/)
  - [Parcer](https://parceljs.org/)
  - [Rome](https://rome.tools/)
  - [WMR](https://wmr.dev/)

## 10. Version control

## 11. Version control bsics

- git
- web service to host repositories online: Github
  alt: Gitlab, Gitea

## 12. Git on the computer / local repo
