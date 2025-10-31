## **Day 1: useReducer, JSON-Server, Axios**

### **Beginner Tasks**
#### 1. **useReducer Counter**
**Question**: Create a simple counter app using `useReducer` instead of `useState`.  
**Solution**:
```jsx
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
}

export default Counter;
```

---

#### 2. **JSON-Server Setup**
**Question**: Set up a local JSON server and fetch data from it.  
**Solution**:
1. Install JSON Server:
   ```bash
   npm install -g json-server
   ```
2. Create a `db.json` file:
   ```json
   {
     "posts": [
       { "id": 1, "title": "Post 1", "author": "Author 1" },
       { "id": 2, "title": "Post 2", "author": "Author 2" }
     ]
   }
   ```
3. Start the server:
   ```bash
   json-server --watch db.json --port 3001
   ```
4. Fetch data in React (Promise .then/.catch approach):
   ```jsx
   import React, { useEffect, useState } from 'react';

   function PostList() {
     const [posts, setPosts] = useState([]);

     useEffect(() => {
       fetch('http://localhost:3001/posts')
         .then((response) => response.json())
         .then((data) => setPosts(data))
         .catch((error) => console.error('Error fetching posts:', error));
     }, []);

     return (
       <ul>
         {posts.map((post) => (
           <li key={post.id}>{post.title} - {post.author}</li>
         ))}
       </ul>
     );
   }

   export default PostList;
   ```

4b. Fetch data in React (async/await alternative)
```jsx
import React, { useEffect, useState } from 'react';

function PostListAsync() {
  const [posts, setPosts] = useState([]);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchPosts() {
      try {
        const response = await fetch('http://localhost:3001/posts');
        if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
        const data = await response.json();
        setPosts(data);
      } catch (err) {
        setError(err.message);
        console.error('Error fetching posts:', err);
      }
    }

    fetchPosts();
  }, []);

  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title} - {post.author}</li>
      ))}
    </ul>
  );
}

export default PostListAsync;
```

---

### **Intermediate Tasks**
#### 1. **Pagination with JSON-Server**
**Question**: Implement pagination for a list of posts fetched from JSON-Server.  
**Solution (Promise .then):**
```jsx
import React, { useEffect, useState } from 'react';

function PaginatedPosts() {
  const [posts, setPosts] = useState([]);
  const [page, setPage] = useState(1);
  const limit = 5;

  useEffect(() => {
    fetch(`http://localhost:3001/posts?_page=${page}&_limit=${limit}`)
      .then((response) => response.json())
      .then((data) => setPosts(data))
      .catch((err) => console.error(err));
  }, [page]);

  return (
    <div>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
      <button onClick={() => setPage(page - 1)} disabled={page === 1}>Previous</button>
      <button onClick={() => setPage(page + 1)}>Next</button>
    </div>
  );
}

export default PaginatedPosts;
```

Alternative using async/await:
```jsx
import React, { useEffect, useState } from 'react';

function PaginatedPostsAsync() {
  const [posts, setPosts] = useState([]);
  const [page, setPage] = useState(1);
  const [error, setError] = useState(null);
  const limit = 5;

  useEffect(() => {
    let isMounted = true;

    async function fetchPage() {
      try {
        const res = await fetch(`http://localhost:3001/posts?_page=${page}&_limit=${limit}`);
        if (!res.ok) throw new Error(`HTTP error! status: ${res.status}`);
        const data = await res.json();
        if (isMounted) setPosts(data);
      } catch (err) {
        if (isMounted) setError(err.message);
        console.error(err);
      }
    }

    fetchPage();

    return () => {
      isMounted = false;
    };
  }, [page]);

  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
      <button onClick={() => setPage(p => Math.max(1, p - 1))} disabled={page === 1}>Previous</button>
      <button onClick={() => setPage(p => p + 1)}>Next</button>
    </div>
  );
}

