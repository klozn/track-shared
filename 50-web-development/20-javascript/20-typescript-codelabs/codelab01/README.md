# Codelab01

Start by installing the required TypeScript tools (instructions in the `README.md` file two directories up.)

## Your first TypeScript file

Create a file called `greetingService.ts` (mind the `.ts` extension).

Copy/paste the following JavaScript (not TypeScript) code to `greetingService.ts`:
```javascript 1.5
function generateGreeting(person) {
    return 'Hello there, ' + person;
}

const user = 'Jimmy McJimmens';

document.getElementById('content').innerText = generateGreeting(user);
```

## Compiling our code
Although we used a `.ts` extension, this code does not use anything specific to TypeScript. It's plain old Javascript.

Now, inspect the `part.html` file. We're executing the following script `<script src="greetingService.js"></script>`.

We could try to replace `<script src="greetingService.js"></script>` with `<script src="greetingService.ts"></script>`, but that won't work. 

We need to 'convert' our `.ts` file to a `.js` file.

When we use TypeScripts compiler, it will transpile all of our TypeScript code to Javascript code. For every `.ts` file, a `.js` file will be created, containing the Javascript code (transpiled from the TypeScript code).

Using the command line, compile your `.ts` file:
```
tsc greetingService.ts
```

As a result, a `greetingService.js` file will be created, containing pure Javascript.
- Sidenote: since our `greetingService.ts` contained no TypeScript specific code, both files' content will look very much alike.

Inspect the `greetingService.js` file. Then, open up your `codelab01.html` file. You should see the following output:
```
Hello there, Jimmy Jimmens
```

## Types

Let's start by adding some TypeScript code.

In the `greetingService.ts` file, provide argument `person` of method ` generateGreeting(person)` with type `string`.

The resulting code should look like this:
```typescript
function generateGreeting(person: string) {
    return 'Hello there, ' + person;
}

const user = 'Jimmy Jimmens';

document.getElementById('content').innerText = generateGreeting(user);
```

Recompile your file, then re-run the `codelab01.html` page.
The result should be as before.

However, now we have type safety in our code. The compiler will notify us by throwing a compilation error when we validate the intended contract of the argument.

Change the code so that variable `user` is an array:

```typescript
function generateGreeting(person: string) {
    return 'Hello there, ' + person;
}

const user = [1,3,3,7];

document.getElementById('content').innerText = generateGreeting(user);
```

Now, recompile your code. You should receive the following compilation error:
```
greetingService.ts:7:65 - error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
```

Notice that although there were errors, the `greetingService.js` file is still created. You can use TypeScript even if there are errors in your code. But in this case, TypeScript is warning that your code will likely not run as expected.

Correct your mistake, recompile your file, then re-run the `codelab01.html` page:
```
Hello there, Jimmy Jimmens
```

## Interfaces

Let’s develop our sample further. Create an interface (in the `greetingService.ts` file) that describes objects that have a firstName and lastName field. In TypeScript, two types are compatible if their internal structure is compatible. 
This allows us to implement an interface just by having the shape the interface requires, without an explicit implements clause.

Your interface should look something like this:
```typescript
interface Person {
    firstName: string;
    lastName: string;
}
```

Then, let method `generateGreeting(person: string)` no longer accept a string as argument, but a `Person` object.
Furthermore, the method should use the `Person` object to get the and return the `firstName` and `lastName` fields.

Finally, call the `generateGreeting` method by calling it with an object that fits the internal structure of interface `Person`.

Your code should like something like this:
```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function generateGreeting(person: Person) {
    return 'Hello there, ' + person.firstName + ' ' + person.lastName;
}

const user = { firstName: 'Roger', lastName: 'Rogerson' };

document.getElementById('content').innerText = generateGreeting(user);
```

recompile your file, then re-run the `codelab01.html` page. You should see the following result
```
Hello there, Roger Rogerson
```

## Classes

Finally, let’s extend the example one last time with classes. TypeScript supports new features in Javascript, like support for class-based object-oriented programming.

In your `greetingService.ts` file, create a `Student` class with a constructor and a few public fields. 
A `Student` has 4 fields, of which 3 are provided as constructor arguments (`firstName`, `lastName` and `middleInitial`).
Field `fullName` is the combination of the other 3 fields. 

Your `Student` class could look like this:
```typescript
class Student {

    public fullName: string;
    public firstName: string;
    public lastName: string;
    public middleInitial: string;

    constructor(firstName: string, middleInitial: string,lastName: string) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.middleInitial = middleInitial;
        this.fullName = firstName + ' ' + middleInitial + ' ' + lastName;
    }

}
```

or, like this:

```typescript
class Student {

    public fullName: string;

    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + ' ' + middleInitial + ' ' + lastName;
    }

}
```
Notice that the use of `public` (or any other access modifier) on arguments to the constructor is a shorthand that allows us to automatically create fields with that name.

Notice that classes and interfaces play well together, letting the programmer decide on the right level of abstraction.

Now, change variable `user` so that it now holds a `Student` object: `const user = new Student('Roger', 'R.', 'Rogerson');

Your final code should look like this:
```typescript
class Student {

    public fullName: string;

    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + ' ' + middleInitial + ' ' + lastName;
    }

}

interface Person {
    firstName: string;
    lastName: string;
}

function generateGreeting(person: Person) {
    return 'Hello there, ' + person.firstName + ' ' + person.lastName;
}

const user = new Student('Roger', 'R.', 'Rogerson');

document.getElementById('content').innerText = generateGreeting(user);
```

Notice how method `generateGreeting(person: Person)` still uses a `Person` object, although we're passing in a `Student` object.
This is however, in Javascript (and TypeScript) allowed, because they share (part-of) a similar structure (`lastName` and `firstName`).

recompile your file, then re-run the `codelab01.html` page. You should see the following result
```
Hello there, Roger Rogerson
```