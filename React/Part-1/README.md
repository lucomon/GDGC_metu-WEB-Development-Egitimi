# React Temelleri - Eğitim Materyali

## 📋 İçindekiler

- [React Nedir?](#react-nedir)
- [React Kurulumu](#react-kurulumu)
- [JSX (JavaScript XML)](#jsx-javascript-xml)
- [Components (Bileşenler)](#components-bileşenler)
- [Props (Özellikler)](#props-özellikler)
- [State (Durum)](#state-durum)
- [Event Handling](#event-handling)
- [Conditional Rendering](#conditional-rendering)
- [Lists ve Keys](#lists-ve-keys)
- [Forms](#forms)
- [CSS ve Styling](#css-ve-styling)
- [Component Lifecycle](#component-lifecycle)

---

## 🚀 React Nedir?

React, Facebook tarafından geliştirilen ve kullanıcı arayüzleri oluşturmak için kullanılan açık kaynaklı bir JavaScript kütüphanesidir. React, component tabanlı mimarisi ile modern web uygulamaları geliştirmeyi kolaylaştırır.

### React'in Temel Özellikleri:
- **Component Tabanlı** - Yeniden kullanılabilir UI parçaları
- **Virtual DOM** - Performanslı DOM güncellemeleri
- **Declarative** - Ne yapmak istediğinizi söylersiniz, nasıl yapılacağını React halleder
- **Unidirectional Data Flow** - Tek yönlü veri akışı
- **Ekosistem** - Zengin kütüphane ve araç desteği

### React vs Vanilla JavaScript:
```javascript
// Vanilla JavaScript
const button = document.createElement('button');
button.textContent = 'Tıkla';
button.addEventListener('click', () => {
    alert('Merhaba!');
});
document.body.appendChild(button);

// React
function Button() {
    const handleClick = () => {
        alert('Merhaba!');
    };
    
    return <button onClick={handleClick}>Tıkla</button>;
}
```

---

## ⚙️ React Kurulumu

React uygulaması oluşturmanın birkaç yolu vardır. En popüler yöntemler Create React App ve Vite'dır.

### Create React App ile Kurulum
```bash
# Yeni React uygulaması oluşturma
npx create-react-app my-app

# Uygulamayı çalıştırma
cd my-app
npm start
```

### Vite ile Kurulum (Önerilen)
```bash
# Vite ile React uygulaması oluşturma
npm create vite@latest my-react-app -- --template react

# Bağımlılıkları yükleme
cd my-react-app
npm install

# Uygulamayı çalıştırma
npm run dev
```

### Proje Yapısı
```
my-react-app/
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── App.js
│   ├── index.js
│   └── App.css
├── package.json
└── README.md
```

---

## 📝 JSX (JavaScript XML)

JSX, JavaScript içinde HTML benzeri syntax kullanmanızı sağlayan bir JavaScript uzantısıdır. JSX, React component'lerini yazmayı kolaylaştırır.

### JSX Temelleri
```javascript
// JSX kullanımı
const element = <h1>Merhaba, React!</h1>;

// JavaScript ifadeleri JSX içinde
const name = "Faruk";
const element = <h1>Merhaba, {name}!</h1>;

// JSX attribute'ları
const element = <div className="container" id="main">İçerik</div>;
```

### JSX Kuralları
```javascript
// 1. Tek root element
function App() {
    return (
        <div>
            <h1>Başlık</h1>
            <p>Paragraf</p>
        </div>
    );
}

// 2. Self-closing tags
const element = <img src="image.jpg" alt="Resim" />;

// 3. className kullanımı (class değil)
const element = <div className="my-class">İçerik</div>;

// 4. JavaScript ifadeleri süslü parantez içinde
const count = 5;
const element = <p>Sayı: {count}</p>;
```

### JSX İçinde Koşullu İfadeler
```javascript
function Greeting({ isLoggedIn, name }) {
    return (
        <div>
            {isLoggedIn ? (
                <h1>Hoş geldin, {name}!</h1>
            ) : (
                <h1>Lütfen giriş yapın</h1>
            )}
        </div>
    );
}
```

---

## 🧩 Components (Bileşenler)

React'te her şey component'tir. Component'ler, UI'ın yeniden kullanılabilir parçalarıdır.

### Function Component
```javascript
// Basit function component
function Welcome() {
    return <h1>Merhaba, React!</h1>;
}

// Arrow function ile
const Welcome = () => {
    return <h1>Merhaba, React!</h1>;
};

// Tek satır return
const Welcome = () => <h1>Merhaba, React!</h1>;
```

### Component Kullanımı
```javascript
// App.js
import Welcome from './Welcome';

function App() {
    return (
        <div>
            <Welcome />
            <Welcome />
            <Welcome />
        </div>
    );
}

export default App;
```

### Component İçinde Component
```javascript
function Header() {
    return (
        <header>
            <h1>Web Sitem</h1>
            <nav>
                <a href="/">Ana Sayfa</a>
                <a href="/about">Hakkında</a>
            </nav>
        </header>
    );
}

function Main() {
    return (
        <main>
            <h2>Ana İçerik</h2>
            <p>Bu sayfanın ana içeriği burada yer alır.</p>
        </main>
    );
}

function App() {
    return (
        <div>
            <Header />
            <Main />
        </div>
    );
}
```

---

## 📦 Props (Özellikler)

Props, component'ler arasında veri geçirmek için kullanılır. Props, read-only'dir ve değiştirilemez.

### Props Kullanımı
```javascript
// Props alan component
function Greeting(props) {
    return <h1>Merhaba, {props.name}!</h1>;
}

// Kullanım
function App() {
    return (
        <div>
            <Greeting name="Faruk" />
            <Greeting name="Ayşe" />
            <Greeting name="Mehmet" />
        </div>
    );
}
```

### Destructuring ile Props
```javascript
// Destructuring ile props kullanımı
function Greeting({ name, age }) {
    return (
        <div>
            <h1>Merhaba, {name}!</h1>
            <p>Yaşınız: {age}</p>
        </div>
    );
}

// Kullanım
function App() {
    return (
        <div>
            <Greeting name="Faruk" age={20} />
            <Greeting name="Ayşe" age={25} />
        </div>
    );
}
```

### Default Props
```javascript
// Default props tanımlama
function Greeting({ name = "Misafir", age = 0 }) {
    return (
        <div>
            <h1>Merhaba, {name}!</h1>
            <p>Yaşınız: {age}</p>
        </div>
    );
}

// Kullanım
function App() {
    return (
        <div>
            <Greeting name="Faruk" age={20} />
            <Greeting /> {/* Default değerler kullanılır */}
        </div>
    );
}
```

### Props Validation (PropTypes)
```javascript
import PropTypes from 'prop-types';

function User({ name, age, email }) {
    return (
        <div>
            <h2>{name}</h2>
            <p>Yaş: {age}</p>
            <p>E-posta: {email}</p>
        </div>
    );
}

// PropTypes tanımlama
User.propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number.isRequired,
    email: PropTypes.string.isRequired
};

// Kullanım
function App() {
    return (
        <User 
            name="Faruk" 
            age={20} 
            email="faruk@example.com" 
        />
    );
}
```

---

## 🔄 State (Durum)

State, component'in iç verisini tutar ve değiştiğinde component'i yeniden render eder.

### useState Hook
```javascript
import { useState } from 'react';

function Counter() {
    // State tanımlama
    const [count, setCount] = useState(0);
    
    return (
        <div>
            <p>Sayı: {count}</p>
            <button onClick={() => setCount(count + 1)}>
                Artır
            </button>
            <button onClick={() => setCount(count - 1)}>
                Azalt
            </button>
        </div>
    );
}
```

### Çoklu State
```javascript
function UserForm() {
    const [name, setName] = useState('');
    const [age, setAge] = useState(0);
    const [email, setEmail] = useState('');
    
    return (
        <form>
            <input 
                type="text" 
                placeholder="İsim" 
                value={name}
                onChange={(e) => setName(e.target.value)}
            />
            <input 
                type="number" 
                placeholder="Yaş" 
                value={age}
                onChange={(e) => setAge(parseInt(e.target.value))}
            />
            <input 
                type="email" 
                placeholder="E-posta" 
                value={email}
                onChange={(e) => setEmail(e.target.value)}
            />
        </form>
    );
}
```

### Object State
```javascript
function UserProfile() {
    const [user, setUser] = useState({
        name: 'Faruk',
        age: 20,
        email: 'faruk@example.com'
    });
    
    const updateName = () => {
        setUser({
            ...user,
            name: 'Yeni İsim'
        });
    };
    
    return (
        <div>
            <h2>{user.name}</h2>
            <p>Yaş: {user.age}</p>
            <p>E-posta: {user.email}</p>
            <button onClick={updateName}>İsmi Güncelle</button>
        </div>
    );
}
```

### Array State
```javascript
function TodoList() {
    const [todos, setTodos] = useState([
        { id: 1, text: 'React öğren', completed: false },
        { id: 2, text: 'Proje yap', completed: true }
    ]);
    
    const addTodo = () => {
        const newTodo = {
            id: Date.now(),
            text: 'Yeni görev',
            completed: false
        };
        setTodos([...todos, newTodo]);
    };
    
    return (
        <div>
            <ul>
                {todos.map(todo => (
                    <li key={todo.id}>
                        {todo.text} - {todo.completed ? 'Tamamlandı' : 'Beklemede'}
                    </li>
                ))}
            </ul>
            <button onClick={addTodo}>Görev Ekle</button>
        </div>
    );
}
```

---

## 🎯 Event Handling

React'te event handling, HTML'deki event handling'e benzer ancak bazı farklılıklar vardır.

### Temel Event Handling
```javascript
function Button() {
    const handleClick = () => {
        alert('Butona tıklandı!');
    };
    
    return <button onClick={handleClick}>Tıkla</button>;
}
```

### Event Parametresi
```javascript
function Input() {
    const handleChange = (event) => {
        console.log('Değer:', event.target.value);
    };
    
    return (
        <input 
            type="text" 
            onChange={handleChange}
            placeholder="Bir şeyler yazın"
        />
    );
}
```

### Çoklu Event Handler
```javascript
function Form() {
    const [formData, setFormData] = useState({
        name: '',
        email: ''
    });
    
    const handleInputChange = (event) => {
        const { name, value } = event.target;
        setFormData({
            ...formData,
            [name]: value
        });
    };
    
    const handleSubmit = (event) => {
        event.preventDefault();
        console.log('Form gönderildi:', formData);
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input 
                type="text" 
                name="name"
                value={formData.name}
                onChange={handleInputChange}
                placeholder="İsim"
            />
            <input 
                type="email" 
                name="email"
                value={formData.email}
                onChange={handleInputChange}
                placeholder="E-posta"
            />
            <button type="submit">Gönder</button>
        </form>
    );
}
```

### Event Handler ile State Güncelleme
```javascript
function Counter() {
    const [count, setCount] = useState(0);
    
    const increment = () => setCount(count + 1);
    const decrement = () => setCount(count - 1);
    const reset = () => setCount(0);
    
    return (
        <div>
            <h2>Sayı: {count}</h2>
            <button onClick={increment}>+</button>
            <button onClick={decrement}>-</button>
            <button onClick={reset}>Sıfırla</button>
        </div>
    );
}
```

---

## 🔀 Conditional Rendering

React'te koşullu rendering, JavaScript'teki koşullu ifadelerle yapılır.

### if/else ile Conditional Rendering
```javascript
function Greeting({ isLoggedIn, name }) {
    if (isLoggedIn) {
        return <h1>Hoş geldin, {name}!</h1>;
    } else {
        return <h1>Lütfen giriş yapın</h1>;
    }
}
```

### Ternary Operator ile
```javascript
function UserStatus({ isOnline }) {
    return (
        <div>
            <h2>Kullanıcı Durumu</h2>
            {isOnline ? (
                <p style={{ color: 'green' }}>Çevrimiçi</p>
            ) : (
                <p style={{ color: 'red' }}>Çevrimdışı</p>
            )}
        </div>
    );
}
```

### Logical && Operator ile
```javascript
function Notification({ message, show }) {
    return (
        <div>
            {show && <div className="notification">{message}</div>}
        </div>
    );
}
```

### Karmaşık Conditional Rendering
```javascript
function UserProfile({ user }) {
    if (!user) {
        return <div>Kullanıcı bulunamadı</div>;
    }
    
    return (
        <div>
            <h2>{user.name}</h2>
            {user.age && <p>Yaş: {user.age}</p>}
            {user.email && <p>E-posta: {user.email}</p>}
            {user.isAdmin ? (
                <button>Admin Paneli</button>
            ) : (
                <button>Profil Düzenle</button>
            )}
        </div>
    );
}
```

---

## 📋 Lists ve Keys

React'te listeleri render etmek için `map()` fonksiyonu kullanılır ve her element için benzersiz bir `key` prop'u gerekir.

### Basit Liste
```javascript
function NumberList() {
    const numbers = [1, 2, 3, 4, 5];
    
    return (
        <ul>
            {numbers.map(number => (
                <li key={number}>{number}</li>
            ))}
        </ul>
    );
}
```

### Object Listesi
```javascript
function UserList() {
    const users = [
        { id: 1, name: 'Faruk', age: 20 },
        { id: 2, name: 'Ayşe', age: 25 },
        { id: 3, name: 'Mehmet', age: 30 }
    ];
    
    return (
        <ul>
            {users.map(user => (
                <li key={user.id}>
                    {user.name} - {user.age} yaşında
                </li>
            ))}
        </ul>
    );
}
```

### Liste ile State
```javascript
function TodoApp() {
    const [todos, setTodos] = useState([
        { id: 1, text: 'React öğren', completed: false },
        { id: 2, text: 'Proje yap', completed: true }
    ]);
    
    const toggleTodo = (id) => {
        setTodos(todos.map(todo => 
            todo.id === id 
                ? { ...todo, completed: !todo.completed }
                : todo
        ));
    };
    
    return (
        <div>
            <h2>Görevler</h2>
            <ul>
                {todos.map(todo => (
                    <li 
                        key={todo.id}
                        onClick={() => toggleTodo(todo.id)}
                        style={{ 
                            textDecoration: todo.completed ? 'line-through' : 'none',
                            cursor: 'pointer'
                        }}
                    >
                        {todo.text}
                    </li>
                ))}
            </ul>
        </div>
    );
}
```

### Key Kullanımı
```javascript
// Yanlış - index kullanımı
function BadList() {
    const items = ['A', 'B', 'C'];
    return (
        <ul>
            {items.map((item, index) => (
                <li key={index}>{item}</li> // Yanlış!
            ))}
        </ul>
    );
}

// Doğru - benzersiz ID kullanımı
function GoodList() {
    const items = [
        { id: 1, text: 'A' },
        { id: 2, text: 'B' },
        { id: 3, text: 'C' }
    ];
    return (
        <ul>
            {items.map(item => (
                <li key={item.id}>{item.text}</li> // Doğru!
            ))}
        </ul>
    );
}
```

---

## 📝 Forms

React'te form handling, controlled component'ler kullanılarak yapılır.

### Basit Form
```javascript
function SimpleForm() {
    const [name, setName] = useState('');
    
    const handleSubmit = (event) => {
        event.preventDefault();
        alert(`Merhaba, ${name}!`);
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <label>
                İsim:
                <input 
                    type="text" 
                    value={name}
                    onChange={(e) => setName(e.target.value)}
                />
            </label>
            <button type="submit">Gönder</button>
        </form>
    );
}
```

### Çoklu Input Form
```javascript
function ContactForm() {
    const [formData, setFormData] = useState({
        name: '',
        email: '',
        message: ''
    });
    
    const handleChange = (event) => {
        const { name, value } = event.target;
        setFormData({
            ...formData,
            [name]: value
        });
    };
    
    const handleSubmit = (event) => {
        event.preventDefault();
        console.log('Form verisi:', formData);
        // Form gönderme işlemi
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <div>
                <label>İsim:</label>
                <input 
                    type="text" 
                    name="name"
                    value={formData.name}
                    onChange={handleChange}
                />
            </div>
            
            <div>
                <label>E-posta:</label>
                <input 
                    type="email" 
                    name="email"
                    value={formData.email}
                    onChange={handleChange}
                />
            </div>
            
            <div>
                <label>Mesaj:</label>
                <textarea 
                    name="message"
                    value={formData.message}
                    onChange={handleChange}
                />
            </div>
            
            <button type="submit">Gönder</button>
        </form>
    );
}
```

### Select ve Checkbox
```javascript
function PreferencesForm() {
    const [preferences, setPreferences] = useState({
        country: 'tr',
        newsletter: false,
        notifications: true
    });
    
    const handleChange = (event) => {
        const { name, value, type, checked } = event.target;
        setPreferences({
            ...preferences,
            [name]: type === 'checkbox' ? checked : value
        });
    };
    
    return (
        <form>
            <div>
                <label>Ülke:</label>
                <select 
                    name="country"
                    value={preferences.country}
                    onChange={handleChange}
                >
                    <option value="tr">Türkiye</option>
                    <option value="us">Amerika</option>
                    <option value="uk">İngiltere</option>
                </select>
            </div>
            
            <div>
                <label>
                    <input 
                        type="checkbox" 
                        name="newsletter"
                        checked={preferences.newsletter}
                        onChange={handleChange}
                    />
                    Bülten almak istiyorum
                </label>
            </div>
            
            <div>
                <label>
                    <input 
                        type="checkbox" 
                        name="notifications"
                        checked={preferences.notifications}
                        onChange={handleChange}
                    />
                    Bildirimleri açık tut
                </label>
            </div>
        </form>
    );
}
```

---

## 🎨 CSS ve Styling

React'te CSS kullanmanın birkaç yolu vardır.

### Inline Styles
```javascript
function StyledComponent() {
    const styles = {
        container: {
            backgroundColor: '#f0f0f0',
            padding: '20px',
            borderRadius: '8px',
            margin: '10px'
        },
        title: {
            color: '#333',
            fontSize: '24px',
            fontWeight: 'bold'
        }
    };
    
    return (
        <div style={styles.container}>
            <h1 style={styles.title}>Başlık</h1>
            <p style={{ color: '#666' }}>Paragraf metni</p>
        </div>
    );
}
```

### CSS Classes
```javascript
// App.css
.container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

.title {
    color: #333;
    text-align: center;
    margin-bottom: 20px;
}

.button {
    background-color: #007bff;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.button:hover {
    background-color: #0056b3;
}

// App.js
import './App.css';

function App() {
    return (
        <div className="container">
            <h1 className="title">Başlık</h1>
            <button className="button">Tıkla</button>
        </div>
    );
}
```

### CSS Modules
```javascript
// Button.module.css
.button {
    background-color: #007bff;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.primary {
    background-color: #007bff;
}

.secondary {
    background-color: #6c757d;
}

// Button.js
import styles from './Button.module.css';

function Button({ variant = 'primary', children }) {
    return (
        <button className={`${styles.button} ${styles[variant]}`}>
            {children}
        </button>
    );
}
```

### Styled Components
```javascript
import styled from 'styled-components';

const StyledButton = styled.button`
    background-color: ${props => props.primary ? '#007bff' : '#6c757d'};
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    
    &:hover {
        opacity: 0.8;
    }
`;

const Container = styled.div`
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
`;

function App() {
    return (
        <Container>
            <StyledButton primary>Birincil Buton</StyledButton>
            <StyledButton>İkincil Buton</StyledButton>
        </Container>
    );
}
```

---

## 🔄 Component Lifecycle

React'te component'lerin yaşam döngüsü vardır. Function component'lerde bu lifecycle hook'lar ile yönetilir.

### useEffect Hook
```javascript
import { useState, useEffect } from 'react';

function Timer() {
    const [seconds, setSeconds] = useState(0);
    
    useEffect(() => {
        const interval = setInterval(() => {
            setSeconds(seconds => seconds + 1);
        }, 1000);
        
        // Cleanup function
        return () => clearInterval(interval);
    }, []); // Boş dependency array - sadece mount'ta çalışır
    
    return <div>Geçen süre: {seconds} saniye</div>;
}
```

### useEffect ile API Çağrısı
```javascript
function UserProfile({ userId }) {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        const fetchUser = async () => {
            try {
                setLoading(true);
                const response = await fetch(`/api/users/${userId}`);
                const userData = await response.json();
                setUser(userData);
            } catch (error) {
                console.error('Kullanıcı yüklenemedi:', error);
            } finally {
                setLoading(false);
            }
        };
        
        fetchUser();
    }, [userId]); // userId değiştiğinde tekrar çalışır
    
    if (loading) return <div>Yükleniyor...</div>;
    if (!user) return <div>Kullanıcı bulunamadı</div>;
    
    return (
        <div>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
        </div>
    );
}
```

### useEffect Cleanup
```javascript
function WindowSize() {
    const [windowSize, setWindowSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    useEffect(() => {
        const handleResize = () => {
            setWindowSize({
                width: window.innerWidth,
                height: window.innerHeight
            });
        };
        
        window.addEventListener('resize', handleResize);
        
        // Cleanup function - component unmount olduğunda çalışır
        return () => {
            window.removeEventListener('resize', handleResize);
        };
    }, []);
    
    return (
        <div>
            <p>Pencere boyutu: {windowSize.width} x {windowSize.height}</p>
        </div>
    );
}
```

### Multiple useEffect
```javascript
function DataComponent({ userId }) {
    const [user, setUser] = useState(null);
    const [posts, setPosts] = useState([]);
    
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
    
    return (
        <div>
            {user && <h2>{user.name}</h2>}
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

## 🎯 Özet

Bu eğitim materyalinde React'in temel kavramlarını öğrendik:

- **React Nedir?** - Modern UI kütüphanesi
- **Kurulum** - Create React App ve Vite
- **JSX** - JavaScript içinde HTML syntax
- **Components** - Yeniden kullanılabilir UI parçaları
- **Props** - Component'ler arası veri geçişi
- **State** - Component'in iç verisi
- **Event Handling** - Kullanıcı etkileşimleri
- **Conditional Rendering** - Koşullu görüntüleme
- **Lists ve Keys** - Dinamik listeler
- **Forms** - Form yönetimi
- **CSS ve Styling** - Görsel tasarım
- **Component Lifecycle** - useEffect hook

Bu temel bilgilerle artık basit React uygulamaları geliştirebilirsiniz. Bir sonraki seviyede daha gelişmiş konuları öğreneceğiz.