export default PaginatedPostsAsync;
```

---

#### 2. **Axios for CRUD Operations**
**Question**: Use Axios to perform CRUD operations (GET, POST, PUT, DELETE) on JSON-Server.  
**Solution**:
1. Install Axios:
   ```bash
   npm install axios
   ```
2. Example (using .then):
   ```jsx
   import React, { useEffect, useState } from 'react';
   import axios from 'axios';

   function PostManager() {
     const [posts, setPosts] = useState([]);
     const [newPost, setNewPost] = useState({ title: '', author: '' });

     useEffect(() => {
       axios.get('http://localhost:3001/posts')
         .then((response) => setPosts(response.data))
         .catch((err) => console.error(err));
     }, []);

     const addPost = () => {
       axios.post('http://localhost:3001/posts', newPost)
         .then((response) => setPosts([...posts, response.data]))
         .catch((err) => console.error(err));
     };

     const deletePost = (id) => {
       axios.delete(`http://localhost:3001/posts/${id}`)
         .then(() => setPosts(posts.filter((post) => post.id !== id)))
         .catch((err) => console.error(err));
     };

     return (
       <div>
         <input
           placeholder="Title"
           value={newPost.title}
           onChange={(e) => setNewPost({ ...newPost, title: e.target.value })}
         />
         <input
           placeholder="Author"
           value={newPost.author}
           onChange={(e) => setNewPost({ ...newPost, author: e.target.value })}
         />
         <button onClick={addPost}>Add Post</button>
         <ul>
           {posts.map((post) => (
             <li key={post.id}>
               {post.title} - {post.author}
               <button onClick={() => deletePost(post.id)}>Delete</button>
             </li>
           ))}
         </ul>
       </div>
     );
   }

   export default PostManager;
   ```

Axios with async/await alternative:
```jsx
import React, { useEffect, useState } from 'react';
import axios from 'axios';

