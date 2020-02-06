# Javascript-Weired-Parts-19

##  Conceptual Aside: Syntax Parsers, Execution Contexts, and Lexical Environments

### Syntax Parsers
* Syntax Parsers -  if you have a function with the variable, that function and variable will be represented in memory. But it's being translated from what you're writing what is more human readable to what the computer can understand.There's a compiler or an interpreter between those two things and
part of that is a syntax parser.

* So that's syntax parsers and thinking about the programs that are actually running every time you run your JavaScript code. That intermediate program that's creating your code

### Lexical Environment
* let's say a function, with a variable inside of it. The variable sits lexically inside the function.So, when we talk about the lexical environment of something in the code we're talking about where it's written and what surrounds it, 

### Execution Contexts
* What is it???...

* A wrapper to help manage the code that is running. There are lots of lexical environments,areas of the code that you are looking at physically. But which one is currently actually running? Is managed via what's called execution contexts.And an execution context contains your code, the running code. It's running your code, but it also can contain things beyond what you've written in your code.

## Conceptual Aside: Name/Value Pairs and Objects

### Name/Value Pair
*  A Name/Value pair is a name which maps to a unique value. However, that value may be more Name/Value pairs.Okay, that sounds a little confusing, but
all we're really saying is that there's a name, and that name is assigned a value,but that value could be other collections of name values.
```jsx
Address = "746/1 Muthu nagar"
```
### Object
* An object is a collection of Name/Value pairs.
```jsx
Address : {
    street: "MuthuNagar",
    DoorNo: {
        groundFloor: "746/1",
        FirstFloor: "746/1A"
    }
}
```

## The Global Environment and The Global Object
* Whenever code is run in JavaScript, it's run inside an execution context.We've seen that word already.Meaning a wrapper that the JavaScript engine, the program that other people wrote that's parsing and looking at and verifying and executing your code.

* thing that's accessible everywhere to everything in you're code, it's global.

* the global execution context creates two things for you, you don't have to in your code.
    * It creates this Global Object.
    * it creates a special variable for you, called 'this'.

* And these two things are created for you by the JavaScript engine.
* Incase if you running a empty js file ... there is notthing to parse because the js is empty but still js engine will create a secial variable(this) and a global object(here its window - if you are using node js  running java script on the serverit won't be the window object there's a different global object.)

* If I had a separate tab open that would be a separate global object because each tab of each window is its own execution context, its own global execution context.

* And at the global level those two things are equal.This, refers to the window object and you can also just say, window and get the global object.

* Now, just to be really clear, when we say global, in JavaScript that means not inside a function. Don't think too much deeper than that.That means code or variables that aren't inside a function is global.

* Now let us assign a global variable and function
```jsx
var a = 'Hello World!';

function b() {
    
}
```
* Both these global variable and functions sits inside window object.So, your variables and your functions when lexically is not sitting inside a function, they're just sitting right there on the global object.

## The Execution Context - Creation and Hoisting

* we just need to dig a bit more into what the JavaScript engine does to create an execution context.
```jsx
var a = 'Hello World!';

function b() {
    console.log("called b !!")
}

b();

console.log(a);
```
* what will be the expected output for above code ??? 

* Yes as we expect function will be called first and then variable console. (Output)
```jsx
called b !!
Hello World!
```
* Now lets do minor changes in the order we exceute like below
```jsx
b();

console.log(a);

var a = 'Hello World!';

function b() {
    console.log("called b !!")
}
```
* The expected output will be undefined for both the console because its not yet defined but it called before that. But the actual ouput is..
```jsx
called b !!
undefined
```
* Now lets do one minor changes in the order we exceute like below, here we are going to remove variable but we will call that 
```jsx
b();

console.log(a);

function b() {
    console.log("called b !!")
}
```
* Output !!!
```jsx
called b !!
Uncaught ReferenceError: a is not defined at app.js:3
```
* This phenomenon is called hoisting
*  if you look online, that hoisting is explained is that people saying well, that variables and functions in JavaScript are hoisted or moved to the top by the JavaScript engine as if they were moved physically up to the top so that they work no matter where you put them.!!!!!
*  we can tell that that's not quite right !!!!!!!!!(Important).
* It won't move the variable to top but it behaves someting like below
```jsx
function b() {
    console.log("called b !!")
}

var a 

b();

console.log(a);
a = 'Hello World!';

```
* Here variable 'a' is declared but not defined with any values that is why we are getting "undefined" in this case.

* execution context is created In two phases. 

* The first phase is called the creation Phase. In this we know the global object and "this" variable which is set up in memory.

* So it sets up in this creation phase, the memory space for the variables and functions. And it's that step that is somewhat confusingly called hoisting.

* It's not actually moving code to the top of the page. 

* All this means is that, before your code begins to be executed line by line, the JavaScript engine has already set aside memory space for
the variables that you've created in that entire code that you've built,and all of the functions that you've created as well.

* So those functions and those variables exist in memory. So when the code begins to execute line by line, it can access them.

* However, when it comes to variables, it's a little bit different.You see the function in its entirety is placed into memory space, meaning
that the function, its name and the code inside the function is being executed.

* So the JavaScript engine when it sets up the memory space for a, it doesn't know what its value will ultimately end up being until it starts executing its code. So instead, it puts a placeholder called undefined.That placeholder means oh, I don't know what this value is yet.

* All variables in JavaScript are initially set to undefined,and functions are sitting in memory in their entirety.

* That's why it's actually a bad idea to rely on hoisting in any way. You could run into trouble when you realize that value is actually undefined, and not the value you're expecting.

## Conceptual Aside: Javascript and 'undefined'
```jsx
console.log(a);

var a = 'Hello World!';
```
* Output !!!
```jsx
Undefined
```
* if we don't declare a variable with var key word
```jsx
console.log(a);

a = 'Hello World!';
```
* Output !!!
```jsx
Uncaught ReferenceError: a is not defined
```
* But this is very confusing not defined and undefined both are samething.. Noooo!! both are not same

* in JavaScript, when we see undefined this isn't simply the word undefined, but it's actually a special value that JavaScript has within it internally that means that the variable hasn't been set.

* Because the initial phase of creating the execution context sets all variables equal to undefined. And if you don't do anything else, that's what that value will be.

* when it went through it didn't find a var a so it never set up the memory space. So when it went to execute this code it said hey, I don't have a in memory at all. So it gave you an Uncaught Reference, a is not defined.

* Undefined - It's a special keyword. A special value that means this is the value that was initially set by JavaScript. And that leads to a little bit of a warning

* Never do this.Never set yourself a variable equal to undefined.
```jsx
a = 'Hello World';
a = undefined;
```
* That's perfectly valid JavaScript. But it's a little dangerous. It's better to let undefined, that special keyword mean I,the programmer, never set the value. That will really help you when debugging code.

## The Execution Context - Code Execution

* Execution will happen line by line

```jsx
function b() {
    console.log("called b !!")
}

b();

console.log(a);

var a = 'Hello World!';

console.log(a);

```
* Output !!

```jsx
called b !!
undefined
Hello World!
```

##  Conceptual Aside: Single Threaded,Synchronous Execution

* Single Threaded - That means that one command is being executed at a time. JavaScript behaves in a single threaded manner.
* Synchronous - So the code is executed one line at a time in the order that it appears.Single threaded synchronous execution,
meaning that in JavaScript, only one thing is happening at a time.

* Now I know what you're probably thinking, but hey, I've heard of asynchronous requests in JavaScript, especially on the web.
AJAX, the A stands for asynchronous. What are you talking about? Well, we'll see that in just a little bit, but for now, just trust me.
JavaScript is single threaded, synchronous execution in its behavior.

## Function Invocation and the Execution Stack

* Invocation - That just means running a function or calling a function.we do that by ().
* What will happen when you invoke a JS function
```jsx
function b(){
var c
}
function a(){
    b()
    var d
}

a()
```
* As we already discussed first it will create global object and special variable and then i will allocate memory space for function b and then function a and then it will start executing
* Here's the thing that actually happens. A new execution context is created and placed on what's called the execution stack.
* And a stack is just what it sounds like,one on top of the other on top of the other. And whichever one is on top is the one that's currently running.
* It will go through that create phase, and then it will execute line by line the code within the function. However, if I have another function invocation,
it's going to stop at that line of code and create another execution context and run that code.
* So the excution context is created in stack manner and then based on that execution will happen.

##  Functions, Context, and Variable Environments

* Variable environment is just talking about where the variables live that you've created and how they relate to each other in memory.
* So when you think about the variable environment just think about where is the variable?
```jsx
function b(){
    var myvar
    console.log(myvar);
}
function a(){

    var myvar =2;
    console.log(myvar);
    b();
}
var myvar =1;
console.log(myvar);
a()
```
* Output !!
```jsx
myvar = 1
```
* First Global execution context is created that will set myvar = 1 in memory space , then it hits the invocation of a() what it happens ?? new execution context is created with myvar = 2 on top of the previous stack , then it will invokes b() which is inside a() ... that will set myvar = undefined. since its not having any value.
* the value of myvar is based on which context we are executing if it is global ie outside the function the value will be 1.

## The Scope Chain

```jsx
function b(){
    console.log(myvar);
}
function a(){

    var myvar =2;
    b();
}
var myvar =1;
a()
```
* Here we didn't declare myvar inside b() so that it will take global value ie myvar = 1, if we declered then the value will be undefined.
* Remember Every execution context has a reference to its outer environment. for b and a  outer environment reference for myvar is 1.
* Function b() is sits on the same level of function a(), its not inside a(), it just executed inside a(), that is not!!! the mean the execution context of  b() is inside a(). 
* so the outer reference of myvar which is inside the b() is global valriable ie myvar = 1
* Remember scope means, where can I access a variable. And the chain is those links of outer environment references. Lexical, that is where it was physically written in your code.

```jsx

function a(){

    function b(){
        console.log(myvar);
    }

    var myvar =2;
    b();
}
var myvar =1;
a()
```
* Output of console myvar!!!
```jsx
2
```
* We've learned that where a function sits determines its outer environment reference. But as it's being executed, those execution contexts are stacking up, and it's running synchronously.

## Scope, ES6, and let

* Scope is where a variable is available in your code.

* Let - This is used or can be used instead of var.Although, it's not replacing it. Var will still be around.But let allows the JavaScript engine to use what's called block scoping.
    * So you can declare a variable, just like you would with var and during the execution phase where it's created,the variable is still placed into memory and set to undefined.
    * However, you're not allowed to use let until the line of code is run during the execution phase that actually declares the variable.
    ```jsx
    if(a>b){
        let c = true;
    }
    ```
    * So if you tried to use c in this example before the let c = true,you'd get an error.Now it's still in memory but the engine just won't allow it.
    * When that variable is declared inside that block, it's only available inside that block at that period of time for the running code.
    * This is true even for for-loops.So if you have a loop and are running the same code over and over but you have a let statement, you'll actually get a different variable in memory each time the loop is running.So this allows for what's called block scoping as we mentioned,But again you can use both let and var in ES6.

 ## What About Asynchronous Callbacks?

* Asynchronous simply means more than one at a time.

* So we may be dealing with code that's executing and that starts off some other code to execute, and that may start other code executing and all of those
pieces of code are actually executing within the engine at the same time.

* But JavaScript, as we've said, is synchronous.It doesn't execute asynchronously.It executes code a line at a time.

* Yet there are things like click events or you can go off and get data in JavaScript and get it back.Where you have callback functions that run when that
action is complete, 

* So since JavaScript is synchronous, how is this handling those asynchronous events?

* let say, let's run a function when someone clicks on a button, what happens? Because that is being handled asynchronously.
Other parts of the browser are running and looking at that code while the JavaScript code is still running.


* We've already learned about the execution stack,that we have these execution contexts that are being created. And as functions are being called, they're being run stacked on top of each other.And as they finish, they leave the stack.

* There's another, however,list that sits inside the JavaScript engine called the "event queue". As we discussed the javascript engine might work with other brower engines like render engine... etc.. in that case So when the browser, somewhere outside the JavaScript engine, has an event that
inside the JavaScript engine we want to be notified of, it gets placed on the queue.

* So a click event, for example, if someone clicks on the screen. that will call another event like HTTP request... while all the events are added in the queue then event queue gets looked at by JavaScript , when the execution stack is empty. ie once all the execution stack is executed then JavaScript periodically looks at the event queue.

* So it sees a click event, it processes that click event and knows, hey, there's a function that needs to be run for that event.
So it creates the execution context for whatever function when that event happened. And so that event is processed and the next item in the queue moves up, and so on and so forth. But again,the event queue won't be processed until the execution stack is empty,until JavaScript is finished running all of that other code line by line.

* So it isn't really asynchronous.What's happening is the browser asynchronously is putting things into the event queue, but the code that is running is still running line by line. And then when this is empty, when the execution contexts are all gone, all finished, then it processes the events.

```jsx
// long running function
function waitThreeSeconds() {
    var ms = 3000 + new Date().getTime();
    while (new Date() < ms){}
    console.log('finished function');
}

function clickHandler() {
    console.log('click event!');   
}

// listen for the click event
document.addEventListener('click', clickHandler);


waitThreeSeconds();
console.log('finished execution');
```
* Output !!!
```jsx
finished function
finished execution
click event!
```
* Notice that the function completed, and the global code completed. before it went off and created an execution context for that clickHandler function.Why?
* Because the JavaScript engine won't look at the event queue until the stack is empty.
* So that means long-running functions can actually interrupt events being handled. But this how JavaScript synchronously is dealing with the fact that asynchronous events are happening,
* So all it does, it just keeps running its normal code, and when that's all done, it will then go and look at the event queue.
* And if it's already done, then it will just continue to watch that event queue in the loop, the event loop. That's what that's called, the continuous check.
* And then when it sees something, if there's supposed to be a function, if there's a handler, if there's a listener that's supposed to run
when that event appears in the event queue, it will run it. So that's how JavaScript,though synchronous, deals with asynchronous events.
* Any events that happen outside of the engine get placed into that queue, and if the execution stack is empty, if JavaScript isn't working on anything else
currently, it'll process those events.It will process the events in the order they happened.
* So if a click event happens and then an HTTP event, it will process the click first and then run that function,complete that function, then start looking at the queue again.
* All right, so again, asynchronous callbacks are possible in JavaScript. But the asynchronous part is really about what's happening outside the JavaScript engine. And JavaScript, via this event loop,via this list of events that are happening, when it's ready,will look at events and process them, but it does so synchronously.

## Types and Operators

### Conceptual Aside - Types and JavaScript.

* When it comes to the types of data that variables hold. And how JavaScript deals with them.

* I'm going to discuss a term that describes how JavaScript deals with types. It's called Dynamic Typing.

* That means that you don't tell the JavaScript engine what type of data a variable holds. You don't put that into the code that you're writing. Instead, the engine figures it out while your code is running, while it's executing.

* And so a single variable can, at different times during the execution of your code.Hold different types of values, because it's all figured out during execution.

* All right, for example, when you're dealing with other programming languages like Java or C#, they use something called Static Typing. That means you tell the engine ahead of time, the compiler, ahead of time what kind of data you intend to hold inside a variable.

* So you might have a keyword like bool to say this variable should hold a Boolean value, either true or false. And then if you try to put a value that is other than a Boolean into that variable you get an error.

* But JavaScript isn't like that. JavaScript is dynamically typed. Meaning that there is no keyword to tell the engine what kind of data you intend to put inside the variable.

* We just say var, for example, and then while the code is running, the JavaScript engine will determine what type you're giving that variable. And you could change it.

```js
/// STATIC TYPIG - !!!! not used in javascript 
bool isNew = 'hello' // this will throw error because here we statically mentioned its type as boolean(true/false)

/// Dynamic Typing 

var isNew = true // no error - here we didn't mention type since this is Dynamic Typing 
isNew = 'test' // still no error - even you could change value with another type of value
isNew = 1 // still no error - here we used numeric
```
* This turns out to be quite powerful, and can also cause you some problems if you don't understand how JavaScript is going to make decisions as a result of dynamic typing.

### Primitive types

* Before we can dive further into how dynamic typing works. we need to talk a bit about the types that exist in JavaScript. The types of data that you can store in a variable.

* Again you don't say this directly through the variable declaration, but there are six primitive types in JavaScript.

* What do we mean by primitive type?

* A primitive type is a type of data that represents a single value. In other words, something that isn't an object.

* Because remember, an object is a collection of name value pairs. A primitive type, on the other hand, is just a single value.

* And again, there are six types in JavaScript.So let's talk about each of the primitive types.

    * undefined - We've already talked about this at some length. Undefined represents a lack of existence. And it's what the JavaScript engine sets variables to initially and it will stay undefined unless you set the variable to have a value. So you really shouldn't use this as far as setting a value. You shouldn't say a variable equals undefined. Just let that be the thing that means the code has never set the value. But you can certainly test for it.

    * null - Null also represents lack of existence. This one is better for you to use when you want to say that something doesn't exist, that the variable has no value. So, leave undefined for the engine, and you can use null

    * Boolean - This is either true or false, just one of two values.

    * number - another primitive or simple type. In JavaScript, actually, there's only one numeric type, and it's called number. It's a floating point number, meaning that essentially, there's always some decimals attached to it. Unlike other programming languages, others might have integer and decimal,and other specific numeric types, JavaScript only has one. And while you can fake it as far as integers and other types of mathematical numbers are concerned, there really is under the hood, only one type called number. And it's a floating point number. And it can make math a little weird.

    * string - this is another base primitive type in JavaScript. It's a sequence of characters and both single quotes and double quotes can be used to specify that we're dealing with a string. In some programming languages, a string is more complex and treated as a sequence of characters. But in JavaScript, a string is considered a primitive type.

    * symbol - It's used in ES6 or ECMAScript 6, which is the next version of JavaScript.It's being constructed and not fully supported by all browsers. So we're not gonna talk about this here , But for our purposes we're just going to ignore the symbol as a primitive type in this course. However it's just good that you know that it's coming.

### Conceptual Aside: Operators

* Now, it's time to talk in depth about another concept which will help us to debug and understand some other kinds of problems people have in JavaScript because of its dynamic typing.

* Let's talk about operators. What do we mean?

* An operator is actually a special function that is syntactically, or that is written differently , than regular functions are written in your code.

* Generally, operators take two parameters and return one result.

```js
var a = 3 + 4;

console.log(a);

// will result 7 
```
* How did the JavaScript engine do that? How did it know that this was my intent to add three and four?

* Well, the syntax parser and the rest of the aspects of the engine looked at this code, saw the plus sign and added these two numbers. This plus sign is an operator.It's the addition operator and it's actually a function.It would be kind of like this.

```js
// instead of function name give it as plus(+)
function +(a,b){
    return // add the two #s
}
```

* The difference is that instead of calling the function as you normally would. If this was a function name +(3,4), I would then put parentheses and pass three and four to it, and that would have been a normal function call.

* But that would be a really annoying way to have to type your functions for common operators like plus. So, instead, the JavaScript engine provides,
along with many programming languages, the option for the ability to write in what's called infix notation.

* Infix means that the function name, the operator, sits between the two parameters. "3 + 4;"

* "+3 4;" this is prefix notation

* "3 4+;" this is postfix notation

* A lot of old school accounting calculators work this way where you put the operator at the end but for our purposes in JavaScript, this is infix notation.
It's very human readable, but this is essentially a function call passing two parameters. And this function returns a value.

* The value that it returns, in the case of the addition operator, is these two parameters added together. So, plus is an operator. There are other operators. like minus,>, < ,So, this function accepted two numbers, and returned a Boolean. So, this is the concept to keep in mind. That when we type these operators, plus, minus, less than, greater than, and a bunch of others 

*  there is pre-written code, essentially, for you that the JavaScript language, the engine provides to do, or run, or to invoke these functions.

* And what's happening inside those functions is important to understand, especially when dealing with a dynamically typed language like JavaScript,

* But for now, here, let's just keep in mind that operators are functions.

### Operator Precedence and Associativity.

* Operator precedence just means which operator function gets called first. And there's more then one on the same line of executable code. And functions then are called an order of precedence. The higher precedence wins so if I have more then one operator, the JavaScript engine will call or invoke the operator with the higher precedence first, or the highest, and then down the line from there.

* operator associativity - what order an operator gets called in. So again we're talking about which operator function in what order. In this case, either left to right associativity, or call left associativity. Or right to left. Which is called right associativity.

* And this means when the operator functions have the same precedence. So if I have a bunch of operators on a line of code, the precedence tells me which ones gets called first. But if I have some of them, they all have the same precedence, then what do I do?

* Well, depending on the associativity of that operator, we're either going to call the functions left to right, or we'll call the functions right to left.


```js
var a = 3 + 4 * 5 ;

console.log(a);

```
* Well if you remember from mathematics class, there is an order, a precedence in mathematics in general to these operators. And JavaScript does the same thing

* because there's one of two options. These are two function calls, and remember JavaScript it synchronous, so it's not going to try to do this simultaneously. It's gonna call one, and then call the other. But which one gets called first. Well first of all, we'll start with operator precedence.

* And in order to answer this question, we need to know what the operator precedence is in JavaScript.("Refer Operator-Precedence-In-Javascript.pdf")

* Notice that multiplication has a precedence of 14, and addition has a precedence of 13. And with operator precedence the higher one gets called first.
So multiplication is higher, has a higher precedence than addition. That means that what's going to happen is this will get called first. And remember that this returns a value.

* Equals is also an operator.

* what if more than one operator, and I could have many in a row, has the same associativity?

* Let's go back to our table. Notice that in some cases, for example, multiplication, division, and remainder, All have the same associativity, or I could have the same operator on one line of code.

```js
var a = 2, b = 3, c = 4

a = b = c

console.log(a);
console.log(b);
console.log(c);
```

* So in the end they should all be equal but the question is, what value will they all have when I have console.log this? Will they all be two, will they all be three, or will they all be four? What do you think?

* Result : They're all equal to four. Why? Because of associativity column in table

* Associativity is either left associativity meaning the furthest one to the left, the furthest operator to the left will be called first. Or right to left or right associativity. Meaning the operator furthest to the right will be called first, and that's really it. Pretty simple.

* All right, so the operator I'm using is the equals operator or the assignment of equal, it's one of the assignment operators. Notice that it's associativity. Is right to left. So, the equals operator is right associative. It has right to left associativity. Meaning that, when I have it on a single line, since it's the same precedence, it then looks at the associativity to decide. Which one should be called first? So it goes to the furthest one to the right, because it's right to left associativity. So it'll call this one first.

* Now here's the interesting thing. We said that operators generally take two parameters and return a value. So, the plus operator returned the value of the addition of the two parameters. But, what does equals return?

* See the method or the function call that was invoked. It did the work of setting the value and memory of b equal to the value and memory of c. And then returned the value of the parameter on the right, the parameter that we're setting equal to. So it gave me back a four. So this statement actually returns a value.

* So that means I can call it in order.So since this is right to left associativity,It sets B equal to four because C is four, and returns a four.
And then this will be called. So A will simply be set equal to four because that's what this returned. A is 4.

* That's operator precedence and associativity precedence helps us determine which functions run first and so does associativity. Associativity comes into play when two or more operators have the same precedence.

* In addition to the PDF provided in the previous lecture, here is the link to the original table on the MDN (Mozilla Developer Network): https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence

### Conceptual Aside: Coercion

* Coercion, converting a value from one type to another, that's all it really means. You might have a number and you coerce it, or attempt to convert it to a string. Or you have a string and you coerce it or attempt to convert it to a number and so on and so forth. This happens quite a bit in JavaScript, because it's dynamically typed.

```js
var a = 1 + 2;

console.log(a);
```

* If I pass two numbers to the plus operator, then I get addition. But if I pass two strings like hello and world, then instead of trying to add it as numbers, it concatenates the string and puts them together.

* Now, here's the question, what if I pass the operator two different types? If I give this function a number and a string, what do you think is going to happen? What will be my value?  1 + "2"  will result 12

* What happened was that this value, the first parameter, was coerced by the JavaScript engine into a string.

* In memory, the string 1 and the number 1 look nothing alike. Now they look awfully similar when you're typing on the screen, but actually in the computer memory the number 1 and the string 1 don't look anything alike.

* When you call the plus operator this function, and you give me a number and a string, then I'm going to make a choice to try to coerce that number. I'm going to try to figure out what it would be if it was a string,

* Notice that I didn't ask JavaScript to coerce this value, I didn't type something here special that sent some kind of convert this to a string value or something like that. I simply called the function with these two parameters and when the function ran, it choose to coerce my value instead of giving me an error, which is what some other programming languages would do.

* And understanding that coercion is happening is very important, because you can avoid some important bugs and debug things when things look a little strange.

### Comparison Operators

```js
console.log(1 < 2 < 3); // true, obviously

console.log(3 < 2 < 1); // this is also true, why ??? 
```

