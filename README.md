# cl-fsm

[中文文档](./README_zh.md)   [document](./README.md)

Simple FSM DSL
- [install](#install)
- [usage](#usage)
  * [API quick run](#api-quick-run)
- [develop](#develop)
  * [file structure](#file-structure)
  * [run tests](#run-tests)
- [license](#license)

## install

`npm i cl-fsm --save` or `npm i cl-fsm --save-dev`

Install on global, using `npm i cl-fsm -g`



## usage








### API quick run



```js
let FSM = require('cl-fsm')
let {
    stateGraphDSL, fsm, WAIT, MATCH
} = FSM;

let {
    g, c, union, range, sequence, circle, left, repeat
} = stateGraphDSL;

let hexDigit = union(range('0', '9'), range('A', 'F'), range('a', 'f'));

let escapeSymbols = union('"', '\\', '\/', 'b', 'f', 'n', 'r', 't');

let stringDFA = g(
    c('"', g('enter',
        c('\\', g(
            c(escapeSymbols, 'enter'),
            c('u',
                g(repeat(hexDigit, 4, 'enter'))
            ))),
        c('"', 'accept'),
        c(left(), 'enter')
    )));

let m = fsm(stringDFA);
console.log(m('"').type === WAIT);
console.log(m('a').type === WAIT);
console.log(m('b').type === WAIT);
console.log(m('"').type === MATCH);
```

```
output

    true
    true
    true
    true

```


## develop

### file structure

```
.    
│──LICENSE    
│──README.md    
│──README_zh.md    
│──apply    
│   └──json    
│       └──index.js    
│──benchmark    
│   └──index.js    
│──coverage    
│   │──coverage.json    
│   │──lcov-report    
│   │   │──base.css    
│   │   │──cl-fsm    
│   │   │   │──index.html    
│   │   │   └──index.js.html    
│   │   │──index.html    
│   │   │──prettify.css    
│   │   │──prettify.js    
│   │   │──sort-arrow-sprite.png    
│   │   └──sorter.js    
│   └──lcov.info    
│──index.js    
│──package.json    
│──src    
│   │──const.js    
│   │──index.js    
│   └──stateGraphDSL    
│       │──actionDSL.js    
│       │──graphDSL.js    
│       └──index.js    
└──test    
    │──index.js    
    └──json.js     
```


### run tests

`npm test`

## license

MIT License

Copyright (c) 2017 chenjunyu

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
