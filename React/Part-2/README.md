# React Orta Seviye - Eğitim Materyali

> **Eğitmen:** Faruk Bora Güvenkaya  
> **Süre:** 4-6 hafta  
> **Seviye:** Orta  
> **Ön Gereksinimler:** React Temelleri, JavaScript İleri Seviye

## 📋 İçindekiler

- [useEffect Hook](#useeffect-hook)
- [useReducer Hook](#usereducer-hook)
- [useContext Hook](#usecontext-hook)
- [Custom Hooks](#custom-hooks)
- [Component Composition](#component-composition)
- [Higher-Order Components](#higher-order-components)
- [Render Props Pattern](#render-props-pattern)
- [Error Boundaries](#error-boundaries)
- [Performance Optimization](#performance-optimization)
- [React.memo, useMemo, useCallback](#reactmemo-usememo-usecallback)
- [Code Splitting](#code-splitting)
- [Lazy Loading](#lazy-loading)
- [React Router](#react-router)
- [API Integration](#api-integration)

---

## 🔄 useEffect Hook

useEffect hook'u, component'lerde side effect'leri (yan etkileri) yönetmek için kullanılır. API çağrıları, DOM manipülasyonu, subscription'lar gibi işlemler side effect'tir.

### useEffect Temel Kullanımı
```javascript
import { useState, useEffect } from 'react';

function DataFetcher() {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        // Component mount olduğunda çalışır
        const fetchData = async () => {
            try {
                const response = await fetch('/api/data');
                const result = await response.json();
                setData(result);
            } catch (error) {
                console.error('Veri yüklenemedi:', error);
            } finally {
                setLoading(false);
            }
        };
        
        fetchData();
    }, []); // Boş dependency array - sadece mount'ta çalışır
    
    if (loading) return <div>Yükleniyor...</div>;
    
    return (
        <div>
            <h2>Veriler</h2>
            <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
    );
}
```

### useEffect Dependency Array
```javascript
function UserProfile({ userId }) {
    const [user, setUser] = useState(null);
    
    // userId değiştiğinde çalışır
    useEffect(() => {
        const fetchUser = async () => {
            const response = await fetch(`/api/users/${userId}`);
            const userData = await response.json();
            setUser(userData);
        };
        
        fetchUser();
    }, [userId]); // userId dependency olarak eklendi
    
    return user ? (
        <div>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
        </div>
    ) : (
        <div>Kullanıcı yükleniyor...</div>
    );
}
```

### useEffect Cleanup
```javascript
function Timer() {
    const [seconds, setSeconds] = useState(0);
    
    useEffect(() => {
        const interval = setInterval(() => {
            setSeconds(prev => prev + 1);
        }, 1000);
        
        // Cleanup function - component unmount olduğunda çalışır
        return () => {
            clearInterval(interval);
        };
    }, []);
    
    return <div>Geçen süre: {seconds} saniye</div>;
}
```

### Multiple useEffect
```javascript
function ComplexComponent({ userId }) {
    const [user, setUser] = useState(null);
    const [posts, setPosts] = useState([]);
    const [windowSize, setWindowSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    // Kullanıcı verilerini yükle
    useEffect(() => {
        const fetchUser = async () => {
            const response = await fetch(`/api/users/${userId}`);
            const userData = await response.json();
            setUser(userData);
        };
        
        fetchUser();
    }, [userId]);
    
    // Kullanıcının postlarını yükle
    useEffect(() => {
        const fetchPosts = async () => {
            const response = await fetch(`/api/users/${userId}/posts`);
            const postsData = await response.json();
            setPosts(postsData);
        };
        
        if (userId) {
            fetchPosts();
        }
    }, [userId]);
    
    // Pencere boyutunu takip et
    useEffect(() => {
        const handleResize = () => {
            setWindowSize({
                width: window.innerWidth,
                height: window.innerHeight
            });
        };
        
        window.addEventListener('resize', handleResize);
        
        return () => {
            window.removeEventListener('resize', handleResize);
        };
    }, []);
    
    return (
        <div>
            {user && <h2>{user.name}</h2>}
            <p>Pencere boyutu: {windowSize.width} x {windowSize.height}</p>
            <ul>
                {posts.map(post => (
                    <li key={post.id}>{post.title}</li>
                ))}
            </ul>
        </div>
    );
}
```

---

## 🔄 useReducer Hook

useReducer, karmaşık state yönetimi için useState'e alternatif olarak kullanılır. Redux pattern'ini takip eder.

### useReducer Temel Kullanımı
```javascript
import { useReducer } from 'react';

// Reducer function
function counterReducer(state, action) {
    switch (action.type) {
        case 'increment':
            return { count: state.count + 1 };
        case 'decrement':
            return { count: state.count - 1 };
        case 'reset':
            return { count: 0 };
        case 'set':
            return { count: action.payload };
        default:
            return state;
    }
}

function Counter() {
    const [state, dispatch] = useReducer(counterReducer, { count: 0 });
    
    return (
        <div>
            <h2>Sayı: {state.count}</h2>
            <button onClick={() => dispatch({ type: 'increment' })}>
                Artır
            </button>
            <button onClick={() => dispatch({ type: 'decrement' })}>
                Azalt
            </button>
            <button onClick={() => dispatch({ type: 'reset' })}>
                Sıfırla
            </button>
            <button onClick={() => dispatch({ type: 'set', payload: 10 })}>
                10'a Ayarla
            </button>
        </div>
    );
}
```

### Karmaşık State ile useReducer
```javascript
function todoReducer(state, action) {
    switch (action.type) {
        case 'ADD_TODO':
            return {
                ...state,
                todos: [...state.todos, {
                    id: Date.now(),
                    text: action.payload,
                    completed: false
                }]
            };
        case 'TOGGLE_TODO':
            return {
                ...state,
                todos: state.todos.map(todo =>
                    todo.id === action.payload
                        ? { ...todo, completed: !todo.completed }
                        : todo
                )
            };
        case 'DELETE_TODO':
            return {
                ...state,
                todos: state.todos.filter(todo => todo.id !== action.payload)
            };
        case 'SET_FILTER':
            return {
                ...state,
                filter: action.payload
            };
        default:
            return state;
    }
}

function TodoApp() {
    const [state, dispatch] = useReducer(todoReducer, {
        todos: [],
        filter: 'all'
    });
    
    const addTodo = (text) => {
        dispatch({ type: 'ADD_TODO', payload: text });
    };
    
    const toggleTodo = (id) => {
        dispatch({ type: 'TOGGLE_TODO', payload: id });
    };
    
    const deleteTodo = (id) => {
        dispatch({ type: 'DELETE_TODO', payload: id });
    };
    
    const filteredTodos = state.todos.filter(todo => {
        if (state.filter === 'completed') return todo.completed;
        if (state.filter === 'active') return !todo.completed;
        return true;
    });
    
    return (
        <div>
            <h2>Todo Listesi</h2>
            <input
                type="text"
                onKeyPress={(e) => {
                    if (e.key === 'Enter') {
                        addTodo(e.target.value);
                        e.target.value = '';
                    }
                }}
                placeholder="Yeni görev ekle"
            />
            <div>
                <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'all' })}>
                    Tümü
                </button>
                <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'active' })}>
                    Aktif
                </button>
                <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'completed' })}>
                    Tamamlanan
                </button>
            </div>
            <ul>
                {filteredTodos.map(todo => (
                    <li key={todo.id}>
                        <span
                            onClick={() => toggleTodo(todo.id)}
                            style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}
                        >
                            {todo.text}
                        </span>
                        <button onClick={() => deleteTodo(todo.id)}>Sil</button>
                    </li>
                ))}
            </ul>
        </div>
    );
}
```

---

## 🌐 useContext Hook

useContext, component tree'de prop drilling'i önlemek için kullanılır. Global state yönetimi sağlar.

### Context Oluşturma
```javascript
import { createContext, useContext, useState } from 'react';

// Context oluşturma
const ThemeContext = createContext();

// Provider component
function ThemeProvider({ children }) {
    const [theme, setTheme] = useState('light');
    
    const toggleTheme = () => {
        setTheme(prev => prev === 'light' ? 'dark' : 'light');
    };
    
    return (
        <ThemeContext.Provider value={{ theme, toggleTheme }}>
            {children}
        </ThemeContext.Provider>
    );
}

// Custom hook
function useTheme() {
    const context = useContext(ThemeContext);
    if (!context) {
        throw new Error('useTheme must be used within ThemeProvider');
    }
    return context;
}
```

### Context Kullanımı
```javascript
function Header() {
    const { theme, toggleTheme } = useTheme();
    
    return (
        <header style={{ 
            backgroundColor: theme === 'light' ? '#fff' : '#333',
            color: theme === 'light' ? '#333' : '#fff'
        }}>
            <h1>Web Sitem</h1>
            <button onClick={toggleTheme}>
                {theme === 'light' ? 'Karanlık' : 'Aydınlık'} Tema
            </button>
        </header>
    );
}

function Main() {
    const { theme } = useTheme();
    
    return (
        <main style={{ 
            backgroundColor: theme === 'light' ? '#f5f5f5' : '#222',
            color: theme === 'light' ? '#333' : '#fff'
        }}>
            <h2>Ana İçerik</h2>
            <p>Bu sayfanın ana içeriği burada yer alır.</p>
        </main>
    );
}

function App() {
    return (
        <ThemeProvider>
            <Header />
            <Main />
        </ThemeProvider>
    );
}
```

### Karmaşık Context
```javascript
const UserContext = createContext();

function UserProvider({ children }) {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(false);
    
    const login = async (email, password) => {
        setLoading(true);
        try {
            const response = await fetch('/api/login', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ email, password })
            });
            const userData = await response.json();
            setUser(userData);
        } catch (error) {
            console.error('Giriş hatası:', error);
        } finally {
            setLoading(false);
        }
    };
    
    const logout = () => {
        setUser(null);
    };
    
    const updateProfile = (updates) => {
        setUser(prev => ({ ...prev, ...updates }));
    };
    
    return (
        <UserContext.Provider value={{
            user,
            loading,
            login,
            logout,
            updateProfile
        }}>
            {children}
        </UserContext.Provider>
    );
}

function useUser() {
    const context = useContext(UserContext);
    if (!context) {
        throw new Error('useUser must be used within UserProvider');
    }
    return context;
}
```

---

## 🎣 Custom Hooks

Custom hook'lar, stateful logic'i component'ler arasında paylaşmak için kullanılır.

### Basit Custom Hook
```javascript
function useCounter(initialValue = 0) {
    const [count, setCount] = useState(initialValue);
    
    const increment = () => setCount(prev => prev + 1);
    const decrement = () => setCount(prev => prev - 1);
    const reset = () => setCount(initialValue);
    
    return { count, increment, decrement, reset };
}

function Counter() {
    const { count, increment, decrement, reset } = useCounter(10);
    
    return (
        <div>
            <h2>Sayı: {count}</h2>
            <button onClick={increment}>Artır</button>
            <button onClick={decrement}>Azalt</button>
            <button onClick={reset}>Sıfırla</button>
        </div>
    );
}
```

### API Custom Hook
```javascript
function useApi(url) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        const fetchData = async () => {
            try {
                setLoading(true);
                setError(null);
                const response = await fetch(url);
                if (!response.ok) {
                    throw new Error('API çağrısı başarısız');
                }
                const result = await response.json();
                setData(result);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };
        
        fetchData();
    }, [url]);
    
    return { data, loading, error };
}

function UserList() {
    const { data: users, loading, error } = useApi('/api/users');
    
    if (loading) return <div>Yükleniyor...</div>;
    if (error) return <div>Hata: {error}</div>;
    
    return (
        <ul>
            {users?.map(user => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    );
}
```

### Local Storage Custom Hook
```javascript
function useLocalStorage(key, initialValue) {
    const [storedValue, setStoredValue] = useState(() => {
        try {
            const item = window.localStorage.getItem(key);
            return item ? JSON.parse(item) : initialValue;
        } catch (error) {
            console.error('LocalStorage okuma hatası:', error);
            return initialValue;
        }
    });
    
    const setValue = (value) => {
        try {
            setStoredValue(value);
            window.localStorage.setItem(key, JSON.stringify(value));
        } catch (error) {
            console.error('LocalStorage yazma hatası:', error);
        }
    };
    
    return [storedValue, setValue];
}

function Settings() {
    const [theme, setTheme] = useLocalStorage('theme', 'light');
    const [language, setLanguage] = useLocalStorage('language', 'tr');
    
    return (
        <div>
            <h2>Ayarlar</h2>
            <div>
                <label>Tema:</label>
                <select value={theme} onChange={(e) => setTheme(e.target.value)}>
                    <option value="light">Aydınlık</option>
                    <option value="dark">Karanlık</option>
                </select>
            </div>
            <div>
                <label>Dil:</label>
                <select value={language} onChange={(e) => setLanguage(e.target.value)}>
                    <option value="tr">Türkçe</option>
                    <option value="en">İngilizce</option>
                </select>
            </div>
        </div>
    );
}
```

---

## 🧩 Component Composition

Component composition, component'leri birleştirerek daha karmaşık UI'lar oluşturma tekniğidir.

### Children Prop
```javascript
function Card({ children, title }) {
    return (
        <div className="card">
            {title && <h3 className="card-title">{title}</h3>}
            <div className="card-content">
                {children}
            </div>
        </div>
    );
}

function App() {
    return (
        <div>
            <Card title="Kullanıcı Bilgileri">
                <p>İsim: Faruk</p>
                <p>Yaş: 20</p>
            </Card>
            
            <Card>
                <h4>Başlık</h4>
                <p>Bu bir card içeriğidir.</p>
            </Card>
        </div>
    );
}
```

### Render Props Pattern
```javascript
function DataProvider({ render }) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        const fetchData = async () => {
            const response = await fetch('/api/data');
            const result = await response.json();
            setData(result);
            setLoading(false);
        };
        
        fetchData();
    }, []);
    
    return render({ data, loading });
}

function App() {
    return (
        <DataProvider
            render={({ data, loading }) => (
                <div>
                    {loading ? (
                        <div>Yükleniyor...</div>
                    ) : (
                        <div>
                            <h2>Veriler</h2>
                            <pre>{JSON.stringify(data, null, 2)}</pre>
                        </div>
                    )}
                </div>
            )}
        />
    );
}
```

### Compound Components
```javascript
function Tabs({ children, defaultTab }) {
    const [activeTab, setActiveTab] = useState(defaultTab);
    
    return (
        <div className="tabs">
            {React.Children.map(children, child => {
                if (child.type === TabsList) {
                    return React.cloneElement(child, { activeTab, setActiveTab });
                }
                if (child.type === TabsContent) {
                    return React.cloneElement(child, { activeTab });
                }
                return child;
            })}
        </div>
    );
}

function TabsList({ children, activeTab, setActiveTab }) {
    return (
        <div className="tabs-list">
            {React.Children.map(children, child => {
                if (child.type === Tab) {
                    return React.cloneElement(child, { 
                        isActive: child.props.id === activeTab,
                        onClick: () => setActiveTab(child.props.id)
                    });
                }
                return child;
            })}
        </div>
    );
}

function Tab({ children, isActive, onClick }) {
    return (
        <button 
            className={`tab ${isActive ? 'active' : ''}`}
            onClick={onClick}
        >
            {children}
        </button>
    );
}

function TabsContent({ children, activeTab }) {
    return (
        <div className="tabs-content">
            {React.Children.map(children, child => {
                if (child.type === TabPanel) {
                    return React.cloneElement(child, { 
                        isActive: child.props.id === activeTab 
                    });
                }
                return child;
            })}
        </div>
    );
}

function TabPanel({ children, isActive }) {
    return isActive ? (
        <div className="tab-panel">{children}</div>
    ) : null;
}

// Kullanım
function App() {
    return (
        <Tabs defaultTab="tab1">
            <TabsList>
                <Tab id="tab1">Tab 1</Tab>
                <Tab id="tab2">Tab 2</Tab>
                <Tab id="tab3">Tab 3</Tab>
            </TabsList>
            
            <TabsContent>
                <TabPanel id="tab1">
                    <h3>Tab 1 İçeriği</h3>
                    <p>Bu tab 1'in içeriğidir.</p>
                </TabPanel>
                <TabPanel id="tab2">
                    <h3>Tab 2 İçeriği</h3>
                    <p>Bu tab 2'nin içeriğidir.</p>
                </TabPanel>
                <TabPanel id="tab3">
                    <h3>Tab 3 İçeriği</h3>
                    <p>Bu tab 3'ün içeriğidir.</p>
                </TabPanel>
            </TabsContent>
        </Tabs>
    );
}
```

---

## 🔄 Higher-Order Components (HOC)

HOC'lar, component'leri alıp yeni component'ler döndüren fonksiyonlardır.

### Basit HOC
```javascript
function withLoading(WrappedComponent) {
    return function WithLoadingComponent({ isLoading, ...props }) {
        if (isLoading) {
            return <div>Yükleniyor...</div>;
        }
        return <WrappedComponent {...props} />;
    };
}

function UserProfile({ user }) {
    return (
        <div>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
        </div>
    );
}

const UserProfileWithLoading = withLoading(UserProfile);

function App() {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        const fetchUser = async () => {
            const response = await fetch('/api/user');
            const userData = await response.json();
            setUser(userData);
            setLoading(false);
        };
        
        fetchUser();
    }, []);
    
    return (
        <UserProfileWithLoading 
            user={user} 
            isLoading={loading} 
        />
    );
}
```

### Karmaşık HOC
```javascript
function withAuth(WrappedComponent) {
    return function WithAuthComponent(props) {
        const [user, setUser] = useState(null);
        const [loading, setLoading] = useState(true);
        
        useEffect(() => {
            const checkAuth = async () => {
                try {
                    const token = localStorage.getItem('token');
                    if (token) {
                        const response = await fetch('/api/verify', {
                            headers: { Authorization: `Bearer ${token}` }
                        });
                        if (response.ok) {
                            const userData = await response.json();
                            setUser(userData);
                        }
                    }
                } catch (error) {
                    console.error('Auth kontrolü hatası:', error);
                } finally {
                    setLoading(false);
                }
            };
            
            checkAuth();
        }, []);
        
        if (loading) {
            return <div>Kimlik doğrulanıyor...</div>;
        }
        
        if (!user) {
            return <div>Lütfen giriş yapın</div>;
        }
        
        return <WrappedComponent {...props} user={user} />;
    };
}

function Dashboard({ user }) {
    return (
        <div>
            <h1>Hoş geldin, {user.name}!</h1>
            <p>Bu dashboard sadece giriş yapmış kullanıcılar için.</p>
        </div>
    );
}

const ProtectedDashboard = withAuth(Dashboard);
```

---

## 🚨 Error Boundaries

Error boundaries, component tree'deki JavaScript hatalarını yakalar ve fallback UI gösterir.

### Class Component Error Boundary
```javascript
class ErrorBoundary extends React.Component {
    constructor(props) {
        super(props);
        this.state = { hasError: false, error: null };
    }
    
    static getDerivedStateFromError(error) {
        return { hasError: true, error };
    }
    
    componentDidCatch(error, errorInfo) {
        console.error('Error Boundary yakaladı:', error, errorInfo);
    }
    
    render() {
        if (this.state.hasError) {
            return (
                <div className="error-boundary">
                    <h2>Bir hata oluştu!</h2>
                    <p>Üzgünüz, beklenmeyen bir hata meydana geldi.</p>
                    <button onClick={() => this.setState({ hasError: false, error: null })}>
                        Tekrar Dene
                    </button>
                </div>
            );
        }
        
        return this.props.children;
    }
}

function BuggyComponent() {
    const [shouldError, setShouldError] = useState(false);
    
    if (shouldError) {
        throw new Error('Bu bir test hatasıdır!');
    }
    
    return (
        <div>
            <h3>Bu component hata verebilir</h3>
            <button onClick={() => setShouldError(true)}>
                Hata Ver
            </button>
        </div>
    );
}

function App() {
    return (
        <ErrorBoundary>
            <BuggyComponent />
        </ErrorBoundary>
    );
}
```

### Hook ile Error Boundary (react-error-boundary)
```javascript
import { ErrorBoundary } from 'react-error-boundary';

function ErrorFallback({ error, resetErrorBoundary }) {
    return (
        <div className="error-fallback">
            <h2>Bir hata oluştu!</h2>
            <pre>{error.message}</pre>
            <button onClick={resetErrorBoundary}>Tekrar Dene</button>
        </div>
    );
}

function App() {
    return (
        <ErrorBoundary
            FallbackComponent={ErrorFallback}
            onError={(error, errorInfo) => {
                console.error('Hata yakalandı:', error, errorInfo);
            }}
        >
            <BuggyComponent />
        </ErrorBoundary>
    );
}
```

---

## ⚡ Performance Optimization

React uygulamalarında performans optimizasyonu için çeşitli teknikler kullanılır.

### React.memo
```javascript
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data }) {
    // Karmaşık hesaplama
    const processedData = useMemo(() => {
        return data.map(item => ({
            ...item,
            processed: item.value * 2
        }));
    }, [data]);
    
    return (
        <div>
            {processedData.map(item => (
                <div key={item.id}>{item.processed}</div>
            ))}
        </div>
    );
});

function App() {
    const [data, setData] = useState([]);
    const [count, setCount] = useState(0);
    
    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                Count: {count}
            </button>
            <ExpensiveComponent data={data} />
        </div>
    );
}
```

### useMemo
```javascript
function ExpensiveCalculation({ items }) {
    const expensiveValue = useMemo(() => {
        console.log('Karmaşık hesaplama yapılıyor...');
        return items.reduce((sum, item) => sum + item.value, 0);
    }, [items]);
    
    return <div>Toplam: {expensiveValue}</div>;
}

function App() {
    const [items, setItems] = useState([
        { id: 1, value: 10 },
        { id: 2, value: 20 }
    ]);
    const [count, setCount] = useState(0);
    
    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                Count: {count}
            </button>
            <ExpensiveCalculation items={items} />
        </div>
    );
}
```

### useCallback
```javascript
function TodoList({ todos, onToggle, onDelete }) {
    return (
        <ul>
            {todos.map(todo => (
                <TodoItem
                    key={todo.id}
                    todo={todo}
                    onToggle={onToggle}
                    onDelete={onDelete}
                />
            ))}
        </ul>
    );
}

const TodoItem = React.memo(function TodoItem({ todo, onToggle, onDelete }) {
    return (
        <li>
            <span onClick={() => onToggle(todo.id)}>
                {todo.text}
            </span>
            <button onClick={() => onDelete(todo.id)}>Sil</button>
        </li>
    );
});

function App() {
    const [todos, setTodos] = useState([
        { id: 1, text: 'React öğren', completed: false }
    ]);
    
    const handleToggle = useCallback((id) => {
        setTodos(prev => prev.map(todo =>
            todo.id === id ? { ...todo, completed: !todo.completed } : todo
        ));
    }, []);
    
    const handleDelete = useCallback((id) => {
        setTodos(prev => prev.filter(todo => todo.id !== id));
    }, []);
    
    return (
        <TodoList
            todos={todos}
            onToggle={handleToggle}
            onDelete={handleDelete}
        />
    );
}
```

---

## 📦 Code Splitting

Code splitting, uygulamanın kodunu daha küçük parçalara böler ve ihtiyaç duyulduğunda yükler.

### React.lazy ile Code Splitting
```javascript
import { Suspense, lazy } from 'react';

// Lazy loading ile component yükleme
const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
    const [showLazy, setShowLazy] = useState(false);
    
    return (
        <div>
            <button onClick={() => setShowLazy(true)}>
                Lazy Component Yükle
            </button>
            
            {showLazy && (
                <Suspense fallback={<div>Yükleniyor...</div>}>
                    <LazyComponent />
                </Suspense>
            )}
        </div>
    );
}
```

### Route-based Code Splitting
```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Suspense, lazy } from 'react';

const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Contact = lazy(() => import('./pages/Contact'));

function App() {
    return (
        <BrowserRouter>
            <Suspense fallback={<div>Sayfa yükleniyor...</div>}>
                <Routes>
                    <Route path="/" element={<Home />} />
                    <Route path="/about" element={<About />} />
                    <Route path="/contact" element={<Contact />} />
                </Routes>
            </Suspense>
        </BrowserRouter>
    );
}
```

---

## 🚀 Lazy Loading

Lazy loading, component'leri ve verileri ihtiyaç duyulduğunda yükler.

### Intersection Observer ile Lazy Loading
```javascript
function useIntersectionObserver(options = {}) {
    const [isIntersecting, setIsIntersecting] = useState(false);
    const [ref, setRef] = useState(null);
    
    useEffect(() => {
        if (!ref) return;
        
        const observer = new IntersectionObserver(([entry]) => {
            setIsIntersecting(entry.isIntersecting);
        }, options);
        
        observer.observe(ref);
        
        return () => {
            observer.unobserve(ref);
        };
    }, [ref, options]);
    
    return [setRef, isIntersecting];
}

function LazyImage({ src, alt, ...props }) {
    const [ref, isIntersecting] = useIntersectionObserver();
    const [imageSrc, setImageSrc] = useState(null);
    
    useEffect(() => {
        if (isIntersecting && !imageSrc) {
            setImageSrc(src);
        }
    }, [isIntersecting, src, imageSrc]);
    
    return (
        <div ref={ref} {...props}>
            {imageSrc ? (
                <img src={imageSrc} alt={alt} />
            ) : (
                <div style={{ height: '200px', backgroundColor: '#f0f0f0' }}>
                    Yükleniyor...
                </div>
            )}
        </div>
    );
}

function ImageGallery() {
    const images = [
        { id: 1, src: '/image1.jpg', alt: 'Resim 1' },
        { id: 2, src: '/image2.jpg', alt: 'Resim 2' },
        { id: 3, src: '/image3.jpg', alt: 'Resim 3' }
    ];
    
    return (
        <div>
            {images.map(image => (
                <LazyImage
                    key={image.id}
                    src={image.src}
                    alt={image.alt}
                    style={{ margin: '20px 0' }}
                />
            ))}
        </div>
    );
}
```

---

## 🛣️ React Router

React Router, single-page application'larda sayfa yönlendirmesi için kullanılır.

### Temel Router Kurulumu
```javascript
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
    return (
        <BrowserRouter>
            <nav>
                <Link to="/">Ana Sayfa</Link>
                <Link to="/about">Hakkında</Link>
                <Link to="/contact">İletişim</Link>
            </nav>
            
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/about" element={<About />} />
                <Route path="/contact" element={<Contact />} />
            </Routes>
        </BrowserRouter>
    );
}

function Home() {
    return <h1>Ana Sayfa</h1>;
}

function About() {
    return <h1>Hakkında</h1>;
}

function Contact() {
    return <h1>İletişim</h1>;
}
```

### Nested Routes
```javascript
function App() {
    return (
        <BrowserRouter>
            <Routes>
                <Route path="/" element={<Layout />}>
                    <Route index element={<Home />} />
                    <Route path="about" element={<About />} />
                    <Route path="users" element={<Users />}>
                        <Route index element={<UserList />} />
                        <Route path=":userId" element={<UserDetail />} />
                    </Route>
                </Route>
            </Routes>
        </BrowserRouter>
    );
}

function Layout() {
    return (
        <div>
            <nav>
                <Link to="/">Ana Sayfa</Link>
                <Link to="/about">Hakkında</Link>
                <Link to="/users">Kullanıcılar</Link>
            </nav>
            <Outlet />
        </div>
    );
}
```

### Programmatic Navigation
```javascript
import { useNavigate, useParams } from 'react-router-dom';

function UserDetail() {
    const { userId } = useParams();
    const navigate = useNavigate();
    const [user, setUser] = useState(null);
    
    useEffect(() => {
        const fetchUser = async () => {
            const response = await fetch(`/api/users/${userId}`);
            const userData = await response.json();
            setUser(userData);
        };
        
        fetchUser();
    }, [userId]);
    
    const handleEdit = () => {
        navigate(`/users/${userId}/edit`);
    };
    
    const handleBack = () => {
        navigate(-1); // Geri git
    };
    
    if (!user) return <div>Yükleniyor...</div>;
    
    return (
        <div>
            <button onClick={handleBack}>Geri</button>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
            <button onClick={handleEdit}>Düzenle</button>
        </div>
    );
}
```

---

## 🌐 API Integration

React uygulamalarında API entegrasyonu için çeşitli yöntemler kullanılır.

### Fetch API
```javascript
function useApi(url) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        const fetchData = async () => {
            try {
                setLoading(true);
                setError(null);
                const response = await fetch(url);
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const result = await response.json();
                setData(result);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };
        
        fetchData();
    }, [url]);
    
    return { data, loading, error };
}

function UserList() {
    const { data: users, loading, error } = useApi('/api/users');
    
    if (loading) return <div>Yükleniyor...</div>;
    if (error) return <div>Hata: {error}</div>;
    
    return (
        <ul>
            {users?.map(user => (
                <li key={user.id}>
                    <h3>{user.name}</h3>
                    <p>{user.email}</p>
                </li>
            ))}
        </ul>
    );
}
```

### Axios ile API
```javascript
import axios from 'axios';

const api = axios.create({
    baseURL: 'https://api.example.com',
    timeout: 5000,
    headers: {
        'Content-Type': 'application/json'
    }
});

// Request interceptor
api.interceptors.request.use(
    (config) => {
        const token = localStorage.getItem('token');
        if (token) {
            config.headers.Authorization = `Bearer ${token}`;
        }
        return config;
    },
    (error) => {
        return Promise.reject(error);
    }
);

// Response interceptor
api.interceptors.response.use(
    (response) => response,
    (error) => {
        if (error.response?.status === 401) {
            localStorage.removeItem('token');
            window.location.href = '/login';
        }
        return Promise.reject(error);
    }
);

function useApiWithAxios(url) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        const fetchData = async () => {
            try {
                setLoading(true);
                setError(null);
                const response = await api.get(url);
                setData(response.data);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };
        
        fetchData();
    }, [url]);
    
    return { data, loading, error };
}
```

### CRUD Operations
```javascript
function UserManager() {
    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        fetchUsers();
    }, []);
    
    const fetchUsers = async () => {
        try {
            const response = await api.get('/users');
            setUsers(response.data);
        } catch (error) {
            console.error('Kullanıcılar yüklenemedi:', error);
        } finally {
            setLoading(false);
        }
    };
    
    const createUser = async (userData) => {
        try {
            const response = await api.post('/users', userData);
            setUsers(prev => [...prev, response.data]);
            return response.data;
        } catch (error) {
            console.error('Kullanıcı oluşturulamadı:', error);
            throw error;
        }
    };
    
    const updateUser = async (id, userData) => {
        try {
            const response = await api.put(`/users/${id}`, userData);
            setUsers(prev => prev.map(user => 
                user.id === id ? response.data : user
            ));
            return response.data;
        } catch (error) {
            console.error('Kullanıcı güncellenemedi:', error);
            throw error;
        }
    };
    
    const deleteUser = async (id) => {
        try {
            await api.delete(`/users/${id}`);
            setUsers(prev => prev.filter(user => user.id !== id));
        } catch (error) {
            console.error('Kullanıcı silinemedi:', error);
            throw error;
        }
    };
    
    if (loading) return <div>Yükleniyor...</div>;
    
    return (
        <div>
            <h2>Kullanıcı Yönetimi</h2>
            <UserForm onSubmit={createUser} />
            <UserList 
                users={users} 
                onUpdate={updateUser}
                onDelete={deleteUser}
            />
        </div>
    );
}
```

