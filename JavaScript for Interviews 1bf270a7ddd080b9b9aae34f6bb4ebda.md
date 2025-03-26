# JavaScript for Interviews

# “use-strict”;

In JavaScript, `"use strict"` is a directive that enables **strict mode**. Strict mode is a way to opt into a restricted version of JavaScript, which helps you avoid common coding mistakes and unsafe actions. When strict mode is enabled, JavaScript will apply stricter parsing and error handling on your code.

## What is a callback in JavaScript?

A **callback** is a **function that is passed as an argument to another function** and gets executed **later**, usually after some operation is completed.

```jsx
// Basic Example

function greet(name, callback) {
  console.log(`Hello, ${name}!`);
  callback(); // Execute the callback function
}

function sayGoodbye() {
  console.log("Goodbye!");
}

greet("Alice", sayGoodbye);

// Ouput:

// Hello, Alice!
// Goodbye!
```

![image.png](image.png)

![image.png](image%201.png)

![image.png](image%202.png)

_____________________________________________________________________________________________________________________

# **Var vs Let vs Const**

Scoping: refers to the accessibility of variables within different parts of a program.

- Global Scope: Variables declared ouside functions or blocks, which means they are accesible from anywhere within the program.
- Function Scope: Variables declared inside a function are only accesible within that function.
- Block Scope: Varibles declared inside a block { } are only accesible within that block. For example Let and Const

Var is function scope, meaning:

- If declared inside a function, it is only accessible within that function.
- If declared outside any function, it becomes a **global variable**.
- Unlike `let` and `const`, `var` does **not** have block scope. It ignores `{}` blocks and can be accessed outside them (except within functions).
- Variables declared with `var` are hoisted but **initialized as `undefined`** until their assignment.

```jsx
const message = () => {
  var x = 5;
  console.log(x);
};

message();

console.log(x);

// Output: 
// 5
// error x is not defined
```

Let and Const are block scope.

```jsx
const message = () => {
  if (true) {
    let x = 5;
  }
  console.log(x);
};

message();

// Output: Error x is mpt defined because let is block scope and console log is trying to access x outside the scope.

// If it was declared with var, the ouput would be 5
```

Also:

- **`var` can be redeclared** in the same scope.
- **`let` and `const` cannot be redeclared** in the same scope.

# Shadowing

Variable shadowing happens when a variable declared in an inner scope (like inside a function or block) **has the same name as a variable in an outer scope**, making the outer variable **temporarily inaccessible** inside the inner scope.

```
let x = 10; // Global variable

function example() {
  let x = 20; // Shadows the global x
  console.log(x); // ✅ 20 (Inner x is used)
}

example();
console.log(x); // ✅ 10 (Outer x is still accessible)

// 20
// 10

```

**Illegal Shadowing**

****If you try to shadow a var variable by using let is going to work fine, but if you try to shadow let variable by using var is going to give the error.

Which means: 

- You **cannot** shadow a `let` or `const` variable with `var` in the same scope.

```jsx
function test() {
  var a = "Hello";
  let b = "Bye";
  
  if(true) {
    let a = "Hi";
    var b = "Goodbye";
    console.log(a);
    console.log(b);
  }
} 

test();

// Hi
// Syntax Error b has already been declared
```

### **Key Takeaways**

1. **Shadowing allows redefining variables in a smaller scope** without affecting the outer one.
2. **Be careful when shadowing `var` inside `let`/`const`**, as it can cause errors.
3. **Shadowing is different from reassigning**—it creates a **new variable** instead of modifying the existing one.

# Declaration without initialization

```jsx
var a; // // can be declared with no value
```

```jsx
let b; // can be declared with no value
```

```jsx
const c; // error: const declaration needs to be initialized
```

# Re-Inicialization

var and let can be updated but const cannot be updated.

```jsx
var x = 10;
x = 20; // ✅ Allowed
console.log(x); // 20

let y = 30;
y = 40; // ✅ Allowed
console.log(y); // 40

const z = 50;
z = 60; // ❌ Error: Assignment to constant variable
console.log(z);
```

_____________________________________________________________________________________________________________________

# Hoisting