* Well, associativity, find the less than operator, I see that it's left to right associativity,

```js
console.log(3 < 2 < 1);  

console.log(false < 1); // since left to right 3 < 2 is false 

```
* And what does an operator in JavaScript try to do when I give it a value that's a type it doesn't expect. This wants two numbers and I'm giving it a number and a boolean.

* Well it's going to try to coerce or convert this boolean to a number. Well what does false become when I try to convert it to a number?

* All right so I'm going to try to coerce a boolean false into a number. What does it become? Zero. Well that's interesting. Actually true and false both can coerce false becomes a zero and true becomes a one. So, what does that mean is happening?

```js
console.log(0 < 1); // coerce actually running this statement, yes its true
```
* Even for 1 < 2 < 3

```js
console.log(1 < 2 < 3)
console.log(true < 3) // ie as per coercion 1 <3 
// this will result in true, not just for human perspective 1 < 2 < 3  its based on computer perspective
```
* But, if I try to coerce, for example, undefined, I get this special thing. NaN, which literally means, not a number

* This isn't exactly a primitive type, but you can basically treat it as such. Not a number means, I have this thing that I tried to convert to a number and
it just isn't a number. There's no way to convert it to a number.So undefined couldn't convert to a number.

* For Null, JavaScript decides, is a zero.

* It isn't always obvious what a particular type is going to coerce to. The JavaScript engine decides.

* This can cause a lot of strange bugs and odd problems if you don't understand again what's going on. You can believe that undefined and null are going to behave the same way, but they won't. So while coercion is very powerful, it can also be dangerous,


* Let's pull up our JavaScript Operator Precedence table again. All right, I'm going to scroll down to find something very common in programming languages.
Equality. It's a double equals. Meaning, are two things equal? We often put this in if statements. If something equals something else.

```js
3 == 3 // true

'3' == 3 // the doubles equal operator coerce the types. It expects two of the same type and if they're not, it will make a choice. True.

false == 0 // true

null == 0 // false - Remember null also coerces to zero as a number. Oh, but that's false. So, there are special cases, like undefined and null, that don't do what you would expect. False equals zero.

```

* Remember null also coerces to zero as a number. Oh, but that's false. So, there are special cases, like undefined and nnull, that don't do what you would expect. False equals zero.

* Yes, but null, although it coerces to zero under other circumstances, like null is less than 1, it doesn't coerce to zero for comparison.
This causes all sorts of confusion and problems. And is actually considered a negative part of the language, in that comparison operators, especially double equals causes strange errors because of unexpected ways in which it behaves.

```js
" " == 0 // true 
" " == false // true 
```
* You see the problem, it makes some sense because it's attempting to convert these values to the same time.But it can make your code very difficult to anticipate as far as what's going to happen. So how do we solve this.

* we saw the double equals, which is equality. But there's also a triple equals, which is called strict equality.

* And the same there's regular inequality, not equal, using the exclamation mark. And there's strict inequality, not equal, using an exclamation mark and a double equals.

* So when people see these triple equals for the first time, especially if coming from another programming language. The first response is what in the world is that?

* That is going to save your life.

* Strict inequality, the triple equals compares two things, it's a standard operator, but doesn't try to coerce the values. If the two values are not the same type, it just says no, they're not equal and returns false.

* The triple equals, built in operator function, says those two things are a different type, so they're not equal.So using the triple equals will prevent us
from having some odd potential errors in our code. When comparing values.

* in general, I would say 99% of the time. Use triple equals when making equality comparisons. Don't use double equals unless you explicitly. Unless you consciously want to coerce the two values, you should start right now if you haven't already. Start using triple equals in your code by default. That's your initial go to equality comparer.

* Note refer : !!!! Equalty-Comparison-And-Sameness.pdf - showing what happens when you compare two different types of values using double equals, the triple equals.

* In addition to the PDF provided in the previous lecture, here is the link to the original table on the MDN (Mozilla Developer Network): https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness

### Existence and Booleans

* So, if I'm converting to a Boolean, true or false some different values, let's see what happens.

```js
Boolean(undefined) // false

Boolean(null) // false

Boolean(""); // false

Boolean(0) // false

Boolean("hi") // true - exception 

```
* So all of these things expect boolean with some string value that imply the lack of existence, they convert to false.

* Can we use that to our advantage? Yes !!!

```js
var a;

// goes to internet and looks for a value

if (a) {
    // Whatever I put inside the parentheses of an if statement, it will attempt to convert or coerce to a Boolean. True or false?
  console.log('Something is there.');  
}
```

* Because remember the creation phase of the execution context is going to set up the memory space for this variable and then what is it going to do? It's going to set it to what value initially? That's right, undefined. And what does undefined convert to? False.

* But as soon as I have something, like in string that's not empty. Then that runs.See how that works?

* We can use coercion to our own advantage and check to see if a variable has a value. Something other than undefined, null, or empty string.

* So all I'm doing here saying if a is something not undefined, not null, not empty string. And you may never need to do this because if there's no real chance that this is going to be a zero. It may be perfectly fine to simply leave it as if this exists, if this even exists at all, if anything was placed into memory other than undefined, or null, or empty string.So that can be very useful. And this pattern of coercion, of using coercion in this case, to check for existence is used in a lot of really great frameworks and libraries. A lot of really strong JavaScript code that's out there. It's very useful.And you can use it.

### Default Values

```js
function greet(name) {
    name = name || '<Your name here>'; // default value - Well, the next version of JavaScript ES 6 is actually going to introduce syntax to set the default value.
    console.log('Hello ' + name);    
}

greet('Tony');
greet(); // But what if I called greet without a parameter? Unlike many programming languages, JavaScript doesn't care. This won't throw an error. It will simply pass nothing to name.
```
* if you didn't pass param and try to console that you will get undefined, Kind of as we expected, right? Undefined. Because what happens when the function is invoked? A new execution context is created, and this variable, which is essentially created inside the function though the value is passed during
its invocation, is initially set when the memory space is set up to undefined.

* So JavaScript simply ignores the fact that I'd called the function without the expected parameters and simply says, that's okay. It's undefined. It's already in memory.It has a value, and you simply didn't give me a new one. 

* And then that Hello, well look what happened there. That's a problem. You see, the plus operator, when it saw a string and undefined, that's the value and name. It coerced name, the undefined primitive type, to a string, and so it literally coerced it to the string undefined. And then put them together, concatenated them.That is not ideal behavior, but it isn't totally unexpected behavior in that we now understand how operators and coercion works.
But, what should we do?

* Well, the next version of JavaScript ES 6 is actually going to introduce syntax to set the default value with || or operator.

* If I returned undefined or hello, it doesn't return true or false. It returns hello. It returns, in this case, the value that can be coerced to true. Because if I coerce hello, a nonempty string, it's true. So that's what the or method returns.

* If I put hi and hello, it gives me hi because it will just return the first one. That coerced to true. So, the or operator's special behavior,

* in that if you pass to it two values that can be coerced to true and false, it will return the first one that coerces to true. One can be converted to true, zero converts to false when converting to Boolean. So I can use the or to say, if something does not exist or is an empty string or is a null, give me this back instead.

* Operators are functions that return values.

* So what will happen is, or will be run before equals because if I look at my precedents, the Assignment operator equals has a lower precedence than or. So or is run first. So or is run.

* So I used this one line or expression to set a default value. Now, I could've done this with an if statement, but that's a lot more typing. This is a pretty neat and clear trick that makes your code very readable.

### Framework Aside: Default Values

* The idea here is that we're going to talk for a moment.about how what we've been discussing is used inside popular and famous frameworks and libraries.

* But, we're going to dive deep into how libraries like jQuery or frameworks like AngularJs use some of the concepts we're discussing in their code.

* If we are attaching multiple library , in case all the libarary using the same variable the library at the bottom will be added as global value rest of them we will lost/replaced with the new value.

* So this can be a bit unusual when dealing with frameworks or libraries. Maybe we've accidentally collided with another library, or maybe another library is there to replace us. So, we wanna be nice about this. What can we do?

* lets go to the library 2 and we can check and see if libraryName is already in the global execution

```js
window.libraryName = window.libraryName || 'Lib 2'
```
* So what am I doing? I'm checking to see if there's already a library name in the global execution context sitting on the global variable which is Window.
If it is, then I'm just not going to do anything. And if not, using the or operator to set my default value because if this wasn't set by anyone, it'd be undefined. Then, I'll set it to the value that I want.

## Objects and Functions

### Objects and the Dot

* It's time for a new section of this course, called Objects and Functions. In a lot of JavaScript courses, they treat these two concepts as two different subjects to teach. I think that's a big mistake.

* Because while in other programming languages, objects and languages are two distinct things to talk about.

* in JavaScript, they're very, very much related. They're really, in many ways the same subject.So let's talk about objects and functions.

* The first subject or the first topic we'll discuss is objects and the dot.

* Remember we've already said that objects are collections of name value pairs and those values can then be other collections of name value pairs. But for our purposes now, let's look at objects from a little bit of a different perspective.

* Let's think about how an object lives or resides in your computer's memory.

* An object like we said is a collection of values that are given names. But, what values are we talking about? Well, an object can have properties and methods. So an object can have a primitive sitting off of it, and that would be called a property.Remember those primitive types that we talked about, so
any one of those like a Boolean or a number or a string.

* An object could have another object connected to it as a child, so to speak. And this is also considered a property.

* An object can also contain functions and in those cases the function is called a method when it's sitting on the object.

* So it's still a function but it's connected to an object. So it's called a method. So objects have properties and methods.

* And these sit in memory so the kind of core object will have some sort of address in your computer's memory.

* And it will have references to these different properties and methods which are also sitting in your computer's memory.(Refer object image)

* Now they may be related to the address of that base object concept, or they may not. But either way the object has references to the addresses or the space or the spots where these different properties and methods live. And those addresses might look something like that. So, this may seem a bit obscure.

* you think about an object as sitting in memory, and then having references to other things sitting in memory that are connected to it. So it knows where its different properties and methods are, that is, primitives, objects, and function that make it up.

* All right, so let's look at in JavaScript how we access those slots in memory,

```js
var person = new Object(); // There are better ways to do this, we will see soon

// person["firstname"] = "Tony"; it's going to create that spot in memory, and give it that name as "firstname". And this object will get a reference to the address of that location in memory.

person["firstname"] = "Tony"; // [] bracket is what's called the computed member access operator.
person["lastname"] = "Alicea"; // inside those brackets I put the name of the value that I'm trying to locate in memory.


var firstNameProperty = "firstname";

console.log(person); // result : object
console.log(person[firstNameProperty]); // result : Tony

console.log(person.firstname); // result : Tony
console.log(person.lastname);   // result : Alicea

person.address = new Object(); // So it's an Object sitting inside an Object
person.address.street = "111 Main St.";
person.address.city = "New York";
person.address.state = "NY";

console.log(person.address.street); // result : 111 Main St.
console.log(person.address.city);   // result : New York
console.log(person["address"]["state"]); // we can access the same with bracket too
```
* If I go back to my PDF of JavaScript operator precedence, you'll see that we have Computed Member Access. It's very near the top. And it's left-to-right associativity, and it's brackets there.

* What the operator does is it takes this Object ("person") and this property ("firstname") or method name, and it looks for this on this Object.

* It actually can do quite a bit more than that, but we'll see that much later

* The dot, is an operator, it's a function. And when used after an Object like that, well, if I go back to my JavaScriptOperatorPrecedence.pdf, you'll see it's just second from the top, right after the parentheses for grouping. It's left to right and it's just a dot and it takes two parameters, the object that you're looking at and the name of the property.

* So the dot operator acted exactly the same as the brackets operator here. This is Computed Member Access. And this is just called Member Access,

* the Member Access operator, members being members of the Object, right? Your fingers and your toes are members of your body. So these are looking for members of the Object, the pieces, the methods, and the properties.

* So Object can sit inside an Object, then I can use the dot operator and add properties to that object like in above code.

* Now remember these are operators, so how do we know which one gets run first? Well, we go back and we see that the associativity of the Member Access operator is left-to-right. meaning that the left most dot will get run first, get called first, because they're functions, and then moving on to the right.

* So again, Objects are name value pairs sitting in memory. They can contain other name value pairs, that is, other Objects. They can contain other properties, strings, Booleans.They can contain numbers. They can also contain functions, which are called methods.

* Remember that this is not the preferred way to create a new Object.We'll get to that in just a second.And also remember that although we can use the dot or
we can use the computed, the bracket notation here for finding properties and methods, the preferred approach is just using the dot operator. It's very clean, it's very clear, and it's also easier to debug and find problems.So I always recommend that you use the dot operator unless you really need to access a property or properties using some kind of dynamic string.

### Objects and Object Literals

* Let's talk about objects and what's called object literals.

* In JavaScript there are often more than one way to do something. So we already saw that I could create a new object with "new Object();"

* But there's a short hand for this called an object literal. And it's just curly braces, like that. That's the same as new object.

```js
var Tony = { 
    firstname: 'Tony', 
    lastname: 'Alicea',
    address: {
        street: '111 Main St.',
        city: 'New York',
        state: 'NY'
    }
};
```

* What's happening is that the JavaScript engine, when it's parsing the syntax, and it comes across this curly brace, and it's not part of something like an if statement or a for loop, or something like that, it assumes that you're creating an object.

* But this is much faster, much quicker to write.

* So I can add another comma, so we separate each name value pair with a comma, when using object literal syntax.

* I can create an object anywhere that I can create any other variable on the fly. Just like I could pass a number to a function or a string and create it on the fly, we can create an object on the fly when passing to a function

```js
function greet(person) {
    console.log('Hi ' + person.firstname);
}

// Tony is a object we already created 
greet(Tony);

// creating object on the fly
greet({ 
    firstname: 'Mary', 
    lastname: 'Doe' 
});

// And notice that I can mix and match this syntax.
Tony.address2 = {
    street: '333 Second St.'
}

```
* The code that you write, the physical code you're writing, isn't what's really happening under the hood. It's being translated by the Java Script engine
into something the computer can understand. So the syntax whether it's object literals or using the dot operator or you're saying new object. Whatever is going on it's all doing the exact same thing under the hood of the JavaScript engine.

###  Framework Aside: Faking Namespaces

* A namespace. In modern coding, a namespace is a container for variables and functions.It's just a holder, a container. And typically it's used to keep variables and functions with the same name seperate.

* But here's the problem, JavaScript doesn't have namespaces.

* in JavaScript, because of the nature of objects, we don't need namespaces as a feature. We can fake it.

```js
var greet = 'Hello!';
var greet = 'Hola!'; 

console.log(greet); // result : Hola! obiviously  synchronously these lines of code will be run in order. So, the greet should be hola.


// So since object literal syntax is creating this on the fly, I could do this like this, and then greetings is created, and then basic is created. And so I don't get any undefined errors because it's all created at once.

var english = { 
    greetings: { 
        basic: 'Hello!' 
    } 
};

var spanish = {};

spanish.greet = 'Hola!';

console.log(english); 
```

* But let's imagine that these two variables "var greet  = 'Hello!';" and "var greet  = 'Hola!';" were created in two different JavaScript files.

* Maybe one for English greetings, and some other library or framework for Spanish greetings 

* This would be a problem because we know that both are setting their value, in this case on the global object, and overwriting each other, as we've seen before.

* Namespaces would help us with this because, in the case of namespaces we would have a container for the English greeting and a container for the Spanish greeting, and it's related methods and properties.

* And that we don't have name spaces in JavaScript, we can do just that with objects.

* So we can prevent this collision for example, by creating an object that will be the container for our properties and methods and the things we want to use.

* So have created an English object, just using object literal syntax, and a Spanish object creating object literal syntax.

* And so this variable has just become, basically a container, to make sure I don't collide with other things, with other JavaScript files, with other variables in the global namespace, etc.

* So this you'll see a lot inside of frameworks and libraries and you can use in your own code as a method, to make sure that when you're writing a function and someone else is writing a function, that we don't have any namespace collisions.

* Now, what's nice is you could make this as deep as you want. You could keep having levels of different containing objects.

* So I can do it either way, but the idea here is that I have contained my properties. I've contained my variables, maybe my functions and methods, and other objects, inside a container object, and that's all it's really doing. It's just keeping us separate from any other code, any other objects sitting in the global namespace from the global object that someone else might have written or even that we've written and forgotten about. So, that's faking namespaces in JavaScript.

### JSON and Object Literals

* JSON, if you haven't heard of it before, stands for JavaScript Object Notation. It's inspired by object literal syntax in JavaScript and so it's called JavaScript object notation.

* And it looks an awful lot like object literal syntax. But a common mistake is to think that they're the exact same thing and then run across some errors.

* Lets look this example

* In previous years, data was sent over the internet in various formats and the format that was landed upon for a while was xml.

* Now that's fine but the problem is when you're dealing with download times, how fast something is and how much data, how much bandwidth are you using, this is a lot of extra unnecessary characters that make the amount of data that you're sending larger. Just to send one piece of data I'm actually sending the property name twice in XML

* That's a huge amount of wasted download bandwidth if you were dealing with a lot of data. So what people did was looked at the JavaScript object notation and say, hey, that would make a really great way to send data across the Internet instead of sending it in this format.

* And so now a days we pretty much send data via the JSON format. Its just a string of data but it looks like object literal syntax except for some differences.

* For example, properties have to be wrapped in quotes

* Your properties can be wrapped in quotes in JavaScript object literal notation in that syntax. This is just fine.

* But they have to be wrapped in quotes in JSON.

* when it says I parse JSON, it expects the properties to be wrapped in quotes.

* So JSON is technically a subset of the object literal syntax. Meaning that anything that is JSON valid is also valid JavaScript object literal syntax.

* But not all object literal syntax is valid JSON. So JSON has stricter rules about what it can be.

* So JSON, JavaScript Object Notation, isn't really a part of JavaScript. But because it's so popular and because it's so easy for JavaScript to understand what this is, JavaScript does come with some built in functionality to transfer between the two. So, for any object I can do JSON, this is a built in feature in JavaScript, .stringify. Give it that object.

* And it will convert the object to a JSON string. And, if I have a string that's JSON, so let me make this a string, I'll wrap it in single quotes. Note that I'm using double quotes here inside and single quotes on the outside so that they don't collide and cause an error.

```js
var objectLiteral = {
    firstname: 'Mary',
    isAProgrammer: true
}

// So here I'm taking an object in JavaScript and stringifying it so it becomes JSON syntax 

console.log(JSON.stringify(objectLiteral));

// in this case I'm taking a string that's proper JSON and converting it back to an object.

var jsonValue = JSON.parse('{ "firstname": "Mary", "isAProgrammer": true }');

console.log(jsonValue);

```
* people get confused about object literal versus JSON string. They're two different things.

### Functions are Objects

* First class functions means that in the programming language. JavaScript isn't the only language that has first class functions, but I would say it's definitely the most popular nowadays.

* Everything you can do with other types, objects, strings, numbers, booleans, you can do with functions.

* You can assign variables to have a value that's a function.

* You can pass functions around as parameters to other functions.

* You can create functions on the fly with the kind of literal syntax.

* First class functions change the way you can program. They can open up the horizons to completely different approaches to solving problems.

* So when we say that functions are objects in JavaScript, what does the function object look like?

* just like any object in JavaScript, it resides in memory. It's a special type of object, though, because it has all the features of a normal object and has some other special properties. So one thing that people are surprised about when they see JavaScript is that you can attach properties and methods to a function. Why? Because it's just an object.

* So I could attach a primitive, a name value pair. I could attach an object. I could attach other functions. This actually does happen and can be useful in JavaScript, and we'll see that later.

* In JavaScript, though, the function object has some hidden special properties. Two important ones are its name.

* Now a function in JavaScript doesn't have to have a name. A function, then, can be what's called anonymous, anonymous meaning it doesn't have a name. But it can have a name.

* There's another property called the code property where the actual lines of code that you've written sit.

* the code that you write gets placed into a special property of the function object. So it isn't like the code that you write is the function.

* The function is an object with other properties. And the code that you write is just one of those properties that you're adding onto it.

* What's special about that property is it's invocable, meaning that you can say, run this code. Run the code sitting on that property. And that's when the entire execution context creation and execution on all of that happens.

* It's imperative that you have this mental model of a function in your mind as we go through the rest of this course.

* You have to think of a function object whose code just happens to be one of the properties of that object.

* There are other things the function can do. And it can be moved around, copied and given to other elements or other areas of your code, just like any object or value, just like a string or just like a number. This allows for some fascinating programming approaches.

* And it allows for some really interesting syntax that can look really weird if you don't understand this fundamental concept.

```js

function greet() {
    console.log('hi');   
}
// But remember we said that functions are actually objects in JavaScript. So using dot notation I can create a property.

greet.language = 'english';

// It's in memory. It's a property attached to the function, just like you could attach a property to any object.

console.log(greet.language); // result : english

```
* Because in JavaScript, functions are what? Objects !!!!

* So when I created this function, what did it look like? I had this greet function, and when it was created, this function object was put in memory, in this case, onto the global object, and it had a name.

* So the name property is greet, because that's what I named my function. And it has a code property that contains the code that I wrote, the body of my function.

* And so if I was to call greet using the parentheses, that invokes that code, causes it to run, causes that execution context to be created, etc., etc.

* See how this works? You have to think of a function as more than just this container of code. It's an object, and as such, you can pass it around. It sits in memory at a specific location. It has properties, it has methods. Why? Because, say it with me, in JavaScript functions are objects.

### Function Statements and Function Expressions

* So now that we've seen that functions are objects in JavaScript. We're going to see how the JavaScript programming language lets us do some really interesting and powerful things with this concept. The concept of first class functions.

* An expression is a unit of code that results in a value. And that's it.

* function expression or any expression in JavaScript ends up creating a value and that value doesn't necessarily have to save inside a variable.

* the equals operator returns a value. This happens to set some values in memory,

* And the plus sign as an operator, takes these two values, and adds them, and returns the result. but notice that I didn't set it equal to anything in memory.

* But in both cases this is an expression. Because it returned a value. Get that? That line of code results in a value.Now that value could be a number or a string or an object, whatever the case. But it's still ends up being a value.

* when we're talking about a statement, on the other hand, let's say I do an if statement, If a equals three and then do something. Inside the parentheses of an if statement, you put an expression because that results in a value. But the if statement itself is just a statement.

* It's called an if statement because it doesn't return a value. I can't say variable b equals and then if, because this just does work. No value is returned.

* The if is a statement, and inside the if statement you have an expression, because the expression this triple equals returns a value ultimately true or false. Get that.

* So statement just does work and an expression results in a value.

* In JavaScript, because functions are objects, I have both function statements and function expressions, which are extraordinarily powerful.



```js
// We've already seen that it gets hoisted. That is, during the creation phase of the execution context, the global execution context, in this case, it's put into memory.

// And so it's available ahead of time.

// I can call greet before I declare it, before I make that function's statement.

// So it's available to me in memory. That's fine.

greet();

// this is a function statement , It doesn't return a value until the function is executed.
// So when I see this, it does say okay, I'm putting the function in memory. But this unit of code doesn't result in a value.


function greet() {
    console.log('hi');   
}

// Now I'm going to use a function expression, I use the equals operator.

var anonymousGreet = function() {
    console.log('hi');   
}

// Now wait a minute, does that look strange? What are we doing? 

// Well remember functions in JavaScript are objects. So I'm creating an object on the fly and setting it equal to this variable in above code . That is this variable, it's spot in memory that it points to. It's address in memory that it points to will contain a function object. Make sense?

anonymousGreet();


```

* So what's really going on here? Well, remember what happened. Let's look at our function declaration again, our function statement. This is the one that gets pulled up into memory when the execution context is created. And it has this name property, and this code property, that get set.So I have an object in memory, the name is greet, because that's where I put before before those parentheses, and it has a line of code. And when I call it.It's connected to that spot in memory where this function object lives,and those parens then invoke the code part of the function.

* Now what about my function expression? In this case, I have an equals operator that's putting this object into memory. And pointing that anonymous greet variable at that address. Remember, it's not really looking at the name of the function or anything like that. It knows the address in memory just like you have an address for where you live. So we still get a function object. The JavaScript engine creates one.

* And in this case I didn't have a name. Because, I didn't put anything before the parenthesis, and I don't have to. Because, I have a variable that already knows where that object lives. So, I don't need a name to reference it by. So this is an anonymous function.

* An anonymous function is a function that doesn't have a name in it's name property. But that's okay, because you can reference it with variable names that are pointing to the address where that object lives.

* So how do we invoke this function? 

* The variable is already pointing to where it lives. And then I put parentheses, which invokes the code."anonymousGreet();"

* And this right here by itself. In this case, is an expression. Because it results in a value. I end up with the function object when this part of the code is evaluated, or considered, or executed. 