function PostManagerAsync() {
  const [posts, setPosts] = useState([]);
  const [newPost, setNewPost] = useState({ title: '', author: '' });
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false;

    async function fetchPosts() {
      try {
        const response = await axios.get('http://localhost:3001/posts');
        if (!cancelled) setPosts(response.data);
      } catch (err) {
        if (!cancelled) setError(err.message);
        console.error(err);
      }
    }

    fetchPosts();

    return () => { cancelled = true; };
  }, []);

  const addPost = async () => {
    try {
      const response = await axios.post('http://localhost:3001/posts', newPost);
      setPosts((prev) => [...prev, response.data]);
      setNewPost({ title: '', author: '' });
    } catch (err) {
      setError(err.message);
      console.error(err);
    }
  };

  const deletePost = async (id) => {
    try {
      await axios.delete(`http://localhost:3001/posts/${id}`);
      setPosts((prev) => prev.filter((post) => post.id !== id));
    } catch (err) {
      setError(err.message);
      console.error(err);
    }
  };

  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <input
        placeholder="Title"
        value={newPost.title}
        onChange={(e) => setNewPost({ ...newPost, title: e.target.value })}
      />
      <input
        placeholder="Author"
        value={newPost.author}
        onChange={(e) => setNewPost({ ...newPost, author: e.target.value })}
      />
      <button onClick={addPost}>Add Post</button>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            {post.title} - {post.author}
            <button onClick={() => deletePost(post.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default PostManagerAsync;
```

---

### **Advanced Tasks**
#### 1. **useReducer with API Calls**
**Question**: Use `useReducer` to manage the state of API calls (loading, success, error).  
**Solution (using axios with .then/.catch):**
```jsx
import React, { useReducer, useEffect } from 'react';
import axios from 'axios';

const initialState = { loading: true, data: [], error: '' };

function reducer(state, action) {
  switch (action.type) {
    case 'FETCH_SUCCESS':
      return { loading: false, data: action.payload, error: '' };
    case 'FETCH_ERROR':
      return { loading: false, data: [], error: 'Something went wrong!' };
    default:
      return state;
  }
}

function PostList() {
  const [state, dispatch] = useReducer(reducer, initialState);

  useEffect(() => {
    axios.get('http://localhost:3001/posts')
      .then((response) => dispatch({ type: 'FETCH_SUCCESS', payload: response.data }))
      .catch(() => dispatch({ type: 'FETCH_ERROR' }));
  }, []);

  return (
    <div>
      {state.loading ? 'Loading...' : state.data.map((post) => <div key={post.id}>{post.title}</div>)}
      {state.error && <p>{state.error}</p>}
    </div>
  );
}

export default PostList;
```

Alternative using async/await inside useEffect:
```jsx
import React, { useReducer, useEffect } from 'react';
import axios from 'axios';

const initialState = { loading: true, data: [], error: '' };

function reducer(state, action) {
  switch (action.type) {
    case 'FETCH_SUCCESS':
      return { loading: false, data: action.payload, error: '' };
    case 'FETCH_ERROR':
      return { loading: false, data: [], error: action.payload || 'Something went wrong!' };
    default:
      return state;
  }
}

function PostListAsync() {
  const [state, dispatch] = useReducer(reducer, initialState);

  useEffect(() => {
    let cancelled = false;

    async function fetchData() {
      try {
        const response = await axios.get('http://localhost:3001/posts');
        if (!cancelled) dispatch({ type: 'FETCH_SUCCESS', payload: response.data });
      } catch (err) {
        if (!cancelled) dispatch({ type: 'FETCH_ERROR', payload: err.message });
      }
    }

    fetchData();

    return () => { cancelled = true; };
  }, []);

  return (
    <div>
      {state.loading ? 'Loading...' : state.data.map((post) => <div key={post.id}>{post.title}</div>)}
      {state.error && <p>{state.error}</p>}
    </div>
  );
}

export default PostListAsync;
```

---

## **Day 2: useContext, Custom Hooks, Lazy Loading**

### **Beginner Tasks**
#### 1. **useContext for Theme Switching**
**Question**: Create a theme switcher using `useContext`.  
**Solution**:
```jsx
import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [isDark, setIsDark] = useState(false);

  return (
    <ThemeContext.Provider value={{ isDark, setIsDark }}>
      {children}
    </ThemeContext.Provider>
  );
}

function App() {
  return (
    <ThemeProvider>
      <ThemeSwitcher />
    </ThemeProvider>
  );
}

function ThemeSwitcher() {
  const { isDark, setIsDark } = useContext(ThemeContext);

  return (
    <div style={{ backgroundColor: isDark ? '#333' : '#fff', color: isDark ? '#fff' : '#333', padding: '20px' }}>
      <button onClick={() => setIsDark(!isDark)}>
        Switch to {isDark ? 'Light' : 'Dark'} Theme
      </button>
    </div>
  );
}

export default App;
```

---

### **Intermediate Tasks**
#### 1. **Custom Hook for Fetching Data**
**Question**: Create a custom hook `useFetch` to fetch data from an API.  
**Solution (Promise .then):**
```jsx
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((err) => {
        console.error(err);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
}

function PostList() {
  const { data, loading } = useFetch('http://localhost:3001/posts');

  if (loading) return <p>Loading...</p>;

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default PostList;
```

Alternative custom hook using async/await and fetch:
```jsx
import { useState, useEffect } from 'react';

function useFetchAsync(url) {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false;

    async function fetchData() {
      try {
        const res = await fetch(url);
        if (!res.ok) throw new Error(`HTTP error! status: ${res.status}`);
        const json = await res.json();
        if (!cancelled) {
          setData(json);
          setLoading(false);
        }
      } catch (err) {
        if (!cancelled) {
          setError(err.message);
          setLoading(false);
        }
        console.error(err);
      }
    }

    fetchData();

    return () => { cancelled = true; };
  }, [url]);

  return { data, loading, error };
}

function PostListAsyncHook() {
  const { data, loading, error } = useFetchAsync('http://localhost:3001/posts');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default PostListAsyncHook;
```

---

### **Advanced Tasks**
#### 1. **Lazy Loading Components**
**Question**: Use `React.lazy` and `Suspense` to lazy load a component.  
**Solution**:
```jsx
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

export default App;
```

---

## **Day 3: React.memo, useCallback, useMemo, Local Storage, HOC**

### **Beginner Tasks**
#### 1. **React.memo for Optimization**
**Question**: Use `React.memo` to prevent unnecessary re-renders of a component.  
**Solution**:
```jsx
import React, { useState } from 'react';

const MemoizedComponent = React.memo(({ value }) => {
  console.log('Rendered!');
  return <div>{value}</div>;
});

function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <MemoizedComponent value="Hello" />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </div>
  );
}

