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




