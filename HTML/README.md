# HTML Temelleri - Eğitim Materyali

## 📚 İçindekiler
- [HTML Nedir?](#html-nedir)
- [Temel HTML Yapısı](#temel-html-yapısı)
- [HTML Etiketleri](#html-etiketleri)
- [Metin Formatlama](#metin-formatlama)
- [Bağlantılar ve Resimler](#bağlantılar-ve-resimler)
- [Listeler](#listeler)
- [Tablolar](#tablolar)
- [Formlar](#formlar)
- [Semantik HTML](#semantik-html)
- [Pratik Örnekler](#pratik-örnekler)
- [Ödevler](#ödevler)

---

## 🌐 HTML Nedir?

**HTML (HyperText Markup Language)**, web sayfalarını oluşturmak için kullanılan standart işaretleme dilidir. HTML, web tarayıcılarına sayfanın nasıl görüntüleneceğini söyler.

### HTML'in Temel Özellikleri:
- **Platform bağımsızdır** - Herhangi bir işletim sisteminde çalışır
- **Tarayıcı bağımsızdır** - Tüm modern web tarayıcıları HTML'i destekler
- **Öğrenmesi kolaydır** - Basit etiket yapısı
- **Açık standarttır** - W3C tarafından standartlaştırılmıştır

---

## 🏗️ Temel HTML Yapısı

Her HTML dosyası aşağıdaki temel yapıya sahip olmalıdır:

```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sayfa Başlığı</title>
</head>
<body>
    <!-- Sayfa içeriği buraya gelecek -->
</body>
</html>
```

### Yapı Açıklaması:

| Etiket | Açıklama |
|--------|----------|
| `<!DOCTYPE html>` | HTML5 doküman türü bildirimi |
| `<html>` | HTML dokümanının kök elementi |
| `<head>` | Meta bilgiler, başlık, CSS bağlantıları |
| `<body>` | Görünür sayfa içeriği |

---

## 🏷️ HTML Etiketleri

### Temel Etiketler

#### 1. Başlık Etiketleri
```html
<h1>Ana Başlık</h1>
<h2>Alt Başlık 1</h2>
<h3>Alt Başlık 2</h3>
<h4>Alt Başlık 3</h4>
<h5>Alt Başlık 4</h5>
<h6>En Küçük Başlık</h6>
```

#### 2. Paragraf Etiketi
```html
<p>Bu bir paragraf metnidir. Paragraflar otomatik olarak alt satıra geçer.</p>
```

#### 3. Satır Sonu
```html
<p>Bu satırın sonunda<br>yeni satıra geçiyoruz.</p>
```

#### 4. Yatay Çizgi
```html
<hr>
```

---

## ✏️ Metin Formatlama

### Temel Formatlama Etiketleri

```html
<!-- Kalın metin -->
<strong>Önemli metin</strong>
<b>Kalın metin</b>

<!-- İtalik metin -->
<em>Vurgulanmış metin</em>
<i>İtalik metin</i>

<!-- Altı çizili metin -->
<u>Altı çizili metin</u>

<!-- Üstü çizili metin -->
<s>Üstü çizili metin</s>

<!-- Üst simge -->
<p>2<sup>3</sup> = 8</p>

<!-- Alt simge -->
<p>H<sub>2</sub>O</p>

<!-- Küçük metin -->
<small>Küçük metin</small>

<!-- İşaretli metin -->
<mark>İşaretli metin</mark>
```

### Örnek Kullanım:
```html
<p>Bu bir <strong>önemli</strong> ve <em>vurgulanmış</em> metindir.</p>
<p>Matematiksel ifade: x<sup>2</sup> + y<sup>2</sup> = z<sup>2</sup></p>
<p>Kimyasal formül: H<sub>2</sub>SO<sub>4</sub></p>
```

---

## 🔗 Bağlantılar ve Resimler

### Bağlantılar (Links)

```html
<!-- Temel bağlantı -->
<a href="https://www.google.com">Google'a Git</a>

<!-- Yeni sekmede açılan bağlantı -->
<a href="https://www.google.com" target="_blank">Google'a Git (Yeni Sekme)</a>

<!-- E-posta bağlantısı -->
<a href="mailto:farukboraguvenkaya.gdgc@gmail.com">E-posta Gönder</a>

<!-- Telefon bağlantısı -->
<a href="tel:+905378336283">Bizi Arayın</a>

<!-- Sayfa içi bağlantı -->
<a href="#bolum1">Bölüm 1'e Git</a>
<h2 id="bolum1">Bölüm 1</h2>
```

### Resimler

```html
<!-- Temel resim -->
<img src="resim.jpg" alt="Resim açıklaması">

<!-- Boyutlu resim -->
<img src="resim.jpg" alt="Resim açıklaması" width="300" height="200">

<!-- Responsive resim -->
<img src="resim.jpg" alt="Resim açıklaması" style="max-width: 100%; height: auto;">
```

### Resim Özellikleri:
- `src`: Resim dosyasının yolu
- `alt`: Resim yüklenemediğinde görünecek alternatif metin
- `width`: Genişlik (piksel)
- `height`: Yükseklik (piksel)

---

## 📋 Listeler

### Sıralı Liste (Ordered List)
```html
<ol>
    <li>İlk madde</li>
    <li>İkinci madde</li>
    <li>Üçüncü madde</li>
</ol>

<!-- Farklı numaralandırma -->
<ol type="A">
    <li>Madde A</li>
    <li>Madde B</li>
</ol>

<ol type="i">
    <li>Madde i</li>
    <li>Madde ii</li>
</ol>
```

### Sırasız Liste (Unordered List)
```html
<ul>
    <li>Madde 1</li>
    <li>Madde 2</li>
    <li>Madde 3</li>
</ul>

<!-- İç içe liste -->
<ul>
    <li>Ana madde
        <ul>
            <li>Alt madde 1</li>
            <li>Alt madde 2</li>
        </ul>
    </li>
    <li>Başka ana madde</li>
</ul>
```

### Tanım Listesi (Definition List)
```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language - Web sayfaları için işaretleme dili</dd>
    
    <dt>CSS</dt>
    <dd>Cascading Style Sheets - Web sayfalarının görünümünü düzenleyen dil</dd>
</dl>
```

---

## 📊 Tablolar

### Temel Tablo Yapısı
```html
<table border="1">
    <thead>
        <tr>
            <th>Başlık 1</th>
            <th>Başlık 2</th>
            <th>Başlık 3</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Hücre 1</td>
            <td>Hücre 2</td>
            <td>Hücre 3</td>
        </tr>
        <tr>
            <td>Hücre 4</td>
            <td>Hücre 5</td>
            <td>Hücre 6</td>
        </tr>
    </tbody>
</table>
```

### Gelişmiş Tablo Özellikleri
```html
<table border="1">
    <caption>Öğrenci Notları</caption>
    <thead>
        <tr>
            <th>Ad Soyad</th>
            <th>PHYS105</th>
            <th>MATH119</th>
            <th>CENG111</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Faruk Bora Güvenkaya</td>
            <td>98</td>
            <td>90</td>
            <td>92.5</td>
        </tr>
        
    </tbody>
</table>
```

### Tablo Özellikleri:
- `colspan`: Hücrenin kaç sütun kaplayacağı
- `rowspan`: Hücrenin kaç satır kaplayacağı

```html
<table border="1">
    <tr>
        <td colspan="2">İki sütun kaplayan hücre</td>
        <td>Normal hücre</td>
    </tr>
    <tr>
        <td rowspan="2">İki satır kaplayan hücre</td>
        <td>Hücre 1</td>
        <td>Hücre 2</td>
    </tr>
    <tr>
        <td>Hücre 3</td>
        <td>Hücre 4</td>
    </tr>
</table>
```

---

## 📝 Formlar

**Formlar**, kullanıcılardan bilgi toplamak için kullanılan HTML elemanlarıdır. Kayıt olma, giriş yapma, iletişim kurma, anket doldurma gibi işlemlerde kullanılır. Formlar, kullanıcı girişlerini sunucuya göndererek web sitelerinin etkileşimli olmasını sağlar.

### Temel Form Yapısı
```html
<form action="/gonder" method="post">
    <!-- Form elemanları buraya gelecek -->
    <input type="submit" value="Gönder">
</form>
```

### Form Elemanları

#### 1. Metin Girişi
```html
<!-- Tek satır metin -->
<input type="text" name="kullanici_adi" placeholder="Kullanıcı adınızı girin">

<!-- Şifre -->
<input type="password" name="sifre" placeholder="Şifrenizi girin">

<!-- E-posta -->
<input type="email" name="email" placeholder="E-posta adresiniz">

<!-- Sayı -->
<input type="number" name="yas" min="0" max="120" placeholder="Yaşınız">

<!-- Tarih -->
<input type="date" name="dogum_tarihi">
```

#### 2. Metin Alanı
```html
<textarea name="mesaj" rows="4" cols="50" placeholder="Mesajınızı yazın..."></textarea>
```

#### 3. Seçim Kutuları
```html
<!-- Onay kutusu -->
<input type="checkbox" name="haberler" id="haberler">
<label for="haberler">Haber bültenine abone ol</label>

<!-- Radyo düğmeleri -->
<input type="radio" name="cinsiyet" value="erkek" id="erkek">
<label for="erkek">Erkek</label>

<input type="radio" name="cinsiyet" value="kadin" id="kadin">
<label for="kadin">Kadın</label>
```

#### 4. Açılır Liste
```html
<select name="sehir">
    <option value="">Şehir seçiniz</option>
    <option value="istanbul">İstanbul</option>
    <option value="ankara">Ankara</option>
    <option value="izmir">İzmir</option>
    <option value="bursa">Bursa</option>
</select>
```

#### 5. Dosya Yükleme
```html
<input type="file" name="dosya" accept="image/*">
```

### Tam Form Örneği
```html
<form action="/kayit" method="post">
    <fieldset>
        <legend>Kişisel Bilgiler</legend>
        
        <label for="ad">Ad:</label>
        <input type="text" id="ad" name="ad" required><br><br>
        
        <label for="soyad">Soyad:</label>
        <input type="text" id="soyad" name="soyad" required><br><br>
        
        <label for="email">E-posta:</label>
        <input type="email" id="email" name="email" required><br><br>
        
        <label for="yas">Yaş:</label>
        <input type="number" id="yas" name="yas" min="0" max="120"><br><br>
        
        <label>Cinsiyet:</label>
        <input type="radio" name="cinsiyet" value="erkek" id="erkek">
        <label for="erkek">Erkek</label>
        <input type="radio" name="cinsiyet" value="kadin" id="kadin">
        <label for="kadin">Kadın</label><br><br>
        
        <label for="sehir">Şehir:</label>
        <select id="sehir" name="sehir">
            <option value="">Seçiniz</option>
            <option value="istanbul">İstanbul</option>
            <option value="ankara">Ankara</option>
            <option value="izmir">İzmir</option>
        </select><br><br>
        
        <label for="mesaj">Mesajınız:</label><br>
        <textarea id="mesaj" name="mesaj" rows="4" cols="50"></textarea><br><br>
        
        <input type="checkbox" id="haberler" name="haberler">
        <label for="haberler">Haber bültenine abone ol</label><br><br>
        
        <input type="submit" value="Kayıt Ol">
        <input type="reset" value="Temizle">
    </fieldset>
</form>
```

---

## 🏛️ Semantik HTML

Semantik HTML, anlamlı ve yapılandırılmış web sayfaları oluşturmak için kullanılır.

### Semantik Etiketler

```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Semantik HTML Örneği</title>
</head>
<body>
    <!-- Sayfa başlığı -->
    <header>
        <h1>Web Sitesi Başlığı</h1>
        <nav>
            <ul>
                <li><a href="#anasayfa">Ana Sayfa</a></li>
                <li><a href="#hakkimizda">Hakkımızda</a></li>
                <li><a href="#iletisim">İletişim</a></li>
            </ul>
        </nav>
    </header>

    <!-- Ana içerik -->
    <main>
        <!-- Ana bölüm -->
        <section>
            <h2>Hakkımızda</h2>
            <article>
                <h3>Şirket Tarihçesi</h3>
                <p>Şirketimizin tarihçesi hakkında bilgiler...</p>
            </article>
        </section>

        <!-- Yan panel -->
        <aside>
            <h3>İlgili Bağlantılar</h3>
            <ul>
                <li><a href="#">Link 1</a></li>
                <li><a href="#">Link 2</a></li>
            </ul>
        </aside>
    </main>

    <!-- Sayfa altı -->
    <footer>
        <p>&copy; 2024 Web Sitesi. Tüm hakları saklıdır.</p>
    </footer>
</body>
</html>
```

### Semantik Etiket Açıklamaları:

| Etiket | Açıklama |
|--------|----------|
| `<header>` | Sayfa veya bölüm başlığı |
| `<nav>` | Navigasyon menüsü |
| `<main>` | Ana içerik alanı |
| `<section>` | İçerik bölümü |
| `<article>` | Bağımsız içerik parçası |
| `<aside>` | Yan panel içeriği |
| `<footer>` | Sayfa altı |
| `<figure>` | Resim, grafik vb. |
| `<figcaption>` | Figure açıklaması |
| `<time>` | Tarih/saat bilgisi |

---

## 💻 Pratik Örnekler

### Örnek 1: Kişisel Web Sayfası
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Faruk Bora Güvenkaya - Kişisel Web Sayfası</title>
</head>
<body>
    <header>
        <h1>Faruk Bora Güvenkaya</h1>
        <p>Web Geliştirici & Tasarımcı</p>
        <nav>
            <a href="#hakkimda">Hakkımda</a> |
            <a href="#projeler">Projeler</a> |
            <a href="#iletisim">İletişim</a>
        </nav>
    </header>

    <main>
        <section id="hakkimda">
            <h2>Hakkımda</h2>
            <img src="profil.jpg" alt="Faruk Bora Güvenkaya" width="200">
            <p>Merhaba! Ben Faruk Bora Güvenkaya. 1 yıldır web geliştirme alanında çalışıyorum.</p>
            
            <h3>Yeteneklerim</h3>
            <ul>
                <li>HTML5 & CSS3</li>
                <li>JavaScript</li>
                <li>React.js</li>
                <li>Node.js</li>
            </ul>
        </section>

        <section id="projeler">
            <h2>Projelerim</h2>
            <article>
                <h3>E-ticaret Sitesi</h3>
                <p>Modern bir e-ticaret platformu geliştirdim.</p>
                <a href="https://github.com/faruk/proje1" target="_blank">GitHub'da Görüntüle</a>
            </article>
        </section>

        <section id="iletisim">
            <h2>İletişim</h2>
            <form action="/gonder" method="post">
                <label for="isim">İsim:</label>
                <input type="text" id="isim" name="isim" required><br><br>
                
                <label for="email">E-posta:</label>
                <input type="email" id="email" name="email" required><br><br>
                
                <label for="mesaj">Mesaj:</label><br>
                <textarea id="mesaj" name="mesaj" rows="4" cols="50" required></textarea><br><br>
                
                <input type="submit" value="Gönder">
            </form>
        </section>
    </main>

    <footer>
        <p>&copy; 2024 Faruk Bora Güvenkaya. Tüm hakları saklıdır.</p>
        <p>E-posta: <a href="mailto:farukboraguvenkaya.gdgc@gmail.com">faruk@email.com</a></p>
    </footer>
</body>
</html>
```

### Örnek 2: Blog Sayfası
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Teknoloji Blogu</title>
</head>
<body>
    <header>
        <h1>Teknoloji Blogu</h1>
        <nav>
            <ul>
                <li><a href="#anasayfa">Ana Sayfa</a></li>
                <li><a href="#yazilar">Yazılar</a></li>
                <li><a href="#hakkimda">Hakkımda</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section id="yazilar">
            <article>
                <h2>HTML5'in Yeni Özellikleri</h2>
                <time datetime="2024-01-15">15 Ocak 2024</time>
                <p>HTML5 ile gelen yeni semantik etiketler ve özellikler...</p>
                <a href="#devami">Devamını Oku</a>
            </article>

            <article>
                <h2>Responsive Web Tasarımı</h2>
                <time datetime="2024-01-10">10 Ocak 2024</time>
                <p>Mobil uyumlu web siteleri nasıl tasarlanır...</p>
                <a href="#devami">Devamını Oku</a>
            </article>
        </section>

        <aside>
            <h3>Popüler Yazılar</h3>
            <ul>
                <li><a href="#">CSS Grid Sistemi</a></li>
                <li><a href="#">JavaScript Temelleri</a></li>
                <li><a href="#">Web Güvenliği</a></li>
            </ul>
        </aside>
    </main>

    <footer>
        <p>&copy; 2024 Teknoloji Blogu</p>
    </footer>
</body>
</html>
```

---


## 📝 Önemli Notlar

### Yaygın Hatalar:
- Etiketleri kapatmayı unutmak
- Yanlış etiket iç içe geçirme
- Alt metinleri atlamak
- Form elemanlarına label eklememek
- Semantik olmayan etiketler kullanmak

---

## 🎯 Sonraki Adımlar

HTML temellerini öğrendikten sonra şunları öğrenebilirsiniz:

1. **CSS** - Stil ve görünüm
2. **JavaScript** - Etkileşim ve dinamik içerik
3. **React** - 