**Hoisting** is JavaScript's default behavior of **moving variable and function declarations to the top of their scope** during the **creation phase** of execution.

![image.png](image%203.png)

![image.png](image%204.png)

![image.png](image%205.png)

![image.png](image%206.png)

![image.png](image%207.png)

![image.png](image%208.png)

![image.png](image%209.png)

## JavaScript Execution Context

The **Execution Context** is the environment where JavaScript code runs. It determines how functions, variables, and objects are stored and accessed..

![image.png](image%2010.png)

![image.png](image%2011.png)

![image.png](image%2012.png)

# Temporal Dead Zone

![image.png](image%2013.png)

![image.png](image%2014.png)

With const it would be same behavior and with var it would console undefined as the variable would be hoisted and will have undefined assingned to it,

![image.png](image%2015.png)

_____________________________________________________________________________________________________________________

# Map, Filter and Reduce

### They are array methods, they are used to iterate over an array and perform a transformation or computation, each may or may not return an array based of the result of the function.

# What is map?

### The map method is used for creating a new array from existing one by applying a function to each one of the elements of the first array.

```jsx
// Example: multiply of element of the given array by 3.

const nums = [1,2,3,4]

const multiplyByThree = nums.map((num, i, arr) => {
	return num * 3;
});

console.log(multiplyByThree);	

// map will generate a new array from an existing one by applying a function to each element of the original array.

// map takes 3 parameters:
// num is a name for each element of the original array.
// i for the index of each array element
// arr is the name of the new array created from the original.

```

## Lets create the map method from scratch:

```jsx
Array.prototype.myMap = function(callback) {
  let newArray = []; // This will store the transformed elements

  for (let i = 0; i < this.length; i++) {
  -
    // Call the callback function with (currentElement, index, array)
    newArray.push(callback(this[i], i, this));
  }
  return newArray; // Return the transformed array
};

// Example of usage

const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.myMap((num) => num * 2);

console.log(doubled); // Output: [2, 4, 6, 8, 10]
```

![image.png](image%2016.png)

![image.png](image%2017.png)

![image.png](image%2018.png)

![image.png](image%2019.png)

![image.png](image%2020.png)

# What is filter?

### Applies a condition or statement to each element of the array, if the condition is true the element gets posted into the output array, if condition is false, the element is not posted on the ouput array. So filter ensures to only return those elements that satisfy the provided criteria.

```jsx

// Example: Return elements greater than 2 from the given array.

const nums = [1,2,3,4]

const moreThanTwo = nums.filter((num, index, array) => {
   return num > 2;
})

console.log(moreThanTwo);

// filter takes the same arguments as map()
```

## Lets create the filter method from scratch:

The difference between map and filter is that map is going to return each and every value and will modify it, but filter will only return those values which satisfy the condition of the callback. 

So as an argument, the custom map function will receive a callback as parameter, like this: 
**Array.map( (num, i ,arr ) ⇒ { } );**

```jsx

Array.prototype.myFilter = function(callback) {
  let newArray = []; // Step 1: Create a new array to store filtered values

  for (let i = 0; i < this.length; i++) { // Step 2: Loop through each element in the array
    if (callback(this[i], i, this)) { // Step 3: Apply callback and check if the condition is true
      newArray.push(this[i]); // Step 4: If true, add the element to the new array
    }
  }

  return newArray; // Step 5: Return the filtered array
};

// Usage example

const nums = [1, 2, 3, 4, 5];

const moreThanTwo = nums.myFilter((num) => {
	return num > 2;
});

console.log(moreThanTwo);

// Output: [3,4,5]
```

![image.png](image%2021.png)

![image.png](image%2022.png)

![image.png](image%2023.png)

# What is Reduce?

## The `reduce()` method in JavaScript is used to iterate over an array and reduce it to a single value. It executes a **callback function** on each element of the array, accumulating the result as it progresses.

The `reduce()` method accepts **two parameters**:

1. **Callback Function** (Required)
2. **Initial Value** (Optional)

```jsx
const sum = numbers.reduce((acc, curr, index, array) => {
	//DO SOMETHING
	return acc + curr;
}, 0);
```

### **Parameters:**

