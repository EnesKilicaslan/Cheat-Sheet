# JS-Good-Parts-Notes

- NaN is number value that is the result of an operation that cannot produce a normal result. NaN is not equal to any value, including itself. I can be detected is isNan(number) function.
- Strings are immutable
- There is no char type, instead one element string can be created
- Two strings containing exactly the same characters in the same order are considered to be the same string. So;
```javascript
'c' + 'a' + 't' === 'cat' //is true
```

- Unlike many other languages, blocks in javascript do not craete new scope, so variables should be defined at the top of the function, not in blocks.
- the following values are falsy for statements (like if statement), all other values are considered truthy.
    - null
    - false
    - undefined
    - Nan
    - the number 0
    - the empty string ''

- When using *for in*, it is usually necesseray to test ```object.hasOwnProperty(variable)``` in the code to determine wheather the property name is truly a member of the object or it is coming from prototype chain.
    ```javascript
        for(variable in obj){
            if(obj.hasOwnProperty(variable)){
                ....
            }
        }
    ```
- If in a function, a return expression did not specified then the return value will be ```undefined```
- The values produced by ```typeof``` are string, number, boolean, undefined, function and object. (If the operand is an array or null then the result is object which is wrong)
- The '/' operator can produce non-integer values even if both operands are integer.
- The simple types of objects are numbers, strings, booleans, null and undefined. All other values are objects.
- Numbers, strings and booleans are object-like in that they have methods, but they are immutable.
- Object in JavaScript are mutable keyed collections. Arrays, functions, regular expressionsa are objects.
- Undefined value is returned if an attempt is made to retrieve a nonexistent member form an object.
- The ```||``` operator can be used to fill in default values: ``` var middle = stoga.noexist || "unknown"```
- Attempt to retrieve value from an undefined will throw TypeError exception. This can be guared wit ```&&``` opetrator: ```stage.nonexist && stage.nonexist.a```
- Objects are passed around by Reference, they are never copied
- Every object is linked to a prototype object from which it can inherit properties.
- The prototype link is used ony in retrieval.
- Prototype relationship is a dynamic relationship. If we add a new property to prototype, then it will immediately be visible in all of the objects that are based on that prototype.
- ```delete``` operator can be used to remove a property from an object
- One way to minimize the use of global variables is to create a single global variable for the application, 
```javascript
MY_APP = {}
MY_APP.stooge = {
    name: "enes",
    age: 20
}

```
- A function encloses a set of statements. Functions in JavaScript are objects and Objects are collections of name-value pairs having a hidden link to a prototype object.
- Objects that are produced from Object literals are linked to Object.prototype, Function Objects are linked to Function.prototype.
- Functions can be stored in variables, objects and arrays. Functions can be passed as arguments to functions and can be returned from functions. Also, since functions are objects, functions can have methods. The thing about functions is that they can be invoked.
- Function objects are created with function literals:
```javascript
// create a variable named add and store a function in it that adds two numbers
var add = function(a, b){
    return a+b;
}
```
- If a function is not given a name, then it is called anonymous.
- Function literals can appear anywhere an expression can appear. Functions can be defined inside of other functions. The function object created by function literal contains a link to that outer context, this is called **closure**.
- Invication pattern determine the value of ```this```.
- When a function is defined as a property of an object, then it is invoked as method. A method can use ```this``` to access the object, the binding of ```this``` to the object happens at invocation time. Methods that get their object context from this are called *public methods*.
```javascript
var myObject = {
    value: 0,
    increment: function(i){
        this.value += typeof i === "number" ? i : 1;
    }
}
```
- When a function is not a property of an object, the it is invoked as function. ```this``` is bounded to the global object. And this is an error in the language. As a consequence of this error, a method can not employ an inner function because the inner function does not share the method's access to the object as its this is bound to the wrong value. But a workaround is saving ```this``` value in a variable.
```javascript
var myObject.double = function(){
    var me = this;

    var helper = function(){
        me.value = add(me.value, me.value) 
    }
}
```
- JavaScript is a protptypal language, that means objects can inherit properties directly from objects. Functions that are intended to be used with ```new``` prefix are called constructors. The ```new``` prefix changes the behaviour of the return statement. If a function is invoked with the ```new``` prefix, a new object with a hidden link to the value of the function's prototype member is created and ```this``` will be bound to that object.

```javascript
var Que = function(string){
    this.state = string
}

Obj.prototype.get_status = function(){
    return this.state
}

var obj = new Que("ss");
```
- Apply method is used to make Apply invocation Pattern. The apply method takes two parameters; first is the value that should be bound to ```this``` and the second is an array of parameters
```javascript

var otherStatusObj = {
    stattus: "other"
}

var status = Que.prototype.get_status.apply(otherStatus, [])

```

- A bonus parameter that is available to functions when they are invoked is the ```arguments``` array, actually it is not an array but an object-like array. It has length property but lacks other properties.
- If the return value of a functions is not specified, then ```undefined``` is returned.
-

