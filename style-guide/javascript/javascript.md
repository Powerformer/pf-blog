# Powerformer JavaScript Style Guide() {

*A mostly reasonable approach to Coding*

> **注意**: 一般的变量命名会以涵盖标题为主，涉及四个值得变量命名会使用`top, right, bottom, left`, 涉及少于四个或者大于四个的变量命名会使用`first, second ...`。

## 代码风格(Code style)

  - [1.1](#code-style-indent) 语句块内每行代码缩进`2`个空格。

    ```javascript
    // bad
    function heros() {
        if (x) {
            return x;
        } else {
            return y;
        }
    }

    // good
    function heros() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }
    ```





## 类型(Types)

- **原始类型** :当使用原始类型时，直接是对其 **值** 进行操作。

  - `string`
  - `number`
  - `boolean`
  - `null`
  - `undefined`
  - `symbol`

  ```javascript
  const foo = 1;
  let bar = foo;

  bar = 9;

  foo === bar; //  => false
  console.log(foo, bar); // => (1, 9)
  ```

  - `Symbol`  不能通过`polyfill`  来模拟实现，只有在 `javascript` 宿主环境（browser / node) 原生支持的时候才能使用。

- **复杂类型** : 当使用复杂类型时，是对 **引用** 进行操作。

  - `object`
  - `array`
  - `function`

  ```javascript
  const foo = [1, 2];
  const bar = foo;

  bar[0] = 9;

  console.log(foo[0], bar[0]); // => 9, 9
  foo === bar; // => true
  ```




## 引用（Reference）

- 对于所有的 **引用** 都用 `const` 来修饰，避免使用 `var` 。

  >为什么要这么做呢？因为它能确保你不能给 **引用** 重新赋值，这样能减少很多bug，以及让代码更易理解。

  ```javascript
  // bad
  var a = 1;
  var b = 2;

  // good
  const a = 1;
  const b = 2;
  ```



- 如果你一定得给 **引用** 重新赋值，那么你也应该使用 `let` 而不是 `var` 。

  > 为什么呢？因为 `let` 是块级作用域而 `var` 是函数作用域。

  ```javascript
  // bad
  var count = 1;
  if (true) {
    count += 1;
  }

  // good
  let count = 1;
  if (true) {
    count += 1;
  }
  ```

- 请注意！ `let` 和 `const` 都是块级作用域。

  ```javascript
  // const 和 let 都只在定义他们的块中存在
  if (true) {
    const a = 1;
    let b = 2;
  }
  console.log(a); //ReferenceError
  console.log(b); // ReferenceError
  ```

  ​

## 字符串

  - 静态字符串一律使用单引号，不使用双引号。动态字符串使用反引号，或者换行字符串。

    ```javascript
    // bad
    const staticString = "foobar";

    // bad
    const staticString = `foobar`;

    // good
    const staticString = 'foobar';
    const dynamicString = `foo${staticString}bar`;
    ```

  - 拼接字符串的时候，使用模板字符串 `template strings` 

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you', ', name, '?'].join();
    }

    // bad
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```



- 当一行字符串超过100个字符时，不要使用字符串链接符将字符串分成多行。

  ```javascript
  // bad
  const errMessage = 'This is a super long error that was thrown because \
  of Batman. When you stop to think about how Batman had anything to do \
  with this, you would get nowhere \
  fast.';

  // bad
  const errMessage = 'This is a super long error that was thrown because ' +
    'of Batman. When you stop to think about how Batman had anything to do ' +
    'with this, you would get nowhere fast.';

  // good
  const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
  ```



- 绝不要对字符串使用 `eval()` ，因为这会带来很多的侵害。

- 不在万不得已，不要使用转义字符 `\`。

  ```javascript
  // bad
  const foo = '\'this\' \i\s \"quoted"';

  // good
  const foo = '\'this\' is "quoted"';
  const foo = `my name is '${name}'`;
  ```

  ​

## 解构赋值

  - 使用数组成员对变量赋值时，优先使用解构赋值。

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const desArrFirst = arr[0];
    const desArrSecond = arr[1];

    // good
    const [desArrFirst, desArrSecond] = arr;
    ```

  - 函数参数如果是对象成员，优先使用解构赋值

    ```javascript
    // bad
    function getDream(pfPartner) {
      const firstDream = pfPartner.firstDream;
      const secondDream = pfPartner.secondDream;
    }

    // good
    function getDream(pfPartner) {
      const { firstDream, secondDream } = pfPartner;
    }

    // also good
    function getDream({ firstDream, secondDream }) {
      
    }
    ```

  - 如果函数返回多个值，优先使用对象的解构赋值，而不是数组的解构赋值。这样便于以后添加返回值，以及更改返回值的顺序。

    ```javascript
    // bad
    function processInput(input) {
      return [left, right, top, bottom];
    }

    // good
    function processInput(input) {
      return { left, right, top, bottom };
    }

    const { left, right } = processInput(input);
    ```