1. **Callback Function**: The function inside `reduce()` is called for each element in the array. It has **4 parameters**:
    - **`acc`**: The **accumulator** is used to hold the running accumulated result as you process each element. Reduce keeps updating this parameter after each iteration. The accumulated value starts with initialValue or the first array element.
    - **`curr`**: The **current element** of the array that the `reduce()` function is currently processing.
    - **`index`**: The **index** of the current element (starting from `0`).
    - **`array`**: The **original array** that `reduce()` is working on.
    
2. **Initial Value**: The `0` after the callback function is the **initial value** of the `accumulator`. This means that the first value of `acc` will be `0`, and the iteration will start from the first element in the array (`1`).

Examples:

![image.png](image%2024.png)

![image.png](image%2025.png)

# Use case examples of reduce()

```jsx
// Example: Return the sum of a given array

const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce((acc, curr) => acc + curr, 0);

console.log(sum); // Output: 15

// The accumulator (acc) starts at 0 (initialValue).
// Each number is added to acc in every iteration.
```

```jsx
// Find Maximum Value

const numbers = [10, 5, 20, 8];

const max = numbers.reduce((acc, curr) => (curr > acc ? curr : acc), numbers[0]);

console.log(max); // Output: 20

// The accumulator starts as the first element (10).
// The function compares each value with acc, keeping the highest.
```

```jsx
// Counting Occurences in an Array.

const fruits = ["apple", "banana", "apple", "orange", "banana", "apple"];

const count = fruits.reduce((acc, curr) => {
  acc[curr] = (acc[curr] || 0) + 1;
  return acc;
}, {});

console.log(count);
// Output: { apple: 3, banana: 2, orange: 1 }

// The accumulator starts as an empty object {}.

// It tracks the count of each fruit by updating the object.
```

## Lets create the reduce method from scratch:

Reduce takes 2 parameters, a callback and the initial value.

```jsx
arr.reduce( () => {}, initialValue)
```

The callback has the accumulator, the current value, the index and the array.

```jsx
arr.reduce( (acc, curr, i, arr) => {}, initialValue)
```

Creating the reduce method from scratch.

```jsx

Array.prototype.myReduce = function(callback, initialValue) {

	let accumulator = initialValue;
	
  for (let i = 0; i < this.length; i++) { // Step 2: Loop through each element in the array
  
	  // check if an initialValue is provided:
	  
	  accumulator = accumulator ? callback(accumulator, this[i], i, this) : this[i];
	  
	  /** So if there's anything inside accumulator, then we initilize the callback with the respective
			  accumulator value, the current value, the index and the array. Else, we will assign the 
			  accumulator the first element of the array.
		**/ 
  }
  
  // Then, when the loop ends means we have defined a value for the accumulator, so just return it

  return accumulator; // Step 5: Return the filtered array
};

// Usage example

const nums = [1, 2, 3, 4, 5];

const sum = nums.myReduce((acc, curr, i, arr) => {
	return acc + curr;;
}, 0);

console.log(sum);

// Output: 15
```

_____________________________________________________________________________________________________________________

## Difference between map vs forEach

### Both are array functions to loop throught the items of the array, but they serve different purposes:

Code Example on both:

```jsx
const arr = [2,5,3,4,7];

const mapResult = arr.map((ar) => {
	 return ar + 2;
});

const forEachResult = arr.forEach((ar) => {
	 return ar + 2;
});

console.log(mapResult, forEachResult);

// Output:  [ 4, 7, 5, 6, 9 ] undefined

// Map returned a new array but forEach didn't returned anything but undefined.

```

### Difference between map and forEach:

![image.png](image%2026.png)

## So if you want to use forEach, you would have to use it like this.

```jsx
// So if you want to use forEach, you would have to use it like this.

const arr = [2,5,3,4,7];

const forEachResult = arr.forEach((ar, i) => {
	arr[i] = ar + 3;
});

console.log(arr)

// Output: [ 5, 8, 6, 7, 10 ]
```

## Interview exercise using map, for, forEach, filter, reduce.

The interviewer provides an array of students with information of each student, 

```jsx
let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];
```

Let’s say the interviewer ask you to return only the name of students in capital letters.

