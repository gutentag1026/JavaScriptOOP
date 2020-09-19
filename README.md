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
## abstration
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
        let x, y; //scope is temporary....finish after executing
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