## 对象（Objects） 

  - 每当创建一个新对象时，使用对象字面量的方法。

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  - 当新创建的对象需要带有**动态属性名**时，使用**计算属性名**。

    > 为什么呢？因为这样能在一处定义对象的所有属性，清晰明了。

    ```javascript
    function getKey(k) {
      return `a key name ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'Shang Hai',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'Shang Hai',
      [getKey('enabled')]: true,
    };
    ```

    ​

  - 在书写对象的方法时，使用简洁写法。

    ```javascript
    // bad
    const vscode = {
      value: 1,
      
      addValue: function (value) {
        return vscode.value + value;
      },
    };

    // good
    const vscode = {
      value: 1,
      
      addValue(value) {
        return vscode.value + value;
      },
    };
    ```

  - 在书写对象的属性时，也使用简洁写法。

    > 为什么呢？因为这样写起来更简洁，而且描述得更清楚。

    ```javascript
    const walker = 'mRc & pftom';

    // bad
    const obj = {
      walker: walker,
    };

    // good
    const obj = {
      walker,
    };
    ```

  - 对象有多个属性时，把能用**简洁写法**的属性都放在对象申明的开头。

    > 为什么呢？因为这样能很容易知道哪些属性在使用**简洁写法**。

    ```javascript
    const pftom = 'A very cool guy.';
    const mRc = 'A smart guy.';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

- 只给不合法的标识符加上单引号

  > 为什么呢？因为主观上我们认为这样更具可读性，而且能增加语法高亮的程度，同时呢，Js引擎也更容易对其进行优化。

  ```javascript
  // bad
  const bad = {
    'foo': 3,
    'bar': 4,
    'data-blash': 5,
  };

  // good
  const good = {
    foo: 3,
    bar: 4,
    'data-blash': 5,
  };
  ```

- 不要在一个对象上直接调用  `Object.prototype` 上的方法，例如： `hasOwnProperty` ，`propertyIsEnumerable`  和 `isPrototypeOf` 。

  > 为什么呢？因为这些方法可能会被对象本身的属性覆盖掉，例如：`{ hasOwnProperty: false }` ；亦或，对象是一个 `null` 对象（`Object.create(null)`）- `null`对象是没有这些方法的

  ```javascript
  // bad
  console.log(object.hasOwnProperty(key));

  // good
  console.log(Object.prototype.hasOwnProperty.call(object, key));

  // best
  const has = Object.prototype.hasOwnProperty; // 调用一次之后，会在模块作用域中缓存
  /* or */
  import has from 'has';
  // ...
  console.log(has.call(object, key));
  ```

  ​

- 在对一个对象进行浅复制的时候，最好用（ `…`） `spread syntax `  语法，而不是使用  `Object.assign` ；在获取一个新对象时，需要忽略其有些属性，使用 `rest operator`。

  ```javascript
  // var bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign(original, { c: 3 }); // 这个改变了 original对象 (⊙_⊙?)
  delete copy.a // 这个也也改变了原对象

  // bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

  // good
  const original = { a: 1, b: 2 };
  const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 };

  const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
  ```

  ​

