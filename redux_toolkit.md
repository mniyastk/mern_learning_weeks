## **Day 1: Introduction to Redux**

### **Beginner Tasks**
#### 1. **Create a Basic Redux Store**
**Question**: Create a simple Redux store with a counter state using vanilla JavaScript.  
**Solution**:
```javascript
const { createStore } = Redux;

// Initial state
const initialState = {
  count: 0
};

// Reducer
function counterReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

// Create store
const store = createStore(counterReducer);

// Subscribe to changes
store.subscribe(() => console.log(store.getState()));

// Dispatch actions
store.dispatch({ type: 'INCREMENT' });
```

---

### **Intermediate Tasks**
#### 1. **Multiple State Properties**
**Question**: Extend the counter store to include a name property and corresponding actions.  
**Solution**:
```javascript
const initialState = {
  count: 0,
  name: ''
};

function reducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'SET_NAME':
      return { ...state, name: action.payload };
    default:
      return state;
  }
}
```

---

### **Advanced Tasks**
#### 1. **Store with Type Safety**
**Question**: Implement type checking for actions and state using TypeScript.  
**Solution**:
```typescript
interface State {
  count: number;
  name: string;
}

type Action = 
  | { type: 'INCREMENT' }
  | { type: 'SET_NAME'; payload: string };

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'SET_NAME':
      return { ...state, name: action.payload };
    default:
      return state;
  }
};
```

---

## **Day 2: Actions and Reducers**

### **Beginner Tasks**
#### 1. **Action Creators**
**Question**: Create action creators for a todo list application.  
**Solution**:
```javascript
// Action Types
const ADD_TODO = 'ADD_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';

// Action Creators
const addTodo = (text) => ({
  type: ADD_TODO,
  payload: {
    id: Date.now(),
    text,
    completed: false
  }
});

const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  payload: id
});
```

---

### **Intermediate Tasks**
#### 1. **Combined Reducers**
**Question**: Create and combine multiple reducers for a task management application.  
**Solution**:
```javascript
import { combineReducers } from 'redux';

const tasksReducer = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TASK':
      return [...state, action.payload];
    default:
      return state;
  }
};

const filterReducer = (state = 'ALL', action) => {
  switch (action.type) {
    case 'SET_FILTER':
      return action.payload;
    default:
      return state;
  }
};

const rootReducer = combineReducers({
  tasks: tasksReducer,
  filter: filterReducer
});
```

---

## **Day 3: Store and Middleware**

### **Beginner Tasks**
#### 1. **Logger Middleware**
**Question**: Create a simple logging middleware that logs all actions and state changes.  
**Solution**:
```javascript
const logger = store => next => action => {
  console.log('dispatching:', action);
  const result = next(action);
  console.log('next state:', store.getState());
  return result;
};

const store = createStore(
  rootReducer,
  applyMiddleware(logger)
);
```

---

### **Intermediate Tasks**
#### 1. **Custom Middleware**
**Question**: Create a middleware that adds a timestamp to every action.  
**Solution**:
```javascript
const timeStampMiddleware = store => next => action => {
  return next({
    ...action,
    timestamp: Date.now()
  });
};
```

---

## **Day 4: React-Redux Integration**

### **Beginner Tasks**
#### 1. **Connect Component to Store**
**Question**: Create a React component that displays and updates the Redux store.  
**Solution**:
```jsx
import { useSelector, useDispatch } from 'react-redux';

function Counter() {
  const count = useSelector(state => state.count);
  const dispatch = useDispatch();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>
        Increment
      </button>
    </div>
  );
}
```

---

### **Intermediate Tasks**
#### 1. **Selective State Updates**
**Question**: Create a component that selectively updates based on specific state changes.  
**Solution**:
```jsx
import { useSelector, useDispatch, shallowEqual } from 'react-redux';

function TodoList() {
  const todos = useSelector(
    state => state.todos,
    shallowEqual
  );

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}
```

---

## **Day 5: Asynchronous Actions**

### **Beginner Tasks**
#### 1. **Basic Thunk**
**Question**: Create a thunk action creator to fetch data from an API.  
**Solution**:
```javascript
const fetchTodos = () => {
  return async dispatch => {
    dispatch({ type: 'FETCH_TODOS_START' });
    try {
      const response = await fetch('/api/todos');
      const todos = await response.json();
      dispatch({ type: 'FETCH_TODOS_SUCCESS', payload: todos });
    } catch (error) {
      dispatch({ type: 'FETCH_TODOS_ERROR', payload: error.message });
    }
  };
};
```

---

### **Advanced Tasks**
#### 1. **Complex Async Flow**
**Question**: Implement a thunk that coordinates multiple API calls and state updates.  
**Solution**:
```javascript
const processTodo = (id) => {
  return async (dispatch, getState) => {
    dispatch({ type: 'PROCESS_START', payload: id });
    
    try {
      const todo = await fetchTodoDetails(id);
      dispatch({ type: 'UPDATE_TODO', payload: todo });
      
      const relatedTodos = await fetchRelatedTodos(todo.category);
      dispatch({ type: 'UPDATE_RELATED', payload: relatedTodos });
      
      dispatch({ type: 'PROCESS_SUCCESS' });
    } catch (error) {
      dispatch({ type: 'PROCESS_ERROR', payload: error.message });
    }
  };
};
```

---

## **Day 6: Final Project - Todo List Application**

### **Advanced Tasks**
#### 1. **Complete Todo Application**
**Question**: Create a full-featured todo list application using Redux.  
**Solution**:
```jsx
// Action Types
const ADD_TODO = 'ADD_TODO';
const DELETE_TODO = 'DELETE_TODO';
const EDIT_TODO = 'EDIT_TODO';

// Action Creators
const addTodo = (text) => ({
  type: ADD_TODO,
  payload: { id: Date.now(), text }
});

const deleteTodo = (id) => ({
  type: DELETE_TODO,
  payload: id
});

const editTodo = (id, text) => ({
  type: EDIT_TODO,
  payload: { id, text }
});

// Reducer
const todosReducer = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload];
    case DELETE_TODO:
      return state.filter(todo => todo.id !== action.payload);
    case EDIT_TODO:
      return state.map(todo =>
        todo.id === action.payload.id
          ? { ...todo, text: action.payload.text }
          : todo
      );
    default:
      return state;
  }
};

// Component
function TodoApp() {
  const todos = useSelector(state => state.todos);
  const dispatch = useDispatch();
  const [text, setText] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (text.trim()) {
      dispatch(addTodo(text));
      setText('');
    }
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          value={text}
          onChange={(e) => setText(e.target.value)}
          placeholder="Add todo"
        />
        <button type="submit">Add</button>
      </form>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            {todo.text}
            <button onClick={() => dispatch(deleteTodo(todo.id))}>
              Delete
            </button>
            <button onClick={() => {
              const newText = prompt('Edit todo:', todo.text);
              if (newText) dispatch(editTodo(todo.id, newText));
            }}>
              Edit
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```
