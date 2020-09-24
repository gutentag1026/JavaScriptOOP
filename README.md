# JavaScriptOOP


## object Literals
```
const circle = {
    radius: 1,
    location: {
        x:1,
        y:1
    }
    draw: function(){

    }
};
```

## Factory
```
function createCircle(radius){
    return {
        radius,
        draw: function(){
            console.log('draw');
        }
    };
}

const circle = createCircle(1);
circle.draw();
```
## constructor 
```
function Circle(radius){
    this.radius = radius;
    this.draw = function(){
        //do sth
    }
}
const another = new Circle(1);//1.create a object{} 2.this refers to the object instead of referring to window(global object)

let x= {}
// let x = new Object();
//new String();  '', "" new Boolean(); new Number(); // 1, 2, 3
```
## values VS reference types
```
let x = 10;
let y = x;

y = 20; // x = 10 y=20

// primitives are copied by their value, Objects are copied by their reference.
let x = { value: 10 }
let y = x;
x.value = 20; // x { value: 20} y { value: 20}
```
Value Types | Reference type
------------ | -------------
number | Object 
string | Function 
Boolean | Array
Symbol |
undefined | 
null
```
let number = 10;

function increase(number){
    number++;
}
increase(number); // number 10

let obj = { value: 10 }
function increase(obj){
    obj.value++;
}
increase(obj); //{ value: 11 }
```
## Add and delete properties
```
function createCircle(radius){
    return {
        radius,
        draw: function(){
            console.log('draw');
        }
    };
}

const circle = createCircle(1);
circle.location = { x: 1 };
circle['location'] = { x: 1 };
delete circle.location;
delete circle['location'];
```
## enumerate properties
```
function Circle(radius){
    this.radius = radius;
    this.draw = function(){
        //do sth
    }
}
const another = new Circle(10);

for (let key in circle) {
    if (typeof cirlce[key] !== 'function')
        console.log(key, circle[key]);
}

const keys = Object.keys(circle); //["radius", "draw"]
if ('radius' in circle)
  console.log("Circle has a radius");
```
## abstration hide details show essentials
```
function Circle(radius) {
    this.radius = radius;
    let computeOptimumLocation = function(factor){

    }
    let defaultLocation = { x: 0, y: 0 }; //change this to let to be private
    
    this.getDefaultLocation = function() {
        return defaultLocation;
    }
   
    this.draw = function(){
        let x, y; //scope is temporary....finish after executing within draw
        computeOptimumLocation(0.1);
        
    };

}

const circle = new Circle(10);
circle.draw();
```
## Getters and Setters
```
function Circle(radius) {
    this.radius = radius;
   
    let defaultLocation = { x: 0, y: 0 }; //private cannot access outside 
    
    this.getDefaultLocation = function() { //accessible
        return defaultLocation;
    }
   
    this.draw = function(){
    };

    Object.defineProperty(this, 'defaultLocation', {
        get: function(){ //READONLY
            return defaultLocation;
        },
        set: function(value) {
            if (!value.x || !value.y) 
                throw new Error('Invalid location.');
            defaultLocation = value;
        }
    });

}

const circle = new Circle(10);
circle.getDefaultLocation();//method 1 to access private var
circle.defaultLocation//method 2 to access private var READONLY
```
```
function Stopwatch() {
    let start, end, duration, running = 0;

    this.start = function () {
        if (running)
            throw new Error('Stop watch has already started.'); 
        running = true;
        start = new Date();
    }
    this.stop = function () {
        if (!running)
            throw new Error("Stop watch hasn't started."); 
        running = false;
        end = new Date();
        const seconds = (end.getTime() - start.getTime()) / 1000;
        duration += seconds;
    }
  
    this.reset = function () {
        start = null;
        end = null;
        duration = 0;
        running = false;
    }

    Object.defineProperty(this, 'duration', {
        get: function(){ //READONLY
            return duration; 
        }
    });
}
```

