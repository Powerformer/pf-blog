# Powerformer JavaScript Style Guide() {

*A mostly reasonable approach to Coding*

> **注意**: 一般的变量命名会以涵盖标题为主，涉及四个值得变量命名会使用`top, right, bottom, left`, 涉及少于四个或者大于四个的变量命名会使用`first, second ...`。

> 这份 `Style Guide` 绝大部分是对 Airbnb的 `Style Guide` 的引用的中文版。

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

- 在一个非函数代码块（`if` ，`while`， etc）里面不要申明一个函数，而应该把这个函数赋值给一个变量。尽管浏览器允许前者的做法，但是不同的浏览器对这种写法的理解不同，这样会造成不一致的结果。

  ```javascript
  // bad
  if (currentUser) {
    function test() {
      console.log('Nope.');
    }
  }

  // good
  let test;
  if (currentUser) {
    test = () => {
      console.log('Yup.');
    };
  }
  ```

- 不要给形参命名为 `arguments` 。这样会取代原来会在每个函数域里面提供的 `arguments` 对象。

  ```javascript
  // bad
  function foo(name, options, arguments) {
    // ...
  }

  // good
  function foo(name, options, args) {
    // ...
  }
  ```

- 绝不要使用 `arguments` ，取而代之的是 `rest` 语法：`...` 。

  > 为什么呢？因为 `...`  不仅明确指出了你需要展现出来的形参，而且其后面跟的剩余参数是一个真正的数组，而不是像 `arguments` 这样的类数组。

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

- 不要企图改变函数参数，而要使用默认参数语法。

  ```javascript
  // really bad
  function handleThings(opts) {
    // 不要这样做，因为这会改变参数
    // 更有甚者，当 opts 是 false 的时候，opts会被赋值为一个对象
    // 这可能会引入一些新的 bug
    opts = opts || {};
    // ...
  }

  // still bad
  function handleThings(opts) {
    if (opts === void 0) {
      opts = {};
    }
    // ...
  }

  // good
  function handleThings(opts = {}) {
    // ...
  }
  ```

- 避免让默认参数产生副作用。

  > 为什么呢？他们让人很难理解。

  ```javascript
  var b = 1;
  // bad
  function count(a = b++) {
    console.log(a);
  }

  count(); // 1
  count(); // 2
  count(3); //3
  count(); //3
  ```

- 总是把默认参数放在参数列表最后。

  ```javascript
  // bad
  function handleThings=(opts = {}, name) {
    // ...
  }

  // good
  function handleThings(name, opts = {}) {
    // ...
  }
  ```

  ​

- 绝不要使用函数的构造函数的形式来创建一个新函数。

  > 为什么呢？用这种方式创建一个新函数类似于使用 eval() 执行了一个字符串，这样会带来很多侵害。

  ```javascript
  // bad
  var add = new Function('a', 'b', 'return a + b');

  // still bad
  var substract = Function('a', 'b', 'return a - b');
  ```

  ​

- 在函数签名与括号之间加上空格。

  > 为什么呢？这样有利于提高连贯性，当你加上或删除一个签名时，不需要同时也加上或删除一个空格。

  ```javascript
  // bad
  const f = function(){};
  const g = function (){};
  const h = function() {};

  // good
  const x = function () {};
  const u = function a() {};
  ```



- 不要让参数发生突变

  > 为什么呢？让作为参数传进来的对象发生突变，会函数的源调用者造成意想不到的副作用。

  ```javascript
  // bad
  function f1(obj) {
    obj.key = 1;
  }

  // good
  function f2(obj) {
    const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
  }
  ```



- 不要给参数重新赋值。

  > 为什么呢？给参数重新赋值能可能导致意想不到的行为，尤其是对 `argument` 操作的时候。而且这样还会造成性能问题，尤其在 V8 上。

  ```javascript
  // bad
  function f1(a) {
    a = 1;
    // ...
  }

  function f2(a) {
    if (!a) { a = 1; }
    // ...
  }

  // good
  function f3(a) {
    const b = a || 1;
    // ...
  }

  function f4(a = 1) {
    // ...
  }
  ```

  ​


- 在调用 `variadic` 函数时，最好使用 `...` 。

  > 为什么呢？这样让代码更干净，而且你都不需要提供上下文 `context` ，同时呢，组合使用 `new` 和 `apply` 不是很容易。

  ```javascript
  // bad
  const x = [1, 2, 3, 4, 5];
  console.log.apply(console, x);

  // good
  const x = [1, 2, 3, 4, 5];
  console.log(...x);

  // bad
  new (Function.prototype.bind.apply(Date, [null, 2016, 8, 6]));

  // good
  new Date(...[2016, 8, 5]);
  ```

  ​