* Good code is about being clear and understandable, while at the same time minimizing the amount of code that we need to write. So, the anonymous function is great for this. It's very clear that I'm creating a function object.And it's not difficult to understand how to invoke, how to call it. How to run the function.

* Now, that said, let me ask you a question. if i move anonymous function call to top like hoisting

```js

anonymousGreet();

var anonymousGreet = function() {
    console.log('hi');   
}

```

* What do you think will happen here and why?

* Think about what you know about the execution context being created, and then execute it. Executing code line by line. Knowing that it puts function statements, and variables into memory first. And what it does with variables when it puts them into memory And the how it runs line by line through the code.

* What do you think you'll see here? Uncaught TypeError: undefined is not a function.

* Did that surprise you? Well it shouldn't.

* If you have a named function it will put it in memory. 

* But here in anonymous function it sees a variable. It puts it into memory and  what does it set the variable equal to before it starts executing the code? All variables. That's right. The special primitive called undefined.

* So function expressions aren't hoisted !!!!!!!*****. And that can confuse people.

* But if we think about it from the perspective of how JavaScript does this under the hood, it's not really surprising. It just only hoisted the variable. And so the variable doesn't contain an object until this line of code is run in the execution phase.

* All right, so let's do one more cool thing.

* But, remember what we just learned. A function is an object. And a function expression creates an object, a function object on the fly.

```js
function log(a) {
    // invoking function which is passed as param
   a();    
}

// I can say function, and create a function on the fly.

// I'm creating a function on the fly, putting some code, it creates that object. Puts that code into that code property of that function object. It's just like a string, or a number, an object. I'm just creating a function on the fly, because functions are objects. And so that will get passed to the function. It's anonymous, but it's referenced with that "a" parameter.

log(function() {
    console.log('hi');   
});
```

### Conceptual Aside: By Value vs By Reference

* understanding this concept will be important. Not only for your Java script development skills, and debugging skills,
but also for some important things we're going to talk about in just a bit.

* In both cases, we're talking about variables

* So let's say I have a primitive value. One of those primitive types, a number, or a bullion, or a string. And, I've set a variable equal to it. So, now that variable, let's say a, has an address location, where it knows where that primitive value sits, in memory.

* Remember, we said that the reference is really to a location in memory.

* And let's suppose then I run b = a. I set up a new variable, and it's equal to a. "b=a"

* Or maybe I pass a to a function, and the parameter name, and the function is b. If it's a primitive value in JavaScript, this is what happens. 

* B, the new variable, points to a new address. A new location in memory. And a copy of the primitive value is placed into that spot in memory.

* So if a is 3, it sits in one address. One location in memory. B then also, will point to 3 for the copy, a separate location, in memory, that has been filled with that same value. This approach is called by value.

* These two variables (a and 3) become the same, by copying the value (3 and 3) into two separate spots in memory.

* Now, if i have an object in JavaScript, and this goes for all objects, that includes functions which are special types of objects and others. When I set a variable equal to an object, I still get a location, an address in memory that it knows where that object lives. And that's how I reference it.

* But when b is set equal to a, when I'm essentially trying to get those two to be the same value, or I pass a to a function, that new variable b, instead of getting a new location in memory, simply points to the same location in memory that a does. No new object is created. No copy of the object is created. Instead, two names point to the same address.

* This would like if you had two names, an alias. And yet, both of those names, it doesn't matter because you still live in the same place. In this way, both a and b, have the same value, because when you ask them what their value is, they look at the same place in memory. This is called by reference.

* And by reference behaves quite differently from by value. It's important to understand that all objects interact by reference, when sending them equal to each other, or passing to a function. And this can cause problems if you don't understand this, weird problems that can be hard to debug.But once you do understand it, it becomes quite clear.


```js
// by value (primitives)
var a = 3;
var b;

b = a;
a = 2;

console.log(a); // result :  2
console.log(b); // result : 3   because b was just a copy of a. It has its own space in memory. So when I changed a, it didn't have any impact on b at all. That's by value.

// by reference (all objects (including functions))
var c = { greeting: 'hi' };
var d;

d = c;
c.greeting = 'hello'; // mutate - change something , immutable  means can't be changed

console.log(c); // result : {greeting: "hello"}
console.log(d); // result : {greeting: "hello"}

// So, with by reference, objects, since they're set equal to each other by reference, once you change one, you change the other, or all of them.


// ********************************* Start ****************

// by reference (even as parameters)
function changeGreeting(obj) {
    obj.greeting = 'Hola'; // mutate   
}

changeGreeting(d);
console.log(c); // {greeting: "Hola"}
console.log(d); // {greeting: "Hola"}

// Because I've mutated the value in that particular memory space. Again, by reference.

// ********************************* END ********************


// equals operator sets up new memory space (new address)
c = { greeting: 'howdy' };

//  the equals operator is going to, in this case, set up a brand new memory space for c, and put this value in it.

console.log(c);    // {greeting: "howdy"}
console.log(d);  // {greeting: "Hola"}
```

* In Javascript all primitive types are by value, and all objects are by reference.

###  Objects, Functions, and 'this'

* We've talked quite a bit about what a function really is. It's an object with properties and various things that happen. Now it's time to take a step back and look again at what happens when the code of a function is invoked, when a function is Run.

* Remember those execution contexts that we talked about at the beginning

* So "this" is Objects Functions and the sometimes confusing keyword "this"

* Just as a reminder, when a function is invoked a new execution context is created.

* Remember not to confuse this with the object that we've been discussing. The object sitting in memory that is a function has properties and methods. It has a name property and a code property where the code lives.

* But when that code is invoked and execution context is created and put on the execution stack.

* And that determines how that code is Run, is executed.

* So think of the execution context as focusing on that code portion of that function object. What happens when I run the code in that code property?

* So we know that an execution context is created. Each executions context has this variable environment. Where the variables created inside that function live. It has a reference to its outer environment.Its outer lexical environment, where it sits physically in the code, which tells it how to look down the scope chain.

* In other words, if I ask for a variable and it's not there inside that function's variable environment. It'll go out and out further, and all the way out until it reaches the global environment looking for that variable or that function. So it has that outer reference.

* And we also know that in JavaScript engine every time an execution context is created, that is every time a function is Run. It gives us, without us having to create it, declare it or anything, it gives us special variable called "this", which can be useful.

* And "this" will be pointing at a different object, a different thing, depending on how the function is invoked. This can cause a lot of confusion.

* There are a few scenarios where "this" will be changed depending on how the function is called. 

* For the JavaScript engine will decide that "this" should point to something different. That the "this" keyword is a particular object or another, depending on where the function is and how it's called.

* We've already seen that this is immediately available, the keyword this. Even at the global execution context level.

* So I can run just console.log(this), and inside the browser, it's going to show me the window object. Because this points to the global object at this level in the code.And inside the browser, the global object is, as you might recall, the Window object.

```js
function a() {
    // So what will the keyword "this" be inside the execution context that's created by invoking a?

    console.log(this); // result : window object 
    // So when you create a function, the this keyword is still going to point to the global object.

    this.newvariable = 'hello'; // here we attached newvariable to global object "this"
}

// Similarly, if I use a function expression to set up the object, and then set that equal to a variable.


var b = function() {
    // So, var b gets this function expression to declare it, to create it. What will "this" be in that case?

    console.log(this);   // result : still its window object 
}


a(); // invocking a() function ,  And the first thing it does is create that execution context, and one of that pieces of the puzzle is the creation of the keyword "this".

// here console.log(newvariable) any variables attached to the global object, I can just reference like that. I don't need to use the dot operator.It just assumes I'm asking for a variable on the global object.

console.log(newvariable); // result :  hello -  not good! beacuse  If you don't understand what this keyword is pointing to and you think you're somehow attaching "this" to the function, you're not. You're actually crashing into the global namespace and you could cause yourself a lot of problems.

b(); // invocking b() function expression


/// So whenever I create a function that's simply a function expression or a function statement. Creating a function at this level in the code, then this will point to the global object.

// And that's true even though there's actually three execution contexts that we see here. The global one, then the one that's created when a is invoked, and another execution context that's created when b is invoked.And in each of those cases, they get their own this keyword.

// But in all those cases, the keyword points to the same address, the same location in your computer's memory. So they're all pointing at the global object


// So when you're just invoking the function, this points to the global variable.

// Now that said, what about an object method? let us create object literal 

// now that we understand function expressions, I can do something that we haven't done before inside an object literal. I can create a method.

// Remember, if the value is a primitive, it's called a property. And if the value a function, it's called a method.

var c = {
    name: 'The c object',
    // anonymus fuction express as value for object log
    log: function() {
        // What will this turn out to be?  Remember every time a function is invoked, a new execution context is created and the JavaScript engine decides what that keyword the keyword "this" should be pointing to.

        // In these other cases, it was the window object. But in this case, it's a method on an object

        console.log(this) // result: object { name: 'The c object' } - In the case where a function is actually a method attached to an object. this keyword becomes the object that, that  method is sitting inside of c.

        // The JavaScript engine said, you're attached to an object. So when you see, when you use the this keyword I'm going to be pointing at that very object that contains you.

        // I can simply say I'm going to set a variable. I'll call it self. And I'm going to set that equal to this.It was the very first line of my object method. So what happened right here?
        
        var self = this; 
        // Well, now we have a new variable called self, and since these are objects, it's going to be set equal to by reference. So self will be pointing at the same location in memory as the this keyword.

        // And right now, the this keyword on this line of code is pointing to my whole object.

        // And then for sanity's sake, we simply use self everywhere where we normally would have used this, even inside these sub-functions. That way I don't have to think about, am I pointing to the right object?
        
        self.name = 'Updated c object';  // mutation

        console.log(self); // result : object { name: 'Updated c object', log : function..... }

        // **************

        // Now there's one more thing to show you and this a lot of people feel is a bug in JavaScript. Now you might be saying, what do you mean a bug in JavaScript?

        // Well, JavaScript is a programming language and the engines and the language was designed by people and so decisions were made about how things should work. And in this one case, this decision a lot of people feel is wrong. Let me show you.
        
        // Let's suppose that I create a function inside this (log) method
        var setname = function(newname) {
            // this.name = newname // this keyword refers to global object not "c" object
            self.name = newname;   
        }

        setname('Updated again! The c object');
         
       // console.log(this); //  result : object { name: 'Updated c object', log : function..... } - !!! not 'Updated again! The c object' why ?? That name property here was instead created and added by the equals operator on the global object.

       // That means that this internal function, when its execution context was created, the this key word points to the global object, even though it's sitting kind of inside an object that I created.

        console.log(self);  //  result : object { name: 'Updated again! The c object', log : function..... } 
        // *********
    }

}
// self keyword refers to whole object

c.log(); // 
```
* So what did we learn? We learned that no programming language is perfect.They all have their quirks, and JavaScript certainly isn't an exception.

* Now I also wanna make mention that this pattern you'll see very often if you're working in any real-world JavaScript scenarios.

* However, the let keyword, which will be an alternative to the var keyword, is meant to clear some of these problems up.So as that becomes available in modern browsers,

* But for now, realize that this pattern(self) is a good one. It is used quite often and is quite useful.

* So we've seen the this key word. It's the global variable, or the global object, when just invoking a function like this. And when the function is a method of an object, the this keyword points to the object.

* However, any internal functions have a problem. So we can use this concept of setting a variable (self) equal to this, and then just carrying that with us the rest of the way to make sure that we don't run across any unintentional errors,or somehow throw things onto the global object .

### Conceptual Aside: Arrays - Collections of Anything

* In most programming languages, an array can hold a list or a collection of things of a particular type. So an array of numbers, or an array of strings, or an array of objects. But JavaScript doesn't like that. It figures out what types of things are on the fly. It's dynamically typed.

* So I can put whatever I want into an array. And I can mix and match each individual item in an array can be a different type let say number, string, boolean, objects and also functions

```js
var arr = [
    1, 
    false, 
    {
        name: 'Tony',
        address: '111 Main St.'
    },
    function(name) {
        var greeting = 'Hello ';
        console.log(greeting + name);
    },
    "hello"
];

console.log(arr);
// So a JavaScript array can hold collections of anything.

// And how do I then run the function? which is in array ?? How do I invoke a function?

// arr[2].name - name pased to function 

arr[3](arr[2].name);
```

###  'arguments' and spread

* It's time to talk about another special keyword that the JavaScript engine sets up for you automatically when you execute a function. It's called arguments.

* And since in the next version of JavaScript, we probably won't use arguments as much, we'll mention the new approach to doing what arguments does currently which was called spread.

* As we've seen many times before, when you execute a function, a new execution context is created, and the JavaScript engine sets up some things for us like a variable environment to hold our variables, an outer environment reference for the scope chain, the special keyword, this, which will point to different things depending on where the function lives or how it's called, and it also sets up another special keyword, called arguments.(Refer arguments image)

* Arguments contains a list of all the values, of all the parameters that you pass to a function.So arguments holds all those values that you pass to whatever function you're calling.

* arguments, the concept of arguments in general, is just another name for the parameters you pass to a function. So you could say your parameters.You could also say your arguments.That's really in the case of any programming language that has functions.

* However, JavaScript gives you a keyword of that same name which contains them all. So when we're talking about the concept of arguments, we're just talking about the parameters you pass to your function. But the special keyword arguments is something special that the JavaScript engine sets up for you.


```js
function greet(firstname, lastname, language) {
 
    language = language || 'en'; // you could use in paramater itself as language = 'en' as default value, However, as that's not available in all modern browsers yet.older ones as well, you can use that default parameter concept,the default value concept, to set up default parameters.
    
    if (arguments.length === 0) {
        console.log('Missing parameters!');
        console.log('-------------');
        return;
    }
    
    console.log(firstname);
    console.log(lastname);
    console.log(language);
    console.log(arguments); // containing a list of all the value of the parameters that I've passed.
    console.log('arg 0: ' + arguments[0]);
    console.log('-------------');
    
}

greet(); // Now here's what's unusual about JavaScript compared to other languages. I can just call greet and not have any parameters passed to it. In other programming languages this would be an error.It would say, well I expect these values, but JavaScript doesn't care.

// So, hoisting, that thing that happens when the function is executed, where it sets up the values, takes care of these parameters for me even though I haven't given them values. It executed the greet function, and the first thing it did was set up memory space for firstname, lastname, and language and set them equal to undefined, just like we've already learned.

greet('John'); // And then, if I start to pass arguments, they'll be processed left to right. And when I run this, you'll see now that firstname holds John and the other two are still undefined because of hoisting.(imagine your are not passing default value/param )
greet('John', 'Doe');
greet('John', 'Doe', 'es');

// So, this means that you can skip the passing of parameters or you can pass only part of this list of parameters, and JavaScript's okay with that.

// in ES6 I can do:  function greet(firstname, ...other)

// and 'other' will be an array that contains the rest of the arguments .So this, ..., means take everything else, and wrap it up into an array of this name 'other'. This isn't completely available yet either, but once it becomes available, then it will be the preferred approach to dealing with various sequences and various numbers of arguments of parameters. So,that's just good to know about.
```

###  Framework Aside: Function Overloading

* Let's talk for a moment about something that JavaScript doesn't have that many other programming languages do, and why it doesn't matter. This is function overloading.

* In other programming languages like C#, C++, Java, there's the idea of function overloading.

* What that means is I can have a function of the same name that has different numbers of parameters.

* Now this doesn't really work in JavaScript because functions are objects. So that functionality isn't available in the way that JavaScript deals with functions.

* But that's okay because having first class functions introduces a lot more options. But, what if you did want to somehow have some alternatives for how you call the greet method? For example, what if I wanted to not always have to pass the language?

* Well, you've already seen that we can default the language. And then have all our logic inside this function to decide what to do when the language is under different circumstances.

```js
function greet(firstname, lastname, language) {
        
    language = language || 'en';
    
    if (language === 'en') {
        console.log('Hello ' + firstname + ' ' + lastname);   
    }
    
    if (language === 'es') {
        console.log('Hola ' + firstname + ' ' + lastname);   
    }
    
}

function greetEnglish(firstname, lastname) {
    greet(firstname, lastname, 'en');   
}

function greetSpanish(firstname, lastname) {
    greet(firstname, lastname, 'es');   
}

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doe');


// They call the same greet function inside of them, but by default, passing a certain parameter value. This is one approach to dealing with having simpler function calls.
```

* But just remember, if you're coming from another programming language and you're worried about function overloading, don't. There are other patterns and in some ways, patterns that are better in JavaScript that we'll see as we go along,because we have first class functions.

### Conceptual Aside: Syntax Parsers

* Let's talk again about syntax parsers. Remember that the code you write isn't directly run on the computer, but there's that intermediate program between your code and the computer that translates your code into something the computer can understand. The JavaScript engine on your browser for example does this.

* And it has different aspects and elements to it, one of them being a syntax parser, which reads your code and determines if it's valid, and what it is you're trying to do.

* So for example, if it sees in your code as it's going character by character an r by itself, it assumes that you're probably going to write a return statement, so it's expecting an e. And it will go character by character, and if it sees something unexpected, it will throw an error.

* But if it sees what it anticipates as valid syntax, character by character, then it keeps on going, it knows what you intend to do. And it may come across an ending or terminating character like a semicolon.

* So, imagine in your mind that the JavaScript engine, the syntax parser that's part of that JavaScript engine, is going through your code character by character, making assumptions, stating certain rules, and can even make changes to your code before it's executed.

* So, that's exactly what happens in certain circumstances in JavaScript, and it's important to think this way about how the JavaScript engine is reading your code. Character by character, using a set of rules for what's valid syntax and deciding what it is that you intend.

* And that's all happening before your code is even executed, so it can make changes if it wants to the code that you've actually written.

### Dangerous Aside: Automatic Semicolon Insertion

* This is when we stop to warn you about a real danger in the programming language. We've already said that no programming language is perfect. And we've seen some caveats in the JavaScript language.

* But a Dangerous Aside is where we're touching a subject that's so dangerous. It's so easy to make a mistake and it's so hard to track down. That you need to always avoid it.

* This Dangerous Aside has to do with syntax parsers in JavaScript. And its automatic semicolon insertion. The syntax parser in JavaScript does something that tries to be helpful. You may have noticed that semicolons are optional in core JavaScript. You don't have to put a semicolon.

* So why is this? Because the JavaScript engine does something for you. If it sees one character at a time, a certain statement, it knows what the language expects. It knows what the syntax should look like. So if it sees that you're finishing a line, that is, a carriage return. Now a carriage return, that is when you hit Enter on the keyboard.It's an invisible character, but it is a character.

* So the syntax parser sees it, knows what it is, and it says, hey you're not allowed to go to the next line with this particular type of syntax, with this particular statement.

* So I'm going to go ahead and insert, inject automatically a semicolon for you. So anywhere where the syntax parser expects that a semicolon would be, it will put one for you. That's why it's optional when you're typing it. Not because it's truly optional, but because the JavaScript engine is putting them where it thinks they should be, if they're missing.

* And that's all well and good, so rule one, with this Dangerous Aside, you should always put your own semicolons. Because you don't want the JavaScript engine to make that decision for you. You want to be certain that you are writing the code as it should be.

* But more than that, especially in the case of return, automatic semicolon insertion can cause a big problem in your code. So I'm gonna create a simple function called getPerson. 

```js
function getPerson() {
 
    return {
        firstname: 'Tony'
    }
    
}

console.log(getPerson()); // undefined Why? Because of automatic semicolon insertion. The JavaScript engine, if it sees a carriage return after the keyword return,it will automatically insert a semicolon.
```

* Even though you wrote something else, the syntax parser chose to change your code. And so by putting this object literal syntax on a new line, it caused the JavaScript engine to decide to put in a semicolon, so it simply returned. It quit out of the function. And all that you got was an undefined result.

* To fix this, we have to tell the syntax parser what we're doing. We have to prevent it from doing automatic semicolon insertion. Because as it goes character by character.

* So, you may have noticed that I always put my curly braces on the same line as my functions and for loops and if statements. That's not always necessary. This is perfectly valid but I do it so I never make this mistake.

* So remember this. This is how, if you're going to return an object from a function, you need to type it. Otherwise it won't work because the syntax parser doing automatic semicolon insertion. And everywhere where you would expect to have a semicolon, you should put one to avoid this problem as much as possible. It's very dangerous and can be hard to track down.

### Framework Aside: Whitespace

* Whitespace, invisible characters that create literal space in your written code, like carriage returns, tabs, or spaces. They make your code more readable.

* But of course that's not what's really being executed on the computer system. But we can use whitespace to help ourselves, and JavaScript, its syntax parser, is very liberal about what it allows when it comes to whitespace. And we can take advantage of it to help ourselves out in our written code.

```js
var 
    // first name of the person
    firstname, 
    
    // last name of the person
    lastname, 
    
    // the language
    // can be 'en' or 'es'
    language;

var person = {
    // the first name
    firstname: 'John',
    
    // the last name
    // (always required)
    lastname: 'Doe'
}

console.log(person);
```

### Immediately Invoked Functions Expressions (IIFEs)

* What are immediately invoked function expressions? How do they work and why are they useful? Let's look.

* So we've already seen the difference between function statements and function expressions.

```js
// function statement
function greet(name) {
    console.log('Hello ' + name);   
}
greet('John');

// using a function expression
var greetFunc = function(name) {
    console.log('Hello ' + name);
};
greetFunc('John');

// using an Immediately Invoked Function Expression (IIFE), we can invoke it on the fly.
var greeting = function(name) {
    
    return 'Hello ' + name;
    
}('John'); // passing params and invoking this function  - This invokes the function immediately after creating it.

console.log(greeting); // returned value from IIFE, since above function is already invoked 



// IIFE - standalone function expression
var firstname = 'John';
 
/// Remember, it can just run an expression and it doesn't have to store in a variable or anything, it just is there. It runs it, it executes it and that's it. That is so not to do anything with it. So, I could do that.

// If I just put function, this isn't a function expression. It's a function statement.

// But the most accepted, because it's the easiest to read and look at and it's the least amount of typing is to wrap your function in parentheses. This is when you want a function expression,instead of a normal function statement that you might be used to.

(function(name) {
    
    var greeting = 'Inside IIFE: Hello';
    console.log(greeting + ' ' + name);
    
}(firstname)); // IIFE


// I wrap it in parentheses just to trick the syntax parser and then I put parentheses to actually run it.

// So, I'm creating a function and running it all at the same time and I can pass values to it.

//  but you could invoke it inside the parentheses or you could invoke it outside the parentheses. It doesn't really matter, both work the same way.
```
### Framework Aside: IIFEs and Safe Code

* So let's take a look at what's happening behind the scenes. Let's think about our execution stack.

* My immediately invoked function expression that's creating this function on the fly and then executing it, invoking it.

* When this code is first loaded, I have my Global Execution Context. And nothing's in it because I have no variables no function statements to be hoisted, or anything like that. At first anyway.

* Then it hits this line, this whole immediately invoked function expression. So when it gets to just the function expression part, it now creates that function objected memory. And it's anonymous. There's no name. So it's just this object that has this code in it.

* And then it sees those parentheses that's actually invoking the function. And so, as we've already learned, a new execution context is created for that anonymous function that I've just created here on the fly. And then the code is run line by line inside that function. And we've talked about that after the creation and then the execution phase of each execution context.

* Then this variable greeting will enter into this anonymous function execution context not into the Global Execution Context. because we're executing a function, we have our own execution context that's running. So, any variable I declare, I create inside that function is not touching the global environment. It's sitting all by itself, inside that execution context.

```js
// greeting.js
var greeting = 'Hola';
```
* add this file source in HTML file

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <script src="greet.js"></script>
    <script src="app.js"></script>
</body>
</html>
```
* in app.js

```js
// IIFE
// I call it global here because maybe you want this code to be reusable on the server where window object wouldn't be the global object,
(function(global, name) {
    
    var greeting = 'Hello';
    global.greeting = 'Hello'; // here we intentionally mutating the global object 
    console.log(greeting + ' ' + name); 
    
}(window, 'John')); // IIFE - Remember that objects pass by reference.So, if I want access to the global object, I just pass it into my function. I'll call this global. And what's the global object in the browser? Window.

console.log(greeting); // result : Hello because we already affected this global object in IIFE, otherwise it will be Hola
```

### Understanding Closures

* Closures is a JavaScript topic that's absolutely vital to understand

```js
// function statement return a function
function greet(whattosay) {
    // here we returning a function expression
    return function(name) {
        console.log(whattosay + ' ' + name);
    }
 
 }
 //greet('Hi')('Tony') - you're invoking a function  "ie greet function" that returns a function. So then I can invoke the function that was returned. 

 //** same as above but bit clear code 

 var sayHi = greet('Hi'); // return a function you can invoke again/later
 sayHi('Tony'); // invoking the function which is returned from the greet function