export default App;
```

---

### **Intermediate Tasks**
#### 1. **useCallback for Callback Optimization**
**Question**: Use `useCallback` to memoize a callback function.  
**Solution**:
```jsx
import React, { useState, useCallback } from 'react';

function App() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount((prevCount) => prevCount + 1);
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default App;
```

---

### **Advanced Tasks**
#### 1. **Higher-Order Component (HOC)**
**Question**: Create a HOC that adds a loading spinner to any component.  
**Solution**:
```jsx
import React, { useEffect, useState } from 'react';

function withLoading(Component) {
  return function WithLoadingComponent({ isLoading, ...props }) {
    if (isLoading) return <div>Loading...</div>;
    return <Component {...props} />;
  };
}

function PostList({ data }) {
  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

const PostListWithLoading = withLoading(PostList);

function App() {
  const [loading, setLoading] = useState(true);
  const [data, setData] = useState([]);

  useEffect(() => {
    setTimeout(() => {
      setData([{ id: 1, title: 'Post 1' }, { id: 2, title: 'Post 2' }]);
      setLoading(false);
    }, 2000);
  }, []);

  return <PostListWithLoading isLoading={loading} data={data} />;
}

export default App;
```

---

## **Day 4: React Router Dom**

### **Beginner Tasks**
#### 1. **Basic Routing**
**Question**: Set up basic routing using `react-router-dom`.  
**Solution**:
```jsx
import { BrowserRouter as Router, Route, Routes, Link } from 'react-router-dom';

function Home() {
  return <h1>Home</h1>;
}

function About() {
  return <h1>About</h1>;
}

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Router>
  );
}

export default App;
```

---

### **Intermediate Tasks**
#### 1. **Nested Routing**
**Question**: Implement nested routing for a dashboard layout.  
**Solution**:
```jsx
import { BrowserRouter as Router, Route, Routes, Link, Outlet } from 'react-router-dom';

function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <Link to="profile">Profile</Link>
        <Link to="settings">Settings</Link>
      </nav>
      <Outlet />
    </div>
  );
}

function Profile() {
  return <h2>Profile</h2>;
}

function Settings() {
  return <h2>Settings</h2>;
}

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />}>
          <Route path="profile" element={<Profile />} />
          <Route path="settings" element={<Settings />} />
        </Route>
      </Routes>
    </Router>
  );
}

export default App;
```

---

### **Advanced Tasks**
#### 1. **Protected Routes**
**Question**: Create a protected route that redirects unauthenticated users to a login page.  
**Solution**:
```jsx
import { BrowserRouter as Router, Route, Routes, Navigate } from 'react-router-dom';

function ProtectedRoute({ children }) {
  const isAuthenticated = true; // Replace with actual auth logic
  return isAuthenticated ? children : <Navigate to="/login" />;
}

function Dashboard() {
  return <h1>Dashboard</h1>;
}

function Login() {
  return <h1>Login</h1>;
}

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/login" element={<Login />} />
        <Route
          path="/dashboard"
          element={
            <ProtectedRoute>
              <Dashboard />
            </ProtectedRoute>
          }
        />
      </Routes>
    </Router>
  );
}

export default App;
```

---
