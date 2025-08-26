
## Object Destructuring

```
const user = {
  id: 123,
  name: "Alex",
  email: "alex@example.com"
};

const id = user.id;
const name = user.name;

console.log(name); // Output: Alex
```

**With destructuring**, you can pull those properties out in one line:

```
const user = {
  id: 123,
  name: "Alex",
  email: "alex@example.com"
};

const { id, name } = user;

console.log(name); // Output: Alex
```

### Array Destructuring

It works for arrays, too, but it uses the order of the elements instead of names.

```
const coordinates = [10, 20, 30];

const x = coordinates[0];
const y = coordinates[1];

console.log(x); // Output: 10
```

**With destructuring**, you can grab them in one go using square brackets `[]`:

```
const coordinates = [10, 20, 30];

const [x, y] = coordinates;

console.log(x); // Output: 10
console.log(y); // Output: 20
```

### Spread Operator (`...`)

You use this to copy or combine arrays and objects.

**Combining Arrays:**

```
const citrus = ['Orange', 'Lemon'];
const berries = ['Strawberry', 'Blueberry'];

// Spread out both arrays into a new one
const allFruits = [...citrus, ...berries]; 

console.log(allFruits); 
// Output: ['Orange', 'Lemon', 'Strawberry', 'Blueberry']
```
**Copying and Adding to Objects:**

```
const user = { name: "Alex", isAdmin: false };

// Create a new object by spreading the properties of user
// and overriding/adding a new property.
const adminUser = { ...user, isAdmin: true };

console.log(adminUser); 
// Output: { name: "Alex", isAdmin: true }
```

### Rest Operator (`...`)

You use this to gather multiple items. It’s often used in function arguments or during destructuring.

**In Array Destructuring:**

```
const scores = [98, 85, 76, 60, 55];

// Get the winner and put the rest in another array
const [winner, secondPlace, ...others] = scores;

console.log(winner);      // Output: 98
console.log(secondPlace); // Output: 85
console.log(others);      // Output: [76, 60, 55]
```


### Arrow Function

**Traditional Function:**

```
const add = function(a, b) {
  return a + b;
};
```

**Arrow Function:**

```
const add = (a, b) => a + b;
```

**How They Handle `this`**

This is the most important difference.

- In a **traditional function**, the value of `this` is determined by _how the function is called_. This can be confusing because `this` can change unexpectedly.
    
- In an **arrow function**, `this` is determined by _where the function is defined_. It takes `this` from its surrounding code (it's called lexical scoping). This makes your code much more predictable.

### Promises (The Buzzer)

A **Promise** is like that buzzer. It's an object that promises you a result _in the future_. It will be in one of three states:

- `pending`: The task isn't finished yet.
    
- `fulfilled`: The task completed successfully (you get your coffee).
    
- `rejected`: The task failed (they're out of oat milk).
    

We handle these states with `.then()` (for success) and `.catch()` (for failure).

```
// fetch(...) is a browser function that returns a Promise
fetch('https://api.example.com/data')
  .then(response => {
    // This runs when the data is successfully fetched
    console.log("Success!", response);
  })
  .catch(error => {
    // This runs if there was an error
    console.log("Failure!", error);
  });
```

### Async/Await (The Cleaner Way)

While Promises are great, chaining many `.then()` calls can get messy. **Async/await** is modern syntax that lets you write asynchronous code that _looks_ synchronous.

- `async`: You put this keyword before a function to tell it that it contains asynchronous operations.
    
- `await`: You use this _inside_ an `async` function to "pause" the code on a line until a Promise is fulfilled.
    

Let's rewrite the `fetch` example:

```
// 1. Make the function async
const fetchData = async () => {
  try {
    // 2. "await" the result of the Promise
    const response = await fetch('https://api.example.com/data');
    console.log("Success!", response);
  } catch (error) {
    // Errors are caught in a standard try...catch block
    console.log("Failure!", error);
  }
};
```

## Interfaces and Types
Both `interface` and `type` are used to define the "shape" of an object or a data structure. Think of them as a blueprint or a contract that your data must follow. 
### Interfaces

An **`interface`** is primarily used to define the structure of an object. It's a powerful way to describe what properties an object should have and what their types should be.
```
interface User {
  id: number;
  name: string;
  isPremium: boolean;
}

const myUser: User = {
  id: 1,
  name: "Alice",
  isPremium: true
}; // This works!

// const badUser: User = { id: 2, name: "Bob" }; 
// This would cause an error because 'isPremium' is missing.
```

A key feature of interfaces is **declaration merging**. If you declare the same interface more than once, TypeScript combines them into a single interface.

### Types

A **`type`** alias can _also_ be used to define an object's shape, just like an interface.

```
type Product = {
  sku: string;
  price: number;
};
```

However, `type` is more flexible. It can also define a **union** (a value that can be one of several types), a **tuple**, or an alias for any other type. Interfaces _cannot_ do this.
```
// A union type: a variable that can be a string OR a number
type ID = string | number;

// A status that can only be one of three specific strings
type Status = "pending" | "completed" | "failed";
```


### What are Generics?

Generics are like a variable for your types. They let you write a function or a class that can work with any type, but without losing the specific type information.

You define a generic with angle brackets `<>`. The `T` is a conventional placeholder name for the type.

Let's rewrite our function with a generic:

```
// We declare a generic type 'T' here
//      ↓
function identity<T>(arg: T): T {
  return arg;
}

// When we call it, we can specify the type...
let resultString = identity<string>("hello");
// ... and TypeScript knows 'resultString' is a string!

// Or, TypeScript can often infer it automatically!
let resultNumber = identity(123);
// TypeScript infers that T is 'number', so it knows
// 'resultNumber' is a number.
```

```
function getFirstElement<T>(arr: T[]): T { return arr[0]; }
//OR 
function getFirstElement<T>(arr: Array<T>): T { return arr[0]; }

```

## Utility Types

Often, you'll find you need a new type that is just a small variation of an existing one. Instead of writing a whole new `interface` or `type`, TypeScript gives you built-in "helper" types to transform existing ones. This keeps your code clean and avoids repetition.

Let's look at a few of the most common ones.

`Partial<T>` takes a type `T` and makes all of its properties **optional**. This is incredibly useful for functions where you're updating an object.

```
interface User {
  id: number;
  name: string;
  email: string;
}

// All properties of User are now optional
type UserUpdate = Partial<User>;

// This is valid, even though 'name' and 'email' are missing.
const userToUpdate: UserUpdate = { id: 1 }; 
```

### `Readonly<T>`

`Readonly<T>` makes all properties of a type `T` read-only, meaning you can't change them after the object is created.

```
interface Config {
  apiUrl: string;
}

const myConfig: Readonly<Config> = {
  apiUrl: "https://api.mysite.com"
};

// myConfig.apiUrl = "new_url"; // ERROR! Cannot assign to 'apiUrl' because it is a read-only property.
```

### `Pick<T, K>`

`Pick<T, K>` is one of the most useful. It lets you create a new type by **picking** a specific set of properties (`K`) from an existing type (`T`).

TypeScript

```
interface Product {
  id: string;
  name: string;
  price: number;
  description: string;
}

// Create a new type with ONLY the 'name' and 'price' properties
type ProductSummary = Pick<Product, 'name' | 'price'>;

const summary: ProductSummary = {
  name: "Laptop",
  price: 1200
}; // This is valid!
```

These utilities save you a ton of time and make your code's intent much clearer.