- 多行函数形参，或者多行实参时，每个参数独占一行，尾行加上逗号。

  > 为什么呢？更好的可读性，尾逗号是为了以后添加参数更加容易。

  ```javascript
  // bad
  function foo(bar,
               baz,
                quux) {
    // ...
  }

  // good
  function foo(
    bar,
    baz,
    quux,
    ) {
      // ...
  }

  // bad
  console.log(foo,
              bar,
              baz);

  // good
  console.log(
    foo,
    bar,
    baz,
  );
  ```

  ​

## 箭头函数

- 当你必须得使用匿名函数的时候（当作为一个行内回调函数传入时），用箭头函数来代替。

  > 为什么呢? 箭头函数内部保留了函数上下文 `this` ，这通常是你需要的，而且也是更简洁的语法。

  > 为什么不这样呢？因为如果你有一个相当复杂的函数时，是时候将这些逻辑移到一个单独的命名函数表达式中呢。

  ```javascript
  // bad
  [1, 2, 3].map(function (x) {
    const y = x + 1;
    return x * y;
  });

  // good
  [1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
  });
  ```

  ​


- 如果函数体由一个没有副作用，只返回表达式的单条语句构成，那么请忽略括号（`{}` ），使用隐式返回。相反，则需要使用括号以及 `return` 语句块。

  > 为什么呢？这是一种语法糖。当多个函数链式调用的时候，这提高了可读性。

  ```javascript
  // bad
  [1, 2, 3].map(number => {
    const nextNumber = number + 1;
    return `A string containing the ${nextNumber}.`;
  });

  // good
  [1, 2, 3].map((number) => {
    const nextNumber = number + 1;
    return `A string containing the ${nextNumber}.`;
  });

  // good
  [1, 2, 3].map(number => `A string containing the ${nextNumber}.`);

  // good
  [1, 2, 3].map((number, index) => ({
    [index]: number,
  }));

  // bad 
  // No implicit return with side effects
  function foo(callback) {
    const val = callback();
    if (val === true) {
      // Do something if callback returns true
    }
  }

  let bool = false;

  // bad
  foo(() => bool = true);

  // good
  foo(() => {
    bool = true;
  });
  ```

  ​


- 如果表达式太长以至于扩展到多行，把它用圆括号括起来，这样能带来更好的可读性。

  > 为什么呢？这样很清楚的展示出了函数在哪里开始，在哪里结束。

  ```javascript
  // bad
  ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod,
  ));

  // good
  ['get', 'post', 'put'].map(httpMethod => (
    Object.prototype.hasOwnProperty.call(
  	httpMagicObjectWithAVeryLongName,
      httpMethod,
    )
  ));
  ```

  ​

