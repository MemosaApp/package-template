Package Template
----------------

Download and copy this template to get started with making a new package that uses all of the memosa presets.

## React Package?

Run:

```
yarn add --dev enzyme react react-addons-test-utils react-dom
yarn add --peer react
```

Change your test-boot.js file as well:

```js
import chai from 'chai';
import sinonChai from 'sinon-chai';
import chaiAsPromised from 'chai-as-promised';
import { jsdom } from 'jsdom';

chai.use(chaiAsPromised);
chai.use(sinonChai);

const exposedProperties = ['window', 'navigator', 'document'];

global.document = jsdom('');
global.window = document.defaultView;
Object.keys(document.defaultView).forEach((property) => {
  if (typeof global[property] === 'undefined') {
    exposedProperties.push(property);
    global[property] = document.defaultView[property];
  }
});

global.navigator = {
  userAgent: 'node.js'
};

// spec/javascripts/helpers/jest-env.js
// window.getSelection isn't in jsdom
// https://github.com/tmpvar/jsdom/issues/937
window.getSelection = function() {
  return {
    addRange: function() {},
    removeAllRanges:function() {}
  };
};

window.document.getSelection = window.getSelection;

// Storage Mock
function storageMock() {
  const storage = {};

  return {
    setItem(key, value) {
      storage[key] = value || '';
    },
    getItem(key) {
      return storage[key] || null;
    },
    removeItem(key) {
      delete storage[key];
    },
    get length() {
      return Object.keys(storage).length;
    },
    key(i) {
      const keys = Object.keys(storage);
      return keys[i] || null;
    },
  };
}

// mock the localStorage
global.localStorage = storageMock();
// mock the sessionStorage
global.sessionStorage = storageMock();
```

In your `.eslintrc` file, change:

`"browser": false,` to `"browser": true,`

## Readme

Your README should at the minimum look like this:

```
# project title

Badges

Description

## Installing

Installation instructions

## Usage

Usage instructions
```
