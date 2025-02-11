

---

## **Day 1: React Basics**
### **Beginner Tasks**
1. **Hello World App**: Create a React app that displays "Hello, World!" on the screen.
2. **Component Creation**: Build a functional component called `Greeting` that takes a `name` prop and displays "Hello, {name}!".
3. **Folder Structure**: Organize your React app by creating separate folders for `components`, `styles`, and `assets`.

### **Intermediate Tasks**
1. **Counter App**: Build a simple counter app with a button that increments the count displayed on the screen.
2. **Props Practice**: Create a `UserCard` component that accepts `name`, `age`, and `email` as props and displays them in a card layout.
3. **Class Component**: Convert the `Greeting` functional component into a class component (optional, for understanding).

### **Advanced Tasks**
1. **Theme Switcher**: Build a React app with a button that toggles between light and dark themes (change background and text color).
2. **Virtual DOM Explanation**: Create a small app that demonstrates how the Virtual DOM works by logging updates to the console.

---

## **Day 2: JavaScript Refresh and React Fundamentals**
### **Beginner Tasks**
1. **useState Counter**: Build a counter app using the `useState` hook.
2. **Props Drilling**: Create a parent component `App` and a child component `Child`. Pass a `message` prop from `App` to `Child` and display it.
3. **Conditional Rendering**: Build a component that displays "Welcome, User!" if a `isLoggedIn` state is `true`, otherwise display "Please log in."

### **Intermediate Tasks**
1. **List Rendering**: Fetch a list of items (e.g., fruits or tasks) from a hardcoded array and render them using the `map` method.
2. **Event Handling**: Create a button that changes the text of a heading when clicked.
3. **State vs Props**: Build a `Parent` component that manages a `count` state and passes it as a prop to a `Child` component. The `Child` component should display the count.

### **Advanced Tasks**
1. **useRef Example**: Create an input field and a button. Use `useRef` to focus the input field when the button is clicked.
2. **Fetch Data**: Fetch data from `https://jsonplaceholder.typicode.com/users` and display the names of the first 5 users in a list.
3. **Search Filter**: Build a search bar that filters the list of users based on their names.

---

## **Day 3: Getting Started with React**
### **Beginner Tasks**
1. **useEffect Example**: Create a component that logs "Component Mounted" to the console when it mounts using `useEffect`.
2. **Form Validation**: Build a simple login form with validation (e.g., username and password cannot be empty).
3. **CSS Styling**: Style a React component using inline CSS, CSS modules, and external CSS.

### **Intermediate Tasks**
1. **Theme Switcher with useEffect**: Extend the theme switcher app to save the theme preference in `localStorage` and load it on page reload using `useEffect`.
2. **React Bootstrap**: Use React Bootstrap to create a responsive navbar with a logo and links.
3. **Previous State**: Build a counter app that uses the previous state to increment the count.

### **Advanced Tasks**
1. **Quote Fetcher**: Fetch quotes from `https://dummyjson.com/quotes` and display the first 10 quotes in a list. Use `useEffect` to fetch the data on component mount.
2. **Form with Validation**: Build a registration form with fields for name, email, and password. Validate each field and display error messages if validation fails.
3. **Dynamic Styling**: Create a component that changes its background color based on user input.

---

## **Day 4: React Advanced Concepts**
### **Beginner Tasks**
1. **React Fragments**: Build a component that uses React Fragments to group multiple elements without adding extra nodes to the DOM.
2. **Strict Mode**: Create a simple app and wrap it in `React.StrictMode`. Observe the console for warnings or double renders.

### **Intermediate Tasks**
1. **Component Composition**: Build a `Card` component that accepts `title`, `description`, and `image` as props and displays them in a card layout.
2. **Quote List**: Fetch quotes from `https://dummyjson.com/quotes` and display them in a list. Use `useEffect` to fetch the data and `map` to render the quotes.

### **Advanced Tasks**
1. **Task Manager**: Build a task manager app where users can add, edit, and delete tasks. Use `useState` to manage the list of tasks.
2. **Modal Component**: Create a reusable `Modal` component that can be triggered by a button click. The modal should display dynamic content passed as props.
3. **API Integration**: Build a React app that fetches and displays data from a public API (e.g., weather data, news articles, or movie listings).

---

## **Additional Challenges**
### **Beginner**
- Build a React app that displays a list of your favorite books.
- Create a simple calculator app using React.

### **Intermediate**
- Build a todo app with add, delete, and mark-as-complete functionality.
- Create a React app that fetches and displays weather data from a public API.