- 单行定义的对象，最后一个成员不以逗号结尾。多行定义的对象，最后一个成员以逗号结尾。

  ```javascript
  // bad
  const objComma1 = { first: value1, second: value2, };
  const objComma2 = {
    first: value1,
    second: value2
  };

  // good
  const objComma1 = { first: value1, second: value2 };
  const objComma2 = {
    first: value1,
    second: value2,
  };
  ```

- 对象要静态化，一旦定义，就不得改变，如果要改变，使用`Spread Syntax '...'`来返回一个新对象。

  ```javascript
  // bad
  const staticObj = {};
  staticObj.first = 3;

  // also bad
  const staticObj = { first: 2 };
  staticObj.first = 3;

  // good
  const staticObj = { first: 2 };
  const nextStaticObj = { ...staticObj, first: 3};

  // also good
  const staticObj = { first: 2 };
  const nextStaticObj = { ...staticObj, first: 3};
  ```

- 如果对象的属性名是动态的，使用属性表达式来定义。

  ```javascript
  function getKey(condition) {
    ...
    return result;
  }

  const propExpObj = {
    first: 'pftom',
    [getKey('enabled')]: true,
  };
  ```

- [4.4](#object-simplify-expression) 对象的属性和方法都要采用简洁表达式。

  ```javascript
  // bad
  const simpExpObj1 = {
    first: first,
  };

  // also bad
  const simpExpObj2 = {
    addFirst: function (value) {
      ...
      return result;
    }
  }

  // good
  const simpExpObj1 = {
    first,
  };

  // also good
  const simpExpObj2 = {
    addFirst(value) {
      ...
      return result;
    },
  };
  ```

**[⬆ back to top](#table-of-contents)**

## 数组 

  - 使用对象字面量创建数组。

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - 给数组添加新元素时，使用  `Array#push` 来添加，而不要直接以直接赋值的形式添加。

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abababab';

    // good
    someStack.push('abababab');
    ```

  - 拷贝数组使用 `Spread Syntax '...'` 。

    ```javascript
    // bad
    const items = [1, 2, 3, 4];
    const copyItems = [];
    let i;
    for (i = 0; i < items.length; i++) {
      copyItems[i] = items[i];
    }

    // good
    const items = [1, 2, 3, 4];
    const copyItems = [ ...items ];
    ```

  - 在对可遍历结构进行映射时，使用 `Array.from` 而不是 `...` ，因为这能避免创建一个中间数组。

    ```javascript
    // bad 
    const baz = [ ...foo ].map(bar); // [] => 即为中间数组。

    // good
    const baz = Array.from(foo, bar);
    ```

  - 在数组方法里的回调函数中使用 `return` 语句；如果回调函数体只由一条返回一个没有副作用的表达式构成，那么可以忽略 `return` 语句。

    ```javascript
    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map(x => x + 1);

    // bad - 因为没有 `return` 语句，`memo`在第一次迭代之后就变成 `undefined`
    [[0, 1], [2, 3], [4, 5]].reduce((meno, item, index) => {
      const flatten = memo.concat(item);
      memo[index] = flatten;
    });

    // good
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      memp[index] = flatten;
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }
      
      return false;
    })
    ```

  - 如果数组有很多行，那么在数组开始括号 `[ ` 之后和结束括号 `]` 之前都要换行。

    ```javascript
    // bad
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2
    ];

    // good
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

  - 使用 `Array.from` 或者 spreads `...` 将类数组对象转换为数组，

    ```javascript
    const analogyArrObj = document.querySelectorAll('.nodes');

    // good
    const arr = Array.from(analogyArrObj)

    // best
    const arr = [ ...foo ];
    ```