```
* Let's stop and think about this for a minute. How does the sayHi function still know the whattosay variable?

* Because the whattosay variable was created here when this greet function was called, and then that function is done, it's over. It completed its execution. It's popped off the execution stack.

* And yet when I call say high, when I invoke it, still has the proper value of whattosay. See that? How is that possible?

* It's possible because of closures.

* Let's take a look at what's happening under the hood when this code is executed.

* I have my code. And remember this seems unusual because the greet function ends. And when I invoke the function that is returned, it seems like that greet function is still hanging around somehow, because the whattosay variable is still there. So what's happening?

* Well, when this code starts, we have our Global Execution Context. When I hit this line sayHi = greet('Hi'), it invokes the greet function, the new execution context is created.

* And that variable that's passed to it, whattosay, is sitting in its variable environment. It returns a new function object. It creates a function on the fly, and returns it. And that's it.

* So after that return, the greet execution context is popped off the stack. It's gone. But here's a question. We said every execution context has this space in memory, where the variables and functions created inside of it live.

* What happens to that memory space when the execution context goes away? Well under normal circumstances, the JavaScript engine would eventually clear it out with a process called garbage collection.But at the moment that execution context finishes, that memory space is still there. It's still hanging around.

* The execution context may be gone, but it's just sitting there somewhere in memory. All right, now we move on and we're inside the global execution context again.

* And then we invoke the function that sayHi is pointing at. It's an anonymous function, because we didn't give our function a name when we returned it.

* And then that creates a new execution context. And I've passed the name variable, Tony. So that will end up in its memory. But when I hit this line, console.log. When its code is invoked, and JavaScript engine sees the whattosay variable, what does the JavaScript engine do? 

* Well it goes up the scope chain. We've learned this. There's an outer lexical environment reference. In other words it goes to the next point outside where the function was created to look for that variable, since it couldn't find it inside the function itself.

* And even though the execution context of that function greet is gone, was popped off the stack, the sayHi execution
context still has a reference to the variables, to the memory space of its outer environment.

* In other words, even though the greet function ended, it finished, any functions created inside of it when they are called will still have a reference to that greet function's memory. That was in its memory, its execution context memory space.

* Think about this for a second. Greet is gone, the execution context is gone. But what's in memory for that execution context isn't and the JavaScript engine makes sure that my function can still go down the scope chain and find it.Even though it's not even on the execution stack anymore.

* And this way we say that the execution context has closed in its outer variables, the variables that it would normally have reference to anyway.Even though those execution contexts are gone.

* And so this phenomenon, of it closing in all the variables that it's supposed to have access to, is called a closure.

* It isn't something, then, that you create, that you type, that you tell the JavaScript engine to do. Closures are simply a feature of the JavaScript programming language. They just happen.

* It doesn't matter when we invoke a function. We don't have to worry if its outer environments are still running.The JavaScript engine will always make sure that whatever function I'm running, that it will have access to the variables that it's supposed to have access to. That its scope is intact.

* This is a feature of the language that's extraordinarily important and powerful. We rely on it a lot. It allows us to make some really interesting coding patterns.

* They're just a feature to make sure that, when you run a function, it works the way it's supposed to; that it has access to those outer variables. It doesn't matter whether the outer functions have finished running or not. So when you say, oh I create a closure. no we won't create closure , The JavaScript engine creates the closure. We're just taking advantage of it.

### Understanding Closures - Part 2

```js
function buildFunctions() {
 
    var arr = []; // empty array
    
    for (var i = 0; i < 3; i++) {
        // pushing functions to array - arrays are collections of anything, so I can put functions inside of them.
        // actullay we are pusing 3 different functions into array

        //  Now, you need to remember that this isn't invoking the function, it's just creating it and putting them into the array
        arr.push(
            function() {
                console.log(i);   
            }
        )
        
    }
    // Equivalent functions, identical functions, but three different functions, and this function will just return that array.
    return arr;
}

// I'm going to get that array by calling buildfunctions.So now I have my array in this variable.
var fs = buildFunctions();


// And then, remember that these are functions inside the array, so I can reference an item, an element inside the array and then I'll call it.

// What do you expect this console.log to output in each of these three cases? It's outputting i.

// So, when this function is actually invoked and it looks at I, and it looks at its outer reference, what will I be?


fs[0](); // result : 3
fs[1](); // result : 3
fs[2](); // result : 3

```


* Are you surprised by that? Why would in every case, when it looks for i and goes out to the outer reference, why would it find a three in all cases?

*  But what have we learned before? What's in memory is still hanging around.

* Well, maybe you've figured this out, but let's go ahead and take a good look at what's happening under the hood.

* We have buildFunctions that is pushing these functions into this array, and then we call those functions at the bottom.

* So we have an array of three functions at the end of this, and each of them console.log's the value of i. And when we ran this, everyone of those function calls logged three. So why?

* Well, how does the execution stack look like as this is happening?

* Well, the global Execution Context is always there at the bottom and it contains the build functions, and this variable we just called fs.

* And so, we hit this line where it executes buildFunctions(). So, when buildFunctions() executes or is invoked, we get an execution context, and it has two variables, i, which was created on this for loop, and arr, which we declared at the beginning of the function.

* But what are the values of those two variables by the time we hit the return statement?

* Well, the for loop runs, and so, i is at first 0, and it pushes the function into the array, it adds this new function to the array right here. But realize that console.log isn't being executed right here, and that's where a lot of people get confused.

* But, we already understand function expressions. That all that's happening here is I'm creating a new function object and putting that line of code into it's code property. But it isn't actually running, it's just creating that object.I haven't invoked the function.

* So it continues, i becomes a 1, because of the i++. It adds another function object into the array, looks identical but it's a separate function object. Then i becomes 2 because of the i++ and we get a third function pushed into the array.Then the i++ is run again, and i is 3, and the engine sees that i is less than 3 expression, and says oh, i is no longer less than 3, and leaves the for loop. That's how for loops work.

* the i, that variable, now is larger than my statement of when to leave the for loop, so since that's now false, I leave the for loop, but realize that i, its last value when i leave that for loop is now a 3.That's what told me to leave the for loop.

* So when i hit this return arr, what's in memory in that execution context, is that i is a 3, an array, that arr, holds 3 functions. We'll just call them f0, f1, f2, but they're anonymous. So then, we go back to the global execution context, and this buildFunctions execution context is popped off the stack.

* So now we hit this first function call where I take the first element in the array, which is a function, and execute it. The code in the code property is console.log(i), so its execution context is created. There is no variable i inside of its code, so it goes up the scope chain. It goes to its outer reference. Where was it created? Inside build functions, and what is inside the memory that used to be in the build functions execution context. I is 3.

* So it says, all right, console.log 3, and then it finishes. We move on to the next function, the next one inside the array,and an execution context is created, but that one has the same outer environment reference, because it was created in the same place as the first function.

* So its outer environment reference, because of its position physically in the code, because of it's left position, is to the same spot in memory as the first one.And so when it looks for i, it looks at that same spot in memory,
where the build functions held its variables, and sees a three. So console.log(i) results in a three, and the same thing for the third function,

* they all point at the same memory spot going up the scope chain because they were all created inside the same function, buildFunctions. So all three of these have the same parent so to speak.

* This would be like three children and you ask them how old is their father. They're not gonna tell you how old their father was when each of them was born. They're each gonna give you the same answer, how old their father is now.

* In the same way, we have three functions that are being executed later, so when we execute the function, it's only going to be able to tell you what the value is in memory of its parent context of that outer environment reference. It's only going to be able to tell you what's in memory right now, not at the time that we created the function.

* The value of I is what it is at the moment that I execute the function.

* We've seen that first class functions plus this language feature of closures, where when I execute the function it still has access to the outer variables. By the way, these are also called free variables. A free variable is a variable that is outside a function, but that you have access to So it closes in, it wraps up these variables, and
at the point of execution, all three of these functions, because they're sitting in the same spot, are going to be pointing to the same memory space where these outer variables were located.


* And you might ask the question, well what if I did want this to work? What if I did want this to do what I just said,to output zero, then a one, then a two? Well there's a couple ways to approach that.

```js
function buildFunctions2() {
 
    var arr = [];
    
    for (var i = 0; i < 3; i++) {

        // let j = i ;  // !!!!!! Es6 but we will use Es5 (current version of JavaScript)

        // What happens here will be that the let variable that's created, is scoped to the block, so, inside these curly braces.

        // So every time the for loop runs, this will be a new variable in memory.beacuse it is let 

        // And it will be segmented inside of memory of this execution context so that when this function is called, it would be pointing each time at a different spot within that memory.

        // These are subsegmented essentially as separately scoped variables.So new JavaScript functionality lets us do it this way,

        // however how could we do it with the ES5 that is the current version of JavaScript functionality. Well, in order to preserve the value of i for this function I'm going to need a separate execution context for each of the functions that I'm pushing into the array.

        // I need a parent scope that holds the current value of I as the loop goes. So, the only way to get an execution context is to execute a function.

        // So, how can I execute a function on the fly? Do you remember? An immediately invoked function expression is a nice, clean way to do that. So I can make a function.

       arr.push(
            (function(j) {
                return function() {
                    console.log(j);   
                }
            }(i)) // Let's say I give it a variable j. I'm going to create it and then execute it, and give it i.
        )

        // So, now I have an immediately invoked function expression.

        // So what's going to happen? Well every time the loop runs, it's going to execute this function, passing 0, then it's going to execute a new one, passing 1, then it's gonna execute a new one, passing 2, and each of those executions creates its own execution context, and j will be stored in each of those three execution contexts.
    }
    
    return arr;
}

var fs2 = buildFunctions2();

fs2[0](); // here j =0
fs2[1](); // here j =1
fs2[2](); // here j =2

```
* we're doing a push and this push is going to push the result of executing this function and executing this function gives us back a function.

* I'm pushing the result of this function and when this function runs, it gives me this (return function) so that's what get's pushed into the array.

* Then when this (return function) gets executed, and it looks for j, it doesn't need to go all the way out up into this for loop. It'll just go out to this execution context. j will store the value at that moment it was executed in the loop.

* So I should see what I originally thought I was going to see, and there it is 0, 1, 2.

* So that's a way to use closures to our advantage to make sure that we have the values that we need when we execute this inner most function later.

### Framework Aside: Function Factories

* let's talk for a moment about how we can use closures to our advantage for making patterns that would be otherwise impossible.

* In this case, let's look at Function Factories.

* We've already seen how we can use some of the language features of JavaScript to allow a function to be called under different circumstances with different default parameters.

* But here, we're going to show how we can do even more powerful and flexible patterns using the fact that closures exist in JavaScript.

* A factory just means a function that returns or makes other things for me.

```js
// Function factories
function makeGreeting(language) {
    // 
    return function(firstname, lastname) {
     
        if (language === 'en') {
            console.log('Hello ' + firstname + ' ' + lastname);   
        }

        if (language === 'es') {
            console.log('Hola ' + firstname + ' ' + lastname);   
        }
        
    }
    
}

var greetEnglish = makeGreeting('en'); // one execution context here
var greetSpanish = makeGreeting('es'); // another execution context here

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doe');

```

* So, this function that's returned is the same logic we had before.

* But the difference is that instead of passing language to inner returned function, I'm passing it to the outer function. And then, returning that inner function.

* So, the language will be trapped, or collected in the closure.

* when I execute this function object, it will look up the scope chain. And even though makeGreeting() will have been done executing and it's execution context is gone, I'll still have access to language.

* the function that was returned will still point to The language variable en, because I executed makeGreeting(), and it was its own execution context.

* Then, in the second case, I execute my greeting again, so it gets its own execution context where language will be es.

* So, a different spot in memory. And the function that's returned from there, it will point at that execution context, spot in memory.

* So, even though these two functions lexically sit inside the same makeGreeting(), they're going to point at two different spots in memory because they were created during two different execution contexts.

* It's important to remember that even though it's the same function every time I execute it, it creates a new execution context. It's a new memory space, no matter how many times I call it.

* So, when I run this, I get Hello John Doe and Hola John Doe. So, my makeGreeting() function has acted as a factory function.

* And I'm taking advantage of closures to essentially set the parameter value that's then used inside the function that's returned.

* And yet at the same time, my greetEnglish, I can't really directly access that value anymore. This is hidden from my being able to use it down here in the global context. But the function itself when it runs has access to it.

* So, when it starts, I have my global execution context, which has the greetEnglish, greetSpanish variables, and the makeGreeting() function.

* When I hit this line, it calls or executes makeGreeting(). And so, that gets it's own execution context.

* And I pass language.

* So, when that execution context variable environment, language is en for English. Then, it returns a function which you store it in greetEnglish, it's pointing to that function, and makeGreeting() that time ends.

* And that memory space for that execution context is still hanging around.

* Then, on my second line, I call makeGreeting() again. And this is the important distinction.

* In the previous example we looked at when we filled an array with functions, we only called that outer function once, so all those functions inside that array pointed to the same memory space.

* But in this case, we're calling the function twice. When I call the second time, I get a new execution context.

* Every time you call a function, you get a new execution context. It doesn't matter how many times you call it.

* And so, that new execution context has its own variable environment and the language is Spanish in this case.And that returns a function object, and finishes.

* So, now, I have two spots in memory that are hanging out, with those two separate execution contacts, once contained.Then, when I get to the greetEnglish, I'm actually calling the function that was returned. That creates a new execution context with first name John and last name Doe. The outer environment reference, well, we know it needs to point to one of the execution contexts created by makeGreeting, cuz that's where it sits lexically. And the JavaScript engine knows that this first one was created during that first execution context, so it points to that one.The one it was created inside of. The execution context that returned it. And so, that's where my closure is.

* Again, the key is realizing that every time you call a function, it gets its own execution context, and any functions created inside of it will point to that execution context. This is doing exactly what it should. It's pointing to the memory space as if those other execution contexts hadn't disappeared. It knows which ones to point to properly where these inner functions were created and when. This is,again, a language feature. And pretty cool.

* You'll see that this is extremely useful. Because we can have a core set of logic, but then make ourselves functions that are then easier to use. I don't have to always pass the same parameters. I could just create some new functions that have some parameters by default, by using closures.

### Closures and Callbacks

* What if I told you that if you use something like setTimeout or jQuery events that you're using closures all the time. Let's talk for a second about closures and callbacks.

```js
function sayHiLater() {

    var greeting = 'Hi!';
    
    setTimeout(function() {
        
        console.log(greeting);
        
    }, 3000);
    
}

sayHiLater();
```

* Do you realize that you are using function expressions, closures right now in above code ?

* You see, setTimeout takes a function object. You're passing it as a parameter, so that's making use of first class functions in JavaScript.And we're creating the function on the fly,so I'm taking advantage of a function expression. 

* So, I'm passing a function object and another parameter the time to setTimeout. Now, sayHiLater then finishes.

* You may recall, early in this course we talked about asynchronous processes and the event loop. Well, setTimeout goes off outside in the browser, counts and waits and then drops an event when this 3000 seconds has passed that says my timeout has finished.

* And then the engine says, are there any other functions listening? And it finds one. And so then, it runs this function. But greeting doesn't exist inside this function and sayHiLater has already finished running.

* So, what happens? Exactly what we saw before. It goes up the scope chain and it has a closure for this variable. It knows the memory space where this was sitting when sayHiLater was running in its execution context.

* So, thanks to the closure, it still has access to greeting three seconds later,even though a long time ago, sayHiLater finished running.

* This only takes milliseconds to run and yet three seconds later, it still has access when this function is executed by the JavaScript engine.So, if you've done any code like this, and you have already used closures and function expressions.

```js
// jQuery uses function expressions and first-class functions!
$("button").click(function() {
   
});
```
* All along, if you've done some jQuery or some JavaScript programming, at some point, you've probably been taking advantage of first-class functions and closures.

* These functions ("code inside setTimeout") that do something after you run another function ("setTimeout"), giving this function to another function (setTimeout execution) and having it execute when it's done, is called a callback.

* You are saying here, take this function, and when you are done working, call the function, execute the function I just gave you. It's a callback.

* I executed you, and you in turn execute this other function for me when you're done.

* A callback function, that's a function you give to another function to be run when the other function is finished.

* So, you call a function, that is you invoke it, you execute it, and you give it your own function. Then, that function calls back by calling the function you gave it when it finishes.

* So, I call function A, and give it function B. When function A finishes running, it calls function B for me. It calls back. Thus the name, the callback function.

* Let's build a function that takes a callback real quick.

```js
function tellMeWhenDone(callback) {

    var a = 1000; // some work
    var b = 2000; // some work
    
    callback(); // the 'callback', it runs the function I give it!
    
}

tellMeWhenDone(function() {

    console.log('I am done!');
    
});

tellMeWhenDone(function() {

    console.log('All done...');
    
});
```
* I call you, then you call the function that I gave you.


### call(), apply(), and bind()

* In our execution context we have our Variable Environment, Outer Environment reference and we have "this" thing, this variable that's set up for us called 'this'.

* We've already seen that the this keyword can point to the global object in some cases, and in other cases it can point to the object that contains the function, if the function is a method attached to an object.

* Wouldn't it be nice to be able to control what the "this" variable ends up being?

* When the execution context is created. Well, Javascript has a way to do just that. And that's where call, apply and bind come in. We could have talked about this before, when we talked about objects, functions, and this variable.

* But to properly and fully understand call, apply and bind. Especially when looking at it syntactically. We need it to have a complete understanding of first class functions.

* So, we already know a function is a special kind of object, it has a hidden optional name property which can be anonymous then if you don't have a name.

* We have a code property that contains the code and that code property is invokable so we can run the code.

* All functions in JavaScript also get access to some special functions, some special methods, on their own.

* Remember, a function is just an object, so it can have properties and methods. So, all functions have access to a call method, an apply method, and a bind method.And all three of these have to do with the this variable and the arguments that you pass to the function as well.

```js
var person = {
    firstname: 'John',
    lastname: 'Doe',
    getFullName: function() {
        // And remember since this is a method on an object and this function is executed this key word will point to this whole object the person object. The object that contains the method.
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
        
    }
}

// All right so, what about call, apply and bind?


var logName = function(lang1, lang2) {
    // So, for starters, we know that this will fail, right here. Why?

    // Because the this keyword here at the this level is going to be pointing to the global object I'm not a method this function isn't a method of the person objects
    console.log('Logged: ' + this.getFullName());
    console.log('Arguments: ' + lang1 + ' ' + lang2);
    console.log('-----------');
}
// logName() -  // Let me go now run this. It says undefined is not a function. The this is the global object and there is no getFullName. So that's undefined and then I tried to invoke undefined. Then it gave me an error.

// wouldn't it be nice if I could control what the this keyword points to? So let's make a new function called logPersonName.

// And I'll use the logName function. And all functions in JavaScript get access to these three methods. The first we'll use is .bind.

// Notice I'm not invoking the function and saying .bind because, the function returns a value. 

// I'm looking to actually use the function as an object and call a method of that object.

// I can pass to it whatever object I want to be the this variable when the function runs. The bind function, the bind method returns a new function.

// So, it actually makes a copy of this logName function. And sets up this new function object, this new copy.

// So that whenever it's run. When its execution context is created. The JavaScript engine sees that this was created with this bind, which sets up some hidden things in the background, so that. When the JavaScript engine decides what is the this variable,

// So I'm affecting the JavaScript engine as far as what it decides when it creates the execution context for this new copy of this (logName) function.

var logPersonName = logName.bind(person);

// So If I call logPersonName instead of just logName, I call this new copy of this function created with bind. When it says this, this will be the person object.

logPersonName('en'); // So I can call the get dot get full name and it logs it. 


// ############## Optional Starts #######################

var logName = function(lang1, lang2) {

    console.log('Logged: ' + person.getFullName()); // So this become essentially, right here, preson.getName. Because I passed it in to the bind.
    console.log('Arguments: ' + lang1 + ' ' + lang2);
    console.log('-----------');
    
}.bind(person)

logName(); // instead if I call just logName() instead of error-ing, this now points to a copy of this function that was created on the fly bound with the this variable to whatever I want to this variable to be.

// ############## Optional ENDS #########################



// If I do logName.call(), this calls the function. And invokes it. I could've also called the function with parenthesis. logName()

// because .call also, lets me decide what the this variable will be. The first thing I pass to call is what the this keyword should point to.And I can also pass it parameters.

// Unlike bind, which creates a copy of the function, call actually executes it. And then just decides what the this variable should be and the rest is just the parameters that I would normally pass to the function.

// bind did not execute the function, it created a copy. but call actually executed it.And I just passed the parameters as well.

logName.call(person, 'en', 'es');

// LogName.apply(), all functions can get .apply. This does the exact same thing. Only one difference. ie the apply method wants an array of parameters, rather than just the normal list.

logName.apply(person, ['en', 'es']);

// And that's the only difference between call and apply.

//And again, I could do this on the fly,
(function(lang1, lang2) {

    console.log('Logged: ' + this.getFullName());
    console.log('Arguments: ' + lang1 + ' ' + lang2);
    console.log('-----------');
    
}).apply(person, ['es', 'en']);


// And I know what you're probably thinking. When would I ever actually use this in real life?

// Well, let's talk about two instances.


// the first thing I'm going to show is something called function borrowing. Let's say I have a second person object with similar property names,but different data, let's say Jane, lastname Doe. This is very similar to my first person object, only the second one doesn't have a getFullName method.

// So I'm going to do something called function borrowing by using call or apply. I'll take that first person object I created which does have the getFullNamemethod and since it's a function, it has access to .call and .apply.


// function borrowing
var person2 = {
    firstname: 'Jane',
    lastname: 'Doe'
}

// applying person2 object in person getFullName function , I'm setting the this keyword in person function to my person2 object.
console.log(person.getFullName.apply(person2));

// So what I've done is I've borrowed a function. we could have do the same thing with call.


// Next is function currying

// This in particular has to do with the bind, the .bind. Because that's creating a copy of the function, and it actually does something rather interesting if you pass parameters to the bind.

// With call and apply, passing parameters just passes the parameters. But with bind, you're creating a new copy of the function. So what happens if you pass parameters to it?

// function currying
function multiply(a, b) {
    return a*b;   
}

// I'll pass it two things, two numbers, and it will multiply them.

// Then I'm gonna create a new function. A copy of the multiply, using .bind.


var multipleByTwo = multiply.bind(this, 2);
// Now remember, bind is not calling, it's not executing the function. So what does this do? Since I'm not executing it, what does giving it parameters do?

// Giving it parameters sets the permanent values of these parameters when the copy is made.So by setting two, what I'm saying is the first parameter, cuz the first thing I put is the value of this keyword, and then my parameters.

// The first parameter will always be a value of 2 in this copy of the function.The variable A will always be a value of 2.

// So, the bind is permanently setting this right there and I can call my new function, my new copy and if I pass to it,let's say a three or a four. This will end up as the second parameter, the B.

console.log(multipleByTwo(4)); // Result : 8 

var multipleByTwoandThree = multiply.bind(this, 2, 3); // If I had passed both parameters to bindI'm essentially telling it this is the value of a and b always
console.log(multipleByTwoandThreeThenByFour(4)); // Result : 6  ie 2*3, and this 4 just becomes an extra parameter passed to the function.

 var multipleByTwoandfive = multiply.bind(this); // IIf I pass no parameters then I have A and B open so I could do 5 and 2 in bind function and that would be 10
console.log(multipleByTwoandfive(5,2));

var multipleByThree = multiply.bind(this, 3);
console.log(multipleByThree(4));
```
* Function currying: creating a copy of a function but with some preset parameters.Their set turns out to be really useful in mathematical situations, so if you're building a library that has to do a lot of mathematical calculations, you can have some fundamental functions that you can then build on with some other default parameters.A really good useful usage for bind.

### Functional Programming - Part 1

* Although JavaScript sounds like it's related to the Java programming language and looks like the C++ C# Java languages a bit.It really has more in common in many ways with other languages called functional programming languages. Languages like Lisp or Scheme or ML. These are languages that have first class functions like we've talked about.

* Functions that behave as objects. You can pass them as parameters. You can return them from functions. And so, having first class functions in a JavaScript programming language means that we can implement what's called functional programming.Where we think and code in terms of functions.

* it introduces an entirely different way of thinking and implementing when programming. And introduces an approach that you simply can't do in other programming languages that don't have first class functions.

* Lets imagine i have array , let's suppose my task here is to create a new array out of this first array. So I need to loop over all of the items in the array using for loop.

* I had to write of code to do that. And we know that as programmers, we want to always minimize how much work we're doing, how much typing we're doing and how often we're repeating ourselves.

* We tend to put things into functions in order to do that, to limit repetition, we put work into functions. In programming languages where functions aren't first class, there is limitations to how much you can put into a function, to how granularly you can segment your code.

* However, with first class functions we an do something entirely different. We can head toward functional programming.

```js
// So let's say, for example, I create a new function.
function mapForEach(arr, fn) {  // It takes an array and it takes a function. 

// Remember I'm using first class functions so I can pass a function object.
    
    var newArr = []; // new empty array
    for (var i=0; i < arr.length; i++) { // looping the given array
        newArr.push(
            fn(arr[i])   // here is where my functional programming comes in. I'm going to call a function and pass in that array item.
            // why we used function here, Your array item, which is processed by some function that you also give me.
        )
    };
    
    return newArr; // And then, this is going to return the new array.
}
```
* I've just abstracted the concept of iterating or looping over an array, creating a new array out of it, and writing some function, whatever function you want to, against each item. Take some action against each item, pass this array item to the function.

* And this function should then return something that it does with that array item. And I'll add that to my new array.Then i can simply call like this 

```js