## Inheritance
classical | prototype
```
Object.getPrototypeOf(myObj);
```
## property descriptor
```
let person = { name: 'Mosh' };
for (let key in person)
    console.log(key); //name
Object.keys(person); // ["name"]
let objectBase = Object.getPrototypeOf(person); // myObj.__proto__(parent of myObj)
let descriptor = Object.getOwnPropertyDescriptor(objectBase, 'toString');
console.log(descriptor);
// {value: f, writable: true, enumerable: false, configurable: true}
// configurable: true enumerable: false value: f toString() writable: true __proto__: Object

objectBase.defineProperty(person, 'name', {
    writable: false, // cannot delete name 
    enumerable: true,
    configurable: false
});

delete person.name;
```
## ProtoType vs Instance memeber
```
function Cirlce(radius) {
    //instance member
    this.radius = radius;

    this.move = function () {
        this.draw(); // prototype and instance member inherite each other
        console.log('move');
    }
}
Cirlce.prototype.draw = function() {
    //this.move();
    console.log("draw");
}

Cirlce.prototype.toString = function () {
    return 'Circle with radius ' + this.radius;
}
const c1 = new Cirlce(1);
c1.move() // draw move
const c2 = new Cirlce(1);

console.log(Object.keys(c1)); //["radius", "move"]
for (let key in c1) console.log(key); // radius, move, draw
c1.hasOwnProperty('radius') // true
c1.hasOwnProperty('draw') // false
```
// don't modify objects you don't own   
## create your own prototypes inheritance
```
function Shape() {
} 

Shape.prototype.duplicate = function() {
    console.log('duplicate');
}

function Circle(radius) {
    this.radius = radius;
} 

Circle.prototype.draw = function() {
    console.log('draw');
}

const s = new Shape(); // s -> shapeBase(Shape.prototype) -> objectBase
const c = new Circle(1);
```
circle inheritate shape
```
Circle.prototype = Object.create(Shape.prototype); 
```
## Resetting the constructor: reset the prototype, reset the constructor
```
new Circle.prototype.constructor(1);
new Circle(1);
Circle.prototype.constructor = Circle; 
```
## Calling the super constructor
```
function Shape(color) {
this.color = color;
} 

Shape.prototype.duplicate = function() {
    console.log('duplicate');
}

function Circle(radius, color) {
    Shape.call(this, color);
    this.radius = radius;
} 

Circle.prototype.draw = function() {
    console.log('draw');
}

const s = new Shape(); // s -> shapeBase(Shape.prototype) -> objectBase
const c = new Circle(1);
```
## Intermediate function Inheritance
```
function extend(Child,Parent){
    Child.prototyp.constructor = Object.create(Parent.prototype);
    Child.prototype.constructor = Child;
}

extend(Circle, Shape);
```
## Method overwriting
```
function extend(Child,Parent){
    Child.prototyp.constructor = Object.create(Parent.prototype);
    Child.prototype.constructor = Child;
}
function Shape() {
} 
Shape.prototype.duplicate = function() {
    console.log('duplicate');
}
function Circle() {
}
extend(Circle, Shape);
Circle.prototype.duplicate = function() {
  Shape.prototype.duplicate.call(this); //without this line it would only call 'duplicate cirlce'
  console.log('duplicate cirlce');
}
c.duplicate() //first call: 'duplicate' second call: 'duplicate cirlce'
```
## Polymorphism
```
function Shape(){}
Shape.prototype.duplicate = function(){ console.log('duplicate') };
function Circle(){};
extend(Circle, Shape);
Circle.prototype.duplicate = function(){ console.log('duplicate circle') };
function Square(){};
extend(Square, Shape);
Square.prototype.duplicate = function(){ console.log('duplicate Square') };

const shapes = [ new Circle(), new Square()];
for (let shape of shapes)
    shape.duplicate();//'duplicate circle' 'duplicate Square'
```
## Mixins
``` 
function mixin(target, ...sources){
    Object.assign(target, ...sources);
}
const canEat = {
    eat: function(){
        this.hunger--;
        console.log('eating');
    }
};
const canWalk = {
    walk: function(){
        console.log('walking');
    }
};
const canSwim = {
    swim: function(){
        console.log('Swimming');
    }
};
function Person(){}
Object.assign({Person.prototype}, canEat, canWalk); 
const person = new Person();
console.log(person);//__proto__: eat:f walk:f constructor: f Person() __proto__:Object
function Goldfish(){}
mixin(Goldfish.prototype, canEat, canSwim); const goldfish = new Goldfish();
```
