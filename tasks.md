# **JavaScript Basics Tasks and Results**

## **Day 1: Arrays and Loops**

### Easy Task: Print each movie in an array of favorite movies.

**Description:** This task involves creating an array of favorite movies and using a for loop to print each movie.

```javascript
const movies = ["Inception", "The Matrix", "Interstellar"];
for (let movie of movies) {
    console.log(movie);
}



```
### Medium Task: Use map to create an array of movie titles in uppercase.

```javascript

const movies = ["Inception", "The Matrix", "Interstellar"];
const upperCaseMovies = movies.map(movie => movie.toUpperCase());
console.log(upperCaseMovies);

```
#### Results = ["INCEPTION", "THE MATRIX", "INTERSTELLAR"]

### Hard Task: Use reduce to count the total number of characters in all movie titles combined.


```javascript
const movies = ["Inception", "The Matrix", "Interstellar"];
const totalCharacters = movies.reduce((acc, movie) => acc + movie.length, 0);
console.log(totalCharacters);

```
# Result  = 31


## **Day 2: Functions**
### Easy Task: Create a function that takes a name and age as arguments and returns a greeting message .

```javascript 
function greet(name, age) {
    return `Hello, my name is ${name} and I am ${age} years old.`
    }
    console.log(greet("John", 30));

```
### Medium Task: Create a function that takes an array of numbers and returns the sum of all numbers    
```javascript
let arr = [2,6,7,8,9,561,78]
function sumNumbers(numbers) {  
    return numbers.reduce((acc, number) => acc + number, 0);
}

```
### Hard Task: Create a function that takes an object with name and age properties and returns an object
### with the name and age properties in uppercase and a new property called "ageGroup" with the value
### "adult" if the age is 18 or more, "teen" if the age is 
### between 13 and 17, and "kid" if the age is less than 13

```javascript
    function processPerson(person) {
        const { name, age } = person;
        const ageGroup = age >= 18 ? "adult" : age >= 13 ? "
        return { name: name.toUpperCase(), age: age.toString(), ageGroup };
        }
        const person = { name: "John", age: 25 };
        console.log(processPerson(person));

```

## **Day 3: Objects and Destructuring** 
### Easy Task: Create an object with the following properties: name, age, occupation, and address
### and then use destructuring to extract the values of these properties into separate variables.

```javascript 

const person = {
    name: "John",
    age: 30,
    occupation: "Developer",
    address: "123 Main St"
    }
    const { name, age, occupation, address } = person;
    console.log(name, age, occupation, address);


```


###  Medium Task: Create an object with the following properties: name, age, occupation, and address
### and then use destructuring to extract the values of these properties into separate variables.
### Then, use the variables to create a new object with the same properties but with the values in
### uppercase.

```javascript
const person = {
    name: "John",
    age: 30,
    occupation: "Developer",
    address: "123 Main St"
    }
    const { name, age, occupation, address } = person;
    const newPerson = {
        name: name.toUpperCase(),
        age: age.toString(),
        occupation: occupation.toUpperCase(),
        address: address.toUpperCase()
        };
        console.log(newPerson);

```
### Hard Task: Create an object with the following properties: name, age, occupation, and address
### and then use destructuring to extract the values of these properties into separate variables.
### Then, use the variables to create a new object with the same properties but with the values in
### uppercase. Finally, use the new object to create a new array with the values of the properties
### in the following order: name, occupation, address, age.

```javascript
const person = {
    name: "John",  
    age: 30,
    occupation: "Developer",
    address: "123 Main St"
    }
    const { name, age, occupation, address } = person;
    const newPerson = {
        name: name.toUpperCase(),
        age: age.toString(),
        occupation: occupation.toUpperCase(),
        address: address.toUpperCase()
        };
        const newArray = [newPerson.name, newPerson.occupation, newPerson.address, new
        Person.age];
        console.log(newArray);

```

# **Day 4: Hoisting, Closure, and More**
## Hoisting

### Easy Task : Demonstrate hoisting with a variable.


