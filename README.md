## JavaScript best practices
### Variables:
`let` should only be used when the ability to reassign a variable is necessary. If a variable is never reassigned, use `const`. 
The old `var` declaration is unnecessary. 
```js
const aConstantValue = 5
let aChangingValue = 4
if (foo) {
  aChangingValue = 2
}
```

Do not reassign arguments within a function. Instead create a new variable
```js
// Bad:
function addOne(number) {
  number = number + 1
  return number
}
// Good:
function addOne(number) {
  const newNumber = number + 1
  return newNumber
}
```

Keep variable names unique. Do not use the same name as another variable in scope
```js
const amount = 55
// Bad:
const foo = bar.map((amount) => {
  console.log('amount:', amount)
  // do more stuff
})
// Good:
const foo = bar.map((barAmount) => {
  console.log('barAmount:', barAmount)
  // do more stuff
})
```
Variable names should be clear and relevant to make the code more readable.
Do not use unclear or shortened names.
There is no reason to shorten or abbreviate labels as the compiler will take care of that.
```js
// Bad:
const r = foo(bar)
const l = customerList.map((c, i) => {
  console.log({ c, i })
  // do stuff
})
// also bad:
const res = foo(bar)
const ls = customerList.map((cust, i) => {
  console.log({ cust, i })
  // do stuff
})
// Good:
const result = foo(bar)
const list = customerList.map((customer, index) => {
  console.log({ customer, index })
  // do stuff
})
```

Default exports should always have the same name as the file!
```js
// In CustomerList.js

// Bad:
export default List

// Good:
export default CustomerList
```

### Syntax 
Always use trailing commas in objects, arrays, deconstructions, etc.
```js
// Bad
const object = {
  value1,
  value2
}
const array = [
  value3,
  value4
]
const {
  value5,
  value6
} = props

// Good:
const object = {
  value1,
  value2,
}
const array = [
  value3,
  value4,
]
const {
  value5,
  value6,
} = props
```

Do not use string concatenation, instead use template literals
```js
// Bad
const someString = 'foo ' + someVariable + ' bar'

// Good:
const someString = `foo ${someVariable} bar`
```
 
 Semicolons, you don't need them. In the rare case when they are necessary,
 add them at the beginning of the line, or better yet refactor. 
 ```js
// Bad:
const foo = 5;
// Good:
const bar = 7
// acceptable:
const something = somethingElse
;(foo + bar).times((n) => {
  // do stuff
})
```

Use curly braces on if statements
```js
// Bad: 
if (aValue)
  console.log('aValue:', aValue)
// Good:
if (aValue) {
  console.log('aValue:', aValue)
}
```

Add spaces between blocks and large chunks of code.
This make your code much more pleasant to read.
```js
// Bad:
const aFunction = () => {
  // do a bunch of stuff
}
const anotherFunction = (aValue) => {
  console.log('in anotherFunction')
  console.log('aValue:', aValue)
}
const {
  thing1,
  thing2,
} = props
const anArray = something.map((value) => {
  // do stuff
})
const anInteger = 1
const anotherInteger = 3
const aString = 'abcd'


// Good:
const aFunction = () => {
  // do a bunch of stuff
}

const anotherFunction = (aValue) => {
  console.log('in anotherFunction')
  console.log('aValue:', aValue)
}

const {
  thing1,
  thing2,
} = props

const anArray = something.map((value) => {
  // do stuff
})

const anInteger = 1
const anotherInteger = 3
const aString = 'abcd'
```

### File Organization:
React files should be ordered as follows:

- Imports
- Utilities and functions 
- Styled components 
- Types
- React components 

For files with multiple React components, the types may be defiled above each corresponding component 

Imports should be ordered as follows:

- Third party dependencies
- Components
- Utilities 
- Types 

Similar imports should be grouped together (such as when there are multiple MaterialUI imports). 
React should be the first import 
Example:
```js
// third party dependencies
import  React, { useState, useEffect } from  'react'
import { isEmpty } from 'lodash'
import  styled  from  'styled-components'
// material-ui components grouped together:
import  Typography  from  '@material-ui/core/Typography'
import  Paper  from  '@material-ui/core/Paper'
import  Box  from  '@material-ui/core/Box'
import  LocationOnIcon  from  '@material-ui/icons/LocationOn'
import  FavoriteBorderIcon  from  '@material-ui/icons/FavoriteBorder'

//Imports from project, ordered by components, utils, types:
import  LocationDisplay  from  '../Location/LocationDisplay'

import { getImageFromInventory } from  '../../utils/inventory'
import { getPath } from  '../../utils/path'

import { InventoryType, InventoryListType } from  '../../types'

```

### Development practices 
Development tools such as `console.log` and `debugger` do not belong in production! Use them liberally during development but remove them before merging your branch.
If you must leave console.logs in your branch to assist with development, please LABEL THEM properly and remove them before you branch is merged. 
When logs are left in the code the console can get very messy and make working more difficult
```js
function processOrder(customerOrder) {
  // a useless log:
  console.log(customerOrder)
  // other developers will see an object show up in the console
  // and have no idea what it is

  // add labels to the console
  // you can pass in an object:
  // (especially useful when logging multiple values at once)
  console.log({ customerOrder })
  // or add a string:
  console.log('customerOrder:', customerOrder)
  // even more specific:
  console.log('customerOrder in processOrder:', customerOrder)

  // the last three examples are acceptable for development 
  // but PLEASE remove them before merging
}
```
  
There are a few examples where keeping logs might be a good idea, such as logging errors. In this situation, use `console.error` and label it appropriately. 
```js
function submitOrder() {
  try {
    // do stuff
  } catch(error) {
    console.error('error submitting order:', error)
    // the more specific your labels are, the easier debugging will be
  }
}
```