var arr1 = [1,2,3];
console.log(arr1);


var arr2 = mapForEach(arr1, function(item) {
   return item * 2; // here we are multiply item with 2 and the returing, which will added to new array newArr and then it will get returned to this variable arr2.
});
console.log(arr2);
```
* I called a function. Gave it an array and told it what to do in each item in the array.

* I can use the same mapForEach function, with that I could do something entirely different.

```js
var arr3 = mapForEach(arr1, function(item) {
    return item > 2; 
});
console.log(arr3);
```
* I was able then to reuse this mapforeach to do entirely different work against the array simply by passing the function, passing the work that I wanted to get done against it.

* This is a classic example of functional programming.Where I'm using first class functions to my advantage to segment my code in even cleaner and tighter ways.

* Let's look at another example.

```js
var checkPastLimit = function(limiter, item) {
    return item > limiter;   // check and see if this item is past a certain limit. limiter is just some value, lets say 1
}

// All right, so how can I use this? I have a bit of a problem. This function accepts two parameters, and the mapforeach wants a function that accepts just one.

// So how can I take a function and call it, or use it in a way that I've preset this parameter.

var arr4 = mapForEach(arr1, checkPastLimit.bind(this, 1)); // here we presets limiter with 1 using bind function, as we know bind will asign first value to first param

// something like this item > 1; item data is our arr1 item

// So I've just created a copy of checkPastLimit function on the fly, with 1 as the limiter.
console.log(arr4);
```

* And that's all in one line of code. But you might say well, it's kind of annoying to call bind all the time. It would be nice just to pass in my limiter as the only parameter.

```js
var checkPastLimitSimplified = function(limiter) {
    return function(limiter, item) {
        return item > limiter;   
    }.bind(this, limiter); 
};// here we are just receiving limiter Then copy that object that was created with bind with this limiter parameter preset to whatever the value is I passed into that outer function.

var arr5 = mapForEach(arr1, checkPastLimitSimplified(1)); 
console.log(arr5);
```
* One other note about functional programming, your functions, especially the tiny ones, as you're moving and passing little functions around that do work, should do their best not mutate data. Mutate remember, means change.

* You can really find yourself in odd circumstances when you're actually changing data in these little tiny functions, that you start to pass around as you're breaking things down in functional programming.

* So it's always better to mutate data, that is, change it as high up in that chain as possible of functions, or to not change it at all or Instead, return something new, like we're doing here with new arrays as supposed to changing that core array that I passed in.

* That's just a side note about functional programming. Functional programming in itself could be an entire course. But I wanted to introduce you to this idea in JavaScript because I think this first class functions and functional programming in JavaScript Is really what takes JavaScript to the next level, and will take you as a JavaScript programmer to the next level.

* Instead of simply using JavaScript the way that you use maybe other programming languages, like PHP or Java or C# or whatever, you can use the full power of the language that's available to you. It's not available in those other languages.

### Functional Programming - Part 2

* Underscore.js library is a very famous library in JavaScript that helps you work with arrays and collections of objects.

* The really neat thing about Underscore, besides how incredibly useful it is, is that it also made an effort to show how it implemented what it did.
```js
// underscore
var arr6 = _.map(arr1, function(item) { return item * 3 });
console.log(arr6);

var arr7 = _.filter([2,3,4,5,6,7], function(item) { return item % 2 === 0; });
console.log(arr7);
```

* And there's a whole bunch of other methods that Underscore and lodash offer to us. And they all involve understanding these functional programming concepts. So mess around with Underscore. Play with it awhile. Look at its source code. You will learn a lot.

## Object-Oriented Javascript and Prototypal Inheritance

### Conceptual Aside: Classical vs Prototypal Inheritance

* Let's talk about object-oriented JavaScript and prototypal inheritance.

* And in this course, when we say object-oriented JavaScript, we're going to focus primarily on the creation of objects, because that's where a great deal of confusion lies.

* Inheritance - one object gets access to the properties and methods of another object.

* The general idea of inheritance is implemented differently in different languages. But for our purposes we only need to understand the most basic concept.

* That one object gets access to the properties and methods of another object. So, I have two objects, and my first object inherits from my second object, it gets access to its properties and methods. You have an object, you have another object, and I can go and get access to the properties and methods on that other object.

* So, what do we mean when we're talking about classical versus prototypal Inheritance ?? 

* Well, classical inheritance, we're talking about what's currently best known and popular. It's there in C#, it's there in Java. It's a way of sharing methods and properties of objects. That's very classical.In other words, it's the way it's been done for a long time. The most popular way it's been done for a long time, anyway.

* And right now, I'm not going to say that classical inheritance is bad. Obviously, it works, otherwise it wouldn't be so popular. However thinking that this is the only approach. And thinking that it doesn't have its downsides is wrong.

* Classical inheritance and I've built plenty of large object structures with lots of objects that inherit from each other that share properties and methods. It's very verbose. There's a lot going on and you can end up with these huge, massive collections and trees of objects that interact and it can be so hard to figure out how one is going to affect the other even if you use good practice once it gets large enough.

* That's how it can feel with classical inheritance sometimes.Then you have all these different keywords flying around, friend and protected and private and interface and who knows what else, and you have to learn what all those mean, and try to use them. Again, it's not bad.

* But I personally have never really liked it all that much. Now when we talk about prototypal inheritance, we're talking about something much simpler.It's very flexible, very extensible, and very easy to understand.

* And i'm not saying that it's perfect either, but you should understand that when we talk about inheritance in JavaScript,it's different than what we may be talking about with other programming languages. And we need to understand it. And it won't be hard to understand.
And once you do understand it, you'll see just how powerful it can be as an approach to sharing properties and methods amongst objects.

* Just remember when we say inheritance, we're just talking that one object gets access to the properties and methods of another object. And we're going to see how JavaScript does that. Using what's called prototypal inheritance.

### Understanding the Prototype

* We've said that JavaScript implements something called prototypal inheritance. That means that there exists this concept called the prototype. So what is that?

* Let's say we have an object in memory, we'll just call it obj. We've already seen that objects can have properties and methods. This obj has a property. Let's say it's property 1. And I can access it with obj.prop1 using the dot operator.

* The dot operator goes out and looks for a reference to something named prop1 and goes, finds that memory and gives it back to me.As long as it's referenced against obj, you've seen that already.

* We also know that the JavaScript engine adds hidden properties and methods to things or properties and methods that we normally don't interact with directly.

* All objects In JavaScript, and that includes functions. All objects have a prototype property.

* The property is simply a reference to another object, we'll call it proto. It's an object that stands on its own, we could use it by itself if we wanted to.

* But the object property that we'll call proto, that's it's prototype.That's the object that it's going to grab and be able to get its properties and methods as well 

* So if this other object, and it's just an object, has some other property, say, Prop 2, when I call obj.prop2, the dot operator goes and looks for Prop 2 referenced on obj itself. And it doesn't find it, so it next goes to the prototype. There, that object proto and looks for that property name there and if it finds it, it returns it.

* So it looks like prop2 is on our object, but it's actually on our objects prototype.

* Similarly that prototype object itself can also point to another object and so on and so forth.

* Each object can have its own prototype, and maybe this prototype (prototype of obj.prototype) of the prototype has another property, prop 3.

* And so I use the dot operator to call obj.prop3. It doesn't find it in obj, so it looks at the prototype, that proto object. It doesn't find Prop 3 there, so it looks at the prototype of that object, and it finds Prop 3 and returns it, gives it to us. (!!!Refer prototype image)

* It looks like it's on our main object but it's actually a ways down what's called the prototype chain.

* Don't confuse that with the scope chain, though it is kind of similar in some ways. The scope chain is about looking for where we have access to a variable,

* however this prototype chain has to do with where we have access to a property or method amongst a sequence of objects that are connected via this prototype property, that we're calling here proto.

* And it's hidden from me in the sense that I don't have to go obj.proto.proto.prop3. I can just say obj.prop3.

* And the JavaScript engine does the work of searching the prototype chain, for those properties and methods.

* The really interesting thing is if I have another object, let's say obj2 (Main object similarly like obj1), it can point to the same object as its prototype It's just another object, these are just references.

* So objects can share all the same prototype if they want to, or if you want them to.

* And so if I call obj2.prop2, interestingly that's going to return the exact same property. The same spot in memory as obj.prop2.

* So they're literally sharing a property but not directly. Instead, via this concept within the engine called the prototype chain. Prop2 isn't on obj and obj2.

* It's just that when the JavaScript engine goes down the chain to search, they happen to be pointing at the same place.This is the prototype chain, the concept of prototypes.

* And it's very simple, don't over-think it. It's just I have this special reference in my object that says where to look for other properties and methods, that's my prototype but I don't have to manually go dot prototype or something like that. I can just go dot property or dot method and the JavaScript engine will go look for it down the chain.

* Lets see some examples

```js
var person = {
    firstname: 'Default',
    lastname: 'Default',
    getFullName: function() {
        return this.firstname + ' ' + this.lastname;  
    }
}

var john = {
    firstname: 'John',
    lastname: 'Doe'
}


//#####!!!!!!!!!!!!!!! don't do this EVER! for demo purposes only!!!

// modern browsers do provide a way to directly access the prototype, but you don't want to use it. You don't want to set it, because it's a performance problem, it can slow down your applications dramatically.

// But we're going to set the prototype of the John object to the person object, and that's all we have to do. John now inherits from person.

john.__proto__ = person;

// In other words, the JavaScript engine, if I was to call a property or method, if I was to try to access a property and method on John, that doesn't exist on John.

// It will go to person to try to find it. And if it's not on person, whatever person's dot proto is, it will go and try to find it there and on down the chain.

// I can just console.log (john.getFullName). Name. I couldn't do that before, there was no getFullName.

console.log(john.getFullName()); // Result :John Doe

console.log(john.firstname); // Result :John

// When this function is invoked, the execution context when it creates the this variable knows what object originally we're talking about.

// So this does not refer to person, it refers to John, whatever object originated the call. Awfully useful, isn't it?

// Let's think about that. Why don't I get the default as value ? Well because of the prototype chain.I'm asking for the first name property of the john object.So it first looks at the john object for that property and if it finds it it returns it and stops.It doesn't search the prototype chain any further once it finds what I'm looking for.

var jane = {
    firstname: 'Jane'   
}

jane.__proto__ = person;
console.log(jane.getFullName()); // Result : Jane Default.

// When it invokes get full name, the this variable, the this keyword points to Jane. Because that object originated the call.

// And then it searches for first name on "Jane", finds it, and says okay, it's Jane. And then it searches for lastname on the Jane object, doesn't find it, and looks at the prototype, which is person, and finds lastname there. So we get "Jane' 'Default'.

person.getFormalFullName = function() {
    return this.lastname + ', ' + this.firstname;   
}

console.log(john.getFormalFullName());// Doe, John
console.log(jane.getFormalFullName());// Default, Jane
```

### Everything is an Object (or a primitive)

* Now that we understand the object prototype we can even more deeply understand what we mean by everything in JavaScript is an object or primitive 

* because everything in JavaScript that isn't a primitive, a number, string, boolean etc.

* Functions, arrays, basic objects. They all have a prototype. Except for one thing, the base object in JavaScript.

```js 
var a = {};

var b = function() {};

var c = [];

```
* But we're only going to use these to explain how everything has a prototype. So if I go and run this there's no errors. And I'll have access to a, b, and c. They're in memory here, running in the browser.

```js
// I have the base object in JavaScript. The base object is just called object, it's the very bottom of the prototype chain always. Everything eventually leads to the base object.

a.__proto__  // Result : Object {} // on its we will have access to other familiar objects like toString etc..

b.__proto__ // Result : function Empty() {} // on its we will have access to other familiar functions like call bind ... apply etc..

// This is why. All functions in JavaScript are automatically have their proto property, their prototype set to this built in object that has these methods on it, apply, bind, call, and some other things.

// So I have access to these methods because it's on the prototype of the function objects.

// So when I call b.__proto__.apply, it looks for it on my function, doesn't find it and goes to the prototype which happens to be set to an automatic built-in JavaScript object that does all of that work.

c.__proto__ // Result : [] Some kind of empty array. But if I do a dot, I see it has a whole bunch of useful functionality like indexOf, length, we've used that already. We've also used push.

// So all arrays in JavaScript, for example, I can do .push, because any array that I create, the JavaScript engine automatically sets its prototype to this built in JavaScript object that has the push method on it.

// So I can call my array .push, and the JavaScript engine searches the prototype chain, doesn't find push on my array, finds it on the prototype. That's why it's automatically available to you, because JavaScript engine just set the prototype for you.
```
* All arrays have a prototype pointing to an object with these methods.

* All functions have a prototype point to an object with those function methods.

* What's the prototype of the prototype? 

```js
 c.__proto__.__proto__  // object {}
 ```
 * Remember, that's (object {}) the bottom of the prototype chain, always? , Eventually you get down to the bottom. An object doesn't have a prototype, it's the base.

 * similarly for function also

 ```js
  b.__proto__.__proto__  // object {}
 ```
 * and for object

 ```js
 a.__proto__ // object {}

 a.__proto__.__proto__ // null 
 ```

 ### Reflection and Extend

 * Let's take a moment to talk about another aspect of creating objects that's quite interesting and quite useful.

 * So useful in fact that many libraries and frameworks include a very similar feature called extend. 

 * And extend is possible because of something called reflection.

 * Reflection: an object can look at itself, listing and changing its properties and methods.

 * So a JavaScript object has the ability to look at its own properties and methods. We can use that to implement a very useful pattern called extend.

* Now, we're going to show an example of refection in JavaScript.

 ```js
 var person = {
    firstname: 'Default',
    lastname: 'Default',
    getFullName: function() {
        return this.firstname + ' ' + this.lastname;  
    }
}

var john = {
    firstname: 'John',
    lastname: 'Doe'
}

// don't do this EVER! for demo purposes only!!!
john.__proto__ = person; 

for (var prop in john) { // similarly like for each this will loop over every member in the object.
    if (john.hasOwnProperty(prop)) { // hasOwnProperty kind of filter to check only on john object not on its prototype (person)
        console.log(prop + ': ' + john[prop]); // name : value
    }
}

var jane = {
    address: '111 Main St.',
    getFormalFullName: function() {
        return this.lastname + ', ' + this.firstname;   
    }
}

var jim = {
    getFirstName: function() {
        return firstname;   
    }
}

// Notice that I'm not putting these into the prototype chain. Maybe I don't want for some reason, maybe they have a lot of other properties and methods I don't want to be in that prototype chain for one reason or another. Or maybe I want to simply use them in a different way.

// Maybe they are useful individually, but not as prototypes of each other. 

// So, what I can do with the underscore library !!!, for example,

_.extend(john, jane, jim); // And what this does is composes or combines these objects.

// It takes all the properties and methods of these other objects that I give it and it could be a big long list and adds them directly to john for me.

console.log(john); // this will have combination of john, jane, jim object 
 ```
 * This physically, actually placed the properties onto the john object. We composed, we combined them. How does this work

 * In underscore.js library

 ```js
 // Extend a given object with all the properties in passed-in object(s).
    _.extend = createAssigner(_.allKeys);


// Let us finc this createAssigner in underscore library

var createAssigner = function( keysFunc, undefinedOnly) {
      return function(obj) {
        var length = arguments.length;
        if (length < 2 || obj == null) return obj;
        for (var index = 1; index < length; index++) {
          var source = arguments[index],
              keys = keysFunc(source),
              l = keys.length;
          for (var i = 0; i < l; i++) {
            var key = keys[i];
            if (!undefinedOnly || obj[key] === void 0) 
                obj[key] = source[key];
          }
        }
        return obj;
      };
    };
 ```

 * createAssigner - Well, this is actually a function that makes sense and it takes some keys and then it returns a function itself.

 * All right, so this is creating a closure. Remember that? So this is the function that's actually getting run when we call the extend.

 * And it has a closure access to these other ones ("undefinedOnly")

 * Remember the arguments is all the properties passed in. And it looks at the length, and it says, oh, if length is less than 2, then just give me back the object that I passed in to add on to. Well, that makes sense.

 * If I didn't pass anything else, if length was 1, then just give me back John cuz I didn't give you anything to add on to john. That's what that's saying right there.If the number of parameters passed to this function is less than two, then just give me back this first one.

* Length is the number of parameters I passed in. So that makes sense too, but it's starting with one. So it's skipping that first one. Because remember, arrays are zero based. So it skips index zero. So it skips john and it moves onto the rest of the parameters.

* All the arguments we are passing to extend will be available in arguments keyword

* All right, so it loops through all of the other parameters that I pass, in this case jane and jim since its staring from 1 john will be excluded.

* keysfunc, we won't bother looking at that, but the keys are those property names, the names part of the name value pair.

* So it's just basically two loops, one inside the other. One loop across all the list of objects I added as sources, and one across each of those properties and methods.

* So, it loops across these two and inside each of these, it loops across its properties and methods and adds them to John.

* Using reflection, using the fact that I can set John's properties and methods with the brackets operator and then string name.

* So do you see how reflection is useful, and especially extend. This shows up in a lot of frameworks and libraries, because it is so useful. You can compose objects. You don't even need the prototype chain always.I can use this pattern and it's quite powerful.

*  !!! One caveat here. In the next version of JavaScript, there also will be something called, extends, with an s. That's going to be used to set the prototype. I think that's a little unfortunate because I liked the concept of extend, but that'll still be around. It's just a very similar keyword will mean the prototype. When we talk about this, we will be talking about this composition, we'll see what happens over time.

## Building Objects

### Function Constructors, 'new', and the History of Javascript

* Now that we understand objects and prototypes, prototypal inheritance and the prototype chain, object properties and methods, it's time to talk more deeply about building objects.

* We've already seen one way to build objects. Which is object-literal syntax. { }

* However, there are a few other ways to build objects, especially when it comes to setting the prototype. So, we're going to look at them now.

* First, our first way of building objects. And we need to understand why they work the way they do. This is function constructors, 'new', and the history of JavaScript.

* Now, we're not going to get too deep into a history lesson, but here's what you need to know.

* JavaScript was built by a guy named Brendon Eich. Back at the time, when much like today, there are technology wars between Google and Apple, there was technology and programming language wars between various companies like Netscape, Microsoft, Oracle and Sun.

* And so, JavaScript was a language that was written for usage in the browser eventually. Not absolutely originally. But eventually that became its purpose, primarily.

* And it was called JavaScript, to attract Java developers to it. 

* Because there were a lot of Java developers, and if you don't have developers using a language, it dies. So, the company that Brandon Eich worked for at the time decided to call it JavaScript to attract Java developers.

* Much like at the time Microsoft was touting a language in the browser called VBScript, because Visual Basic was very popular and it wanted to attract its own Visual Basic developers to VBScript. So JavaScript sounds like Java and looks a little like Java, but is nothing like Java. This is marketing.

* And one of the elements of marketing JavaScript was this kind of line of code.

```js
var john = new Person();
```

* Java developers were used to creating objects like this, using the new keyword and something called a class.

* A class in Java is not an object but it defines an object and you use the new keyword to then create the object from that class.

* JavaScript doesn't really have classes although there is a class keyword coming In the next version of JavaScript, but even then that isn't really a class the way it is in Java or C# or C++.

* So, Java developers were used to creating objects like this. And so JavaScript needed a way to be able to create objects with this kind of syntax, so that when Java developers looked at JavaScript, they would say, oh see, JavaScript is like Java. You should use it, even though it's not really.

* So what we have and what we still have in JavaScript is one approach to building objects that isn't that bad when you get down to it, once you get used to it.

* But it does have some problems.

* However, even though there are new ways coming to build objects in JavaScript, this is one of those things you need to know because its ubiquitous (ie  found everywhere).

* It's everywhere and it will be in a lot of the open source that you might read or other projects that you might get involved in if you're writing JavaScript or reading someone else's. So, let's talk about function constructors and the keyword new.

* Well, remember that everything we're going to talk about in this section is just about different ways to construct objects.

* We're not changing what objects are or how they work. We're simply utilizing features in the JavaScript language that lets us Construct or build objects.

* To build objects we need to create an object, give it properties and methods, and set its prototype.We set the prototype the wrong way in the previous lesson just to show how it worked.

* But now we're going into the right ways or the various ways that are accepted to create objects in JavaScript, adding properties, adding methods, and setting the prototype.

```js
function Person(firstname, lastname) {
 
    console.log(this); // Result : {}
    this.firstname = firstname;
    this.lastname = lastname;
    console.log('This function is invoked.');
    
}

var john = new Person('John', 'Doe'); // Notice I'm not just calling the function. I'm not just invoking it. I'm putting the new keyword in front of it.

console.log(john); // Result: Person {firstname : 'John', lastname : 'Doe'}

```
* So, the new keyword, which we said was basically introduced to make Java and similar language developers feel comfortable, is actually an operator.

* Remember, our JavaScript operator precedence list? You can see that new is actually an operator.

* So, when we say new something special happens. Immediately, an empty object is created. It would be as if we said, var and then some variable and then an empty object. An empty object is created.

* And then it invokes the function. It calls the function.

* When the function is called we know that the execution context generates for us a variable called "this" keyword.

* In this case, where you use the keyword new, it changes what the this variable points to.

* The this variable points to that new empty object.

* Imagine, basically that this operator creates an empty object and then calls this function.

* So now the this variable is pointing to that empty object in memory that this new operator created.

* Now when I add .firstname and .lastname, I'm adding it to that empty object.

* And as long as this function, the function that I used the new operator on, doesn't return a value, the JavaScript engine will return that object that was created by the new operator.

* So it created a new object, then called the function with the this variable pointing to that empty object. And then whatever I do to that empty object using the this variable will end up as part of that object and that's what's returned.

* In case if i return object inside the function that will get retured in console.log(john), means those object creation and return all those will be taken care by new keyword.

* But if I don't return anything, the JavaScript engine says, okay, you invoked this function using the new operator, so I'm going to give you back the object that was set as the this variable before the function started executing, when the execution context was created.

* So this is letting me construct an object via a function. So we call a function that's used specifically to construct an object a function constructor.

* Now I know You might be thinking but what if I want to just create more people with the same properties and methods? What happens?

```js
var jane = new Person('Jane', 'Doe');
console.log(jane);
// {firstname: "Jane", lastname: "Doe"} , It's invoked again, so a different empty object is created.
// The function is run, and that empty object is work on inside the function and returned
```

* Function constructors are just functions. We haven't really changed how functions work by calling the new operator.

* The important things are that putting the new operator in front of a function sets the this keyword to a brand new empty object, and if you don't return anything from that function, instead of returning undefined it will return that empty object.

* So, I'm saying, whatever I passed to the function, set that value as the value of this new property on my empty object that was created by the new keyword.

* Each invocation of the function using the new operator invokes the function but first creates a new empty object.

* These are going to be different empty objects. Then work is done using the this keyword, if you like. Then as long as you don't have a return statement, the JavaScript engine will return that empty object which you have now likely manipulated.

* So in most cases with function constructors, you're passing some default values or set values to set as part of the object.

* So now I'm constructing objects using functions. And again that's a function constructor.

* So, big word alert, function constructors. Not that complex after all. A normal function that is used to construct objects. When you put that new keyword in front of a function call, the 'this' variable, which is created during the creation phase of the execution context, it points to a brand new empty object. And that object is returned from the function automatically when the function finishes execution.

* All right, so that gets us the new properties and the new methods, but what about setting the prototype? All right, when it comes to setting the prototype with function constructors, it's actually quite easy, but looks a little confusing. That's next.

### Function Constructors and '.prototype'

* So we've seen that function constructors can be used to set up properties and methods on new objects, but how do we set the prototype,
the other important part of creating objects in JavaScript?

* And the question now is, how do we set the prototype? Well there's good news. When you use a function constructor, it already set the prototype for you.

* if you type john.__proto__ , we will get empty person object.

* well any time you create a function object, remember how we said that it has certain special properties? Like the name and the code?

* Well, there's another property that all functions get that is actually completely invisible to you and that you can and should use when you're using a function as a function constructor.

* So remember, our function is an object, and JavaScript functions are objects. Some properties, we don't really think about that much, like name. A function can be anonymous. We've learned that a function has a special code property that holds the code that gets executed when you invoke it. It's invokable.

* And all functions, every function in JavaScript you've ever written in your life, has a prototype property that starts off its life as an empty object, and unless you're using the function as a function constructor, it just hangs out. It's never used.

* But as soon as you use the new operator to invoke your function, then it means something. It sits there. It lives only for when you're using a function as a function constructor.

* For when you're using a function specifically to build objects in this special way, the prototype property is used.

* And it's a confusing name, because if I see something dot prototype, I think, oh, I'm setting or accessing the prototype of this object,

* Remember we already saw that in some cases you can use underscore underscore proto underscore underscore (__proto__) to get access to the prototype.

* The prototype property on a function is not the prototype of the function.It's the prototype of any objects created if you're using the function as a function constructor.

```js
function Person(firstname, lastname) {

    console.log(this);
    this.firstname = firstname;
    this.lastname = lastname;
    console.log('This function is invoked.');
    
}