## 函数 

  - 能使用箭头函数尽量使用箭头函数。简洁而且可以绑定`this` 。

    ```javascript
    // bad
    const items = [1, 2, 3];
    items.map(function (item) {
      return item * item;
    });

    // good single line statments
    const items = [1, 2, 3];
    items.map(item => item * item);

    // also good multiline statements
    const items = [1, 2, 3];
    items.map((item) => {
      ...
      return item * item;
    })
    ```

  - [6.2](#function-arguments) 不要使用 `arguments`， 使用 `rest '...'` 运算符来替代。 `arguments` 是类数组， 而 `rest` 是真的数组。

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  - [6.3](#function-default-syntax) 使用默认值语法来设置函数参数的默认值。

    ```javascript
    // bad
    function setDefault(opts) {
      opts = opts || {};
      ...
    }

    // good
    function setDefault(opts = {}) {
      ...
    }
    ```




- 使用命名表达式定义函数，而不要使用函数声明。

  > 为什么呢？因为函数声明存在变量提升，这就意味着很容易在定义这个函数之前就能引用这个函数。这样的情况极大的损害了代码的可读性和可维护性。当你发现，在一个文件中，一个函数已经复杂到足以干扰理解剩下的代码，那么是时候将它作为一个单独的模块提取出来了！永远不要忘记要明确给函数表达式命名，尽管这可能会与已有的变量命名冲突。（这在现代浏览器或者使用了 `Babel` 编译器的情况下很常见；因为在出错时，我们能在 错误调用栈中通过名字定位到这个函数！

  ```javascript
  // bad
  function foo() {
    // ...
  }

  // bad
  const foo = function () {
    // ...
  };

  // good
  // `longUniqueMoreDescriptiveLexicalFoo` 是为了与变量调用时的引用 
  // `short` 进行区分，为了达到调用简便，但是出错提示调用栈明确显示具体函数
  const short = function longUniqueMoreDescriptiveLexicalFoo() {
    // ...
  };
  ```



- 使用圆括号`()` 包含立即执行的函数表达式。

  > 为什么呢？使用圆括号包裹立即执行的函数表达式（IIFE），这样能很明确的表达：这是一个单一单元。但是请注意，你可能永远不需要（IIFE），因为你总能将它写入一个单独的命名模块中来调用。

  ```javascript
  // IIFE
  (function () {
    console.log('Welcome to the Internet. Please follow me');
  }());
  ```

  ​

## 类 

  - 在需要使用  `Prototype` 的操作时，都用 `Class` 来取代。

    ```javascript
    // bad
    function Queue(contents = []) {
      this._queue = [...contents];
    }

    Queue.prototype.pop = function () {
      const value = this._queue[0];
      return value;
    }

    // good
    class Queue {
      constructor(contents = []) {
        this._queue = [ ...contents ];
      }

      pop () {
        const value = this._queue[0];
        return value;
      }
    }
    ```

## 模块 

  - [8.1](#module-import) 使用 `import` 代替 `require` 。

    ```javascript
    // bad
    const moduleA = require('moduleA');
    const { firstFunc, secondFunc } = moduleA;

    // good
    import { firstFunc, secondFunc } from 'moduleA';
    ```

  - [8.2](#module-export) 使用 `export` 代替 `module.exports` 。

    ```javascript
    // bad
    class moduleA {
      ...
    }

    module.exports = moduleA;

    // good

    class moduleA {
      ...
    }

    export default moduleA;
    ```

  - [8.3](#module-export-&default) `export default` 与 `export` 不要同时使用。

    ```javascript
    // bad
    export function myFunc () {
      ...
    }

    const value = 2;
    export default value;

    // good
    const value = 2;
    export default value;

    // also good
    function myFunc1 () {
      ...
    }

    function myFunc2 () {

    }

    export {
      myFunc1,
      myFunc2,
    }
    ```

  - [8.4](#module-default) 不要在模块中使用通配符，如果要输出多个模块，使用 `export default`

    ```javascript
    // bad
    import * as myModule from './myModule';

    // good
    export default {
      moduleA,
      moduleB,
    }
    import myModule from './myModule';
    ```

  - [8.5](#module-function) 如果默认输出的是函数，那么函数名首字母需要小写。

    ```javascript
    // bad
    function MyFunc () {
      ...
    }

    export default MyFunc;

    // good
    function myFunc () {
      ...
    }

    export default myFunc;
    ```

  - [8.6](#module-class) 如果默认输出的是对象，那么对象名首字母要大写。

    ```javascript
    // bad 
    const myObj = {
      ...
    };

    export default myObj;

    // good
    const MyObj = {
      ...
    };

    export default MyObj;
    ```

**[⬆ back to top](#table-of-contents)**

# };