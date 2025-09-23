# CSS Temelleri ve Tailwind CSS - Eğitim Materyali

## 📚 İçindekiler
- [CSS Nedir?](#css-nedir)
- [CSS Temel Yapısı](#css-temel-yapısı)
- [CSS Seçiciler](#css-seçiciler)
- [Temel CSS Özellikleri](#temel-css-özellikleri)
- [Flexbox ve Grid](#flexbox-ve-grid)
- [Responsive Design](#responsive-design)
- [Tailwind CSS](#tailwind-css)
- [Pratik Örnekler](#pratik-örnekler)
- [Ödevler](#ödevler)

---

## 🎨 CSS Nedir?

**CSS (Cascading Style Sheets)**, web sayfalarının görünümünü ve düzenini kontrol etmek için kullanılan stil dilidir. HTML yapıyı oluştururken, CSS bu yapıyı güzelleştirir ve düzenler.

### CSS'in Temel Özellikleri:
- **Görsel tasarım** - Renkler, yazı tipleri, boyutlar
- **Düzen kontrolü** - Sayfa yerleşimi ve hizalama
- **Responsive tasarım** - Farklı cihazlara uyum
- **Animasyonlar** - Hareketli efektler

---

## 🏗️ CSS Temel Yapısı

### CSS Yazım Kuralları
```css
seçici {
    özellik: değer;
    özellik: değer;
}
```

### CSS'i HTML'e Bağlama

#### 1. Harici CSS Dosyası (Önerilen Yöntem)

Harici CSS dosyası kullanmak en iyi uygulamadır çünkü:
- **Ayrılık**: HTML yapısı ve CSS stilleri ayrı dosyalarda tutulur
- **Yeniden kullanım**: Aynı CSS dosyası birden fazla HTML sayfasında kullanılabilir
- **Bakım kolaylığı**: Stil değişiklikleri tek yerden yapılır

**Küçük HTML örneği:**
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Basit Örnek</title>
    <!-- Harici CSS dosyasını bağlama -->
    <link rel="stylesheet" href="stil.css">
</head>
<body>
    <h1>Merhaba Dünya</h1>
    <p>Bu bir paragraf metnidir.</p>
    <div class="kutu">Bu bir kutudur</div>
</body>
</html>
```

**Harici CSS dosyası (stil.css):**
```css
/* Başlık stili */
h1 {
    color: blue;
    text-align: center;
    font-size: 2rem;
}

/* Paragraf stili */
p {
    color: #666;
    font-size: 1.2rem;
    margin: 20px;
}

/* Kutu stili */
.kutu {
    background-color: #f0f0f0;
    border: 2px solid #333;
    padding: 15px;
    margin: 10px;
    border-radius: 5px;
}
```

#### 2. Diğer CSS Bağlama Yöntemleri

```html
<!-- İç CSS -->
<style>
    h1 { color: blue; }
</style>

<!-- Satır İçi CSS -->
<h1 style="color: blue;">Başlık</h1>
```


---

## 🎯 CSS Seçiciler

### Temel Seçiciler
```css
/* Etiket seçici */
h1 { color: red; }

/* Sınıf seçici */
.baslik { font-size: 18px; }

/* ID seçici */
#ana-baslik { font-weight: bold; }

/* Alt öğe seçici */
div p { color: blue; }

/* Hover efekti */
a:hover { color: red; }
```

---

## 🎨 Temel CSS Özellikleri

### Metin ve Arka Plan
```css
.text {
    font-family: Arial, sans-serif;
    font-size: 16px;
    font-weight: bold;
    color: #333;
    text-align: center;
    background-color: #f0f0f0;
}
```

### Kenar Boşlukları ve Kenarlıklar
```css
.kutu {
    margin: 10px;
    padding: 15px;
    border: 2px solid #333;
    border-radius: 5px;
}
```

---

## 🔄 Flexbox ve Grid

### Flexbox
```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
}

.item {
    flex: 1;
}
```

### CSS Grid
```css
.container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 20px;
}
```

---

## 📱 Responsive Design

### Media Queries
```css
/* Mobil cihazlar */
@media (max-width: 768px) {
    .container {
        flex-direction: column;
    }
}

/* Büyük ekranlar */
@media (min-width: 1025px) {
    .container {
        max-width: 1200px;
        margin: 0 auto;
    }
}
```

---

## ⚡ Tailwind CSS

Tailwind CSS, utility-first CSS framework'üdür. Önceden tanımlanmış sınıfları kullanarak hızlı tasarım yapmanızı sağlar.

### Kurulum
```html
<!-- CDN ile -->
<script src="https://cdn.tailwindcss.com"></script>
```

```bash
<!-- NPM ile -->
npm install -D tailwindcss
npx tailwindcss init
```

### Temel Sınıflar
```html
<!-- Renkler -->
<div class="bg-blue-500 text-white">Mavi arka plan</div>

<!-- Boyutlar -->
<div class="w-full h-32">Tam genişlik, 128px yükseklik</div>

<!-- Boşluklar -->
<div class="m-4 p-6">Margin ve padding</div>

<!-- Flexbox -->
<div class="flex justify-center items-center">Merkezi hizalama</div>

<!-- Grid -->
<div class="grid grid-cols-3 gap-4">3 sütunlu grid</div>

<!-- Typography -->
<h1 class="text-4xl font-bold">Büyük başlık</h1>
<p class="text-gray-600">Gri metin</p>

<!-- Kenarlıklar ve Gölgeler -->
<div class="border border-gray-300 rounded-lg shadow-md">Kart</div>
```

### Responsive Sınıflar
```html
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
    Responsive grid
</div>
```

---

## 💻 Pratik Örnekler

### Basit Kart Tasarımı
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Basit Kart</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-8">
    <div class="max-w-sm bg-white rounded-lg shadow-lg p-6">
        <h2 class="text-2xl font-bold mb-4">Kart Başlığı</h2>
        <p class="text-gray-600 mb-4">Kart açıklaması buraya gelecek.</p>
        <button class="bg-blue-500 text-white px-4 py-2 rounded">
            Buton
        </button>
    </div>
</body>
</html>
```

### Responsive Layout
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Layout</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">
    <header class="bg-white shadow p-4">
        <h1 class="text-2xl font-bold">Logo</h1>
    </header>
    
    <main class="max-w-6xl mx-auto p-4">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div class="bg-white p-6 rounded shadow">Kart 1</div>
            <div class="bg-white p-6 rounded shadow">Kart 2</div>
            <div class="bg-white p-6 rounded shadow">Kart 3</div>
        </div>
    </main>
</body>
</html>
```

---

## 📝 Önemli Notlar

### CSS En İyi Uygulamaları:
1. **Semantik CSS** - Anlamlı sınıf isimleri
2. **Responsive tasarım** - Mobil öncelikli düşünün
3. **Performans** - Gereksiz CSS'ten kaçının

### Tailwind CSS Avantajları:
- **Hızlı geliştirme** - Önceden hazır sınıflar
- **Tutarlı tasarım** - Standart spacing ve renkler
- **Responsive** - Kolay responsive tasarım
- **Küçük bundle boyutu** - Kullanılmayan CSS'ler çıkarılır

---

## 🎯 Sonraki Adımlar

CSS ve Tailwind CSS öğrendikten sonra:
1. **JavaScript** - Etkileşimli web siteleri