So, to resolve this question you could use the traditional for, or you could use forEach or you can use map.

**Approach using the traditional for:**

```jsx
let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];

let names = [];

for(let i = 0; i < students.length; i++) {
	names.push(students[i].name.toUpperCase());
}

console.log(names);
```

**Approach using Map:**

```jsx

let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];

const names = students.map(stu => stu.name.toUpperCase());

console.log(names);
```

**Approach using ForEach:**

```jsx
let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];

let names = []; // Create an empty array to hold the results

students.forEach(stu => {
	names.push(stu.name.toUpperCase()); // Use forEach to perform the operation and push to the array
});

console.log(names); // Output: ["ERICK", "JENNY", "REGINA", "ARTHUR"]

```

## Question 2: Return only details of those who scored more than 60.

```jsx
let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];

const details = students.filter((stu) => {
   return stu.marks > 60; 
})

console.log(details);

// Ouput
// [ { name: 'Erick', rollNumber: 31, marks: 80 }, { name: 'Jenny', rollNumber: 15, marks: 69 } ]
```

## Question 3: Return only details of those who scored more than 60 marks and rollNumber is greater than 15.

```jsx
let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];

const details = students.filter((stu) => stu.marks > 60 && stu.rollNumber > 15); 

console.log(details);

// Output

// [ { name: 'Erick', rollNumber: 31, marks: 80 } ]
```

## Question 4: Return the sum of marks of all students.

```jsx
let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];

const sum = students.reduce((acc, curr) => acc + curr.marks, 0);

console.log(sum);
```

## Chaining methods:

## Question 5: Return only names of students who scored more than 60.

```jsx
let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];

//This will return all the student's with marks greater than 60

const names = students.filter((stu) => stu.marks > 60)
console.log(names);

// This will return only the names of those with marks greater than 60.

const names2 = students.filter((stu) => stu.marks > 60).map((stu) => stu.name);

console.log(names2);
```

## Question 6: Return total marks for students with marks greater than 60 after 20 marks have been added to those who scored less than 60.

```jsx
let students = [
	{ name: "Erick", rollNumber: 31, marks: 80 },
	{ name: "Jenny", rollNumber: 15, marks: 69 },
	{ name: "Regina", rollNumber: 16, marks: 35 },
	{ name: "Arthur", rollNumber: 7, marks: 55 },
];

const totalMarks = students.map((stu) => {
	if (stu.marks < 60) {
		stu.marks += 20;
	}
	return stu;
});

console.log(totalMarks);

// Step 1: Until now, this is just going to add 20 to those who have marks less than 60.

/* Output: [{ name: 'Erick', rollNumber: 31, marks: 80 }, { name: 'Jenny', rollNumber: 15, marks: 69 },,
 { name: 'Regina', rollNumber: 16, marks: 55 }, { name: 'Arthur', rollNumber: 7, marks: 75 }] */
 
 
 
 // Step 2: Now we have to filter those with marks greater than 60.
 
 const totalMarks = students.map((stu) => {
 
 		// Add 20 marks to students who scored less than 60

	if (stu.marks < 60) {
		stu.marks += 20;
	}
	return stu;
}).filter((stu) => stu.marks > 60);

console.log(totalMarks);

/* Output: [ { name: 'Erick', rollNumber: 31, marks: 80 }, { name: 'Jenny', rollNumber: 15, marks: 69 },
					 { name: 'Regina', rollNumber: 16, marks: 75 }, { name: 'Arthur', rollNumber: 7, marks: 75 } */ ]
					
					
	
	// Step 3: Calculate the sum of marks
	
	 const totalMarks = students.map((stu) => {
		if (stu.marks < 60) {
			stu.marks += 20;
		}
		return stu;
	}).filter((stu) => stu.marks > 60).reduce((acc, curr) => acc + curr.marks, 0);
	
	console.log(totalMarks);

// Output: 224
	
```

__________________________________________________________________________________

# TypeScript

## TypeScript is JavaScript with sintax for types. It is a strongly typed programming language that builds on JavaScript, giving you better tooling at any scale.

![image.png](image%2027.png)

### TypeScript se compila, ya que el navegador lo que entiende es JavaScript.

