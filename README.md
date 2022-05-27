# Problem importing react-ace editor transitively

This project shows a problem encountered when using CRA 5 (incl Webpack 5) and trying to import a custom component
that wraps ReactAce.

The project has a sub-module `react-ace-module` with just a simple `index.js` as follows:

```javascript
import * as React from "react"
import ReactAce from "react-ace";

export const MyReactAce=() => {
    // switch the two lines below to get the app working!

    return React.createElement(ReactAce, {}, null)
    // return React.createElement(ReactAce.default, {}, null)
}
```

The module uses ESM `exports` in its `package.json`:
```json
  "type": "module",
  "exports": {
    ".": {
      "import": "./index.js"
    }
  }
```
This module is imported into the main CRA app. 

To reproduce the error run `npm install` and `npm start`. In the browser console you can see the error:

```
Uncaught Error: Element type is invalid: expected a string (for built-in components) or a class/function (for composite components) but got: object.

Check the render method of `MyReactAce`.
    at createFiberFromTypeAndProps (react-dom.development.js:28389:1)
    at createFiberFromElement (react-dom.development.js:28415:1)
    at reconcileSingleElement (react-dom.development.js:15620:1)
    at reconcileChildFibers (react-dom.development.js:15678:1)
    at reconcileChildren (react-dom.development.js:19971:1)
    at updateFunctionComponent (react-dom.development.js:20419:1)
    at beginWork (react-dom.development.js:22430:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4161:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4210:1)
    at invokeGuardedCallback (react-dom.development.js:4274:1)
```

If you flip the comment in `react-ace-module/index.js` to use `ReactAce.default` you can get it to work.