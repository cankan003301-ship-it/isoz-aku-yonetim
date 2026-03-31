# 🔋 Mutlu Akü – Müşteri Yönetim Sistemi

## 📁 Dosyalar
- `index.html` – Ana uygulama
- `mersin_data.json` – Tüm Mersin müşteri verileri (2023-2026)
- `firebase-config.js` – Firebase yapılandırması (kendi bilgilerinizi girin)
- `vercel.json` – Vercel deploy ayarları

---

## 🚀 ADIM 1: Firebase Kurulumu

1. **https://console.firebase.google.com** adresine gidin
2. **Yeni Proje** oluşturun (örn: `mutlu-aku-sistem`)
3. Sol menü → **Authentication** → **Sign-in method** → **Email/Password** → Etkinleştir
4. Sol menü → **Firestore Database** → **Veritabanı oluştur** → **Test mode** seçin
5. Sol menü → **Proje Ayarları** (⚙️) → **Genel** sekmesi → **Web uygulaması** ekle
6. Firebase Config bilgilerini kopyalayın

### `index.html` içindeki Firebase Config'i Güncelleyin:
Dosyada `YOUR_API_KEY` yazan kısımları kendi bilgilerinizle doldurun:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "mutlu-aku.firebaseapp.com",
  projectId: "mutlu-aku",
  storageBucket: "mutlu-aku.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

### İlk Admin Kullanıcısı Oluşturun:
Firebase Console → Authentication → **Kullanıcı ekle**:
- E-posta: `admin@mutluaku.com`
- Şifre: (güçlü bir şifre belirleyin)

Sonra Firestore → **users** koleksiyonu oluşturun:
- Document ID: (kullanıcının UID'si)
- Alanlar:
  - `email`: admin@mutluaku.com
  - `name`: Admin
  - `role`: admin

---

## 🚀 ADIM 2: Vercel Deploy

### Yöntem A – Vercel CLI (Önerilen):
```bash
# Vercel CLI yükle
npm install -g vercel

# Proje klasörüne gir
cd mutlu-aku

# Deploy et
vercel

# Soruları yanıtla:
# Project name: mutlu-aku
# Framework: Other
# Deploy? Yes
```

### Yöntem B – GitHub üzerinden:
1. GitHub'da yeni repo oluşturun
2. Dosyaları push edin:
```bash
git init
git add .
git commit -m "Mutlu Akü Sistem"
git remote add origin https://github.com/KULLANICI/mutlu-aku.git
git push -u origin main
```
3. **https://vercel.com** → Import Project → GitHub repoyu seçin
4. Deploy edin

---

## 🔒 Firestore Güvenlik Kuralları

Firebase Console → Firestore → **Rules** sekmesine yapıştırın:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Kullanıcılar sadece kendi verilerini görebilir
    match /users/{userId} {
      allow read, write: if request.auth != null && 
        (request.auth.uid == userId || 
         get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin');
    }
    // İşlemler - giriş yapılmış kullanıcılar okuyabilir, yazabilir
    match /transactions/{txnId} {
      allow read, write: if request.auth != null;
    }
    // Müşteriler
    match /customers/{custId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && 
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
  }
}
```

---

## 📋 Özellikler

### 📊 Dashboard
- Toplam müşteri, yıllık satış, hedef gerçekleşme
- Aylık satış bar chart
- Bölge dağılımı chart
- En çok satan 10 müşteri

### 🔍 Müşteri Ara
- İsme/ilçeye göre anlık arama
- Her müşteri için detaylı kart
- Aylık satış tablosu (satış + hurda)
- Hedef gerçekleşme bar'ı

### 👥 Tüm Müşteriler
- Sayfalı tablo (30/sayfa)
- Satış/hedef/kalan filtresi
- Sıralama seçenekleri

### ➕ İşlem Ekle
- Müşteri seçimi (otomatik tamamlama)
- Yıl/ay/satış/hurda bilgileri
- Firebase veya yerel kayıt

### 🗺️ Bölge Satışları
- 2023-2026 tüm yıllar
- Bölge bazlı bar chart

### 📅 Aylık Satışlar
- Merkez aylık satış tablosu
- Yıllara göre karşılaştırma

### 🔑 Kullanıcı Yönetimi (Admin)
- Yeni kullanıcı oluşturma
- Rol atama (Admin/Kullanıcı)
- Kullanıcı listesi

---

## 📞 Destek
Sorun yaşarsanız Firebase Console loglarını kontrol edin.