One difference between TypeScript and JavaScript is that JavaScript is a weakly typed and dynamic typed programming language.

For example with JavaScript:

```jsx
let a = 'hola';  //String
a = 2; // number

console.log(typeof a) // number

/* Here we are seeing the dynamic typed nature of JavaScript,
	 by letting the programmer change the type of variable with no issue.
	 
	 Also we are seeing JavaScript is weakly typed as we never asigned 
	 a variable type to a. JavaScript assing types dinamicly.
	 
	 That does not happen with TypeScript.
*/
```

### Same example using TypeScript

```jsx
let a = 'hola';  //String
a = 2; // number

console.log(typeof a) 
// Error: Type 'number' is not assignable to type 'string'.
```

### So with TypeScript you can have type annotations, fo example:

```jsx
//With JavaScript
function suma (a,b) {
	return a + b;
}
suma("Hola",3);
// Output: Hola3

// With TypeScript

function suma (a: number, b: number) {
	return a + b;
}
console.log(suma(1,3)); //4
```

## IMPORTANT: TypeScript does not work at runtime, this is because browsers only understand JavaScript so your TypeScript code first needs to be compilated.

![image.png](image%2028.png)

You can see this more clearly on the playground

TypeScript:  

![image.png](image%2029.png)

JavaScript:

![image.png](image%2030.png)

When TypeScript is compiled, you can see JavaScript does not read the types, so everything about types disappears at execution time / runtime.

### TypeScript Infer:

TypeScript is capable of detecting your javaScript errors even without writing a single line with TypeScript, that means if you do something like this with an object:

```jsx
const persona = {
	name: "Pepe",
	age: 30,
}

persona
```

### If you hover on persona TypeScript would infer the properties of the object you are creating as well as it’s types. So, hovering on persona will show:

![image.png](image%2031.png)

So it is not strickly necesaary to assign types on some variables like a number, string or boolean , cause TypeScript would directly infer them.  Like this:

```jsx
const text = "Hola"

/* TypeScript would directly understand that the variable text is of type string without the
 necessity of assigning directly a type. 
 
 Obviusly if you try to asign a new type to this variable you would get a type erro.
*/

text = 5;  // Type error, text is already asigned with String type
```

## Let list all the types available in TS.

![image.png](image%2032.png)

### **Defining an array in TS**

![image.png](image%2033.png)

### **Defining an Object in TS**

![image.png](image%2034.png)

### **Defining a Function in TS**

![image.png](image%2035.png)

# ANY in TS

### **If you do not asign a type to your variables they will become of type Any. For example:**

### let a;

![image.png](image%2036.png)

### So **any** does not really indicates that  your variable can have any type as the name would suggest.

### Instead if you define an **any** value, you are saying to basicly ignore typescript on that particular variable.

### So if you are defining an **any** type on your code, your saying that basicly that variable con be a string, a number, boolean etc, without any type of control.

### **So in short terms, any will ignore the type, even if you have assigned it to a string like in the following example.**

### So toUpperCase would not really understand you are trying to uppercase a string and the editor won’t autocomplete with possible string methods as the editor would not know if it actually is a string.

![image.png](image%2037.png)

### Always avoid implicit any in your code.

______________________________________________________________________________________________________________

## **Functions in TS**

![image.png](image%2038.png)

## If you have an object as parameter within your function…

### Once could think the logic way of doing this is like this:

```jsx
function saludar({ name: string. age: number}) {
	
	console.log(`Hola ${name}, tienes ${age} años`)
}

saludar({ name: 2, age: 'Pepe' })
```

## BUT THIS SINTAXIS IN WRONG, because it interfere with JavaScript syntax.

### So there are 2 ways of doing this:

```jsx
function saludar({ name, age }: { name: string, age: number }) {
	
	console.log(`Hola ${name}, tienes ${age} años`)
}

saludar({ name: "Pepe", age: 2 })
```

### The second way is—-

![image.png](image%2039.png)

### Functions also typed thir return value. Notice that TS is powerful enough to infer the type of the return value as well:  You can do it like this:

### In the following example, TypeScript has already infer the return value must be a number, so assign it a return value of type string would get a type parsing error.

![image.png](image%2040.png)