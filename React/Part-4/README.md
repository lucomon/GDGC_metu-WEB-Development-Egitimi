# React'ta API Kullanımı - Detaylı Doküman

> **Eğitmen:** Faruk Bora Güvenkaya  
> **Tarih:** 2024  
> **Versiyon:** 1.0

## 📋 İçindekiler

- [API Nedir?](#api-nedir)
- [React'ta API Nasıl Çalışır?](#reactta-api-nasıl-çalışır)
- [Fetch API ile Çalışma](#fetch-api-ile-çalışma)
- [Axios ile Çalışma](#axios-ile-çalışma)
- [React Query / TanStack Query](#react-query--tanstack-query)
- [Custom Hooks ile API Yönetimi](#custom-hooks-ile-api-yönetimi)
- [Error Handling](#error-handling)
- [Loading States](#loading-states)
- [Best Practices](#best-practices)
- [Örnek Projeler](#örnek-projeler)

---

## 🌐 API Nedir?

API (Application Programming Interface), farklı yazılım sistemlerinin birbirleriyle iletişim kurmasını sağlayan bir arayüzdür. Web API'ları, HTTP protokolü üzerinden veri alışverişi yapılmasını sağlar.

### API'nin Temel Özellikleri:
- **HTTP Metodları**: GET, POST, PUT, DELETE, PATCH
- **Endpoint'ler**: API'nin erişim noktaları
- **Request/Response**: İstek ve yanıt yapısı
- **Status Code'lar**: İşlem durumunu gösteren kodlar (200, 404, 500, vb.)

---

## ⚛️ React'ta API Nasıl Çalışır?

React'ta API çağrıları genellikle şu şekilde çalışır:

1. **Component Mount Olduğunda**: `useEffect` hook'u ile API çağrısı yapılır
2. **Kullanıcı Etkileşiminde**: Event handler'lar ile API çağrısı yapılır
3. **State Güncellemesi**: API yanıtı state'e kaydedilir
4. **UI Güncellemesi**: State değiştiğinde component yeniden render olur

### React'ta API Çağrısı Akışı:

```
Component Render → useEffect Çalışır → API İsteği → 
Yanıt Gelir → State Güncellenir → Component Re-render → UI Güncellenir
```

---

## 📡 Fetch API ile Çalışma

Fetch API, tarayıcıda yerleşik olarak bulunan ve HTTP istekleri yapmak için kullanılan bir API'dir.

### Temel Fetch Kullanımı

```javascript
import { useState, useEffect } from 'react';

function UserList() {
    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        const fetchUsers = async () => {
            try {
                setLoading(true);
                setError(null);
                
                const response = await fetch('https://api.example.com/users');
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const data = await response.json();
                setUsers(data);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };
        
        fetchUsers();
    }, []);
    
    if (loading) return <div>Yükleniyor...</div>;
    if (error) return <div>Hata: {error}</div>;
    
    return (
        <ul>
            {users.map(user => (
                <li key={user.id}>
                    <h3>{user.name}</h3>
                    <p>{user.email}</p>
                </li>
            ))}
        </ul>
    );
}
```

### POST İsteği ile Fetch

```javascript
async function createUser(userData) {
    try {
        const response = await fetch('https://api.example.com/users', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(userData),
        });
        
        if (!response.ok) {
            throw new Error('Kullanıcı oluşturulamadı');
        }
        
        const newUser = await response.json();
        return newUser;
    } catch (error) {
        console.error('Hata:', error);
        throw error;
    }
}
```

### PUT ve DELETE İstekleri

```javascript
// PUT - Güncelleme
async function updateUser(id, userData) {
    const response = await fetch(`https://api.example.com/users/${id}`, {
        method: 'PUT',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(userData),
    });
    
    if (!response.ok) {
        throw new Error('Güncelleme başarısız');
    }
    
    return await response.json();
}

// DELETE - Silme
async function deleteUser(id) {
    const response = await fetch(`https://api.example.com/users/${id}`, {
        method: 'DELETE',
    });
    
    if (!response.ok) {
        throw new Error('Silme başarısız');
    }
}
```

---

## 🔧 Axios ile Çalışma

Axios, HTTP istekleri yapmak için kullanılan popüler bir JavaScript kütüphanesidir. Fetch API'ye göre daha fazla özellik sunar.

### Axios Kurulumu

```bash
npm install axios
```

### Axios Instance Oluşturma

```javascript
import axios from 'axios';

const api = axios.create({
    baseURL: 'https://api.example.com',
    timeout: 10000,
    headers: {
        'Content-Type': 'application/json',
    },
});

export default api;
```

### Request Interceptor (İstek Öncesi)

```javascript
api.interceptors.request.use(
    (config) => {
        // Her istekten önce çalışır
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
```

### Response Interceptor (Yanıt Sonrası)

```javascript
api.interceptors.response.use(
    (response) => {
        // Başarılı yanıtlar için
        return response;
    },
    (error) => {
        // Hata durumları için
        if (error.response?.status === 401) {
            // Unauthorized - Token geçersiz
            localStorage.removeItem('token');
            window.location.href = '/login';
        }
        return Promise.reject(error);
    }
);
```

### Axios ile GET İsteği

```javascript
import { useState, useEffect } from 'react';
import api from './api';

function UserList() {
    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        const fetchUsers = async () => {
            try {
                setLoading(true);
                const response = await api.get('/users');
                setUsers(response.data);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };
        
        fetchUsers();
    }, []);
    
    if (loading) return <div>Yükleniyor...</div>;
    if (error) return <div>Hata: {error}</div>;
    
    return (
        <ul>
            {users.map(user => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    );
}
```

### Axios ile POST, PUT, DELETE

```javascript
// POST
const createUser = async (userData) => {
    const response = await api.post('/users', userData);
    return response.data;
};

// PUT
const updateUser = async (id, userData) => {
    const response = await api.put(`/users/${id}`, userData);
    return response.data;
};

// DELETE
const deleteUser = async (id) => {
    await api.delete(`/users/${id}`);
};
```

---

## 🚀 React Query / TanStack Query

React Query, server state yönetimi için güçlü bir kütüphanedir. Otomatik caching, background refetching ve optimistic updates gibi özellikler sunar.

### React Query Kurulumu

```bash
npm install @tanstack/react-query
```

### QueryClient Kurulumu

```javascript
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

const queryClient = new QueryClient({
    defaultOptions: {
        queries: {
            staleTime: 5 * 60 * 1000, // 5 dakika
            cacheTime: 10 * 60 * 1000, // 10 dakika
            retry: 3,
            refetchOnWindowFocus: false,
        },
    },
});

function App() {
    return (
        <QueryClientProvider client={queryClient}>
            <UserList />
            <ReactQueryDevtools initialIsOpen={false} />
        </QueryClientProvider>
    );
}
```

### useQuery ile Veri Çekme

```javascript
import { useQuery } from '@tanstack/react-query';

function UserList() {
    const { data: users, isLoading, error } = useQuery({
        queryKey: ['users'],
        queryFn: async () => {
            const response = await fetch('/api/users');
            if (!response.ok) {
                throw new Error('Kullanıcılar yüklenemedi');
            }
            return response.json();
        },
    });
    
    if (isLoading) return <div>Yükleniyor...</div>;
    if (error) return <div>Hata: {error.message}</div>;
    
    return (
        <ul>
            {users?.map(user => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    );
}
```

### useMutation ile Veri Güncelleme

```javascript
import { useMutation, useQueryClient } from '@tanstack/react-query';

function CreateUser() {
    const queryClient = useQueryClient();
    
    const mutation = useMutation({
        mutationFn: async (userData) => {
            const response = await fetch('/api/users', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(userData),
            });
            if (!response.ok) {
                throw new Error('Kullanıcı oluşturulamadı');
            }
            return response.json();
        },
        onSuccess: () => {
            // Cache'i güncelle
            queryClient.invalidateQueries({ queryKey: ['users'] });
        },
    });
    
    const handleSubmit = (e) => {
        e.preventDefault();
        mutation.mutate({
            name: e.target.name.value,
            email: e.target.email.value,
        });
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input name="name" placeholder="İsim" />
            <input name="email" placeholder="E-posta" />
            <button type="submit" disabled={mutation.isLoading}>
                {mutation.isLoading ? 'Oluşturuluyor...' : 'Oluştur'}
            </button>
        </form>
    );
}
```

---

## 🎣 Custom Hooks ile API Yönetimi

Custom hook'lar, API çağrılarını yeniden kullanılabilir hale getirir.

### Basit Custom Hook

```javascript
import { useState, useEffect } from 'react';

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

// Kullanım
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

### Gelişmiş Custom Hook

```javascript
import { useState, useCallback } from 'react';

function useApiMutation(mutationFn) {
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState(null);
    const [data, setData] = useState(null);
    
    const mutate = useCallback(async (variables) => {
        setLoading(true);
        setError(null);
        
        try {
            const result = await mutationFn(variables);
            setData(result);
            return result;
        } catch (err) {
            setError(err.message);
            throw err;
        } finally {
            setLoading(false);
        }
    }, [mutationFn]);
    
    return { mutate, loading, error, data };
}

// Kullanım
function CreateUser() {
    const { mutate: createUser, loading, error } = useApiMutation(
        async (userData) => {
            const response = await fetch('/api/users', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(userData),
            });
            if (!response.ok) {
                throw new Error('Kullanıcı oluşturulamadı');
            }
            return response.json();
        }
    );
    
    const handleSubmit = async (userData) => {
        try {
            await createUser(userData);
            alert('Kullanıcı oluşturuldu!');
        } catch (err) {
            alert('Hata: ' + err.message);
        }
    };
    
    return (
        <form onSubmit={(e) => {
            e.preventDefault();
            handleSubmit({
                name: e.target.name.value,
                email: e.target.email.value,
            });
        }}>
            <input name="name" placeholder="İsim" />
            <input name="email" placeholder="E-posta" />
            <button type="submit" disabled={loading}>
                {loading ? 'Oluşturuluyor...' : 'Oluştur'}
            </button>
        </form>
    );
}
```

---

## 🚨 Error Handling

API çağrılarında hata yönetimi çok önemlidir.

### Try-Catch ile Hata Yönetimi

```javascript
async function fetchUsers() {
    try {
        const response = await fetch('/api/users');
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('API hatası:', error);
        throw error;
    }
}
```

### Merkezi Hata Yönetimi

```javascript
class ApiError extends Error {
    constructor(message, status, data) {
        super(message);
        this.name = 'ApiError';
        this.status = status;
        this.data = data;
    }
}

function handleApiError(error) {
    if (error.response) {
        // Server yanıt verdi ama hata var
        const { status, data } = error.response;
        
        switch (status) {
            case 400:
                return new ApiError('Geçersiz istek', 400, data);
            case 401:
                return new ApiError('Yetkisiz erişim', 401, data);
            case 404:
                return new ApiError('Bulunamadı', 404, data);
            case 500:
                return new ApiError('Sunucu hatası', 500, data);
            default:
                return new ApiError('Bilinmeyen hata', status, data);
        }
    } else if (error.request) {
        // İstek gönderildi ama yanıt alınamadı
        return new ApiError('Sunucuya ulaşılamadı', 0, null);
    } else {
        // İstek hazırlanırken hata oluştu
        return new ApiError(error.message, 0, null);
    }
}
```

### Error Boundary ile Hata Yönetimi

```javascript
import { ErrorBoundary } from 'react-error-boundary';

function ErrorFallback({ error, resetErrorBoundary }) {
    return (
        <div role="alert">
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
                // Hata raporlama servisine gönder
            }}
        >
            <UserList />
        </ErrorBoundary>
    );
}
```

---

## ⏳ Loading States

API çağrıları sırasında loading durumunu göstermek önemlidir.

### Basit Loading State

```javascript
function UserList() {
    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        const fetchUsers = async () => {
            setLoading(true);
            const response = await fetch('/api/users');
            const data = await response.json();
            setUsers(data);
            setLoading(false);
        };
        
        fetchUsers();
    }, []);
    
    if (loading) {
        return <div>Yükleniyor...</div>;
    }
    
    return (
        <ul>
            {users.map(user => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    );
}
```

### Skeleton Loading

```javascript
function UserSkeleton() {
    return (
        <div className="skeleton">
            <div className="skeleton-avatar"></div>
            <div className="skeleton-text"></div>
            <div className="skeleton-text"></div>
        </div>
    );
}

function UserList() {
    const { data: users, isLoading } = useQuery({
        queryKey: ['users'],
        queryFn: fetchUsers,
    });
    
    if (isLoading) {
        return (
            <div>
                {[1, 2, 3].map(i => (
                    <UserSkeleton key={i} />
                ))}
            </div>
        );
    }
    
    return (
        <ul>
            {users?.map(user => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    );
}
```

---

## ✅ Best Practices

### 1. API Service Layer Kullan

```javascript
// services/userService.js
import api from './api';

export const userService = {
    getAll: () => api.get('/users'),
    getById: (id) => api.get(`/users/${id}`),
    create: (data) => api.post('/users', data),
    update: (id, data) => api.put(`/users/${id}`, data),
    delete: (id) => api.delete(`/users/${id}`),
};
```

### 2. Environment Variables Kullan

```javascript
// .env
REACT_APP_API_URL=https://api.example.com

// api.js
const api = axios.create({
    baseURL: process.env.REACT_APP_API_URL,
});
```

### 3. Request Cancellation

```javascript
useEffect(() => {
    const controller = new AbortController();
    
    const fetchData = async () => {
        try {
            const response = await fetch('/api/users', {
                signal: controller.signal,
            });
            const data = await response.json();
            setUsers(data);
        } catch (error) {
            if (error.name !== 'AbortError') {
                console.error('Hata:', error);
            }
        }
    };
    
    fetchData();
    
    return () => {
        controller.abort();
    };
}, []);
```

### 4. Debouncing ile Arama

```javascript
import { useState, useEffect } from 'react';

function useDebounce(value, delay) {
    const [debouncedValue, setDebouncedValue] = useState(value);
    
    useEffect(() => {
        const handler = setTimeout(() => {
            setDebouncedValue(value);
        }, delay);
        
        return () => {
            clearTimeout(handler);
        };
    }, [value, delay]);
    
    return debouncedValue;
}

function UserSearch() {
    const [searchTerm, setSearchTerm] = useState('');
    const debouncedSearchTerm = useDebounce(searchTerm, 500);
    
    const { data: users } = useQuery({
        queryKey: ['users', 'search', debouncedSearchTerm],
        queryFn: () => searchUsers(debouncedSearchTerm),
        enabled: debouncedSearchTerm.length > 2,
    });
    
    return (
        <div>
            <input
                value={searchTerm}
                onChange={(e) => setSearchTerm(e.target.value)}
                placeholder="Kullanıcı ara..."
            />
            <ul>
                {users?.map(user => (
                    <li key={user.id}>{user.name}</li>
                ))}
            </ul>
        </div>
    );
}
```

### 5. Optimistic Updates

```javascript
const mutation = useMutation({
    mutationFn: updateUser,
    onMutate: async (newUser) => {
        // Optimistic update
        await queryClient.cancelQueries({ queryKey: ['users'] });
        const previousUsers = queryClient.getQueryData(['users']);
        
        queryClient.setQueryData(['users'], (old) =>
            old.map(user => user.id === newUser.id ? newUser : user)
        );
        
        return { previousUsers };
    },
    onError: (err, newUser, context) => {
        // Rollback
        queryClient.setQueryData(['users'], context.previousUsers);
    },
    onSettled: () => {
        queryClient.invalidateQueries({ queryKey: ['users'] });
    },
});
```

---

## 📚 Örnek Projeler

### 1. Basit CRUD Uygulaması

```javascript
// UserManager.jsx
import { useState, useEffect } from 'react';
import api from './api';

function UserManager() {
    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    const [editingUser, setEditingUser] = useState(null);
    
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
        } catch (error) {
            console.error('Kullanıcı oluşturulamadı:', error);
        }
    };
    
    const updateUser = async (id, userData) => {
        try {
            const response = await api.put(`/users/${id}`, userData);
            setUsers(prev => prev.map(user =>
                user.id === id ? response.data : user
            ));
            setEditingUser(null);
        } catch (error) {
            console.error('Kullanıcı güncellenemedi:', error);
        }
    };
    
    const deleteUser = async (id) => {
        try {
            await api.delete(`/users/${id}`);
            setUsers(prev => prev.filter(user => user.id !== id));
        } catch (error) {
            console.error('Kullanıcı silinemedi:', error);
        }
    };
    
    if (loading) return <div>Yükleniyor...</div>;
    
    return (
        <div>
            <h2>Kullanıcı Yönetimi</h2>
            <UserForm onSubmit={createUser} />
            <UserList
                users={users}
                onEdit={setEditingUser}
                onUpdate={updateUser}
                onDelete={deleteUser}
                editingUser={editingUser}
            />
        </div>
    );
}
```

### 2. Infinite Scroll ile Liste

```javascript
import { useInfiniteQuery } from '@tanstack/react-query';

function InfiniteUserList() {
    const {
        data,
        fetchNextPage,
        hasNextPage,
        isFetchingNextPage,
        isLoading,
    } = useInfiniteQuery({
        queryKey: ['users', 'infinite'],
        queryFn: async ({ pageParam = 1 }) => {
            const response = await fetch(`/api/users?page=${pageParam}&limit=10`);
            return response.json();
        },
        getNextPageParam: (lastPage) => lastPage.nextPage,
    });
    
    if (isLoading) return <div>Yükleniyor...</div>;
    
    return (
        <div>
            {data.pages.map((page, i) => (
                <div key={i}>
                    {page.users.map(user => (
                        <div key={user.id}>{user.name}</div>
                    ))}
                </div>
            ))}
            {hasNextPage && (
                <button
                    onClick={() => fetchNextPage()}
                    disabled={isFetchingNextPage}
                >
                    {isFetchingNextPage ? 'Yükleniyor...' : 'Daha Fazla'}
                </button>
            )}
        </div>
    );
}
```

---

## 🎯 Özet

React'ta API kullanımı için:

1. **Fetch API**: Basit istekler için
2. **Axios**: Daha fazla özellik için
3. **React Query**: Server state yönetimi için
4. **Custom Hooks**: Yeniden kullanılabilir logic için
5. **Error Handling**: Hata yönetimi için
6. **Loading States**: Kullanıcı deneyimi için