Person.prototype.getFullName = function() { // setting property
    return this.firstname + ' ' + this.lastname;   
}

var john = new Person('John', 'Doe');
console.log(john);

var jane = new Person('Jane', 'Doe');
console.log(jane);

Person.prototype.getFormalFullName = function() {
    return this.lastname + ', ' + this.firstname;   
}

console.log(john.getFormalFullName()); // And because I've added it to the prototype object already,when this is called it searches down the prototype chain.

//It searches John. It doesn't find getformalFullName, so it searches its proto, its own prototype, which is pointing at this right here.Person.prototype, and I've already added getFormalFullName.

// That means that any object that I create with this function, sometime later I can add features to all of those objects at once by using this .prototype property of the function constructor. It's pretty neat right?

// If I had created a 1000 different objects using new person, I could give them all, in one stroke, access to a new method later, even after they were created.

console.log(jane.getFormalFullName());
```

*  every function you create in JavaScript, gets this special property, .prototype.

* It starts its life as an empty object, and it's always there. And you can add on to it.

* Remember we said the prototype chain is that every object has this special property that points to another object that is its prototype so it looks for properties and methods down that chain.

* When you call the new keyword it creates an empty object, and it sets the prototype of that empty object to the prototype property of the function that you then call.

* So, any objects you create using this function as a function constructor, specifically using the new keyword and not returning a value letting JavaScript automatically return that value, means that the object therefore created not only has whatever properties and methods you attached to it inside the function, but it has a prototype, which is .prototype property of that function.

* And also So I could add a getFormalFullName to the prototype later also

* So often you'll see, in really good JavaScript code, that properties are set up inside the function constructor because they're often different values. We need that. But methods are sitting on the prototype.

* So why wouldn't you add getFullName inside here? Inside the person object? You could, inside that function constructor, inside that object that this is pointing to, but here's the problem.

* And remember that functions in JavaScript are objects. They take up memory space. Anything you add to them takes up memory space.

* So, if I added getFullName, for example, to every object, then that means every object gets its own copy of getFullName and takes up more memory space.

* If I have 1,000 of these person objects, I'll have 1,000 getFullName methods. But if I add it to the prototype, I'll only have one. Even though I have 1,000 objects, I only have this method once.

* So, from a efficiency standpoint, it's better to put your methods on the prototype because they only need one copy to be used.

* I need my properties for each object because it's going to have different values per object, but for methods, I only need one. And when the object calls that method, the JavaScript engine will go down the prototype chain to find it.I don't need it to be copied over and over again.I'm saving memory space.

* All right, because there's only one prototype for all of these objects, so it's only in one spot.

* So that's function constructors in the prototype. I can create objects, and the prototype is already set for me. Then I can just add properties and methods to the prototype in order to give all of my objects that I create with this function access to those properties and methods.

### Dangerous Aside: 'new' and functions

* It's time for another Dangerous Aside. This is New and Functions. So we've already said that when we're using function constructors that they're really just functions. They're regular functions.

* I'm adding the new operator in front of them and that changes some things. Creates a new empty object, when the execution context runs it points to "this" variable at that new empty object.

* And if you don't return anything explicitly it returns that new object for you.

* But this is still just a function and that's where the dangerous part comes in because if I forgot to put on the new keyword.

* What's going to happen? Well it will still execute the function.

* The JavaScript engine doesn't know whether or not you intend to execute the function or to execute the function using the new keyword. It's just a function.

* So it'll let you just try to execute it. And since it doesn't return anything explicitly, it returns undefined,

* which means your objects that you're trying to create will actually be just be set equal to undefined.

* And when I try to use any properties or methods on that object since it's undefined I'll get an error.

* This is one of the reason why people say bad things about function constructor.

* But, function constructors are only there in the first place to try to appease syntactically programmers coming from other languages.

* But once you get used to it, it's not that bad. And, you can make certain that you are doing this properly by just following a simple convention.

* Any function that we intend to be a function constructor, we use a capital letter for its name.The first letter is a capital letter.

* That way if I'm looking around and I see that I have a bunch of errors and I notice a function call with a capital letter without the new. Then I know I probably forgot the new keyword. That's just a convention.

* There are some programs then that could try to help you with that. They're called Linters. And they might see a function with a first letter that's capital, and that you don't use a new keyword. And you can run your JavaScript through those programs, so it warns you of those kinds of problems.

* None the less, it's not a great solution because the JavaScript engine itself doesn't Care whether you do or you don't have the new operator on there.So you can make unintentional errors that the JavaScript engine won't catch for you when it goes to run your program. You'll just just end up having errors in the program itself as it's executing.

* So this is an aside about the danger of this. I recommend if you're dealing with function constructors, that you always have a capital letter as the name of the constructor. That it starts with a capital letter. So that you can quickly pick up and remind yourself that you need to use the new operator before invoking this function to get your object back properly.

* Now that said, there are new ways coming in JavaScript to create objects. So function constructors are likely going away. They're not going to go away completely though, because we still need to support years of JavaScript programs that have been built.

### Conceptual Aside: Built-In Function Constructors

* Now that we understand what function constructors are, we can talk for a moment in this conceptual aside about built-in function constructors in JavaScript.

* Function constructors that are just there, already inside the JavaScript architecture and ready to use inside the Javascript engine, and how you might use them. 

* And let's show some of the built-in function constructors that exist. Meaning that there are just some functions and function prototypes that exist that many of which you might have used already, perhaps even without realizing it.

* For example I could say that some value is a new number. And then give it a number to initiate with. Or even give it a string to convert it to a number.

```js
var a = new Number("2");
// What is this? Well, it's using the new operator, so this is a function, a function constructor.
a
```
* output what A is, notice that A is not a number, it's not a primitive, it's an object.

* Because function constructors create what? Objects. Then added a primitive value inside of it, two, because it's an object there's a prototype.

* So there's a number dot prototype which is what all number objects will have access to. ("Number.prototype.") this will show all methods.. we could use with this function constructor.

* Same for string 

```jsx
var a = new String ("test");
```
* And A dot has access to a whole bunch of methods of things that I could do with a string.

* Because those methods don't actually live on A, butn live on string.prototype, which is the function constructor's prototype property where this variable's prototype is pointing, right?

* So, that's actually where these interesting things like indexof(), And I give it a character, and it tells me whether or not it's there. Or I give it a value and it tells me whether or not it's there. if it returns 1 its true or if it is -1 condition failed.

* So the string isn't itself though. If I hit enter here, notice that string is not a string, it's not a primitive. It's an object.

* It has all of these properties and methods on it and then inside of it, what's called boxed inside of it, is the primitive value itself.

* !!! So these built in function constructors look like you're creating primitives, but you're not. You're actually creating objects that contain primitives and give them extra abilities.

* In some cases, JavaScript understands that you intend to look at an object and not a primitive. So I could do "John".length. What happened is that the primitive is just a primitive, it doesn't have properties or methods.

* But the JavaScript engine boxed it inside of a string object which has all these properties and methods, and then gave me access to it kinda automatically.

* Basically the same as going new String("John").length on that.

* So there are cases where the JavaScript engine wraps up the primitive in its object for you, just so you can use properties and methods you might want to.

```js
var a = new Date("3/01/2015");

```
* Well A is an object because when I hit dot, I have a whole bunch of features that the JavaScript engine provided on where are all of these properties and methods actually living?

* They're not actually living on A. Where are they actually living? So they're living on date dot prototype. And that's where actually all those methods and properties live on. !!!!! 

* So when I'm using these function constructors, you have to remember that you're actually creating objects.

* And it turns out that this can be somewhat useful, especially if you're building extra features in libraries or frameworks to tack on to these primitive values.Or really any of the pre-existing values like arrays, or objects, or functions inside JavaScript.

```js
// I added a method to the prototype, so all strings instantly have access to this method. All strings everywhere. That's the power of prototypal inheritance.

String.prototype.isLengthGreaterThan = function(limit) {
    return this.length > limit;  
}

console.log("John".isLengthGreaterThan(3));
console.log(new String("John").isLengthGreaterThan(500));

// similarly for number prototype we added new method isPositive

Number.prototype.isPositive = function() {
    return this > 0;   
}

3.isPositive() // Uncaught SyntaxError: Invalid or unexpected token

// why ? error because JavaScript, while it's nice enough to convert string for me automatically won't convert a number to an object automatically. however we could do like the below one

console.log(new Number(3).isPositive()); // true
```
* So this whole thing of built in function constructors. It's neat and you can add some abilities. Especially to things like arrays or strings pretty easily.

* But it gets a little dicey as you try to deal with primitives versus things that were created with the function constructors that look like primitives.

* new Number(3).isPositive() looks like it's a number. It's not a number.It's an object that wraps or boxes up a number and adds a bunch of features. So it can get confusing.

* We're going to talk in the next lesson about why this is dangerous for a moment. And why in general you shouldn't use function constructors, the built in function constructors, for these primitive types.Unless you have to.

* Even though it is useful to add features in some cases.

### Dangerous Aside: Built-In Function Constructors

* We've already talked about the built-in function constructors and how they can be kind of neat, but they're also dangerous.

* Here's a simple example to show why built-in function constructors for primitive types, especially like boolean, number, string, are dangerous.lets say

```js
var a = 3

var b = new Number(3); // this will create object with primitive number inside not number directly

a == b // that's true. Why? Because this coerces types. The ==, that operator, looks at a and sees a primitive, looks at b and sees an object, and tries to convert them to the same type.And once it does, they're equal.

a === b // If I use the recommended triple equals, a and b are not equal. Why? Because a is a primitive and b is an Object, created with this function constructor. And the === function, that operator, if it sees that the two things are not equal it immediately returns false.
```
* Doesn't even try, doesn't even bother. It just says, nope, they're not the same type, there's no way they can be equal. Because in reality, these two things aren't equal. So do you see the danger there?

* By using built-in function constructors for creating primitives, you aren't really creating primitives.

* And strange things can happen during comparison with operators and coercion. It's better, in general, do not use the built-in function constructors.

* Use literals. Use the actual primitive values.

* And in cases where you absolutely have to use them, understand what you're doing to yourself.

* One other thing I want to note, what if you're dealing with dates that we already saw? Or we use the date built-in function constructor. There's an absolutely terrific library called Moment.js. You can go to momentjs.com.

* And again, we just want to be certain we have this clear. It's dangerous to use function constructors, the built-in ones, for primitives.

```js
var c = Number("3") // this time i used as regular old function we didn't used new keyword, and also we should send number not string but here we are sending string lets see..

c // it will return 3 as number here not as string "3", here c is the converted value from string to number.
```
* So that can be useful under certain circumstances. But understand again the difference there between using the new keyword and not. You're calling the function.

###  Dangerous Aside: Arrays and for..in

* Now that we understand function constructors, built-in function constructors, and how they can be used in libraries and frameworks to add features. We just wanna mention about arrays and for in.

* Do you remember what we did in the lesson on reflection and extend. Where we looped through all of the properties and methods on an object. Well, arrays are objects, and we can do the same thing.

```js
var arr = ['John', 'Jane', 'Jim'];

for(var prop in arr){
    console.log(prop+ ' : '+ arr[prop]);
}

// Result:
0 : John
1 : Jane
2 : Jim

```
* Arrays in Javascript are a bit different than they are in other languages.

* When I reference zero, one, two, and pointing at a particular item in the array list. That zero, one, and two is actually the name. The property name on the name value pair, which is why I can use brackets to grab it.

* But the fact that an array is an object. And that each of it's items is a named value pair. That each item becomes a new property, I've added three properties essentially, to this array.

* Means, that there's a bit of a problem if somewhere else.I have some nice framework that adds some features to my arrays. lets say

```js
Array.prototype.myCustomFeature = 'Cool !';
```
* Who knows what this could be? This could be some to do with arrays. And it adds it to the array.prototype. Because this object literal is calling essentially new array. It's just a shorthand for doing that.

* but if you use this with above code like below will face some issue..

```js
Array.prototype.myCustomFeature = 'Cool !'; // it adds it to the array.prototype.

var arr = ['John', 'Jane', 'Jim']; // Because this object literal is calling essentially new array. It's just a shorthand for doing that.

// so arr prototype points to Array.prototype  as the prototype of this object.

for(var prop in arr){
    console.log(prop+ ' : '+ arr[prop]);
}

// Result:
0 : John
1 : Jane
2 : Jim
myCustomFeature : Cool ! // If I refresh, I get dot extra property when I use for.. in

```
* So, in the case of arrays don't use for in, use instead the standard.  That's safe.

* However, iterating overall properties is not safe because arrays are objects and you could iterate down into the prototype.

### Object.create and Pure Prototypal Inheritance

* What we've seen up to now is one way to create objects in JavaScript using function constructors.

* But we've already seen that function constructors were designed to mimic other languages that don't implement prototypical inheritance, and so they're a little awkward.

* Other languages implement something called classes, where a class defines what an object should look like and then you use the new keyword to create the object.And that's what function constructors are trying to mimic.

* On the other hand, many consider it better to simply focus on the fact that JavaScript does use prototypal inheritance, and not what's called classical inheritance, and accept it, embrace it.

* And so yet another way to create objects that doesn't try to mimic other programming languages, and it's something that newer browsers all have built in. It's called Object.create.

* So let's look at it and think about pure prototypal inheritance.

```js


var person = {
    firstname: 'Default',
    lastname: 'Default',
    greet: function() {
        return 'Hi ' + this.firstname;   // if i didn't use "this" keyword here if i just used firstname alone, it would look for it in this function context, when executed.
        // if this is Not find it, and go out to the global execution context.
        // And it wouldn't find first name there, because it's inside the person object.
        // And objects don't create new execution context, just to be clear.
        // So if you accidentally forget the this variable in a method on an object, then it's probably because you forgot that this variable that you're getting errors.
    }
}

// So I want to build an object, a new one, and set its prototype.

// this is the base object concept in JavaScript,

// Now what does this create for me when I do this?

var john = Object.create(person); 
// And the pattern is that you override whatever you want to by simply adding the properties and methods yourself to the created object.
john.firstname = 'John';
john.lastname = 'Doe';
console.log(john); 

// But notice my object is completely empty. There's nothing there. And its prototype is the person object, the first name, the greet method and last name. See that?
```
* So Object.create creates an empty object with its prototype pointing at whatever you passed into Object.create.

```js
john.greet() // Result before override : Hi Default 

// after override it will be :  Hi John
```
* That's pure prototypal inheritance. There is no other concept that tries to define how an object is structured.

* You simply make objects and then create new objects from them, pointing to other objects as their prototype.

* If you want to define a new object, you create a new object that becomes the basis for all others. And then you simply override, hide properties and methods on those created objects by setting the values of those properties and methods on the new objects themselves.

* This is pure prototypal inheritance and it drives some people crazy cuz it's so simple.

* But when you start to use it in real world scenarios, you'll see it's just lovely. You don't have a lot of confusion, a lot of verbosity. It's very straightforward, and it's very powerful because you can mutate, you can change the prototype along the way.


* All right, now we said that this (Object.create) was... A newer thing that modern browsers, that's JavaScript engines and more newer browsers are implementing.

* But what if you are involved in a project that requires you to support users that are on older browsers, or maybe an older environment where the JavaScript engine doesn't support Object.create, it doesn't have it.Well you can use what's called polyfill.

* A polyfill is code that adds a feature which the engine may lack. So whatever code you're dealing with, because there's usually different versions of engines.

* For example, there's older and newer browsers, Internet browsers, so they have older and newer JavaScript engines.

* We can have some code that checks to see if the engine has a feature, and if it doesn't we write some code that does the same thing that that new feature would do in the newer browsers.

* So I'm filling in the gaps where an older engine might not have some features that the newer features do. So let's look at Object.create.

* What if I'd like to use Object.create in my project but I'm forced to support some older browsers that don't have Object.create built in. Their JavaScript engines don't have this built in.

* Am I done? Do I have to use function constructors? No.....

* In a lot of cases for a lot of features there are things called these polyfills. I'm gonna drop on in here and you can see how it uses some of the things we've learned to fill in the gaps for any engines that don't have the feature.

```js
// polyfill
if (!Object.create) { // checking Object.create exist or not , we need to implement this only if this doesn't exist. for that I use the not operator, that's what's called a unary operator.

// here we are adding Object.create to the global object

// the need here is it should create function and add a object as prototype to that function then return the newly created function 


    Object.create = function (o) {
        // the arguments ie the parameter should be a single object which we will pass from object.create , if we pass more than one argument we need to throw error
        if (arguments.length > 1) {
            throw new Error('Object.create implementation'
            + ' only accepts the first parameter.');
        }
        // But this is the important part, it creates an empty function.
        function F() {}
        // And then sets the prototype, that variable of prototype that is used for function constructors, and sets it equal to the object that you passed in.

        // So you can completely overwrite this prototype, set it to whatever object you want. And any objects created using the function constructor will get this as its proto property.
        F.prototype = o;
        
        // All right, so this returns a new, it uses the new keyword. And then a new empty function here, which is just a constructor function.

        // So, if we think about what this new keyword is doing. The new creates an empty object, it runs this function, which is empty and points the prototype of that new empty object to whatever you passed in.

        // simply saying we already created some function with prototype as the object we passed in, now here we are wrapping that function with a empty object and then we are returning...    

        return new F();
    };
}
// So this is a polyfill.
```

* All right so let's go back to thinking about the usage of Object.create.

* I create an object that forms the basis of my constructing all other objects.

* Then I use Object.create just to create other versions of it, and then can overwrite it or hide its properties. So, this is pure prototypal inheritance.

* It's not trying to squeeze in any other concepts from other programming languages. It's just, create an object, and then use that object as the prototype for other objects. Very simple, very straightforward.

* And yet, if you think, oh, this is too simple, stop and realize the power of being able to mutate, to change the prototype on the fly.

* You can add features depending on what features you need for example, at any point in the application. It opens up a freer approach to constructing objects. And you're not unnecessarily creating complex layers and interactions.

### ES6 and Classes

* The next version of JavaScript, EcmaScript 2015, or EcmaScript 6 or ES6, whatever you want to call it, has a new concept coming.

* And yet, it's just another way to create objects, and to set the prototype. Let's look at classes.

* Classes in other programming languages are extraordinarily popular. They're a way of defining an object, defining what its methods and properties should be.

* JavaScript doesn't have classes. However, in the next version it will, but in a different way.

* A JavaScript class defines an object. Just like we have done all along.lets say

```js
class Person {
    // We have an constructor that acts somewhat like the constructor functions that we've seen in that you can preset its values.
    constructor(firstName, lastName){
        this.firstName = firstName;
        this.lastName = lastName;
    }
    // I can also have methods like greet sitting inside the class.
    greet(){
        return 'Hi ' + this.firstname+' '+this.lastName; 
    }
}

// So when I create a new person in this instance, for example, and I use the new key word to do this.
var John = new Person("John", "Abraham");

```

* But here's the problem, people coming from other programming languages, when they see this class they think oh, great. This is just like what I'm use to.

* The difference is that in other programming languages, class is not an object, it's just a definition, it's like a template.It tells you what objects should look like, But you don't actually get an object until you use that new key word.

* But JavaScript even though it's adding the class key word, still doesn't have classes in that sense. Because this is an object in JavaScript. This class person actually is an object that's created.

* And then you're creating new objects from that object, and that's okay.

* My fear though is that rather than appreciating the differences and the beauty of prototypal inheritance, those who are coming from other
programming languages will simply see the class key word and immediately begin to design object structures the way they do in C# or Java or C++. I think that's a huge mistake.

* And I also am worry about still having to use the new key word. It's still an attempt to appease those coming form other languages. And I personally aren't crazy about it.However, it's one of those things. It's in the language.

* It's definitely better than function constructors because at least the language will be able to say this is a class. You have to use the new keyword so the engine can stop you from doing something really silly.

* My hope is that if you are learning JavaScript, if you're using JavaScript, you'll stop just like this class is trying to help you do, and understand what's happening under the hood and appreciate prototypal inheritance. The simplicity of it as opposed to trying to mimic some other programming language.

* Oh, by the way, how do you set the prototype, then? Well, that's another new key word.

* Let's say that I was creating an informal person, or a person is my prototype, I simply use the key word "extends".

```js
class InformalPerson extends Person {  // extends set the prototype

    constructor(firstName, lastName){
       super(firstName, lastName) // And in my constructor, I can call the key word super. Which simply we'll call the constructor of the object that is my prototype. So I can pass my initial values down the chain.
    }

    greet(){ // And then I can override or hide, just like I did. With object.create.
        return 'Yo yo ' + this.firstname+' '+this.lastName; 
    }
}
```
* And extends sets the prototype for any of my objects created with this class, essentially that __proto__ property at least in Chrome.

* But we will talk in a bit about how you can use these features right now. Even if your users are using older browsers, or that these concepts aren't even ready at all in the current browsers.

* But, for now, this is what's coming for creating objects, creating methods and properties on objects, and setting their prototype.

* It's just another syntactical way, another way to type this. Another approach but under the hood, it's all still working the same.

* In fact, if you read on the Internet about classes, you'll often see for classes in JavaScript a phrase that it's just syntactic sugar.

* syntactic sugar is just when you have a different way to type something into your code. It doesn't change how it actually works under the hood.

* We've already seen that function constructors, object.create, are doing the same thing ultimately.Essentially class in JavaScript is doing just that. It's not changing anything about how the JavaScript engine does things, about how objects and prototypes work under the hood. It's just syntactic sugar.

* Just giving you a different way to type it in your code. But when that code is executed, what's generated by the JavaScript engine, is still the same old thing. It's still prototypal inheritance.

## Odds and Ends

### Initialization

* We've seen how we can use object literals, array literals, and function expressions to set up our objects, arrays, and functions.

* What I find as when folks who aren't yet used to all that literal notation come across a large initialization of objects in JavaScript, they can get a little overwhelmed by the syntax.

```js
var people = [
    {
        // the 'john' object
        firstname: 'John',
        lastname: 'Doe',
        addresses: [
            '111 Main St.',
            '222 Third St.'
        ]
    },
    {
        // the 'jane' object
        firstname: 'Jane',
        lastname: 'Doe',
        addresses: [
            '333 Main St.',
            '444 Fifth St.'
        ],
        greet: function() {
            return 'Hello!';   
        }
    }
]

console.log(people);
```

* And as you can see as we're typing, it starts to get large and those that aren't used to it can be a bit intimidated by what they're looking at.

* So, when you look at this, when this gets extremely large, a lot of data, it can get a bit intimidating.

* This is really clean and useful way to initialize data. And it's also really nice for prototyping.

* Say you're building some software that's not yet connected to a database or an API. You can easily set up some data like this to use in your interface.

* but if you miss any syntax accidently you will get unexpected syntax error.

### 'typeof' , 'instanceof', and Figuring Out What Something Is

* We've seen how dynamic typing in JavaScript lets us do some interesting things and can also be a little dangerous.

* But what if you have a variable and you want to know what type it is, programmatically? Well, JavaScript does come with some utilities to help us with that, but they aren't perfect.

* typeof - What type of thing is this. So, for example,

```js
var a = 3;
console.log(typeof a); // number 

var b = "Hello";
console.log(typeof b); // string 

var c = {};
console.log(typeof c); // object

var d = [];
console.log(typeof d); // weird! object
console.log(Object.prototype.toString.call(d)); // better! [object Array] // here we invoked toString on variable d

function Person(name) {
    this.name = name;
}

var e = new Person('Jane');
console.log(typeof e); // object - expected 

// instanceof - tells me if any object is down the prototype chain.

// If anywhere down that whole prototype chain of going proto, to proto, to proto, I find this type of object.

console.log(e instanceof Person); // true - because it's an instance of person because we can find the person down the prototype chain. And it does come back true.

console.log(typeof undefined); // undefined - makes sense Well, undefined isn't anything, so its type is undefined.