### **Advanced**
- Implement a React app with routing using **React Router**.
- Build a React app that integrates with a backend API (e.g., Firebase or a custom Node.js backend).

---






## **Day 1: React Basics**

### 1. Hello World App
```jsx
import React from 'react';

function App() {
  return <h1>Hello, World!</h1>;
}

export default App;
```

### 2. Functional Component with Props
```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Usage in App.js
function App() {
  return <Greeting name="John" />;
}
```

### 3. Counter App
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

---

## **Day 2: JavaScript Refresh and React Fundamentals**

### 1. Conditional Rendering
```jsx
function Greeting({ isLoggedIn }) {
  return isLoggedIn ? <h1>Welcome, User!</h1> : <h1>Please log in.</h1>;
}

// Usage in App.js
function App() {
  return <Greeting isLoggedIn={true} />;
}
```

### 2. List Rendering with `map`
```jsx
function FruitList() {
  const fruits = ['Apple', 'Banana', 'Orange'];

  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
}

export default FruitList;
```

### 3. Fetch Data and Display
```jsx
import React, { useEffect, useState } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then((response) => response.json())
      .then((data) => setUsers(data.slice(0, 5)));
  }, []);

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

export default UserList;
```

---

## **Day 3: Getting Started with React**

### 1. useEffect Example
```jsx
import React, { useEffect } from 'react';

function App() {
  useEffect(() => {
    console.log('Component Mounted');
  }, []);

  return <h1>useEffect Example</h1>;
}

export default App;
```

### 2. Theme Switcher
```jsx
import React, { useState } from 'react';

function ThemeSwitcher() {
  const [isDark, setIsDark] = useState(false);

  return (
    <div style={{ backgroundColor: isDark ? '#333' : '#fff', color: isDark ? '#fff' : '#333', padding: '20px' }}>
      <button onClick={() => setIsDark(!isDark)}>
        Switch to {isDark ? 'Light' : 'Dark'} Theme
      </button>
    </div>
  );
}

export default ThemeSwitcher;
```

### 3. Form Validation
```jsx
import React, { useState } from 'react';

function LoginForm() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!username || !password) {
      setError('Please fill in all fields.');
    } else {
      setError('');
      alert('Form submitted successfully!');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Username"
        value={username}
        onChange={(e) => setUsername(e.target.value)}
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button type="submit">Submit</button>
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </form>
  );
}

export default LoginForm;
```

---

## **Day 4: React Advanced Concepts**

### 1. React Fragments
```jsx
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>This is a paragraph.</p>
    </>
  );
}

export default App;
```

### 2. Fetch Quotes and Display
```jsx
import React, { useEffect, useState } from 'react';

function QuoteList() {
  const [quotes, setQuotes] = useState([]);

  useEffect(() => {
    fetch('https://dummyjson.com/quotes')
      .then((response) => response.json())
      .then((data) => setQuotes(data.quotes.slice(0, 10)));
  }, []);

  return (
    <div>
      {quotes.map((quote) => (
        <h1 key={quote.id}>{quote.quote}</h1>
      ))}
    </div>
  );
}

export default QuoteList;
```

### 3. Task Manager App
```jsx
import React, { useState } from 'react';

function TaskManager() {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState('');

  const addTask = () => {
    if (newTask.trim()) {
      setTasks([...tasks, { id: Date.now(), text: newTask }]);
      setNewTask('');
    }
  };

  const deleteTask = (id) => {
    setTasks(tasks.filter((task) => task.id !== id));
  };

  return (
    <div>
      <input
        type="text"
        value={newTask}
        onChange={(e) => setNewTask(e.target.value)}
        placeholder="Add a new task"
      />
      <button onClick={addTask}>Add Task</button>
      <ul>
        {tasks.map((task) => (
          <li key={task.id}>
            {task.text} <button onClick={() => deleteTask(task.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TaskManager;
```

---

## **Additional Challenges**

### 1. Todo App
```jsx
import React, { useState } from 'react';

function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');

  const addTodo = () => {
    if (input.trim()) {
      setTodos([...todos, { id: Date.now(), text: input, completed: false }]);
      setInput('');
    }
  };

  const toggleTodo = (id) => {
    setTodos(
      todos.map((todo) =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };

  return (
    <div>
      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Add a new todo"
      />
      <button onClick={addTodo}>Add Todo</button>
      <ul>
        {todos.map((todo) => (
          <li
            key={todo.id}
            style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}
            onClick={() => toggleTodo(todo.id)}
          >
            {todo.text}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TodoApp;
```

---