```javascript
// walk the dom function

var walk_the_dom = function walk(node, func){
    func(node);
    node = node.firstChild;

    while(node){
        walk_the_dom(node);
        node = node.nextSibling;
    }
}

// define your own getElementsByAtteribute function

var getElementsByAttribute = function (attr, value){
    var elements = [];
    
    var addToElements = function (node){
        if(node.getAttribute(attr) === value)
            elements.add(node);
    }

    walk_the_dom(root, addToElements);
}

```
- javascript does not have tail recursion optimization
- javascript does not have block scope, it does have function scope. That means that the parameters and variables in a function are not visible from outside and a variable that is defined anywhere inside of the function is accessible within the function.

```javascript
var foo = function(){
    var a=5, b=7;

    var bar = function(){
        var a=10, b=8, c=2;
        // a:10, b:8, c:2

        a += b + c;
        // a:20, b:8, c:2
    }
    // a:5, b:8, c:undefined

    bar();

    // a: 20, b: 8
}
```
- lets create an object private ```status``` property and public ```get_status``` method
```javascript
var que = function(status){
    return {
        get_status: function(){
            return status;
        }
    }
}

var myQue = que("running");
```
- callback is a function that is passed to an asynchronous function to be called later.
- A module is a function or an object that presents an interface and hides its state and implementation.

```javascript
//closure example
var serial_maker = function ( ) {
// Produce an object that produces unique strings. A
// number, and a gensym method that produces unique
// strings.
    var prefix = "";
    var seq = 0;

    return {
        set_prefix: function (p) {
            prefix = p;
        },
        set_seq: function (s) {
            seq = s;
        },
        gensym: function ( ) {
            var result = prefix + seq;
            seq += 1;
            return result;
        }
    };
};

var seqer = serial_maker( );
seqer.set_prefix("Q");
seqer.set_seq(1000);

console.log(seqer.gensym());
```
- use of module pattern can eliminate the use of global variables. It promotes information hiding and other good design practices.
- some methods don't have return values, if we return ```this``` instead of ```undefined```, then we can enable **cascading**.
- functions are values and we can manipulate functions by combining the function and arguments, this is called **currying**
```javascript
var add1 = add.curry(1);
add1(6) // produces 7
```

JavaScript does not have curry method, but we can easily create one.

```javascript
Function.method('curry', function (){
    var args = arguments, that = this;

    return function (){
        return that.apply(null, args.append(arguments));
    }
})
```
- functions can use objects to remember the result of previous operations in order to avoid redundant calculations. This optimization is called ***memoization***

```javascript
var fibonacci = function (n){
    return n < 2 ? n : fibonacci(n-1) + fibonacci(n-1)
}

var fibonacci = function(){ //memoized version
    var memo = [0,1];

    var fibo = function(n){
        if(!memo[n])
            return memo[n]
        else{
            var res = fibo(n-1) + fibo(n-2);
            memo[n] = res;
            return res;
        }
    }

    return fibo;
}();

fibonacci(5);
```
- JavaScript is a prototypal language, which means that object can inherit directly from other objects.
- When a function object is created, the function constructor that produces the function object runs some code like this.
```javascript
this.prototype = {constructor: this};
```
- An array literal is a pair of square brackets surrounding zero or more elements seperated by commas.
- length property is largest integer integer property in the array plus one.
- length property can be set explicitely
- delete method leaves a whole in te array
- enumaration via ```for in``` statement can be done on arrays but it does not gurantie the order and some unexpected properties be dredged up from prototype chain.
- when property names are small sequential integers, then array should be used, otherwise objects.
- The array methods are functions stored in ```Array.prototype```which can be augmented like following
```javascript

Array.nethod('reduce', function(f, value){
    var i;

    for(i=0; i<this.length; i += 1){
        value = f(this[i], value);
    }
    return value;
})

```

- Regular expression cheatsheet:
    - '/^': indicates starting of the expression
    - '$/': indicates end of the expression
    - '.': any character except line ending character
    - '?:' non capturing group
    - (...) capturing group
    - '+': one or more times
    - '.': zero or more times
    - '?': optional
    - '^': not
    - '|': choice operator
    - '{a,b}': will be matched from a to b times

- Even though there is a high compatibility between JS processors, regular expressions are the least portable implementation.
- '/.../i', the i flag means igonre case
- it is better to use non-capturing group instead of less ugly capturing group, because capturing groups have performance penalty.
- Regular expression literals are enclosed by slashes ```/ ... /```
- Beside literals, regular expressions can be created by **RegExp** constructor and it is very useful to create regular expressions in run time.
- RegExp objects made by regular expression literals share a single instance.
- '\d' is same as [0-9] and '\D\ is opposite, [^0-9]
- '\w' is same as [0-9A-Z_a-Z] and '\W' is opposite
- '\1' is the text that was captured by group 1 so that it can be matched again.
- '?' is same as  {0,1}, '*' is same as {0,} '+' is same as {1,}