// One last thing that's been a bug in JavaScript since forever, a long time, so much so that there are libraries and a lot of code that rely on it being so, so they've never fixed it.

// If you try to do typeof and something is null, you'll end up with an object.

// I know that's kind of terrible, but it's a bug and it's been around too long to fix in some ways.

// This is a bug and one that unfortunately can't be fixed, because it would break existing code. ... 

console.log(typeof null); // object - a bug since, like, forever...

var z = function() { };
console.log(typeof z); // function - Remember, I can check the type of a variable to see if it's a function because functions in JavaScript are objects.

// So since it's a first class function, it's really an object. I can also see if a variable is a function.
```

###  Strict Mode

* We've seen throughout this course that JavaScript can be somewhat liberal about things when it comes to what it allows.

* There is a way that you may have heard of to opt in, to tell the JavaScript engine that you would like it to process your code in a stricter way, to be more regimented, to be pickier about what it lets you do.

* Now this doesn't solve every possible case where JavaScript is more liberal, because JavaScript is a very flexible language. And with flexibility, comes a lack of rules, sometimes. But strict mode can help you prevent errors under certain circumstances. Let's show you one right now.

```js
var person; // we created a variable person
persom = {}; // But let's suppose that I mistype the variable. And I put persom instead of person.

// console persom, Do you think this is going to error?

// No.. i didn't show any error I have an empty object. It's sitting on the global object window.persom.

// And is also window.person, which is undefined because I never set it, because I mistyped the variable. That can cause some really squirrely errors that are tough to track down. So JavaScript has an option.

console.log(persom);
```

* So JavaScript has an option. And you're quite literally telling the JavaScript engine to please be stricter, to implement some extra rules when you parse my code, when you try to execute it. And so use strict.

```js
  "use strict";
function logNewPerson() {
    var person2;
    persom2 = {};
    console.log(persom2);
}

var person;
persom = {};
console.log(persom); // persom is not defined uncaught reference error  
logNewPerson();
```
* "use strict"; it must go either at the top of your file, or at the top of a function. Cuz you can make a function be executed with use strict,

* essentially a new execution context to have its code run with use strict, and not the rest of the file, not the global execution context or other context.

* use strict would have to be at the top if I wanted to use it everywhere.

* if you use "use strict";" inside a function that will apply only to the particular function.

* So you may be asking why didn't we use, use strict throughout this entire course? Because this is an opt in. This is an extra feature.
And not every JavaScript engine implements use strict the same way. They don't all agree on every rule that they'll implement. So this is an extra thing, and not something you can 100% rely on.

* But it can be useful. So you might want to use it at the top of your file or functions.

* Also a note there, if you have several JavaScript files, and then as part of your pushing them to production, you concatenate and minify them, that's very common where you put all of your JavaScript files together in one file so it only has to be downloaded once.

* If that first file has use strict at the top, then the whole thing will be processed using that strict JavaScript engine flag. So you might cause yourself some trouble if other JavaScript files don't follow the strict rules.

* Extras : Read more about strict mode on the MDN (Mozilla Developer Network): https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode

## Examining Famous Frameworks and Libraries

### Learning From Other's Good Code

* There are many aspects to improving as a programmer, as a developer. One of the most powerful is learning from others' good code.

* And what's really terrific about working inside of the JavaScript architecture and the JavaScript community is that there's a fantastic treasure trove of good code out there for you to learn from.

* One of the goals of this course thus, was to give you an understanding of the more advanced concepts in JavaScript so that you can go off and read interesting source code and learn from it, and not be confused by what your seeing inside of it.

* But this is one of those things that you should do. Don't be afraid to open what at first could feel like intimidating source code. You can learn from these famous frameworks and libraries like AngularJS, jQuery, and others.

* So, where can you find all this goodness? Well, on the internet of course. GitHub, GitHub.com is not only a great place to store your code, but it's also a great place to learn. To read source code. (https://github.com/collections/front-end-javascript-frameworks)

###  Deep Dive into Source Code: jQuery - Part 1

* It's time to take a deep dive into famous source code.

* We're going to take all that we've learned and see if we can't use it to understand the structure and style of famous well-used open source code.

* The library's perhaps the most famous JavaScript library there is, is jQuery. We're going to examine it.

* We're going to dive into its source code. We're not going to try and understand how every feature is implemented, but we're going to try to see one if we can read the code. And two, If we can learn from how it's structured, if we can gain some knowledge and some techniques, and borrow some ideas from inside source code of jQuery.

* jQuery is a JavaScript library that doesn't add any features to the browser or to JavaScript itself. It's just a JavaScript library.

* What it does do is make it easier to type syntactically to write certain things and it deals with cross browser issues, meaning that each browser, Firefox, Safari, Google Chrome, Internet Explorer, and various mobile versions of these browsers, all have their quirks and differences.

* jQuery handles those so you don't have to worry about it. You just are doing what you want to do and the code inside of jQuery is handling the browser quirks.

* What jQuery does essentially is let you manipulate the DOM. The DOM is the Document Object Model inside a browser. It's separate from the JavaScript engine.

* It's the thing that lets the browser look at HTML and decide how to render it or paint it on the screen. And JavaScript has access to the dom. It can manipulate it. It can manipulate the structure of an HTML page after it's been loaded by manipulating the dom that's in memory.

* That is this tree like structure that houses or stores. A representation of your HTML that's used to paint or render on the screen.

* jQuery then makes it easy to look at that tree, to look at the DOM, to find things, to find elements on your page and manipulate them.

* lets say we are having the below html with list ...
```html
<html>
    <head>
        
    </head>
    <body>
        <div id="main" class="container">
            <h1>People</h1>
            <ul class="people">
                <li>John Doe</li>
                <li>Jane Doe</li>
                <li>Jim Doe</li>
            </ul>
        </div>
        <script src="jquery-1.11.2.js"></script>
        <script src="app.js"></script>
    </body>
</html>
```

* So in jQuery now, what's happened is this is now sitting in reality in a tree-like representation in memory inside the browser.

* And JavaScript has access to all of these. This would be a parent and then a child and then a grandchild and on down the line.These are parent and child kind of relationships.

* Lets add jquery to find all ul in the tree

```js
var q = $("ul.people li");
console.log(q); // I'll refresh and open my console. Interesting. I get an objective of type jquery.fn.init[3], not just jquery.

// And it looks to be an array, or at least array like. each array from 0 to 2 is li

