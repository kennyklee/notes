# Table of Contents
* [JavaScript: Arrays](#js_arrays)
* [JavaSCript: Helper Functions](#js_helper_functions)
* [JavaScript: ES6](#javascript_-_es6)

# How to read source code
### Why it’s important

1. Most of your time will be spent reading, not writing.
2. Simulates working at a company or open source project.
3. Fastest way to learn.
4. Reading makes you a better writer (just like English).
5. Learn how to ignore large parts of a codebase and get a piece-by-piece understanding.

### Before you start

1. Read the docs (if they exist).
2. Run the code.
3. Play with the app to see what the code is supposed to do.
4. Think about how the code might be implemented.
5. Get the code into an editor.

### The process

1. Look at the file structure.
2. Get a sense for the vocabulary.
3. Keep a note of unfamiliar concepts that you'll need to research later.
4. Do a quick read-through without diving into concepts from #3.
5. Test one feature with the debugger.
6. Document and add comments to confusing areas.
7. Research items in #3 only if required.
8. Repeat.

### Next level

1. Replicate parts of the app by hand (in the console).
2. Make small changes and see what happens.
3. Add a new feature.

### Unfamiliar concepts

1. capture unfamiliar concepts
2. don't waste time. figure out code later.


###Useful links

1. https://github.com/tastejs/todomvc/blob/master/app-spec.md

### Notes

1. Capture notes.
2. More notes.

# JS: 'this' keyword
1. Default:
`this` will be window.

2. In a method:
`this` will point to the object method is on.
but once you go into another function,
this will default back to window.

3. Explicitly define `this`.
Use `bind`, `apply`, `call`.

4. When passing in a callback function, value of `this`
inside callback depends on whether the outer function
explicitly sets a `this` value. If not,
it'll default to window.

5. Constructors
`this` points to the newly created object.

# JS: Array Iterator Methods

```c
var test = function() {};
asdf
// wow pretty cool.
```

```c
var array = [1,2,3,4];
// ADD & REMOVE
array.push(5); // [1,2,3,4,5]
array.pop(); // [1,2,3]
array.shift(); // [2,3,4];
array.unshift('new'); // ['new',1,2,3,4];

// POSITION
array.indexOf('new'); // 0
array[array.indexOf('new')] = 'old'; // ['old',1,2,3,4]

// SELECT - slice()
var newArray = array.slice(2, 4); // [2, 3] - Second arg is for the ending position.
console.log(array); // ['old',1,2,3,4] - Slice does not modify

// EXTRACT - splice()
var newArray = array.splice(3); // [3,4]
var newArray = array.splice(2, 2) // [2,3] - Second arg is for # of positions to splice.
console.log(array); // ['old',1,4] From last line.

// REVERSE
array.reverse(); [4,3,2,1,"old"] - modifies existing array

// COMBINE
var newArray = ['join', 'me'];
array.concat(newArray); // ['old',1,2,3,4,'join','me']
array.join('-'); // old-1-2-3-4
```

# JS: Helper functions

### Open questions:
* What is the difference between 'forEach' and 'map'?
    * forEach typically modifies existing array/object. Map creates a new one.
    * Map is typically used to pluck properties from existing arrays/objects.

### "forEach" function
* Can replace the "for" loop.
* All other helpers can be reimplemented using the forEach.

```c
var colors = [ 'red', 'blue', 'green' ];

// Traditional 'for' loop
for (var i = 0; i < colors.length; i++) {
  console.log(colors[i]);
}

// 'forEach' function
colors.forEach(function(color) {
  console.log(color);
});

// EXAMPLE: Create an array of numbers
var numbers = [1,2,3,4,5];

// Create a variable to hold the sum
var sum = 0;

// Iterator function
function adder(number) {
  sum += number;
}

// Loop over the arrary, incrementing the sum variable
numbers.forEach(adder);

// Print the sum variable
sum;

```

### "map" function
* New array is created
* Pluck records in a list of data and make new array.
* The original array remains immutable.
* Make sure to include 'return' statement.

![Screenshot1](http://d.weblife.us/1bE0V+ "every function")

#### Example #1

```c
var numbers = [1,2,3];

// Make original arrary immutable, and create a new array.

// Original for loop
var oldDoubledNumbers = [];
for (var i = 0; i < numbers.length; i++) {
  oldDoubledNumbers.push(numbers[i] * 2);
}

// Incorrect Kenny's try
var newDoubledNumbers = [];
numbers.map(function(number) {
  newDoubledNumbers.push(number * 2);
})

// Best, correct way.  Make sure to include 'return'.
var doubled = numbers.map(function(number) {
  return number * 2; //common mistake is to forget the return statement.
});

oldDoubledNumbers;
newDoubledNumbers;
doubled;
```

#### Example #2
```c
var cars = [
  { model: 'Buick', price: 'CHEAP' },
  { model: 'Camaro', price: 'expensive' }
];

// Also called 'pluck'
var prices = cars.map(function(car) {
  return car.price;
});

prices;
```

### "filter" function
![](http://d.weblife.us/a8Pp+ "Filter function")
* New array is created
* Only true results are passed to the result array.
* Do not forget the 'return' statement.

#### Example #1
```c

var products = [
  { name: 'cucumber', type: 'vegetable' },
  { name: 'banana', type: 'fruit' },
  { name: 'celery', type: 'vegetable' },
  { name: 'orange', type: 'fruit' }
];

var filteredProducts1 = [];
for (var i=0; i < products.length; i++) {
  if (products[i].type === 'fruit') {
    filteredProducts1.push(products[i]);
  }
}
console.log(filteredProducts1);

var filteredProducts2 = products.filter(function(product) {
  return product.type === 'fruit';
});
console.log(filteredProducts2);
```

#### Example #1
```c
var products = [
  { name: 'cucumber', type: 'vegetable', quantity: 0, price: 1 },
  { name: 'banana', type: 'fruit', quantity: 10, price: 15 },
  { name: 'celery', type: 'vegetable', quantity: 30, price: 9 },
  { name: 'orange', type: 'fruit', quantity: 3, price: 5 }
];

// Type is 'vegetable', quantity is greater than 0, price is less than 10

var filteredProducts = products.filter(function(product) {
    return product.type === 'vegetable'
      && product.quantity > 0
      && product.price < 10
});

console.log(filteredProducts);

```
#### Read-life example
!["Filter" function diagram](http://d.weblife.us/147xW+ "filter function")
```c
var post = { id: 4, title: 'New Post' };
var comments = [
  { postId: 4, content: 'awesome post' },
  { postId: 3, content: 'it was ok' },
  { postId: 4, content: 'neat' },
];

function commentsForPost(post, comments) {
  return comments.filter(function(comment) {
    return comment.postId === post.id;
  });
}

var filteredComments = commentsForPost(post, comments);
console.log(filteredComments);

```

### "find" function
* New array is created
* Search through an array and search for a single item. Very common.
* Iterates through arrary and immediately exits when it finds a truthy value.
* Only the 1st one will be returned, and the rest will be ignored.
!["Find" function diagram](http://d.weblife.us/1c8yv+ "find function")

#### Example 1
```c
var users = [
  { name: 'Jill' },
  { name: 'Alex' },
  { name: 'Bill' },
];

// Old for loop
var user;
for (var i = 0; i < users.length; i++) {
  if (users[i].name === 'Alex') {
    user = users[i];
    break;
  }
}
console.log(user);

// New find loop
var findUser = users.find(function(user) {
 return user.name === 'Alex'; //Don't forget the return
});
console.log(findUser);
```
#### Example 2
```c
function Car(model){
  this.model = model;
}

var cars = [
  new Car('Buick'),
  new Car('Camaro'),
  new Car('Focus')
];

cars.find(function(car) {
  return car.model === 'Focus';
});
```

#### Example 3
```c
var posts = [
  { id: 1, title: 'New Post'},
  { id: 2, title: 'Old Post'}
];

var comment = { postId: 1, content: 'Great Post' };

function postForComment(post, comment) {
  return posts.find(function(post){
    return post.id === comment.postId;
  });
}

postForComment(posts, comment);
```

#### Read-world Scenario
![](http://d.weblife.us/1628R+ "find function")
![](http://d.weblife.us/6Zra+ "find function")

### "every" function
All iterations must be true for it to resolve to true.
Think of '&&' operator.
!["Every" function diagram](http://d.weblife.us/1ec5S+ "every function")
See example below.

### "some" function
Some iterations must be true for it to resolve to true.
Think of '||' operator.
!["Some" function diagram](http://d.weblife.us/EVfK+ "some function")
See example below.

```c
var computers = [
  { name: 'Apple', ram: 24 },
  { name: 'Compaq', ram: 4 },
  { name: 'Acer', ram: 32 }
];

var allComputersCanRunProgram = true;
var onlySomeComputersCanRunProgram = false;

for (var i = 0; i < computers.length; i++) {
  var computer = computers[i];
  if (computer.ram < 16) {
    allComputersCanRunProgram = false;
  } else {
    onlySomeComputersCanRunProgram = true;
  }
}

"---"
allComputersCanRunProgram
onlySomeComputersCanRunProgram;
"+++"

computers.every(function(computer) {
  return computer.ram > 16;
});

computers.some(function(computer) {
  return computer.ram > 16;
});
```
### "reduce" helper
* Can modify or unchange existing array
* reduces an array to a single value

!["reduce" function diagram](http://d.weblife.us/rKXm+ "reduce function")
See example below.

```c
var numbers = [ 10, 20, 30 ];
var sum = 0;

// Traditional for loop
for (var i = 0; i < numbers.lenght; i++) {
  sum += numbers[i];
}

// Using reduce
numbers.reduce(function(sum, numbers){ // sum is the intital value, and numbers is the array
  return sum + number;
}, 0);  // 0 is the initial sum value.  Can be any value

```

```c
var primaryColors = [
  { color: 'red' },
  { color: 'yellow' },
  { color: 'blue'}
];

primaryColors.reduce(function(previous, primaryColor) { //previous is initial value.
  previous.push(primaryColor.color);
  return previous;
}, []); // Empty object is the initial
```
COMMON whiteboard question: Are the paranthese balanced?
"()()()()" // YES
"(((())))" // YES
")))" // NO
"(()" // NO
")(" // NO

```c
function balancedParens(string) {
  return !string.split("").reduce(function(previous, char) { //'!' turns it into boolean
    if (previous < 0) { return previous; }
    if (char === "(") { return ++previous; }
    if (char === ")") { return --previous; }
    return previous;
  }, 0);
}

```

Write a function called 'unique' that will remove all the duplicate values from an array.

For example, given the following array:

```c
var numbers = [1, 1, 2, 3, 4, 4];
```

Your function should return
`[1, 2, 3, 4]`
Hint: Use the 'reduce' and 'find' helpers.

3 different solutions below. The last 'indexOf' is the preferred way.

```c
function unique(array) {
 return array.reduce(function(newArray,number){
     if(newArray.includes(number) === false) {
         newArray.push(number);
     }
     return newArray;
 },[]);
}

function unique(array) {
  return array.reduce(function(previous, element){
    var inPrevious = previous.find(function(inPrevious){
       return inPrevious === element;
    });
    if (!inPrevious) {
        previous.push(element)
    }
        return previous;
  }, [])
}

// Preferred method
function unique(array) {
    return array.reduce(function(previous, element){
      if (previous.indexOf(element) === -1) {
          previous.push(element);
      }
      return previous;
     },[])
 }

```

# JavaScript - ES6

## LET & CONSTANT
* It's more legible.  Easy to eye/see/understand which variables change, and which don't.
* `let` and `const` are block scoped.  Different than ES5.

```c
if  (x > 0){
  let y;
  let const;
}
<!-- cannot access `let` or `const` -->
```

* `constant` is immutable.  Cannot be reassigned once defined.  However, the properties of `constant` can be changed.  e.g., `const person; person.age = '30';`
* If need to completely make immutable, include properties, then use `const me = Object.freeze(person);`
* This 1st code snippet won't work as expected.  Use the 2nd snippet with `let` to make it work.

```c
for(var i = 0; i < 10; i++) {
  console.log(i);
  setTimeout(function() {
      console.log('The number is ' + i);
    }, 1000)
}

<!-- To fix this, use `let` -->

for(let i = 0; i < 10; i++) {
  console.log(i);
  setTimeout(function() {
      console.log('The number is ' + i);
    }, 1000)
}
```
* Hoisting difference:
```c
console.log(person);  undefined
var person = 'Kenny';

console.log(person2);  Uncaught ReferenceError: person2 is not defined
let person2 = 'Kenny';  same with `const`
```

## TEMPLATE STRINGS
* Wrap in backticks \` blah blah \`
* Variables are in ${var}, instead of the plus (+) to concatenate.
* **OLD:** "This is " + var1 + "and this is " + var2;
* **NEW:** \`This is ${var1} and this is ${var2}\`

## ARROW FUNCTIONS (Syntatic sugar)
#### Fat arrow functions

```c
// ES5
const add = function(a, b) {
  return a + b;
}

// ES6 Fat Arrow
const add = (a, b) => {
   return a + b;
 }

// ES6 Abbreviated for single expression. Implicit return.
const add = (a, b) => a + b;

// ES6 for single arguement, can drop the paranthesis.  Compact syntax.
const double = number => 2 * number;

// Another example combined with destructuring
const profile = {
  title: 'Engineer',
  department: 'Engineering'
};

// Before ES6
function isEngineer(profile) {
  var title = profile.title;
  var department = profile.department;
  return title === 'Engineer' && department === 'Engineering';
}

// 1 liner: Fat arrow with destructuring
const isEngineer = ({ title, department }) => title === 'Engineer' && department === 'Engineering';
```

* Fat arrow functions also solves the 'this' context binding issues.
* It uses 'lexical this'.

## DEFAULT FUNCTION ARGUMENTS

```c
function ajaxRequest(url, method = 'GET') {
  // logic to make request
}

ajaxRequest('google.com', null); // method is null (no value)
ajaxRequest('google.com', undefined); // method is 'GET'
ajaxRequest('google.com', 'POST'); // method is 'POST'
```

## REST & SPREAD OPERATOR

```c
// REST OPERATOR
function addNumbers(...numbers) { //Turns the arguments into an array
  return numbers.reduce((sum, number) => {
    return sum + number;
  }, 0);
}

addNumbers(1,2,3,4,5);

// SPREAD OPERATOR
const defaultColors = ['red', 'green'];
const userFavoriteColors = ['orange', 'yellow'];
const fallColors = ['fire red', 'fall orange'];

[ 'blue', 'green', ...fallColors, ...defaultColors, ...userFavoriteColors ];
//["blue","green","fire red","fall orange","red","green","orange","yellow"]

[ 'blue', 'green', fallColors, defaultColors, userFavoriteColors ];
// ["blue","green",["fire red","fall orange"],["red","green"],["orange","yellow"]]

// Another SPREAD example
function join(array1, array2) {
  return [ ...array1, ...array2];
}


// Point to another function for legacy purposes.
const MathLibrary = {
  calculateProduct(...rest) {
    console.log('Please use the multuply method instead');
    return this.multiply(...rest);
  },
  multiply(a, b) {
    return a * b;
  }
}

MathLibrary.calculateProduct(2, 3);

```

## DESTRUCTURING

```c
var expense = {
  type: 'Business',
  amount: '$45 USD'
};

//var type = expense.type;
//var amount = expense.amount;

// ES6
// Pull out the property and creates the variable/constant.
const { type } = expense;
const { amount } = expense;

// Also can do
const { type, amount } = expense;

//Array destructuring
const companies = [
  'Google',
  'Facebook',
  'Uber'
]

const firstCompany = companies[0]; // 'Google' assigned to 'firstCompany'
const [ name ] = companies;  // 'Google' assigned to 'name'
const { length } = companies; // Get length (3) from companies

// Combining with spread operators.
const { name, ...rest } = companies;
// name = 'Google'.  Grabs 1st variable.
// ...rest = ['Facebook', 'Uber']

//Another example
const companies = {
  { name: 'Google', location: 'Mountain View' },
  { name: 'Facebook', location: 'Menlo Park' },
  { name: 'Uber', location: 'San Francisco' }
}

// Gets first variable, then destructures.
const [{ location }] = companies; //'Mountain View'
```

####Real-life examples

```c
// Destructured and the ORDER of arguments do not matter anymore.
function signup({ username, password, email, dateOfBirth, city }) {
  //create new user
}


// other code

const user = {
  username: 'myname',
  password: 'mypassword',
  email: 'myemail@example.com',
  dateOfBirth: '1/1/1900',
  city: 'New York'
};

signup(user);

// ANOTHER REAL-LIFE

// Received data
const point = [
  [4, 5],
  [10, 1],
  [0, 40]
];

// Need in the following format
const point = [
  {x: 4, y: 5},
  {x: 10, y: 1},
  {x: 0, y: 40}
];

// Solution
points.map(([x, y]) => {
  return { x: x, y: y};
});

// ANOTHER DESTRUCTURING EXAMPLE - ARRAYS

const classes = [
  [ 'Chemistry', '9AM', 'Mr. Darnick' ],
  [ 'Physics', '10:15AM', 'Mrs. Lithun'],
  [ 'Math', '11:30AM', 'Mrs. Vitalis' ]
];

//ES5
const classesAsObject = classes.map(function([subject, time, teacher]) {
  return { subject: subject, time: time, teacher: teacher };
});

//ES6
const classesAsObject = classes.map(([subject, time, teacher]) => ({subject, time, teacher}) );
```

## CLASSES
####ES5 Example

```c
function Car(options) {
  this.title = options.title;
}

Car.prototype.drive = function() {
  return 'vroom';
}

function Toyota(options) {
  Car.call(this, options);
  this.color = options.color;
}

Toyota.prototype = Object.create(Car.prototype);
Toyota.prototype.constructor = Toyota;

Toyota.prototype.honk = function() {
  return 'beep';
}

const toyota = new Toyota({ color: 'red', title: 'driver' });
console.log(toyota);
console.log(toyota.drive());
console.log(toyota.honk());
```

####ES6 Example

```c
class Car {
  constructor({ title }) {
    this.title = title;
  }
  // No comma needed
  drive() {
    return 'vroom';
  }
}

class Toyota extends Car {
  constructor(options) {
    super(options);
    this.color = options.color;
  }

  honk() {
    return 'beep';
  }
}

const car = new Car({ title: 'Toyota' });
car.drive();

const toyota = new Toyota({ color: 'red' });
console.log(toyota.honk());
console.log(toyota.drive());
console.log(toyota);

```

## GENERATORS
Functions where we can enter and exit multiple times.
When you declare a function, we use the star.
```c
function* shopping() {
  // stuff on the sidewalk

  // walkting down the sidewalk

  // go into the store with cash
  const stuffFromStore =  yield 'cash';
  // exists to after 'gen.next()' below.
  //groceries get passed in...becomes
  // const stuffFromStore = 'groceries'

  // walking to laundry place
  const cleanClothes = yield 'laundry';
  // const cleanClothes = 'clean clothes';

  // walking back home;
  return [stuffFromStore, cleanClothes]
}

// stuff in the store
const gen = shopping();
gen.next(); // leaving our house
// walked into the store
// walking up and down the aisles.
// purchase our stuff.

gen.next('groceries'); //leaving the store with groceries
gen.next('clean clothes');

// ANOTHER EXAMPLE
function* colors() {
  yield 'red';
  yield 'blue';
  yield 'green';
}

const myColors = [];

for (let color of colors()) {
  myColors.push(color);
}

console.log(myColors);

// ANOTHER EXAMPLE
const engineeringTeam = {
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave',
}

function* TeamIterator(team) {
  yield team.lead;
  yield team.manager;
  yield team.engineer;
}

const names = [];

for (let name of TeamIterator(engineeringTeam)) {
  names.push(name);
}

console.log(names); // ["Jill", "Alex", "Dave"]

// ANOTHER EXAMPLE (Generator delegation - trap door)
const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill',
};

const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave',
};

function* TeamIterator(team) {
  yield team.lead;
  yield team.manager;
  yield team.engineer;
  const testingTeamGenerator = TestingTeamIterator(team.testingTeam);
  yield* testingTeamGenerator; // Kinda like a trap door to the other iterator.
}

function* TestingTeamIterator(team) {
  yield team.lead;
  yield team.tester;
}

const names = [];

// How do we combine the function generators?
for (let name of TeamIterator(engineeringTeam)) {
  names.push(name);
}

console.log(names); //["Jill","Alex","Dave","Amanda","Bill"]

// REFACTORING ABOVE
// Issues - 2 functions, and have to call the 1st function.
// Symbol iterator is a tool that teaches objects how to respond to 'for of' loop.
// I don't want a separate function to handle testingTeam object.

const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill',
  [Symbol.iterator]: function* () {
    yield this.lead;
    yield this.tester;
  }
};

const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave',
};

function* TeamIterator(team) {
  yield team.lead;
  yield team.manager;
  yield team.engineer;
  yield* team.testingTeam; // Kinda like a trap door to the other iterator.
}

const names = [];

// How do we combine the function generators?
for (let name of TeamIterator(engineeringTeam)) {
  names.push(name);
}

console.logs(names);

// REFACTOR FURTHER
const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill',
  [Symbol.iterator]: function* () {
    yield this.lead;
    yield this.tester;
  }
};

const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave',
  // Special ES6 object. Job is to tell 'for of' loop to how to loop.
  // Arrays already have iterator, but objects will need iterator.
  [Symbol.iterator]: function* () {
    yield this.lead;
    yield this.manager;
    yield this.engineer;
    // Fall through
    yield* this.testingTeam;
  }
};

const names = [];

// How do we combine the function generators?
for (let name of engineeringTeam) {
  names.push(name);
}

console.log(names);
```

#### Real-world example
Using reddit comment tree as an example
```c
class Comment {
  constructor(content, children) {
    this.content = content;
    this.children = children;
  }

  // Iterate through the child comments
  *[Symbol.iterator]() {
    yield this.content;
    for (let child of this.children) {
      yield* child;
    }
  }
}

// Child - child comments
const children = [
  new Comment('good comment', []),
  new Comment('bad comment', []),
  new Comment('meh', []),
];

// Parent comment (child comments above)
const tree = new Comment('Great post!', children);

const values = [];
for (let value of tree) {
  values.push(value);
}

console.log(values);
```

## PROMISES
3 states

* pending/unresolved (status)
* resolve (status)
    * then (callback)
* reject (status)
    * catch (callback)

```c
promise = new Promise((resolve, reject) => {
    resolve(); //Or resolve().  These are native functions.
});

// Only callled when resolved.  My preferred formatting
promise
    .then(() => {
        console.log('finally finished!');
    })
    .then(() => {
        console.log('also ran...second callback');
    })

// Same above refactored
promise
    .then(() => console.log('finally finished!'))
    .then(() => console.log('also ran...second callback'))
    .catch(() => console.log('uh oh!!')) // only runs on reject();
```

#### Most common use case with 'fetch'

```c
url = 'https://jsonplaceholder.typicode.com/posts/';

fetch(url) // Promise comes back with 'pending' status.
    .then(response => response.json()) //This is a complaint.  Recommend Axio, SuperAgent
    .then(data => console.log(data));

// show a weakness in fetch
fetch(url)
    .then(respnse => console.log(response))
    .catch(error => console.log('BAD', error));

// With fetch any status code above 300 does NOT trigger an error.
// fetch/catch applies only if the url is completely bad, does not resolve at all.

// User Axios, SuperAgent
```