```javascript
console.log(x); // undefined
var x = 10;
console.log(x); // 10

``` 
### Medium Task: Create a closure to keep track of how many times a function is called.
```javascript
function counter() {
    let count = 0;
    return function() {
        count++;
        return count;
        }
        }
       console.log(counter()); //1
       console.log(counter()); //2
       console.log(counter());//3
```


### Hard Task: Demonstrate hoisting with a function.
```javascript

console.log(add(5, 10)); // 15
function add(a, b) {
    return a + b;
    }
    console.log(add(5, 10)); // 15

```
### Hard Task: Implement a simple prototype chain.

```javascript
function Person(name) {
    this.name = name;
}
Person.prototype.greet = function() {
    console.log(`Hello, my name is ${this.name}`);
};

function Student(name, course) {
    Person.call(this, name);
    this.course = course;
}
Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;
Student.prototype.study = function() {
    console.log(`${this.name} is studying ${this.course}`);
};

const student = new Student("Alice", "JavaScript");
student.greet(); //Hello, my name is Alice
student.study();// Alice is studying JavaScript


```

# **Day 5: Advanced Functions and Collections**

### Easy Task: Use call to invoke a function with a specific this value.

```javascript

function greet() {
    console.log(`Hello, my name is ${this.name}`);
}
const person = { name: "Bob" };
greet.call(person);
// Hello, my name is Bob

```

### Medium Task: Create a Set of unique items from an array of duplicates.
```javascript

const numbers = [1, 2, 2, 3, 4, 4, 5];
const uniqueNumbers = new Set(numbers);
console.log([...uniqueNumbers]);

// [1, 2, 3, 4, 5]

```

### Hard Task: Implement a custom bind function.

```javascript

Function.prototype.customBind = function(context, ...args) {
    const fn = this;
    return function(...newArgs) {
        return fn.apply(context, args.concat(newArgs));
    };
};
function greet(greeting, punctuation) {
    console.log(`${greeting}, my name is ${this.name}${punctuation}`);
}
const person = { name: "Charlie" };
const boundGreet = greet.customBind(person, "Hello");
boundGreet("!");

// Hello, my name is Charlie!

```

# **Day 6: Math Functions and Wrap-Up**

### Easy Task: Generate a random number between 1 and 10.

```javascript
const randomNum = Math.floor(Math.random() * 10) + 1;
console.log(randomNum);
// (Random number between 1 and 10)

```

### Medium Task: Calculate mean and median of an array of numbers.

```javascript

function calculateMeanMedian(numbers) {
    const mean = numbers.reduce((acc, num) => acc + num, 0) / numbers.length;
    numbers.sort((a, b) => a - b);
    const mid = Math.floor(numbers.length / 2);
    const median = (numbers.length % 2 !== 0) ? numbers[mid] : (numbers[mid - 1] + numbers[mid]) / 2;
    return { mean, median };
}
const numbers = [1, 2, 3, 4, 5, 6, 7];
const { mean, median } = calculateMeanMedian(numbers);
console.log(`Mean: ${mean}, Median: ${median}`);
// Mean: 4, Median: 4


```
### Hard Task: Create a simple to-do list application.

```javascript
class ToDoList {
    constructor() {
        this.tasks = [];
    }

    addTask(task) {
        this.tasks.push({ task, completed: false });
    }

    completeTask(index) {
        if (this.tasks[index]) {
            this.tasks[index].completed = true;
        }
    }

    removeTask(index) {
        this.tasks.splice(index, 1);
    }

    displayTasks() {
        this.tasks.forEach((task, index) => {
            console.log(`${index + 1}. ${task.task} - ${task.completed ? "Completed" : "Not Completed"}`);
        });
    }
}

const myToDoList = new ToDoList();
myToDoList.addTask("Learn JavaScript");
myToDoList.addTask("Write a blog post");
myToDoList.completeTask(0);
myToDoList.displayTasks();

// 1. Learn JavaScript - Completed
// 2. Write a blog post - Not Completed


```











