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