- 如果你的函数只有单一形参，而且函数体没有使用括号（`{}`），那么，请忽略圆括号。如果不是这种情况，那么，请一直使用圆括号包裹形参，因为这会带来可读性和一致性。请注意：总是使用圆括号也是可以接受的。

  > 为什么呢？更少的视觉可见块。

  ```javascript
  // bad
  [1, 2, 3].map((x) => x * x);

  // good
  [1, 2, 3].map(x => x * x);

  // good
  [1, 2, 3].map(number => (
   `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
  ));

  // bad
  [1, 2, 3].map(x => {
    const y = x + 1;
    return x * y;
  });

  // good
  [1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
  });
  ```

  ​



- 避免把箭头函数 `=>` 和比较操作符 `<=` 和 `>=` 弄混。

  ```javascript
  // bad
  const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

  // bad
  const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

  // good
  const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

  // best
  const itemHeight = (item) => {
    const { height, largeSize, smallSize } = item;
    return height > 256 ? largeSize : smallSize;
  };
  ```

  ​

## 类和构造函数 

  - 在需要使用  `Prototype` 的操作时，都用 `Class` 来取代。

    > 为什么呢？`class` 语法更简洁，且更易理解。

    ```javascript
    // bad
    function Queue(contents = []) {
      this._queue = [...contents];
    }

    Queue.prototype.pop = function () {
      const value = this._queue[0];
      this._queue.splice(0, 1);
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



- 在需要继承时，使用 `extend` 。

  > 为什么呢？这是一种内建的原型继承功能，而且不会破坏 [instanceof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof)

  ```javascript
  // bad
  const inherits = require('inherits');
  function PeekableQueue(contents) {
    Queue.apply(this, contents);
  }
  inherits(PeekableQueue, Queue);
  PeekableQueue.prototype.peek = function () {
    return this.queue[0];
  };

  // good
  class PeekableQueue extends Queue {
    peek() {
      return this.queue[0];
    }
  }
  ```

  ​


- 方法能返回 `this` 帮助进行链式调用。

  ```javascript
  // bad
  function Jedi() {}
  Jedi.prototype.jump = function () {
    this.jumping = true;
    return true;
  };

  Jedi.prototype.setHeight = function (height) {
    this.height = height;
  };

  const luke = new Jedi();
  // 必须得一个一个调用
  luke.jump(); // => true
  luke.setHeight(20); // => undefined

  // good
  class Jedi {
    jump() {
      this.jumping = true;
      return this;
    }
    
    setHeight(height) {
      this.height = height;
      return this;
    }
  }

  const luke = new Jedi();
  luke.jump()
   .setHeight(20);
  ```

  ​


- 手写一个 `toString()` 方法是比较好的，仅仅是为了确保这个方法能正确工作而且没有副作用。

  ```javascript
  class Jedi {
    constructor(options = {}) {
      this.name = options.name || 'no name';
    }
    
    getName() {
      return this.name;
    }
    
    toString() {
      return `Jedi - ${this.getName()}`;
    }
  }
  ```

  ​


- 如果类没有明确指定 `constructor` 也会有一个默认的 `constructor` 。所以，一个空的，或者只是单纯代理父类的构造函数是不必要的。

  ```javascript
  // bad
  class Jedi {
    constructor() {}
    
    getName() {
      return this.name;
    }
  }

  // bad
  class Rey extends Jedi {
    constructor(...args) {
      super(...args);
    }
  }

  // good
  class Rey extends Jedi {
    constructor(...args) {
      super(...args);
      this.name = 'Rey';
    }
  }
  ```

  ​

- 避免重复定义类的属性。

  > 为什么呢？因为重复类的成员属性将会静默使用最后的-能重复本身就是一个bug。

  ```javascript
  // bad
  class Foo {
    bar() { return 1; }
    bar() { return 2; }
  }

  // good
  class Foo {
    bar() { return 1; }
  }

  // good
  class Foo {
    bar() { return 2; }
  }
  ```

  ​

## 模块 

  - 相比使用非标准的模块系统，你应该总是使用像（`import` / `export` ) 这样的模块系统。而且你总能将其编译成你更喜欢的模块系统。

    > 为什么呢？模块化是未来，让我们从现在开始就享受未来。

    ```javascript
    // bad
    const PowerformerStyleGuide = require('./PowerformerStyleGuide');
    module.exports = PowerformerStyleGuide.es6;

    // ok
    import PowerformerStyleGuide from './PowerformerStyleGuide';
    export default PowerformerStyleGuide;

    // best
    import { es6 } from './PowerformerStyleGuide';
    export default es6;
    ```

    ​

  - 不要使用通配符导入。

    > 为什么呢？这能确保你有唯一的，默认导出。(`export default`)

    ```javascript
    // bad
    import * as PowerformerStyleGuide from './PowerformerStyleGuide';

    // good
    import PowerformerStyleGuide from './PowerformerStyleGuide';
    ```

    ​

  - 不要在导入的同时直接导出。

    > 为什么呢？尽管使用一行代码看起来比较简洁，但是有明确的 `import` 和 `export` 使得事物变得连贯。

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './PowerformerStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './PowerformerStyleGuide';
    export default es6;
    ```

    ​

  - 只从一个路径导出一次。

    > 为什么呢？从相同路径导入多次会占多行，这会使得代码非常难以维护。

    ```javascript
    // bad
    import foo from 'foo';
    // ... some other imports ... //
    import { named1, named2 } from 'foo';

    // good
    import foo, { named1, named2 } from 'foo';

    // good
    import foo, {
      named1, 
      named2,
    } from 'foo';
    ```

    ​

  - 不要导出可变的绑定。

    > 为什么呢？可变的绑定一般是要避免的，只有在特殊情况下可能需要，所以只允许不变的引用能导出。

    ```javascript
    // bad
    let foo = 3;
    export { foo };

    // good
    const foo = 3;
    export { foo };
    ```

    ​

  - 如果一个模块只有单一导出，那么相比较 `export` ，使用 `export default ` 更好。

    > 为什么呢？这样能鼓励尽可能多的文件只进行单一导出，这样能提高可读性和可维护性。

    ```javascript
    // bad
    export function foo() {}

    // good
    export default function foo() {}
    ```

    ​

  - 所有的 `import` 语句都在非 `import` 语句前面。

    > 为什么呢？因为 `import` 存在提升，帮他们都放在文件头能避免无法预期的行为。

    ```javascript
    // bad
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // good
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

    ​

  - 多行导入需要换行显示，就像多行数组和对象字面量一样。

    > 为什么呢？花括号应该要遵循这份编程风格指南中其他的每个花括号相同的缩进规则，末尾的逗号也一样。

    ```javascript
    // bad
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // good
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path'
    ```

    ​

  - 不允许在模块导入语句中使用 `webpack loader` 的语法。

    > 为什么呢？在导入语句中使用 `Webpack` 语法会把代码和模块打包工具搅在一起，在`webpack.config.js` 中使用 `loader` 语法更好。

    ```javascript
    // bad
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // good
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

    ​

  - 使用 `import` 代替 `require` 。

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



## 迭代器和生成器

- 不要使用迭代器（`iterators`）。相比使用 `for-in` 或者 `for-of` ，应该更倾向与使用 `JavaScript` 的高阶函数。

  > 为什么呢？这样能确保我们的不变形规则起作用。处理纯函数的返回值比有副作用的函数更加容易。

  > 在遍历数组时，使用 `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()`

  > 在遍历对象时，使用 `Object.keys()` / `Object.values()` / `Object.entries()` 来生成数组，从而取到对象的值。

  ```javascript
  const numbers = [1, 2, 3, 4, 5];

  // bad
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  }
  sum === 15;

  // good
  let sum = 0;
  numbers.forEach((num) => {
    sum += num;
  });
  sum === 15;

  // best (使用了函数的力量)
  const sum = numbers.reduce((total, num) => total + num, 0)
  sum === 15;

  // bad
  const increasedByOne = [];
  for (let i = 0; i < numbers.length; i++) {
    increasedByOne.push(numbers[i] + 1);
  }

  // good
  const increasedByOne = [];
  numbers.forEach((num) => {
      increasedByOne.push(num + 1);
  });

  // best (继续使用函数....)
  const increasedByOne = numbers.map(num => num + 1);
  ```

  ​


- 目前不要使用生成器

  > 为什么呢？他们目前还不能很好的被编译成 ES5


- 如果你非得使用生成器，或者你不太认可我们下面的建议，至少得保证你的函数签名是用空格隔开的。

  > 为什么呢？因为`function` 和 `*` 同一个概念的两个部分， `*` 不是 `function` 的修饰符，`function*` 是一个独立的构造函数，和 `function` 不同。

  ```javascript
  // bad
  function * foo() {
    // ...
  }

  // bad
  const bar = function * () {
    // ...
  };

  // bad
  const baz = function *() {
    // ...
  };

  // bad
  const quux = function*() {
    // ...
  };

  // bad
  function*foo() {
    // ...
  };

  // bad
  function *foo() {
    // ...
  }

  // very bad
  function
  *
  foo() {
    // ...
  }

  // very bad
  const wat = function
  *
  () {
    // ...
  };

  // good
  function* foo() {
    // ...
  }

  // good
  const foo = function* () {
    // ...
  };
  ```

  ​

## 属性

- 使用 `dot notation` 来获取属性。

  ```javascript
  const tom = {
    student: true,
    age: 21,
  };

  // bad 
  const isStudent = tom['student'];

  // good
  const isStudent = tom.student;
  ```

  ​


- 如果你需要使用 `[]` 去获取属性时，使用变量。

  ```javascript
  const tom = {
    student: true,
    age: 21,
  };

  function getProp(prop) {
    return tom[prop];
  }

  const isJedi = getProp('student');
  ```

  ​


- 当计算乘方时，使用 `**` 操作符。

  ```javascript
  // bad
  const binary = Math.pow(2, 10);

  // good
  const binary = 2 ** 10;
  ```

  ​



## 变量

- 总是使用 `const` 和 `let` 来申明变量。如果不这样做，就会导致全局变量。而我们要避免污染全局命名空间。

  ```javascript
  // bad
  superPower = new SuperPower();

  // good
  const superPower = new SuperPower();
  ```

  ​



- 每申明一个变量就使用一个 `const`， `let` 。

  > 为什么呢？这样子很容易添加新的变量，而且你也不需要担心会把 `,` 写成 `;` 或者会因为标点符号的变化而引入版本管理工具的变化。在调试时，你也能每个变量申明都跳一步，而不是跳一次就跳到所有的变量。

  ```javascript
  // bad
  const items = getItems(),
        	goSportsTeam = true,
        	dragonball = 'z';

  // bad
  // 相比上面的，试图指出这样会造成的问题。
  const items = getItems(),
        	goSportsTeam = true;
        	dragonball = 'z';

  // good
  const items = getItems();
  const goSportsTeam = true;
  const dragonball = 'z';
  ```

  ​


- 将所有的 `const` 和 所有的 `let` 分别放在一起。

  > 为什么呢？对于以后你想赋值的变量，这个变量依赖于前一个已经被赋值的变量时，是很有用得。

  ```javascript
  // bad
  let i, len, dragonball,
      item = getItems(),
      goSportsTeam = true;

  // bad
  let i;
  const items = getItems();
  let dragonball;
  const goSportsTeam = true;
  let len;

  // good
  const goSportsTeam = true;
  const items = getItems();
  let dragonball;
  let i;
  let length;
  ```

  ​


- 在需要变量时，给其赋值，当时要把它们放置到合理的地方。

  > 为什么呢？`let` 和 `const` 是块级作用域的，而不是函数作用域。 

  ```javascript
  // bad - 没有必要的函数调用
  function checkName(hasName) {
    // 可能没有必要的
    const name = getName();
    
    if (hasName === 'test') {
      return false;
    }
    
    if (name === 'test') {
      this.setName('');
      return false;
    }
    
    return name;
  }

  // good
  function checkName(hasName) {
    if (hasName === 'test') {
      return false;
    }
    
    const name = getName();
    
    if (name === 'test') {
      this.setName('');
      return false;
    }
    
    return name;
  }
  ```

  ​


- 不要进行变量链式赋值。

  > 为什么呢？链式赋值创造出了隐性全局变量。

  ```javascript
  // bad
  (function example() {
    // JavaScript 引擎将其解释为
    // let a = ( b = ( c = 1 ) );
    // 只有变量 a 是用let申明的，变量 b 和 c 变成了全局变量
    let a = b = c = 1;
  }());

  console.log(a); // throws ReferenceError
  console.log(b); // 1
  console.log(c); // 1

  // good
  (function example() {
    let a = 1;
    let b = a;
    let c = a;
  }());

  console.log(a); // throws ReferenceError
  console.log(b); // throws ReferenceError
  console.log(c); // throws ReferenceError

  // 使用 const 时，情况和上面一致
  ```

  ​


- 避免使用自加和自减（  `++` or `—`  ）。

  > 为什么呢？根据 eslint 的文档，自加和自减语句会自动加入分号，而且在应用中使用自加和自减的值可能会造成静默失败错误。使用像 `num += 1` 这样的语句相比`num++` 或者 `num ++` 会表意更清楚。不允许使用自加和自减同时也避免你无意识的使用 `pre-incrementing / pre-decrementing` 值，这会给程序造成无法预期的行为。

  ```javascript
  // bad
  const array = [1, 2, 3];
  let num = 1;
  num++;
  --num;

  let sum = 0;
  let truthyCount = 0;
  for (let i = 0; i < array.length; i++) {
    let value = array[i];
    sum += value;
    if (value) {
      truthyCount++;
    }
  }

  // good
  const array = [1, 2, 3];
  let num = 1;
  num += 1;
  num -= 1;

  const sum = array.reduce((a, b) => a + b, 0);
  const truthyCount = array.filter(Boolean).length;
  ```

  ​



## 提升 （ `Hoisting`）

- `var` 申明会被提升到作用域顶部，而赋给他们的值不会提升。`const` 和`let` 申明带来了一个概念叫做：临时死区（Temporal Dead Zones（TDZ））。

  ```javascript
  // 我们知道下面是不可能起作用的(假定)
  // 不存在 notDefined 全局变量
  function example() {
    console.log(notDefined); // => throws a ReferenceError
  }

  // 在你引用变量之后创建一个 var 变量会因为
  // 变量提升
  function example() {
    console.log(declaredButNotAssigned); // => undefined
    var declareButNotAssigned = true;
  }
  ```

  ​

# };