// Let's look at my __proto__. That's a jQuery object, and if I open it up, It has a whole bunch of methods on it.
```
* So remember how we said it's better to put methods on the prototype to save memory space. Well here it is. We have a prototype that has a huge number of features and functionality that make jQuery so popular, all kinds of stuff we can do.

* and it's kind of unusual that we're seeing .fn.init, that's a little unexpected. What is that?

* Well let's see how this is structured. And also note that we didn't use the new operator. We just said $ and pass the string. Let's see how this is working under the hood.

* Open our Jquery, Let's look and see if we can find things that look familiar. If we can understand it's structure, and see what we can learn.

* First thing you will see is IIFE, It's an immediately invoked function expression. It's using the parentheses to trick the syntax parser into treating this as a function expression. So it's wrapping those entire lines, all the features of jQuery. Inside a function call.

* The function itself, takes two parameters. Global and factory. Well that makes sense, we did that before. Deciding what is the global object.


* In fact, if I look here. It's checking for something called module, and using the typeof, you've seen that. It's checking to see of module is an object and module.exports is an object. And the comment tells us this is for CommonJS like environments. Or node.js. So it looks like what it's doing, is it's checking to see what the environment is that jQuery is living in.

```js
(function( global, factory ) {

	if ( typeof module === "object" && typeof module.exports === "object" ) {
		// For CommonJS and CommonJS-like environments where a proper window is present,
		// execute the factory and get jQuery
		// For environments that do not inherently posses a window with a document
		// (such as Node.js), expose a jQuery-making factory as module.exports
		// This accentuates the need for the creation of a real window
```

* Lets see what it actually calls

```js
(typeof window !== "undefined" ? window : this, function( window, noGlobal )
```
* So it immediately invokes this here, and passes as the global. There's the global, and here's where it's invoking this function expression. 

* It's checking to see if there's a window object. We understand that in the browser, the global object is window. So if window is not equal to undefined. Oh this is a ternary operator. It takes three parameters. The first parameter is a boolean. If the boolean is true, it returns this. If the boolean is False. Then it returns whatever's on the other side of the colon.

* So it's saying, if window is not equal to undefined.And notice, it uses the strict, not going to coerce. Not equals. Because it's not double equals. If window is not undefined then pass window to this global (IIFE param). Otherwise pass whatever "this" is pointing to. Which would be the global object in whatever environment you're sitting in. Maybe Node JS or something.

* then for factor(IIFE), we will pass whole bunch of code

```js
, function( window, noGlobal ) { .....
```

* So this is just taking care of where is jQuery sitting. And this is actually the jQuery code.

* Alright. So, this is another function expression. This one is window, and noGlobal.

* Let's try to remember that. But we do know that we're getting the window object as some point in here. When this factory is actually being invoked.

```js
module.exports = global.document ? factory( global, true ) : function( w ) {
                                                                    if ( !w.document ) {
                                                                        throw new Error( "jQuery requires a window with a document" );
                                                                    }
                                                                    return factory( w );
                                                                };
```

* This function expression ```, function( window, noGlobal ) { .....)``` is being passed to factory. And factory's being invoked at some point. All right. So what happens when this is invoked?

* Well, it sets up some variables in this new execution context.

```js
var deletedIds = [];

var slice = deletedIds.slice;

var concat = deletedIds.concat;

var push = deletedIds.push;

var indexOf = deletedIds.indexOf;

...
...
...
```
* When the factory is invoked, which ends up being this function object that's passed in. I get a new execution context, and it starts creating some variables.

* All right, here's the important one here. Let's see. jQuery.

```js
// Define a local copy of jQuery
	jQuery = function( selector, context ) {
		// The jQuery object is actually just the init constructor 'enhanced'
		// Need init if jQuery is called (just allow error to be thrown if not included)
		return new jQuery.fn.init( selector, context );
	},


```
* We know that's the one that we can use in our code, in order to call this. And as expected, it's a function. And it takes a selector, and a context.

* Well we've already used the selector. That's what jQuery calls that string where you're passing in what HTML elements you're trying to find.ie in our app.js

* But I'm surprised here, that I don't have a bunch of code. Instead it's returning and then using the new operator jQuery.fn.init.

* Oh and remember, when we created a jQuery object. In here and console.logged it. Remember what that was? It was a jQuery.fn.init.

* The jQuery function isn't a function constructor. It's just a function. That's why we don't have to use the new operator in our app.js. just we are using $ or jquery not as new jquery

* All it is , a function that returns an object. But it does return an object by calling a function constructor. See that? It uses the new keyword. So it keeps me from having to use the new keyword myself all the time. And possibly forgetting it when using jQuery.That's a neat trick.

* A function that returns a call to a function constructor. Keeps me from having to use the new keyword myself, when I use Jquery.Alright, so let's keep that in mind.

* So somewhere down the line we're going to find out that this somewhere is a function constructor. So init function constructor sitting on some fn object. Which will be sitting on this jQuery object.

```js
return new jQuery.fn.init( selector, context );
```

* Alright, so moving on. Let's see if we can find, oh here we go.

```js
jQuery.fn = jQuery.prototype = {
	// The current version of jQuery being used
	jquery: version,

	constructor: jQuery,

	// Start with an empty selector
	selector: "",

	// The default length of a jQuery object is 0
	length: 0,

	toArray: function() {
		return slice.call( this );
    },
    ...
    ....
```

* Jquery.fm, there it is. What is it? Well, it's a reference.

* Remember that these are objects, and therefore they're passed by reference. So fn is pointing at the same memory spot, as the prototype property of the jQuery function.

* Remember all functions which is assigned to jQuery, get a .prototype property. Which is used if they're used as a function constructor.

* But it's really just hanging out there. Even though we're not using this as a function constructor, it's still there. It's just an empty object.

* So it's using this empty object (jQuery.prototype) and having an alias for it (jQuery.fn). I guess that's just so it doesn't have to type .prototype all the time. We can just type .fn, it's a lot faster to type. All right

* So when we see that .fn, we need to remember that this just means function prototype. The prototype property on this function, the same prototype all functions get. And it's being overwritten with a new object, see the object literal syntax. It's creating a bunch of name value pairs, a bunch of properties.

* let see the jQuery prototype methods lets say..


```js
    ...
    each: function( callback, args ) {
            return jQuery.each( this, callback, args );
        },

    map: function( callback ) {
        return this.pushStack( jQuery.map(this, function( elem, i ) {
            return callback.call( elem, i, elem );
        }));
    },
    ...
    ...
```
* Well, here are some things we've seen, each and map using a callback. So you're passing a callback to a function. And look, on that for example, it does callback.call.

* So it is invoking whatever function you give it in the map and probably acting on that element. Well that's kind of like out map for each, remember?

* It's calling back whatever we give it on some list of things. We'll just go on, cuz it looks like there's some other deeper functions that do this.

* Well, this is interesting. Another double reference, so essentially an alias giving two names to the same function.

```js
jQuery.extend = jQuery.fn.extend = function() {
	var src, copyIsArray, copy, name, options, clone,
		target = arguments[0] || {},
		i = 1,
		length = arguments.length,
		deep = false;

	// Handle a deep copy situation
	if ( typeof target === "boolean" ) {
		deep = target;

		// skip the boolean and the target
		target = arguments[ i ] || {};
		i++;
	}

	// Handle case when target is a string or something (possible in deep copy)
	if ( typeof target !== "object" && !jQuery.isFunction(target) ) {
		target = {};
	}

	// extend jQuery itself if only one argument is passed
	if ( i === length ) {
		target = this;
		i--;
	}

	for ( ; i < length; i++ ) {
		// Only deal with non-null/undefined values
		if ( (options = arguments[ i ]) != null ) {
			// Extend the base object
			for ( name in options ) {
				src = target[ name ];
				copy = options[ name ];

				// Prevent never-ending loop
				if ( target === copy ) {
					continue;
				}

				// Recurse if we're merging plain objects or arrays
				if ( deep && copy && ( jQuery.isPlainObject(copy) || (copyIsArray = jQuery.isArray(copy)) ) ) {
					if ( copyIsArray ) {
						copyIsArray = false;
						clone = src && jQuery.isArray(src) ? src : [];

					} else {
						clone = src && jQuery.isPlainObject(src) ? src : {};
					}

					// Never move original objects, clone them
					target[ name ] = jQuery.extend( deep, clone, copy );

				// Don't bring in undefined values
				} else if ( copy !== undefined ) {
					target[ name ] = copy;
				}
			}
		}
	}

	// Return the modified object
	return target;
};
```
* It's a function object. We've got a function expression here. And it's called extend. Hey, I bet that's the same concept that we saw in underscore.

* So it's being added to the .fn and to jQuery itself, as a reference to the one single function (jQuery.extend).

* And inside it, it looks like it's doing some kind of deep copy, and adding on objects.Yeah, this is the same as the underscore essentially, that we saw where it's adding things to an object.

* It's taking properties and methods off one object and adding them to another.

* So it's looking at the arguments.length, so I probably can pass in as many objects as I want. Let's see and yeah we have a for loop on the length. So on all of the arguments so if I pass in a bunch of objects as arguments to this function as parameters it'll go through all of them and add all of there.


* Yeah see the for in. So if I add three objects for example to this extend function, I'll end up with a length of three.

* And it'll find each argument and then it'll go through using for in that we saw before. It's using reflection to go through all the properties and methods of every object that I add or that I passed in to this function.

* And what is it doing with this for in?

* For names, we need to find whatever name is used and see what it's doing, well, it's doing a source and a copy. So, without getting too much into what it's doing, it basically looks like it's doing what we said.

* It's adding on properties and methods of an object to an object.That's interesting.

* We have a target, and our target is, what is our target? Our target is our first argument. 

* So the first thing we pass to extend is what we want to actually extend, where we want to drop these properties and methods, which object is receiving them. And the rest of the parameters and which objects hold them. So we're adding all of the other parameters to the first one. ```target = arguments[0]```

* So source is where we're copying our methods and properties to. And so it's adding a new method and property.

* But it's using extend again, probably because objects can hold other objects. So it's making sure that every possible method and property, even of sub-objects, is being added on to our target.

```js
// Never move original objects, clone them
	target[ name ] = jQuery.extend( deep, clone, copy );
```

* All right, so we have this really cool extend method that's really serious. I mean, it does a good job of extending. And we can pass it an object or even sub-objects, and I'll get all those methods and properties on my target object where I want them to be.

* In fact, right away it uses it. Look at that. It calls extend and adds methods and properties to the jQuery object.

```js
jQuery.extend({
	// Unique for each copy of jQuery on the page
	expando: "jQuery" + ( version + Math.random() ).replace( /\D/g, "" ),

	// Assume jQuery is ready without the ready module
	isReady: true,

	error: function( msg ) {
		throw new Error( msg );
	},

	noop: function() {},

	// See test/unit/core.js for details concerning isFunction.
	// Since version 1.3, DOM methods and functions like alert
	// aren't supported. They return false on IE (#2968).
	isFunction: function( obj ) {
		return jQuery.type(obj) === "function";
	},

	isArray: Array.isArray || function( obj ) {
		return jQuery.type(obj) === "array";
    },
    ...
```

* Okay, so wait a minute, wasn't the first argument the target??? !!! will see

### Deep Dive into Source Code: jQuery - Part 2

* So how is it doing this?

* Extend jQuery itself if only one argument is passed. So if there's only one argument the target becomes this. And this keyword is going to point to the jQuery object, because this is a method on the jQuery object. And so, this will be jQuery.

```js
// extend jQuery itself if only one argument is passed
	if ( i === length ) {
		target = this;
		i--;
	}
```
* If I pass only one parameter to the extend function, it will extend the object itself that it's calling on.

* So it adds some more methods. Well here's some interesting ones isArray, isNumeric

* Oh hey, look at this. This is interesting.

```js
// Populate the class2type map
jQuery.each("Boolean Number String Function Array Date RegExp Object Error".split(" "), function(i, name) {
	class2type[ "[object " + name + "]" ] = name.toLowerCase();
});
```
* A class2type. Array, really an object. It takes all of these names, which are standard names in JavaScript, as a string and then splits them, which is a JavaScript function to create an array on a delimiter.

* So it says split on a space, wherever you see a space and make each of those things its own item in an array. And then it's concatenating oh that bracket object and then end bracket.

* So this is used to understand, to help us figure out what something is. By calling two string you get this. Bracket object in bracket that we saw with arrays, remember that? And so, that way you can definitely figure out what something is, that's neat.

* Let see what else we have..

```js
var Sizzle =
/*!
 * Sizzle CSS Selector Engine v2.2.0-pre
 * http://sizzlejs.com/
 *
 * Copyright 2008, 2014 jQuery Foundation, Inc. and other contributors
 * Released under the MIT license
 * http://jquery.org/license
 *
 * Date: 2014-12-16
 */
(function( window ) {

var i,
	support,
	Expr,
	getText,
	isXML,
	tokenize,
	compile,
    select,
    
```

* Sizzlejs, a CSS Selector Engine, and its Sizzle is its own variable.

* This is another immediately invoked function that I'm passing a global object to.

* This is a whole other engine inside of jQuery. Wait a minute, sizzlejs.com, let's go look at that for a second. Sizzlejs.com, a pure JavaScript CSS selector engine designed to be easily dropped in a host library. So inside of jQuery is an entire other library called Sizzle that does all of the give you a string to find things in the HTML part. So that string that I passed the selector, that's using Sizzle to do that. So that's interesting.

* I can have an immediately invoked function expression inside an immediately invoked function expression. And I can sit whole other libraries inside my library.
So jQuery has Sizzle sitting inside of it.

* Alright well we're not interesting in how Sizzle works, but that's good to know, right? That's an interesting pattern. Did you know that? 

* That there's an entire other library inside jQuery that's doing this. Let's see if we can. I'm just gonna look for the word jQuery to see if I can find the end of all this Sizzle code.


```js
var Sizzle =
/*!
 * Sizzle CSS Selector Engine v2.2.0-pre
 * http://sizzlejs.com/
 *
 * Copyright 2008, 2014 jQuery Foundation, Inc. and other contributors
 * Released under the MIT license
 * http://jquery.org/license
 *
 * Date: 2014-12-16
 */
(function( window ) {

var i,
	support,
	Expr,
	getText,
	isXML,
	tokenize,
	compile,
    select,
    ....
    ...
    ..
return Sizzle;

})( window );

jQuery.find = Sizzle;
jQuery.expr = Sizzle.selectors;
jQuery.expr[":"] = jQuery.expr.pseudos;
jQuery.unique = Sizzle.uniqueSort;
jQuery.text = Sizzle.getText;
jQuery.isXMLDoc = Sizzle.isXML;
jQuery.contains = Sizzle.contains;

```
* Return Sizzle and then it ends the immediately invoked function for the Sizzle library and passing the window object to that immediately invoked function.

* Alright so that's the end of Sizzle. And then I'm getting some references to the stuff that Sizzle created, that it put on the global object, probably. Yeah, so jQuery.find is really just Sizzle. They're pointing to the same memory space, the same function. So jQuery is using that Sizzle library.

* Initialize a jQuery object.

```js
// Initialize a jQuery object


// A central reference to the root jQuery(document)
var rootjQuery,

	// Use the correct document accordingly with window argument (sandbox)
	document = window.document,

	// A simple way to check for HTML strings
	// Prioritize #id over <tag> to avoid XSS via location.hash (#9521)
	// Strict HTML recognition (#11290: must start with <)
	rquickExpr = /^(?:\s*(<[\w\W]+>)[^>]*|#([\w-]*))$/,

	init = jQuery.fn.init = function( selector, context ) { // So, here's that function constructor.
		var match, elem;
        ....
        .....


    return jQuery.makeArray( selector, this );
	};

	
```


* All right so let's see how this object is being made, we're still confused about that .fn.init and there it is. Okay, so this is the constructor function that's calling new jQuery that fn.emit way at the top remember?

* So, here's that function constructor.

* It takes a selector and it has some properties just managing different usages.

* in the end It's returning a value. Remember we said that calling new operator creates a new empty object. And that's what's returned as long as you don't return a value yourself.

* So it's calling new on a function and then returning a value.

* Isn't that wrong? Well let's see. What is it returning? It's returning .makeArray and passing this.

* Well this is pointing to what? If I'm using a function as a function constructor. What is it this variable when I'm using a function with a new keyword. It's that empty object that's created by the new operator, remember?

* And then passing it by reference to some makeArray function.

```js
makeArray: function( arr, results ) {
    var ret = results || [];

    if ( arr != null ) {
        if ( isArraylike( Object(arr) ) ) {
            jQuery.merge( ret,
                typeof arr === "string" ?
                [ arr ] : arr
            );
        } else {
            push.call( ret, arr );
        }
    }

    return ret;
},
```
* And it's returning the results of that. And results is this var ret = results || [];. So it's taking that empty object which is results, doing some things with it, converting it to an array, and then returning it.

* So it took my empty object, made it into an array of sorts, and then returned that. So it's not really getting in the way of the function constructor. Cuz it's still giving me back my empty object that was created with the new operator, it's just doing some stuff to it first.

* So you could, potentially, take this variable which is pssed to makeArray and then, pass it around to other functions to do things to it and return it from a function constructor.

* As long as you're still returning the same object created by the new operator you aren't really getting in the way of that new operator feature.

* It's okay to return a value from a function constructor. As long as what I'm returning is the same as this variable. I can use the same pattern successfully because the prototype has been set up properly.


* All right, so I'm a little confused by this line here. This seems pretty important.


```js
// Give the init function the jQuery prototype for later instantiation
init.prototype = jQuery.fn;

// Initialize central reference
rootjQuery = jQuery( document );
```

* Init is just pointing to jQuery.fn.init. So same memory spot, same function. They're both pointing to this function in memory and all functions get a prototype. And, this is actually the new object that I'm creating, cuz it's actually calling new jQuery.fn.init.

* Init is just pointing to jQuery.fn.init. So same memory spot, same function. They're both pointing to this function in memory and all functions get a prototype. And, this is actually the new object that I'm creating, cuz it's actually calling new jQuery.fn.init.

* the object that's outputted from jQuery we're a bit confused because it's jQuery.fn.init, instead of just a jQuery. But all of these cool properties are sitting where? Remember?

* It's sitting on the jQuery.prototype, that has a whole bunch of stuff. 

* Even though I'm creating a new object inside this init function constructor, I'm making sure that that object's prototype, any new object created with that init, which I'm doing, let's see, here I'm creating a new object with that init function constructor.

* Because this function cause new jQuery.fn.init, it's prototype is the same memory spot as that jQuery.prototype.

* So all I'm basically saying is make sure that any objects that come back from this function constructor have access to all of the properties and methods on this object right here (jQuery.fn;). I'm overriding the prototype.


* I don't have to call the new operator when I use jQuery. Instead this calls a function that then calls a function constructor, creating a new object. But that new object has access to all the properties and methods here. And instead of always in this jQuery code type in jQuery.fn.init.something, it's just the prototype of the jQuery object itself.All right, so that's a neat trick to be able to avoid having to use the new keyword out where I'm using this library itself.Think about that for a bit.

* okay lets see all togeter again

* When i call app.js

```js 
var q = $("ul.people li");
console.log(q);
```

* when i call above code the below function I'm actually calling when I do this. I'm passing a string.

```js
jQuery = function( selector, context ) {
		// The jQuery object is actually just the init constructor 'enhanced'
		// Need init if jQuery is called (just allow error to be thrown if not included)
		return new jQuery.fn.init( selector, context );
	},

```
* The string is the selector, and then what it's giving me back is a new object created with the new keyword calling jQuery.fn.init function. This is a function constructor.

* A function constructor creates a brand new empty object with the new keyword and then points the prototype of that object to the .prototype property of this function.So, here's my function that's actually called.

```js
// Give the init function the jQuery prototype for later instantiation
init.prototype = jQuery.fn;
```
* here, we make sure that that prototype property is this object I've already created with all kind of properties and methods.

* We're adding a whole bunch more properties. See that using using extend again, we're not gonna worry about what these do. Features, features, features.

### Deep Dive into Source Code: jQuery - Part 3

```js
var q = $("ul.people").addClass("newclass").removeClass("people");
```
* So the add class acted on this jQuery object and added a new class to that ul, and then I called the removeClass within another dot operator and it acted on that same object.

* we are adding a class by refering a class and immediately we are removing that reference class.. shouldn't be affect this addClass ??

* How is jQuery doing that?

* I really like that as a style, allowing other coders to quickly run methods like this, this is called method chaining.

* Method chaining: calling one method after another, and each method affects the parent object.

* I'd like to know how to do that, let's look at jQuery.

* I think the fastest way to do this would be to just find the addClass method inside the jQuery source code.

```js
jQuery.fn.extend({
	addClass: function( value ) {
		var classes, elem, cur, clazz, j, finalValue,
			i = 0,
            len = this.lengt
            ...
            ....
            return this;
	},

	removeClass: function( value ) {
        ...
        return this;
    }
```
* it's added onto the prototype via the extend method.What does it do? Well, I don't really need to know what it does, I just wanna know how is it that I can chain this method?

* after addclass return, then that is followed by remove class return this

* what's this pointing at, the this variable? Well since these are methods attached to the object, well, it's attached to the prototype and remember when we call the method from the object, it will look for it on the object.It won't find it, it'll find it on the prototype, right?

* But when I execute a method or call a method, and it's found down the prototype chain, the this variable points at that originating object, the object that actually made the call.

* So that means the this variable points to my jQuery object, the one that was newly created by this query ($("ul.people")) and that's what the this variable points to. So the add class returns it, I have an object I call addClass, it does some work and returns this.

* So when this is done it's essentially like that, the object that it returned is this object and so I can call remove class on it. So all I have to do to make methods chainable, methods of an object, is finish the method, the last line essentially of the method, the last thing I do is return the this variable.

* Lets scroll down to the bottom and see this

```js
var
	// Map over jQuery in case of overwrite
	_jQuery = window.jQuery,

	// Map over the $ in case of overwrite
	_$ = window.$;

jQuery.noConflict = function( deep ) {
	if ( window.$ === jQuery ) {
		window.$ = _$;
	}

	if ( deep && window.jQuery === jQuery ) {
		window.jQuery = _jQuery;
	}

	return jQuery;
};

// Expose jQuery and $ identifiers, even in
// AMD (#7102#comment:10, https://github.com/jquery/jquery/pull/557)
// and CommonJS for browser emulators (#13566)
if ( typeof noGlobal === strundefined ) {
	window.jQuery = window.$ = jQuery;
}




return jQuery;

}));
```

* so jQuery is first making sure, remember we said we could when we add something to the window object make sure that it's not already there?

* So, it's checking to see if there is a jQuery function at the global object. Just put an underscore there, just to keep it separate, and it's checking to see if there's a $ on the window object, and it's just keeping it separate.

* And then, there's a no conflict that you you can call the checks to see if this thing that's already on the global object is jQuery. And if it is, then it just keeps it there basically, and if not, it kind of ignores it, it says, oh, it returns jQuery.

* So, this is just making sure there isn't something called jQuery or $ on the global object first

* Looks like it's making sure that there's a global object, and if there is, it does window.jQuery. So it puts that in the global namespace, the global object, and
it just does another reference, so this is just an alias, window.$ so the $ is just a function name. And it's pointing at the same spot in memory as the word jQuery,

```js
if ( typeof noGlobal === strundefined ) {
	window.jQuery = window.$ = jQuery;
}
```
* So this is exposing jQuery or $, both of these, it's exposing this function, globally. So I can use it here, so when I call $, I'm calling a function, that's why I have parentheses.

* and this jQuery sitting on top ....

```js
jQuery = function( selector, context ) {
		// The jQuery object is actually just the init constructor 'enhanced'
		// Need init if jQuery is called (just allow error to be thrown if not included)
		return new jQuery.fn.init( selector, context );
	},
```
* And then it's doing the whole thing of calling the new, calling a function constructor that returns an adjusted object that's adjusted into an array. And make sure that it's prototype, this which is the prototype, has access to all of these functions and methods and properties, all this stuff that it added on.

* So it makes sense then that when I see my jQuery object, that it's actually an array.

## Let's Build a Framework / Library!

### Requirements

* It's time to use everything we've done in this course so far to build our own little mini framework library.

* Frameworks and libraries are similar concepts, although frameworks are a more complex.

* But for the purposes of this course we're going to talk about them as if they were the exact same thing. Because we're focusing on structure.

* Not so much what it does. So let's build our own mini-framework/library. And let's build something that you could use in a project if you wanted to or in many projects.

* All right, let's do something first, that we need to do for any software project. Let's look at the requirements.What should the software do?

* Well, first of all, what is our little framework library going to be called? Let's call it greetr.

* All this time in this course we've been using greet as a example, and I like to use those simple examples because we can focus on the concept and not get lost in the implementation.

* Let's say we wanted to build a library or frame work that helps us give greetings. Maybe for when I'm on a website and someone logs in I can show them a greeting after they login in the upper right corner or right after they log in.And maybe I'd like to do it in different languages, depending on what language they choose, their preferred language in the application. Something like that.

* Let's do the official requirements.

    * When I'm given a first name, a last name and an optional language, it should generate formal and informal greetings that I could use throughout my app.

    * It should support both English and Spanish languages for starters.

    * It should be reusable, meaning that it won't interfere with any of the other JavaScript code in my app, and someone else can just grab it and use it in their apps.

    * And lastly, even though it's called greetr, I'd like to have an easy to type structure, kind of jQuery like, and a G$, maybe, like the dollar sign for jQuery. Let's do G$.

* All my developers use jQuery on a lot of their projects. So greetr should also support jQuery.

* Even though it returns greetings what we'd like is to be able to give it a jQuery object that points at some HTML element. And it'll fill that element with the greeting.

* So I could just pass my greetr, let's say a div or a span in my HTML that just contains text.

* And it'll fill that with my appropriate greeting text.

### Structuring Safe Code

* To start with, let's look at structuring our greeter frame work in a way that's safe so that it can be reused inside any Java script application.

* I've got an index, that HTML page, and three JavaScript files. I have jQuery. Since we said we wanted jQuery support, I need to load that first. I have my Greetr.js file, which will be my framework slash library. That's where I'm gonna put my code, and an app.js file we'll use our frameworks and libraries.

* First of all we want to create a new execution context for our entire framework/library. So all of our variables declared are safe. And we're only exposing on the global object what we want.So how do we do that?

* Wrap up some code in the way that you think would be appropriate, in order to make any code inside of it safe.

* And pass to it what we need access to. We need access to the global variable which is Window and the jQuery variable which is either the word jQuery or a dollar sign.

```js
// Greeter.js
(function(global, $) {
    
    
    
}(window, jQuery));
```
* So we're going to create our immediately invoked function. I can't just do function. I have to do a parentheses or something like that to trick the syntax parser.

* And then I'm going to invoke it with parentheses. I'm gonna pass a global object and in jQuery which I'll just make a dollar sign as function alise param.

* And when I invoke it (IIFE), I pass in my global object (window) and my jQuery object as param in bottom invoke paranthesis, which I could either put dollar sign or jQuery right here.Make sense?

* So now I have safe code. I've structure around the code that I'm going to write.

* So the first thing that happens is it executes this above code, creating a new execution context giving me the variables I need to run.

* And so now my whole greeter is safe inside of here and ready to be reused by anybody. All right, pretty simple start

### Our Object and Its Prototype

* Now it's time to set up our Greetr object and this will be a bit tricky because we want to set it up the way that jQuery is set up.

* We're going to imitate jQuery's structure.

* What I want to do is set up my Greetr so that it generates an object so it'll be a function, a function that generates an object.

* But what I'd like to do whenever I use Greetr is I'd like to just say G$, and then pass firstName, lastName, and maybe language, and that's it. And what it gives me back is an object, kind of like jQuery does.

```js
// something like this 

var g = G$(firstname, lastname, language); // So, I don't want to have to say new all the time.
```
* So, let's do that now.

* How can I set up a function that does this?

```js
var Greetr = function(firstName, lastName, language) {

    // And now I want it, instead of being a function constructor, so I have to use new, 
    // I want it to return the results of a different function constructor.Maybe like a .init, just like jQuery did.
        
    return new Greetr.init(firstName, lastName, language);   

    // So it's return new, so I'm going to return and then use a function constructor to generate the object.
    // That way I don't have to always setup the object with the new keyword.
}

// this is the actual function.
// And notice that it's okay that I'm setting this up after Greetr (above) because this (above function expression) won't be called until I actually we use Greetr.

Greetr.init = function(firstName, lastName, language) {

        // All right, so I'm going to set up some default properties.

        // How can I set up my new object, because that's what I'm doing?

        // I'm doing a function constructor so I'm building an object, building this new object that's going to be returned by the greeter function.

        var self = this;
        // now self(ie this keyword) points to the empty object created by the new operator.
        // in that empty object we are going to setup firstName, lastName and language
        self.firstName = firstName || '';
        self.lastName = lastName || '';
        self.language = language || 'en';

}
// So by the time this (var Greetr function expression) is actually called, this will all be set up. All this other code will be already run.

// I have a function constructor that builds an object and gives it three properties and sets its value if you pass something into the function constructor, otherwise set some defaults.

// Now what about the prototype?

Greetr.prototype = {};

// Well I want to use Greetr.prototype because it looks nicer in my code and that's just gonna be an empty object for now.

// And here (prototype object) is where I'll put any methods that I want to use inside my object that's returned from Greetr.

// object created from Greetr.init needs to point/reference to this (Greetr.prototype) as its prototype.

// ie Greetr.prototype should assigned to all object created form Greetr.init as its prototype

Greetr.init.prototype = Greetr.prototype;

// It just says any objects created with this (Greetr.init) function should actually point here (Greetr.prototype) for its prototype chain.

// All right now I want to expose my Greetr to the outside world. I want to attach it to my global object so that I can call this function anywhere. Because it's sitting on the global object.

// We're not gonna worry about checking to see whether it exists or not. We're just going to attach it to the global object.

// And oh, we also want that nice alias, G$, so I don't have to type Greetr all the time when I'm using this. How do we do that? How do we add it to the global object?

global.Greetr = global.G$ = Greetr;

// I'm exposing the Greetr function, so it's just are assign Greetr function (top one) to global.Greetr and global.G$
```

* So I have the basic setup of my object and my prototype. So if I go to my app, I can  say

```js
var gre = G$( 'Guna', 'M', 'tamil');
console.log(gre) // Greeter.init  { firstName : 'Guna', lastName : 'M', language : 'tamil'}

// because just like jQuery, I'm returning a new Greetr.init.
```
* Now we have to add different methods to our prototype.

### Properties and chainable methods

* All right, so let's add some functionality. Let's add some properties, and some chainable methods to our object,And we'll also set up some setup features that are within the greeter but not exposed to the outside world.

* So I've already set up my object in here (Greetr.init) where I'm building my object and my function constructor, the one that I'll actually be using out in my app.js file.

* So, in order to save memory space, where should I put any methods and other properties that would be shared by all of the objects generated here?

* Where should I put it? I could put self dot and add methods here(inside Greetr.init), but it's better to put it where? 

* On prototype because our Greetr.init.prototype is already pointing to Greetr.prototype, and also adding methods to prototype will save memory space it wont create new instance for every new object. it will reference to the single memory space.

* And what about things that I want to use in the logic of this entire framework/library, but I don't want it to be exposed to the outside world at all?

```js 
var supportedLangs = ['en', 'es']; // this will never get exposed to the outside world this is only for our internal useage

// Right? It's not a property, it's not a method of the object being returned. It's inside this memory space of this function.

// However, I can use it inside my object, because of what? Why is it that the object that's returned, well then any methods on that object created here(inside Greetr.init) would have access to this variable. Why would any objects created here(inside Greetr.init) have access to these variable?

// Because this object's lexical environment is this whole function. And so thanks to closures it'll close in this variable even when this immediately invoked function is done running.

// So it will have access to these variables. But they're hidden to many other developers from changing them without coming into the source code itself.

    var greetings = { // this too will never get exposed to the outside world 
        en: 'Hello',
        es: 'Hola'
    };
    
    var formalGreetings = { // this too will never get exposed to the outside world 
        en: 'Greetings',
        es: 'Saludos'
    };
    
    var logMessages = { // this too will never get exposed to the outside world 
        en: 'Logged in',
        es: 'Inició sesión'
    };

    // All right, so I have three sets of messages, but they're objects, not arrays, because I want to reference them by the name/value pair, by the name of the property. And this will let me do this dynamically very easily.


    
    
    Greetr.prototype = {
        // Let's add some things that will be exposed inside this prototype object.

        fullName: function() {
            return this.firstName + ' ' + this.lastName;   
        },
        
        // I'm using object literal syntax to create a method called fullName with this function expression to define it. Then a comma, right, because I'm adding properties and methods in object literal syntax. So I separate them with a comma.

        validate: function() {
            // I set up a hidden variable here that'll be hidden to the outside world but accessible via the closure called supported languages.

            // this.language - Remember this, the keyword this, will point to the object  (Greetr.init) that's calling this function.

            // And the object will store its own language that we're requesting. And I'm gonna see if it's found or not.if not throw error

            if (supportedLangs.indexOf(this.language)  === -1) {
                throw "Invalid language";   
            }
        },
        
        greeting: function() {
            return greetings[this.language] + ' ' + this.firstName + '!';
        },
        
        formalGreeting: function() {
            return formalGreetings[this.language] + ', ' + this.fullName();  
        },

        // All right, so now I want some methods that I can really use and be chainable. Remember we saw chainable methods in jQuery?
        
        greet: function(formal) {
            var msg;
            
            // if undefined or null it will be coerced to 'false'
            if (formal) {
                msg = this.formalGreeting();  
            }
            else {
                msg = this.greeting();  
            }

            if (console) {
                console.log(msg);
            }

            // 'this' refers to the calling object at execution time
            // makes the method chainable
            return this;
        },
        
        log: function() {
            // I'll log it to the console, Internet Explorer actually doesn't have a console variable unless its console is open.

            // So I'm going to make sure that it is by just saying if console. That's an object.

            if (console) {
                console.log(logMessages[this.language] + ': ' + this.fullName()); 
            }
                            
            return this;
        },
                            
        setLang: function(lang) {
            this.language = lang;
                    
            this.validate();
            
            return this;
        }
        // So I'm just creating properties and methods along the way and each one of these is returning the this variable so that they're chainable.
    };
```
* All right. Let's try using this.

* all of these methods, inside a single object literal. See that? One giant object literal that's the prototype of all of these methods defined on it.

* I've got my object returned,

```js
var g = G$('John', 'Doe');
g.greet().setLang('es').greet(true); 

// Response

// Hello John!

//####### in between language swaped to spanish

// Saludos, John Doe
```
*  G$ points to Greetr( top )function than inturn return  Greetr.init function (which is defined down) which builds the object, sets the values. Greetr.init.prototype  makes sure that all of those objects created from here (Greetr.init) has access to all of these methods on Greetr.prototype property., 

###  Adding jQuery Support

* We've seen one of our requirements then is now that we have a greeting object that I can output a greeting based on a language and whether it's formal or not. I'd like to actually be able to use it. 

* One way would be to simply use the greeting or formal greeting methods on the object and manually update my web application, for example. But I'd like to add a bit of jQuery support to make it easier for any developers using this. They could just give it a selector and the selector would then be used to create a jQuery object and then we fill the text of that element.

```html
<html>
    <head>
        
    </head>
    <body>
        <div id="logindiv">
            <select id="lang">
                <option value="en">English</option>
                <option value="es">Spanish</option>
            </select>
            <input type="button" value="Login" id="login" />
        </div>
        <h1 id='greeting'></h1> ***** to draw greeting message form our framework ***
        <script src="jquery-1.11.2.js"></script>
        <script src="Greetr.js"></script>
        <script src="app.js"></script>
    </body>
</html>
```

* Now I'd like to add a method that accepts a jQuery selector. And then updates whatever the selector is.

```js
// Greetr.js - Greet prototype

HTMLGreeting: function(selector, formal) {
    if (!$) {
        throw 'jQuery not loaded';   
    }
    
    if (!selector) {
        throw 'Missing jQuery selector';   
    }
    
    var msg;
    if (formal) {
        msg = this.formalGreeting();   
    }
    else {
        msg = this.greeting();   
    }
    // here we are using jquery which is available globally.
    $(selector).html(msg);
    
    return this; // And then I'm making it chainable.
}
```
* So I have a jQuery method that accepts a selector, a jQuery selector and whether or not it's a formal greeting and sets up the greeting itself. And then updates whatever value is there.

### Good Commenting

```js
(function(global, $) {
    
    // 'new' an object
    var Greetr = function(firstName, lastName, language) {
        return new Greetr.init(firstName, lastName, language);   
    }
    
    // hidden within the scope of the IIFE and never directly accessible
    var supportedLangs = ['en', 'es'];
    
    // informal greetings
    var greetings = {
        en: 'Hello',
        es: 'Hola'
    };
    
    // formal greetings
    var formalGreetings = {
        en: 'Greetings',
        es: 'Saludos'
    };
    
    // logger messages
    var logMessages = {
        en: 'Logged in',
        es: 'Inició sesión'
    };
    
    // prototype holds methods (to save memory space)
    Greetr.prototype = {
        
        // 'this' refers to the calling object at execution time
        fullName: function() {
            return this.firstName + ' ' + this.lastName;   
        },
        
        validate: function() {
            // check that is a valid language
            // references the externally inaccessible 'supportedLangs' within the closure
             if (supportedLangs.indexOf(this.language)  === -1) {
                throw "Invalid language";   
             }
        },
        
        // retrieve messages from object by referring to properties using [] syntax
        greeting: function() {
            return greetings[this.language] + ' ' + this.firstName + '!';
        },
        
        formalGreeting: function() {
            return formalGreetings[this.language] + ', ' + this.fullName();  
        },
        
        // chainable methods return their own containing object
        greet: function(formal) {
            var msg;
            
            // if undefined or null it will be coerced to 'false'
            if (formal) {
                msg = this.formalGreeting();  
            }
            else {
                msg = this.greeting();  
            }

            if (console) {
                console.log(msg);
            }

            // 'this' refers to the calling object at execution time
            // makes the method chainable
            return this;
        },
        
        log: function() {
            if (console) {
                console.log(logMessages[this.language] + ': ' + this.fullName()); 
            }
            
            // make chainable
            return this;
        },
                            
        setLang: function(lang) {
            
            // set the language
            this.language = lang;
        
            // validate
            this.validate();
            
            // make chainable
            return this;
        },
        
        HTMLGreeting: function(selector, formal) {
            if (!$) {
                throw 'jQuery not loaded';   
            }
            
            if (!selector) {
                throw 'Missing jQuery selector';   
            }
            
            // determine the message
            var msg;
            if (formal) {
                msg = this.formalGreeting();   
            }
            else {
                msg = this.greeting();   
            }
            
            // inject the message in the chosen place in the DOM
            $(selector).html(msg);
            
            // make chainable
            return this;
        }
        
    };
    
    // the actual object is created here, allowing us to 'new' an object without calling 'new'
    Greetr.init = function(firstName, lastName, language) {
        
        var self = this;
        self.firstName = firstName || '';
        self.lastName = lastName || '';
        self.language = language || 'en';
        
        self.validate();
        
    }
    
    // trick borrowed from jQuery so we don't have to use the 'new' keyword
    Greetr.init.prototype = Greetr.prototype;
    
    // attach our Greetr to the global object, and provide a shorthand '$G' for ease our poor fingers
    global.Greetr = global.G$ = Greetr;
    
}(window, jQuery));
```
* I even did a check of the code while I was at it, and I had missed something. Did you notice? I didn't call the validate when you initially create the object. I did when you set the language later in the chainable function,but not when you actually create it.

### Let's Use Our Framework

```js
;(function(global, $) {
    
    // 'new' an object
    var Greetr = function(firstName, lastName, language) {
        return new Greetr.init(firstName, lastName, language);   
    }
    ...
```
* By the way, if you ever see a library that does this, puts a semicolon before it, that's another trick.

* Just in case there's some other code, some other library that may be injected before the Greetr.js in a script file, maybe above it, that doesn't quite finish out its semicolons properly.

* You can also put a semicolon there to make it more completely useful in that even if the other code doesn't finish its semicolons out properly, a code that's used above here, your code will still run fine.I just wanted to make mention of that because you will see it sometimes used.

* And remember that the HTML greeting method already sets up the jQuery object. You just need to give it the selector.

* Below code is self explainable

```js
// gets a new object (the architecture allows us to not have to use the 'new' keyword here)
var g = G$('John', 'Doe');

// use our chainable methods
g.greet().setLang('es').greet(true).log();

// let's use our object on the click of the login button
$('#login').click(function() {
   
    // create a new 'Greetr' object (let's pretend we know the name from the login)
    var loginGrtr = G$('John', 'Doe');
    
     // hide the login on the screen
    $('#logindiv').hide();
    
     // fire off an HTML greeting, passing the '#greeting' as the selector and the chosen language, and log the welcome as well
    loginGrtr.setLang($('#lang').val()).HTMLGreeting('#greeting', true).log();
    
});


```

## Bonus 

### TypeScript, ES6, and Transpiled Languages

* We talked about features that are coming that not every browser will support yet. And yet people are so inclined to use these new features or add on these features that JavaScript doesn't have that we have something out there called Transpiled Languages.

* To transpile means to convert the syntax of one programming language to another.

* we're talking about languages that don't ever actually run anywhere. There's no engine that runs them. Instead, they're processed by a transpiler and all that means is it generates JavaScript.

* So you write in one language and what you actually put out there is just pure JavaScript that runs in a JavaScript engine just like we've talked about this whole course.

* The first one that's already quite popular and will continue to be is called TypeScript and it's provided by Microsoft. You can go to typescriptlang.org.

* The biggest thing about TypeScript is that it provides types for your variables. So if you really, really wish JavaScript had types strongly type instead of dynamically type that is, that you could say what type a variable should be, you can do it in TypeScript.

* It also has things like class and some other features that are coming in ES6, so you can write your entire application in TypeScript,transpile it to normal JavaScript and it will work on a whole bunch of browsers. So, that's TypeScript.

* The other one that's out here on GitHub is called Traceur.And it lets you write ES6 today. In other words, the features of the upcoming version of JavaScript but it transpiles that into normal standard ES5 JavaScript, that is what's there now.

* So this is another transpiled language that, for example, if you have a project starting right now, a big JavaScript project, and you know it's gonna be a couple years long you might wanna consider using Traceur. You might wanna start writing in the next version of JavaScript.

* And for now it'll just convert down to the current version of JavaScript and then in a couple years you can stop using Traceur and just directly send your code out to production

* But remember, you shouldn't use a transpiled language like Traceur or JavaScript transpiler type script without understanding the code that it's creating for you otherwise you'll get in a lot of trouble.

* For more on Typescript head to: http://www.typescriptlang.org

* and try out writing Typescript code in your browser here: http://www.typescriptlang.org/Playground

* For more on Traceur head to: https://github.com/google/traceur-compiler

* and try out writing ES6 code in Traceur in your browser here: https://google.github.io/traceur-compiler/demo/repl.html#

### Getting Ready for ECMAScript 6

* To read a list of features existing or coming in ES6, head here: https://github.com/lukehoban/es6features