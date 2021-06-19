# Description 😋

> This is a collection of JavaScript tips and tricks. you can refer to it and apply it to make your code more concise. **But don't overdo it**, it can make your code difficult to read and maintain. Hope everyone contributes, thanks.

<!-- table of content -->

# Table Of Content 📃

- [Description](#description)
- [Table Of Content](#table-of-content)
- [Array](#array)
- [Object](#object)
- [Destructuring](#destructuring)
- [Operator](#operator)
- [Comparison](#comparison)
- [Others](#others)

<!-- Tips for array -->

# Array

<details >
  <summary>
    1. Generate an Array
  </summary>

- Create an empty array of length **`n`**

  ```js
  var arr = new Array(3);

  // result: arr = [undefined, undefined, undefined]
  ```

- Create an empty array of length **`n`** & fill value **`x`**

  ```js
  var arr = [...Array(3).fill(1)];
  var arr2 = [...Array(5).fill(1, 0, 3)];

  /* 
    result: arr = [1, 1, 1]
            arr2 = [1, 1, 1, undefined, undefined]
  */
  ```

- Create an array containing `0...n`

  ```js
  var arr = [...Array.keys(5)];

  // result: arr = [0, 1, 2, 3, 4]
  ```

- Create an array containing `1...n`

  ```js
  var arr = [];
  for (let i = 0; arr.push(++i) < 4; );

  var arr2 = Array.from({ length: 4 }, (_, i) => i + 1);
  var arr3 = Array.from({ length: 4 }, (_, i) => i * 2);
  var arr4 = Array.from({ length: 4 }, () => Math.random());

  /* 
    result: arr =  [1, 2, 3, 4]
            arr2 = [1, 2, 3, 4]
            arr3 = [0, 2, 4, 6]
            arr4 = [0.211, 0.5123, 0.612, 0.8921]
  */
  ```

</details>

<details >
  <summary>
    2. Extract Unique Values of Array
  </summary>

<br />

```js
var arr = [1, 2, 2, 3, 5, 5, 4];
var newArr = [...new Set(arr)];

// result: newArr = [1, 2, 3, 5, 4]
```

</details>

<details >
  <summary>
    3. Shuffle Elements from Array
  </summary>

<br />

```js
var arr = [1, 2, 3, 4, 5];
var newArr = arr.sort(() => Math.random() - 0.5);

// result: newArr = [3, 1, 2, 4, 5]
```

</details>

<details >
  <summary>
    4. Flatten a Multidimensional Array
  </summary>

<br />

```js
var arr = [1, [2, 3], [4, 5, 6], 7];
var newArr = [].concat(...arr);

// result: [1, 2, 3, 4, 5, 6, 7]
```

</details>

<details >
  <summary>
    5. Resize an Array
  </summary>

> The length array isn't a read only property.

```js
var arr = [1, 2, 3, 4, 5];
arr.length = 2;

var arr2 = [1, 2, 3, 4, 5];
arr2.length = 0;

var arr3 = [1, 2, 3, 4, 5];
arr3.length = 7;

/*
  result: arr = [1, 2]
          arr2 = []
          arr3 = [1, 2, 3, 4, 5, undefined, undefined]
*/
```

</details>

<details >
  <summary>
    6. Random an Item in Array
  </summary>

<br />

```js
var arr = [2, 4, 5];
var item = arr[Math.floor(Math.random() * arr.length)];
```

</details>

<details >
  <summary>
    7. Remove an Item from Array
  </summary>

<br />

```js
var arr = [1, 2, 3];

// Not Recommended
delete arr[1]; // arr = [1, undefined, 3], length = 3

// Recommended
arr.splice(1, 1); // arr = [1, 3], length = 2
```

</details>

# Object

<details >
  <summary>
    1. Dynamic Property Name
  </summary>

  <br/>

```js
const dynamic = 'age',
	dynamicValue = 18;

var obj = {
	name: 'Dyno',
	[dynamic]: dynamicValue,
};

// result: obj = { name: 'Dyno', age: 18 }
```

</details>

<br />

<details >
  <summary>
    2. Clone an Object
  </summary>

- Shallow copy `(Not Recommended)`

  > Use the `=` operator to copy object 1 into object 2. These 2 objects point to the same memory area `(reference)`. Therefore, if we change object 1, object 2 will also change.

  ```js
  var obj1 = { a: 1, b: 2 };
  var obj2 = obj1; // obj2 = { a: 1, b: 2 }

  obj1.a = 3; // change value of a property

  console.log(obj1); // { a: 3, b: 2 }
  console.log(obj2); // { a: 3, b: 2 } => property a of obj2 changed 🙂❗
  console.log(obj3); // { a: 3, b: 2 } => property a of obj2 changed 🙂❗
  ```

- Deep copy

  > **Way 1**: Use Spread operator `{...}` or `Object.assign()` to fix "Shallow copy". **_Issue:_** `Nested objects` still have shallow copy problem.

  ```js
  var obj1 = { a: 1, b: 2, c: { nested: 3 } };
  var obj2 = { ...obj1 }; // obj2 = { a: 1, b: 2, c: { nested: 3 } }
  var obj3 = Object.assign({}, obj1); // obj3 = { a: 1, b: 2, c: { nested: 3 } }

  obj1.b = 3;
  obj1.c.nested = 4;

  console.log(obj1); // { a: 1, b: 3, c: { nested: 4 } }
  console.log(obj2); // { a: 1, b: 2, c: { nested: 4 } } 🙂
  console.log(obj3); // { a: 1, b: 2, c: { nested: 4 } } 🙂
  ```

  > **Way 2 `(Recommended)`**: Use `JSON.stringify()` & `JSON.parse()` to solve the above problems.

  ```js
  var obj1 = { a: 1, b: 2, c: { nested: 3 } };
  var obj2 = JSON.parse(JSON.stringify(obj1)); // obj2 = { a: 1, b: 2, c: { nested: 3 } }

  obj1.b = 3;
  obj1.c.nested = 4;

  console.log(obj1); // { a: 1, b: 3, c: { nested: 4 } }
  console.log(obj2); // { a: 1, b: 2, c: { nested: 3 } } 😉😘
  ```

<br />

</details>

# Destructuring (ES6+)

<details>
  <summary>
    1. With Array
  </summary>

  <br/>

```js
var [a, b] = [1, 2];
// same: var a = 1, b = 2;

var [a, b, c] = [1, 2, 3, 4, 5];
// same: var a = 1, b = 2, c = 3;

var [a, , c] = [1, 2, 3, 4, 5];
// same: var a = 1, c = 3
// ignore values

var [a, b, ...rest] = [1, 2, 3, 4, 5];
// same: var a = 1, b = 2, rest = [4, 5]
// use "rest params ES6"

var [a, b, c] = [1, 2];
// same: var a = 1, b = 2, c = undefined;

var [a, b = 0, c = 0] = [1, 2];
// same: var a = 1, b = 2, c = 0;
// declare and set default value

var [a, b, [c, d], e] = [1, 2, [3, 4], 5];
// same: var a = 1, b = 2, c = 3, d = 4, e = 5
// nested array destructuring
```

</details>

<details>
  <summary>
    2. With Object
  </summary>

  <br/>

```js
var person = { name: 'Dyno', age: 18 };

var { name, age } = person;
// same: var name = person.name, age = person.age;

var { name = 'Anonymous', age = 1, address = 'HCM city' } = person;
// same: var name = person.name, age = person.age, address: 'HCM city'
// declare and set default value

var { name: personName, age: personAge } = person;
// same: var personName =  person.name, personAge = person.age
// decleare and change variable name

console.log({ name, age });
// same: console.log({ name: name, age: age })

var person = { name: 'Dyno', age: 18, infor: { address: 'HCM', phone: '123' } };
var {
	name,
	age,
	infor: { address, phone },
} = person;
// same: name = person.name, age = person.agem, address = person.infor.address, phone = person.infor.phone
// nested object destructuring
```

</details>

# Operator

<details>
  <summary>
    1. Optional chaining (?.)
  </summary>

<br/>

> "The optional chaining operator `?.` enables you to read the value of a property located deep within a chain of connected objects without having to check that each reference in the chain is valid." [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

  <br/>

```js
const person = {
	name: 'Dyno',
	age: 18,
	sayHello: function () {
		console.log('Hello');
	},
};

// ❗ Wrong way
console.log(person.infor.address); // ❌ Uncaught TypeError: Cannot read property 'address' of undefined

// ✅ Right way (check condition)
if (person.infor) console.log(person.infor.address); // Not log

// ✅ Right way (use ?.)
console.log(person.infor?.address); // undefined

// Optional chaining with function calls
console.log(person.sayHello?.()); // Hello
console.log(person.callPhone?.()); // undefined

// A chain Optional chaining
console.log(person.infor?.address?.province?.name); // undefined
```

```js
// syntax
obj.val?.prop;
obj.val?.[expr];
obj.arr?.[index];
obj.func?.(args);
```

</details>

<details>
  <summary>
    2. Nullish coalescing operator (??)
  </summary>

  <br/>

> "The nullish coalescing operator `??` is a logical operator that returns its right-hand side operand when its left-hand side operand is `null` or `undefined`, and otherwise returns its left-hand side operand." [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)

```js
var a = null ?? 'Default'; // a = 'Default'
var a = false ?? 'Default'; // a = false
```

</details>

<details>
  <summary>
    3. Logical OR (||)
  </summary>

```js
var a = 1,
	b = 2;

if (a > 2 || b > 1) console.log('Dyno');

// result: Dyno
```

> The OR operator `||` is a logical operator that returns its right-hand side operand when its left-hand side operand is `falsy`, and otherwise returns its left-hand side operand.

  <br/>

```js
var a = null || 'Default'; // a = 'Default'
var a = false || 'Default'; // a = 'Default'
```

</details>

<details>
  <summary>
    4. Logical AND (&&)
  </summary>

  <br/>

```js
let a = true,
	b = true,
	c = false;

if (a && b) console.log('Hello'); // Hello (a, b = true)

if (a && c) console.log('Dyno'); // not log (c = false)

// other usage
function sayHi() {
	console.log('Hi');
}

a && sayHi(); // Hi
c && sayHi(); // false
```

</details>

<details>
  <summary>
    5. Double tilde operator (~~)
  </summary>

  <br/>

```js
let num = 2.6;
console.log(~~num); // 2 = Math.floor(2)
```

</details>

<details>
  <summary>
    6. Logical Assignment Operator ES12  (||=, ??=) 
  </summary>

  <br/>

```js
a ||= b; // same a = a || b;
a ??= b; // same a = a ?? b;
```

</details>

<details>
  <summary>
    7. Numeric separator ES12 (_)
  </summary>

  <br/>

```js
const n = 1_000_000_000; // same: n = 1000000000;
```

</details>

# Comparison

<details>
  <summary>
    1. Use === instead of ==
  </summary>

<br/>

> The operator `== (!=)` will automatically cast if 2 variables are not of the same type, then compare. The `=== (!==)` operator compares the value and the type => `===` faster than `==`.

<br/>

```js
  1 == '1' // true
  1 === '1' // false

  0 == false // true
  0 === false // false

  '' == false // true
  '' === false // false

  [] == 0 // true
  [] === 0 // false

```

</details>

<details>
  <summary>
    2. The difference between isNaN() and Number.isNaN() 
  </summary>

<br/>

> The `isNaN()` method (is Not a Number ?) use to check if a variable is **a Number**. The `Number.isNaN()` (is NaN ?) method use to check if a variable is **NaN**

<br/>

```js
isNaN('string');
// true, 'string' is not Number

isNaN([]);
// true, [] is not Number

isNaN(0 / 0);
// true, 0/0 is not Number

isNaN(1);
// false, 1 is Number

Number.isNaN('string');
// false, 'string' is not NaN

Number.isNaN([]);
// false, [] is not NaN

Number.isNaN(0 / 0);
// true, 0/0 is NaN

Number.isNaN(NaN);
// true
```

</details>

# Others

<details>

  <summary>
    1. Swapping use Destructuring
  </summary>

  <br/>

```js
let a = 1,
	b = 2;

[a, b] = [b, a];

// result: a = 2, b = 1;
```

</details>

<details>
  <summary>
    2. Create function that returns only 1 object
  </summary>

  <br/>

```js
const fn = () => ({ obj: 1 });

/*
  same: const fn = () => {
    return { obj: 1 }
  }
*/
```

</details>

<details>
  <summary>
    3. Immediately Invoked Function Expression (IIFE)
  </summary>

  <br/>

> The function will execute automatically when you create it.

  <br/>

```js
  // Way 1:
  var res = ()(function(){
    // do something...
    console.log("Hello");
    return true;
  })();
  // result: Hello, res = true;

  // Way 2:
  var res = (() => {
    console.log('Hello');
    return true;
  })();
  // result: Hello, res = true;
```

</details>

<details>
  <summary>
    4. typeof vs instanceof 
  </summary>

  <br/>

> `typeof`: return a string that represents the primitive type of a variable.

> `instanceof`: check in all the prototypes chain the constructor it returns true if it’s found and false if not.

  <br/>

```js
var arr = [1, 2, 3];
console.log(typeof arr); // "object"
console.log(arr instanceof Array); // true

typeof 1; // "number"
typeof NaN; // "number"
typeof 'str'; // "string"
typeof true; // "boolean"
typeof {}; // "object"
typeof []; // "object"
typeof null; // "object"
typeof undefined; // "undefined"
typeof function name() {}; // "function"
```

</details>

<details>
  <summary>
    5. Falsy
  </summary>

  <br/>

> A `Falsy value` is a value that is considered false when encountered in a Boolean context . [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)

> Complete list of JavaScript falsy values ` false, 0, -0, 0n, "", '', ``, NaN, null, undefined, document.all`

> Falsy value bypass the if block. Ex:

<br/>

```js
if (null) {
} else {
	console.log('Falsy');
}

const a = undefined || 'Falsy';

// result: Falsy, a = "Falsy"
```

> Filter out Falsy values

<br/>

```js
const arr = [1, 'Dyno', false, 0, true, NaN, 2000];
var filteredArr = arr.filter(Boolean);

// result: filteredArr = [1, "Dyno", true, 2000]
```

</details>

<details>
  <summary>
    6. Template string `${}`
  </summary>

  <br/>

```js
const name = 'Dyno';
const hello1 = 'Hello ' + name + ', how are you?';
const hello2 = `Hello ${name}, how are you?`; // template string
```

</details>

<details>
  <summary>
    7. Rounding number to n decimal place

  </summary>

  <br/>

```js
var num = 25.0420001;
console.log(typeof num); // "number"

num = num.toFixed(2); // num = "25.04"
console.log(typeof num); // ❗ "string"
```

</details>
	
	
	
	
	
# Tính năng nổi bật từ ES2015(ES6) ======-> nay

## ES 2015 (ES6)

### 1. Arrow function expressions
 * Arrow function có thể thay thế cho biểu thức hàm thông thường trong một số trường hợp 

 ```javascript
 const array = [
     1,
     2
 ];

 console.log( array.map( value => value *2 ) );

 // result [2, 4];
 ```

### 2. Enhanced object  literals
* Object literals được mở rộng để hỗ trợ thiết lập cấu trúc đối tượng 
```javascript
// old define object
var a = 1, b = 2, c = 3;
var obj = {
    a : a,
    b : b,
    c : c
};
// obj.a = 1, obj.b = 2, obj.c = 3
// new define in Es6

const a = 1, b = 2, c = 3 ;

var obj = {
    a,
    b, 
    c
}
// obj.a = 1, obj.b = 2, obj.c = 3
```

### 3. Classes 
* Chúng ta có thể tạo class(lớp) mới sử dụng từ khóa  **class**  
```javascript
class Profile {   
   constructor(surname, firstname= '') { // class constructor
      this.surname= surname;
      this.firstname= firstname;     
   }  
    
   getName() { // class method       
     console.log(`Full Name: ${this.surname} ${this.firstname}`);    
   } 
}
let obj = new Profile('patel', 'atit');
obj.getName(); // Full Name: patel atit
```

### 4. Template Literals 
* chúng ta có thể sử dụng biến trong chuỗi bằng cánh sử dụng ``` `${variable}` ```

```javascript
const name = `Your name is ${surname} ${firstname}.`
```
### 5. Destructuring Assignment
* Cú pháp này giúp giải nén các giá trị từ mảng hoặc thuộc tính từ các đối tượng.

```javascript
var data = {name: 'atit', surname: 'patel'};
var {name, surname} = data;
console.log(name); // atit
console.log(surname); // patel
```
### 6.  Default + Rest + Spread

* Bây giờ, hàm hỗ trợ giá trị mặc định, tham số spread(dạng ...param) hoặc mảng dưới dạng đối số. 
```javascript
function add(a, b=2) {
  // b is 2 if not passed (or passed as undefined)
  return a + b;
}
add(2) == 4
function test(a, ...b) {
  // b is an Array
  return a * b.length;
}
test(3, "test", true) == 6
function add(a, b, c) {
  return a + b + c;
}
// Pass each element of array as argument
add(...[1,2,3]) == 6
```
### 7.  Let + Const
* 1 biến có phạm vi sử dụng ( block scope variable) khi khai báo với  let hoặc const.  

```javascript
function check() {
  {
    let test;
    {
      // okay, block scoped name
      const test = "abc";
      // error, const
      test = "bvc";
    }
    // error, already declared in block
    let test = "testsdd";
  }
}
```
### 8. Promises
 * Dùng giải quyết các vấn đề về bất đồng bộ 

```javascript
var asyncCall =  new Promise((resolve, reject) => {
   // do something async 
   resolve();
}).then(()=> {   
   console.log('done!');
})
```
### 9. Modules
* Chúng ta có thể export biến hoặc method vơi modules   

```javascript
export var param = 1; 
export function add() {   
   //code  
};
```
*  Và có thể import biến hoặc thêm method sử dụng import 

```javascript
import {param, add} from 'module';
console.log(param); // 1
``` 
### 10. Map + Set + WeakMap + WeakSet
* Cấu trúc hiệu quả cho các thuật toán phổ biến  

```javascript
// Sets
var data = new Set();
data.add("test").add("abc").add("cde");
s.size === 2;
s.has("test") === true;
// Maps
var testmap  = new Map();
testmap.set("test", 12);
testmap.set(s, 3);
testmap.get(s) == 3;
// Weak Maps
var testWeakmap = new WeakMap();
testWeakmap.set(s, { extra: 1 });
testWeakmap.size === undefined
// Weak Sets
var testWeakSet = new WeakSet();
testWeakSet.add({ data: 5 });
// Because the added object has no other references, it will not be held in the set
```
### 11. Math + Number + String + Array + Object APIs

```javascript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false
"abcde".includes("cd") // true
"test".repeat(3) // "testtesttest"
Array.from(document.querySelectorAll('*')) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...)
[1, 4, 3].find(x => x == 4) // 4
[1, 3, 4].findIndex(x => x == 3) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"
Object.assign(data, { origin: new data(0,0) })
```
## ES 2016

### 12. Array.prototype.includes()
* Check mảng có giá trị nào đó hay không return boolean 

```javascript
const data = [1, 2, 3];
console.log(array1.includes(2));
// expected output: true
```
### 13. Exponentiation
* Lũy thừa với ``` a**b ```
```javascript
console.log(3 ** 4);
// expected output: 81
```
## ES 2017
### 14. Object.entries()
* Phương thức này trả về một mảng gồm các cặp khóa và giá trị đối tượng nhất định. 
```javascript
const data = {
  name: 'atit',
  surname: 'patel'
};
for (const [key, value] of Object.entries(data)) {
  console.log(`${key}: ${value}`);
}
// expected output:
// "name: atit"
// "surname: patel"
// order is not guaranteed
```

### 15.  Object.values()
* Phương thức này trả về một mảng các giá trị thuộc tính của đối tượng đã cho.  
```javascript
const data = {
  name: 'atit',
  surname: 'patel'
};
console.log(Object.values(data));
// expected output: Array ["atit","patel"]
```
### 16. padStart()
* Phương thức này thêm vào đầu  chuỗi hiện tại bằng một chuỗi khác cho đến khi chuỗi kết quả đạt đến độ dài. 
```javascript
const test1 = '1';
console.log(test1.padStart(2, '0'));
// expected output: "01"
const data = '2024359082125581';
const last4Digits = fullNumber.slice(-3);
const maskedNumber = last4Digits.padStart(data.length, '*');
console.log(maskedNumber);
// expected output: "*************581"
```
### 17. padEnd()
* Phương thức này thêm vào cuối  chuỗi hiện tại bằng một chuỗi khác cho đến khi chuỗi kết quả đạt đến độ dài. 
```javascript
const test1 = 'data data';
console.log(test1.padEnd(12, '.'));
// expected output: "data data..."
```
### 18.  Object.getOwnPropertyDescriptors()
* Phương thức này trả về tất cả các bộ mô tả thuộc tính riêng của một đối tượng nhất định 
```javascript
const data = {
  property: 12
};
const descriptors1 = Object.getOwnPropertyDescriptors(data);
console.log(descriptors1.property.writable);
// expected output: true
console.log(descriptors1.property.value);
// expected output: 12
```
### 19. Async functions
* **Async** functions mở rộng ** Promises** để thực hiện các cuộc gọi không đồng bộ.
```javascript
async function test() {
  const
    response1 = await test1(),
    response2 = await test2(response1),
    response3 = await test3(response2);
}
```
* ** await ** àm cho mỗi lệnh gọi xuất hiện như thể nó đồng bộ trong khi không giữ chuỗi xử lý đơn lẻ của JavaScript  

## ES 2018
### 20. Asynchronous Iteration
* Với sự giúp đỡ của ** async ** và ** await **  chúng ta có thể chạy chuỗi lặp không đồng bộ trong vòng lặp ** for **. 
```javascript
async function test(array) {
  for await (let i of data) {
    callMethod(i);
  }
}
```
### 21.  Promise.finally()
* ** finally ** phương thức trả về một lời hứa khi nó được giải quyết hoặc bị từ chối. Nó sẽ giúp tránh sao chép sau đó và bắt các trình xử lý. 
```javascript
function callMethod() {
     test1()
     .then(test2)
     .then(test3)
     .catch(err => {
       console.log(err);
     })
     .finally(() => {
       // finished here
     });
   }
```
### 22. Rest/Spread Properties
* Chúng ta có thể sử dụng rest/spread functionality  cho đối tượng và ** destructuring  ** và mảng 
```javascript
const test = {
  a: 1,
  b: 2,
  c: 3
};
const { a, ...x } = test;
// a = 1
// x = { b: 2, c: 3 }
```

* Hoặc chúng ta có thể sử dụng nó để chuyển các giá trị cho một hàm: 
```javascript
test({
  a: 1,
  b: 2,
  c: 3
});
function test({ a, ...x }) {
  // a = 1
  // x = { b: 2, c: 3 }
}
```
* Giống như mảng, chúng ta chỉ có thể sử dụng một tham số nghỉ duy nhất ở cuối khai báo 
* Toán tử spread có thể được sử dụng trong các đối tượng khác. Ví dụ:

```javascript
const test1= { a: 1, b: 2, c: 3 };
const test2 ={ ...test1, d: 4 };
// obj2 is { a: 1, b: 2, c: 3, d: 4 }
```
### 23. Regular Expression Named Capture Groups
* Bây giờ chúng ta có thể nhóm để được đặt tên bằng cách sử dụng ký hiệu ```? <name> ```sau dấu ngoặc mở.
```javascript
const
  reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,
  match  = reDate.exec('2021-01-04'),
  year   = match.groups.year,  // 2021
  month  = match.groups.month, // 01
  day    = match.groups.day;   // 04
```
* có thể được sử dụng với phương thức thay thế để định dạng ngày tháng.
```javascript
const
  reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,
  d      = '2021-01-04',
  usDate = d.replace(reDate, '$<month>-$<day>-$<year>');
```

### 24. Regular Expression s (dotAll) Flag
* Dấu chấm ```.``` Regex. khớp với bất kỳ ký tự đơn nào ngoại trừ ký tự xuống dòng. Flag ``` s ``` thay đổi hành vi này để các dấu cuối dòng được phép. Ví dụ: 
```javascript
/hi.test/s.test('hi\ntest'); // true

```
### 25.  Regular Expression Unicode Property Escapes
* Chúng tôi có thể đặt các thoát thuộc tính Unicode bằng bộ flag Unicode u và \ p {…} và \ p {…}
```javascript
const reGreekSymbol = /\p{Script=Greek}/u;
reGreekSymbol.test('π'); // true
```

## ES 2019
### 26.  Array.flat()
* Phương thức này tạo một mảng mới bằng cách kết hợp (combining) các mảng khác trong mảng chính. Lưu ý: chúng ta có thể thiết lập độ sâu để kết hợp các mảng.
```javascript
const test = [0, 1, 2, [3, 4]];
console.log(test.flat());
// expected output: [0, 1, 2, 3, 4]
const test2 = [0, 1, 2, [[[3, 4]]]];
console.log(test2.flat(2));
// expected output: [0, 1, 2, [3, 4]]
```
### 27. Array.flatmap
* Phương thức này tạo một mảng mới bằng cách áp dụng hàm gọi lại(call back) cho mỗi phần tử của mảng. Nó cũng giống như một bản đồ với hoạt động phẳng. 
```javascript
let test = [1, 2, 3, 4];
test.map(x => [x * 2]);
// [[3], [6], [9], [12]]
test.flatMap(x => [x * 3]);
// [2, 4, 6, 8]
// only one level is flattened
test.flatMap(x => [[x * 3]]);
// [[3], [6], [9], [12]]
```
### 28. Object.fromEntries()
* Phương thức này biến đổi danh sách các cặp khóa-giá trị thành một đối tượng (Object). 
```javascript
const entries_data = new Map([
  ['test', 'data'],
  ['test1', data2]
]);
const object = Object.fromEntries(entries_data);
console.log(object);
// expected output: Object { test: "data", test1: data2}
```
### 29.  String.trimStart() & String.trimEnd()
* ``` trimStart() ``` phương thức loại bỏ khoảng trắng từ đầu một chuỗi. 
```javascript
const test = '   test   ';
console.log(test.trimStart());
// expected output: "test   ";
```
* ``` trimEnd() ``` phương thức loại bỏ khoảng trắng từ cuối một chuỗi. 
```javascript
const test = '   test   ';
console.log(test.trimEnd());
// expected output: "   test";
```
### 30.try…catch
* ``` try…catch ``` câu lệnh đánh dấu một khối câu lệnh để thử và nếu có lỗi xảy ra, bắt sẽ xử lý nó 
```javascript
try {
  noMethodFound();
} catch (error) {
  console.error(error);
  // expected output: ReferenceError: noMethodFound is not defined
}

```

### 31. Function.toString()
* Phương thức này chuyển đổi bất kỳ phương thức / code nào thành chuỗi
```javascript
function multiply(a, b) {
  return a * b;
}
console.log(multiply.toString());
// expected output: "function multiply(a, b) {
//                     return a * b;
//                   }"

```
### 32. Symbol.prototype.description
* Thuộc tính này trả về một mô tả tùy chọn về đối tượng ``` Symbol  ```  
```javascript
console.log(Symbol('desc').description);
// expected output: "desc"
console.log(Symbol.iterator.description);
// expected output: "Symbol.iterator"
console.log(Symbol.for('test').description);
// expected output: "foo"
console.log(`${Symbol('test').description}data`);
// expected output: "testdata"

```
## ES2020
### 33. BigInt
* ** BigInt  ** là một đối tượng tích hợp cung cấp cách biểu diễn các số (toàn bộ) lớn hơn 253–1 có thể được biểu diễn bằng hằng số Number.MAX_SAFE_INTEGER 
```javascript
const previousMaxSafe = BigInt(Number.MAX_SAFE_INTEGER)
// ↪ 9007199254740991n
const maxPlusOne = previousMaxSafe + 1n
// ↪ 9007199254740992n
const theFuture = previousMaxSafe + 2n
// ↪ 9007199254740993n, this works now!
const previouslyMaxSafeInteger = 9007199254740991n
const alsoHuge = BigInt(9007199254740991)
// ↪ 9007199254740991n

```

### 34. Dynamic Import
* Dynamic Import cung cấp tùy chọn nhập động tệp JS dưới dạng mô-đun. Nó sẽ giúp bạn nhận được các mô-đun theo yêu cầu
* Một lợi thế nữa là khi chúng ta import mô-đun, do đó nó sẽ không làm ô nhiễm không gian tên chung 
```javascript
import('/modules/test-module.js')
  .then((module) => {
    // logic for using module
  });

```
* Biểu mẫu này cũng hỗ trợ từ khóa await 
```javascript
let module = await import('/modules/test-module.js');
```
### 35. Nullish coalescing Operator
* ``` nullish coalescing Operator ??  ``` được trả về giá trị phía bên phải nếu phía bên trái là ** null  ** hoặc ** undefined  **. Theo mặc định, nó sẽ trả về giá trị bên trái. 
```javascript
const test= null ?? 'default';
console.log(test);
// expected output: "default"
const test1 = 0 ?? 2;
console.log(test1);
// expected output: 0

```
### 36. globalThis
* ** globalThis  ** thuộc tính chứa giá trị này toàn cục, về cơ bản hoạt động như một đối tượng toàn cục. 
```javascript
function canMakeHTTPRequest() {
  return typeof globalThis.XMLHttpRequest === 'function';
}
console.log(canMakeHTTPRequest());
// expected output (in a browser): true

```
###  37. Promise.allSettled()
* **  Promise.allSettled() ** phương thức trả về một promise  về cơ bản chứa mảng đối tượng với kết quả của mỗi promise . 
```javascript
const promise1 = Promise.resolve(2);
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 200, 'test'));
const promises_data = [promise1, promise2];
Promise.allSettled(promises_data).
  then((val) => val.forEach((val) => console.log(val.status)));
// expected output:
// "fulfilled"
// "rejected"

```

### 38. Optional Chaining
* ```  optional chaining operator?  ``` ọc giá trị với bất kỳ đối tượng được kết nối nào hoặc phương pháp kiểm tra và kiểm tra xem thuộc tính có tồn tại hay không. Nó sẽ trả về không xác định nếu tham chiếu là null hoặc không có sẵn. 
```javascript
const data = {
  name: 'patel',
  sub_data: {
    name: 'atit'
  }
};
const sub_data = data.sub_data?.name;
console.log(sub_data);
// expected output: undefined
console.log(data.abc?.());
// expected output: undefined

```
### 39. String.prototype.matchAll()
* ``` matchAll() ``` phương thức trả về một iterator  của tất cả các kết quả khớp với một chuỗi với regex. 
```javascript

const regexp = /t(e)(st(\d?))/g;
const str = 'test1test2';
const array = [...str.matchAll(regexp)];
console.log(array[0]);
// expected output: Array ["test1", "e", "st1", "1"]
console.log(array[1]);
// expected output: Array ["test2", "e", "st2", "2"]
```


### 40. Named Export
* Với tính năng này, chúng tôi có thể có nhiều bản export được đặt tên cho mỗi tệp. Chúng tôi có thể xác định một tên mới với từ “as” và import hoặc export nếu cần. 
```javascript
// imports
// ex. importing a single named export
import { TestComponent} from "./TestComponent";
// ex. importing multiple named exports
import { TestComponent2 as abcComponent } from "./TestComponent";
// exports from ./TestComponent.js file
export const MyComponent = () => {}
export const abcComponent= () => {}

```
* Import tất cả các bản export đã đặt tên vào một đối tượng: ``` * ``` 
```javascript
import * as TestComponents from "./TestComponent";
// use TestComponents.TestComponent and    TestComponents.TestComponent2 here

```


### 41. . Well defined for-in order
* ECMA không chỉ định thứ tự cho (x in y) sẽ chạy. Mặc dù trước đây các trình duyệt đã tự thực hiện một trật tự nhất quán nhưng bây giờ nó đã được chuẩn hóa chính thức trong ES2020 


### 42. import.meta
* Đối tượng này hiển thị siêu dữ liệu theo ngữ cảnh cụ thể cho một mô-đun JS. Về cơ bản, nó chứa thông tin về mô-đun. 
```javascript

<script type="module" src="test-module.js"></script>

```

* bạn có thể truy cập thông tin meta về mô-đun bằng cách sử dụng đối tượng import.meta 
```javascript
console.log(import.meta);
 // { url: "file:///home/path/test-module.js" }

```
## ES2021 
### 43. replaceAll
* ** replaceAll() ** phương thức trả về một chuỗi mới với tất cả các kết quả phù hợp của một mẫu được thay thế bằng từ thay thế mới.
* Mẫu có thể là chuỗi hoặc Regexp 
```javascript
const p = 'my name is atit';
const regex = /atit/gi;
console.log(p.replaceAll(regex, 'patel'));
// expected output: my name is patel
console.log(p.replaceAll('atit', 'patel'));
// expected output: my name is patel

```
### 44. Promise.any()
* Nó cần một đối tượng Promise có thể lặp lại và khi một lời hứa hoàn thành, trả về mộtpromise  duy nhất với giá trị. 
```javascript
const promise1 = Promise.reject(0);
const promise2 = new Promise((resolve) => setTimeout(resolve, 50, 'fast'));
const promise3 = new Promise((resolve) => setTimeout(resolve, 100, 'slow'));
const promises = [promise1, promise2, promise3];
Promise.any(promises).then((value) => console.log(value)); //fast

```


### 45. Weakref
* Đối tượng này giữ một tham chiếu yếu đến một đối tượng khác mà không ngăn đối tượng đó được thu gom rác. 
* Link VD: Link : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef 
```javascript
class Counter {
  constructor(element) {
    // Remember a weak reference to the DOM element
    this.ref = new WeakRef(element);
    this.start();
  }

  start() {
    if (this.timer) {
      return;
    }

    this.count = 0;

    const tick = () => {
      // Get the element from the weak reference, if it still exists
      const element = this.ref.deref();
      if (element) {
        element.textContent = ++this.count;
      } else {
        // The element doesn't exist anymore
        console.log("The element is gone.");
        this.stop();
        this.ref = null;
      }
    };

    tick();
    this.timer = setInterval(tick, 1000);
  }

  stop() {
    if (this.timer) {
      clearInterval(this.timer);
      this.timer = 0;
    }
  }
}

const counter = new Counter(document.getElementById("counter"));
setTimeout(() => {
  document.getElementById("counter").remove();
}, 5000);

```



### 46.FinalizationRegistry
* Cho phép bạn yêu cầu gọi lại khi một đối tượng là rác được thu thập. 
* Link VD: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry 

### 47.FinalizationRegistry
* Các thuộc tính của lớp là public theo mặc định và có thể được kiểm tra hoặc sửa đổi bên ngoài lớp. Tuy nhiên, có một đề xuất ở giai đoạn 3 để cho phép xác định các trường private  bằng cách sử dụng``` #<property_name> ```. 
```javascript
class TestClass {
  #privateVariable
}
class TestClass {
  #privateMethod() {
    return 'test'
  }
}
class TestClass {
  static #PRIVATE_STATIC_FIELD
}

```

### 48.Logical Operators
* Toán tử gán OR (x || = y) logic chỉ gán nếu x là sai. 
```javascript
const a = { duration: 50, title: '' };
a.duration ||= 10;
console.log(a.duration);
// expected output: 50
a.title ||= 'title is empty.';
console.log(a.title);
// expected output: "title is empty"

```
* Toán tử gán logic AND (x && = y) chỉ gán nếu x là true. 
```javascript
let a = 1;
let b = 0;
a &&= 2;
console.log(a);
// expected output: 2
b &&= 2;
console.log(b);
// expected output: 0

```

### 49. Numeric Separators
* Đây là tính năng giúp chúng ta đỡ đau khi đọc các số lớn. 
```javascript
const value = 1000000000000;
 // Có một tính năng trong JavaScript cho phép gạch dưới làm dấu phân cách trong các ký tự số để cải thiện khả năng đọc
const value = 1_000_000_000_000;
//Bạn cũng có thể sử dụng nó cho các số nhị phân, bát phân và hex.

const n1 = 0b1010_0101_1001; // binary
const n2 = 0o2_3_5_7; // octal
const n3 = 0xA_B_C_D_E; // hex
```

### 50. Intl.ListFormat
* Đối tượng này cho phép định dạng danh sách nhạy cảm với ngôn ngữ. 
```javascript
const data = ['abc', 'test', 'cde'];
const formatter1 = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });
console.log(formatter.format(data));
// expected output: "abc, test, and cde"
const formatter2 = new Intl.ListFormat('en', { style: 'narrow', type: 'unit' });
console.log(formatter3.format(data));
// expected output: "abc test cde"

```

### 51.  Intl.DateTimeFormat
* Đối tượng này cho phép định dạng ngày và giờ nhạy cảm với ngôn ngữ. 
```javascript
// Specify default date formatting for language (locale)
console.log(new Intl.DateTimeFormat('en-US').format(date));
// expected output: "01/04/2021"
// Specify date and time format using "style" options (i.e. full, long, medium, short)
console.log(new Intl.DateTimeFormat('en-GB', { dateStyle: 'full', timeStyle: 'long' }).format(date));
// Expected output "Monday, 04 January 2021 at 14:23:16 GMT+11"

```


