# Eğitim Yönetim Sistemi -(LMS -Learning Management System)

![Angular](https://img.shields.io/badge/Angular-19-DD0031?logo=angular&logoColor=white) ![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?logo=typescript&logoColor=white) ![RxJS](https://img.shields.io/badge/RxJS-7-B7178C?logo=reactivex&logoColor=white) ![JSON Server](https://img.shields.io/badge/JSON%20Server-auth-000000?logo=json&logoColor=white) ![Standalone](https://img.shields.io/badge/Standalone%20Components-✔️-brightgreen) ![JWT](https://img.shields.io/badge/JWT-Auth-FFB000)

📚 Modern Angular (v19) Standalone mimarisiyle geliştirilen LMS örneği. `json-server-auth` ile JWT tabanlı sahte kimlik doğrulama kullanır; Öğrenci ve Eğitmen rolleri için kurs listeleme, kayıt, yorum ve eğitmen tarafında CRUD akışlarını içerir.

---

## 📖 İçindekiler
- 🚀 Proje Özeti
- 🧱 Mimari Genel Bakış
- 🧩 Veri ve API
- 🔐 Kimlik Doğrulama ve Guard
- ✨ Özellikler ve Akışlar
- ✅ Validasyon ve Yardımcılar
- ⚙️ Kurulum ve Çalıştırma
- 🧪 Test
- ℹ️ Teknik Notlar
- 🖼️ Ekran Görüntüleri ve Giriş Bilgileri

---

## 🚀 Proje Özeti
- Angular 19 Standalone bileşen yapısı ile başlatma (`bootstrapApplication`).
- Mock backend: `json-server-auth` + `db.json` (JWT token üretimi ve veri)
- Roller: 🧑‍🎓 Öğrenci, 🧑‍🏫 Eğitmen
- Kurs arama, kayıt/çıkış, yorum, eğitmen tarafında kurs oluşturma/düzenleme/silme.

---

## 🧱 Mimari Genel Bakış
- `src/main.ts`: Uygulama `bootstrapApplication(AppComponent, appConfig)` ile başlar.
- `src/app/app.config.ts`:
  - ⚡ `provideZoneChangeDetection({ eventCoalescing: true })`
  - 🧭 `provideRouter(routes)` ve 🌐 `provideHttpClient()`
- `src/app/app.routes.ts`: `home`, `login`, `register`, `courses`, `courses/:id`, `profile` rotaları. `authGuard` ile korunan sayfalar: `/courses`, `/courses/:id`, `/profile`.
- `src/app/app.component.html`: `tokenStatus` true ise `app-navbar` görünür, değilse gizlenir. Durum `NavigationEnd` eventi ile güncellenir.
- Standalone bileşenler: `Home`, `Login`, `Register`, `Courses`, `CoursesItem`, `Profile`, `Navbar`.

---

## 🧩 Veri ve API
- `src/app/utils/apiUrl.ts`:
  - 🌍 `baseURL`: `http://localhost:3001/`
  - 🔗 Endpoint’ler: `login`, `register`, `users`, `courses`, `logout`, `profile`
- `db.json` veri modeli:
  - 👤 `users`: email, hash’lenmiş password, name, surname, type (`student`/`instructor`), `id`
  - 📚 `courses`: title, icon, description, instructor adı/ID, stars, comments (userId, userName, comment, date)
- `ApiService` özet uç noktalar:
  - 🔑 Auth: `userLogin`, `userRegister`, `userLogout`, `getCurrentUser`
  - 📘 Kurs: `allCourses`, `getCourseById`, `createCourse`, `updateCourse`, `deleteCourse`
  - 🧑‍🎓 Öğrenci: `getStudentCourses` (localStorage `enrolledCourses`)
  - 🧑‍🏫 Eğitmen: `getInstructorCourses`
  - 🔎 Arama: `searchCourses`
  - 💬 Yorum: `addComment`, `getCourseComments`

---

## 🔐 Kimlik Doğrulama ve Guard
- 🔒 Giriş: `userLogin(email, password)` → dönen `accessToken` `localStorage`’a yazılır.
- 👤 Kullanıcı bilgisi: `getCurrentUser()` JWT payload’daki `sub` ile `/users/:id` isteği yapar.
- 🛡️ Guard: `authGuard` token yoksa `window.location.replace('/')` ile ana sayfaya döndürür.

---

## ✨ Özellikler ve Akışlar
- 🧑‍🎓 Öğrenci
  - 📚 Kursları listeleme ve 🔎 arama (navbar araması URL `search` query ile senkron)
  - ➕ Kursa kayıt (localStorage: `enrolledCourses`)
  - ➖ Kurstan çıkma
  - 💬 Kurslara yorum ekleme (kurs kaydı `PUT` ile güncellenir)
- 🧑‍🏫 Eğitmen
  - ➕ Yeni kurs ekleme
  - ✏️ Kurs düzenleme
  - 🗑️ Kurs silme
- 🧭 Navbar
  - 👤 Ad/Soyad gösterimi (`getCurrentUser`)
  - 🔄 `NavigationEnd` ile URL `search` senkronizasyonu

---

## ✅ Validasyon ve Yardımcılar (`src/app/utils/valid.ts`)
- 📧 `emailValid`, 🔐 `passwordValid` regex kontrolleri
- 🪪 `nameValid`, `surnameValid` (TR yerelleştirme; `firstCharUpper`)
- 🧾 JWT yardımcıları: `getCurrentUser`, `isUserLoggedIn`, `logout`

---

## ⚙️ Kurulum ve Çalıştırma
- Ön koşullar: Node.js, Angular CLI (devDependencies’de `json-server-auth` ve `concurrently` mevcut)
- 📦 Kurulum:
  - `npm install`
- ▶️ Çalıştırma seçenekleri:
  - Tek komutla her iki servis: `npm start`
    - Backend: `http://localhost:3001/`
    - Frontend: `http://localhost:4200/`
  - Ayrı ayrı:
    - Sadece Angular: `npm run start:angular`
    - Sadece backend: `npm run start:server`

---

## 🧪 Test
- `npm test`

---

## ℹ️ Teknik Notlar
- ⚡ Zone event coalescing: `provideZoneChangeDetection({ eventCoalescing: true })`
- 🛡️ Guard yönlendirmesi: Token yoksa `window.location.replace('/')`
- 🗂️ Kayıtlı kurslar `localStorage`’da tutulur ve ID’ler üzerinden REST çağrıları ile toplanır
- 🔎 Navbar arama alanı URL `search` param’ı ile senkronize olur (router events)

---

- İlgili Proje Angular ile tasarlanan bir Eğitim Merkezi.
- Kullancıların benzersiz bir giriş yapmaları için validasyonlar Email ve password validasyonları oluşturuldu.
- Kayıt esnasında kullanıcının boş veri göndermemesi için boşluk denetimi, aynı mail bilgilerini getirmemesi içinde mail kontrolü yapıldı.
- Giriş yapmayan kullanıcıya guard yöntemi ile navbar gösterilmedi, yine aynı şekilde girişi olmayan ya da JWT'si bulunmayan kullanıcı Ana Sayfaya yönlendirildi.
- ilgili projede 2 farklı kullanıcı tanımlandı, Öğrenci ve Öğretmen.
- Kullanıcı girişlerine göre programda kullanıcıya ait alanlar oluşturuldu. Eğer kullanıcı öğrenci ise kurs seçme, yorum yapma ya da ilgili kurstan ayrılma özellikleri eklendi.
- Eğer kullanıcı Öğretmen ise de kurs ekleme, kendisinin eklemiş olduğu kursları silme ya da düzeltme alanları eklendi.
- json Server bağlantısı ile program dinamikliği korundu. Eş zamanlı servera kayıt-giriş, kurslar için de yorum ve silme gibi işlemler gösterildi.

Eğitmen İçin Örnek Giriş Bilgileri
Mail:caglar@mail.com
Şifre:Aa12345!

Öğrenci İçin Örnek Giriş Bilgileri
Mail:ahmet@mail.com
şifre:Aa12345!

## ⚡ Hızlı Başlangıç

- Gereksinimler:
  - Node `>=18`, npm `>=9`
  - Tarayıcıda `localStorage` aktif olmalı
- Adımlar:
  1. `npm install`
  2. `npm start` (iki servis aynı anda başlar)
  3. Tarayıcı otomatik açılır: `http://localhost:4200/`
  4. Örnek kullanıcı bilgileriyle giriş yapın ve `Kurslar` sayfasına gidin.
- Alternatif çalışma:
  - `npm run start:server` → JSON Server Auth: `http://localhost:3001/`
  - `npm run start:angular` → Angular dev server: `http://localhost:4200/`
- Komutlar ve açıklama (package.json):
  - `start`: `json-server-auth --watch db.json --port 3001` + `ng serve --open`
  - `start:server`: `json-server-auth --watch db.json --port 3001`
  - `start:angular`: `ng serve --open`
- Sık karşılaşılanlar:
  - Port dolu (`EADDRINUSE`): 3001/4200 kullanımda ise durdurun ya da portu değiştirin.
  - Giriş başarısız/Token yok: `localStorage` temizleyin, tekrar giriş yapın.
  - Backend hatası: `db.json` dosyasının proje kökünde olduğundan emin olun.
- Veri sıfırlama (tarayıcı konsolu):
  - `localStorage.removeItem('accessToken')`
  - `localStorage.removeItem('enrolledCourses')`
- Port değiştirme:
  - Backend portu: `package.json` içindeki `--port` değerini güncelleyin.
  - Sonra `src/app/utils/apiUrl.ts` içindeki `baseURL`’i aynı porta çekin.

## 🗺️ Route Haritası
- `/` → `Home` (public)
- `/login` → `Login` (public)
- `/register` → `Register` (public)
- `/courses` → `Courses` (authGuard)
- `/courses/:id` → `Course Details` (authGuard)
- `/profile` → `Profile` (authGuard; rol bazlı görünüm)
- Arama: `/courses?search=<kelime>` → Navbar araması ile URL query senkron.

Akış Diyagramı:
`/` → `/login` → `/courses` → `/courses/:id` → `/profile`
`/login` ↔ `/register`

## 👥 Öğrenci / Eğitmen Akışı

| Akış / Özellik | Öğrenci | Eğitmen |
|---|---|---|
| Giriş / Kayıt | ✔️ Giriş yapar, kayıt olabilir | ✔️ Giriş yapar, kayıt olabilir |
| Kursları Listeleme | ✔️ Tüm kursları görür | ✔️ Kendi kurslarını görür |
| Arama (Navbar) | ✔️ URL `search` ile senkron | ✔️ URL `search` ile senkron |
| Kursa Kayıt | ✔️ localStorage `enrolledCourses` | ❌ |
| Kurstan Çıkma | ✔️ | ❌ |
| Yorum Ekleme | ✔️ Kurs sayfasında | ✔️ Kurs sayfasında |
| Kurs Oluşturma | ❌ | ✔️ Form ile ekler |
| Kurs Düzenleme | ❌ | ✔️ Kursu günceller |
| Kurs Silme | ❌ | ✔️ Kursu siler |
| Profil Görünümü | ✔️ Kayıtlı kurslar + ilerleme | ✔️ Oluşturulan kurslar + form |
| Navbar | ✔️ Ad/Soyad + arama | ✔️ Ad/Soyad + arama |

🖼️ Ekran Görüntüleri

<img width="1401" height="863" alt="giriş ekranıı" src="https://github.com/user-attachments/assets/6922a01b-8943-415e-b0ff-fa6e53eb6006" />
<img width="1431" height="869" alt="kayıtol ekranı" src="https://github.com/user-attachments/assets/a1786cc7-2a30-4ea4-a1c3-5cd061f56ad7" />
<img width="1234" height="838" alt="giriş ekranı" src="https://github.com/user-attachments/assets/e6aa7d01-209d-436c-b86f-0b37a7a4f446" />
<img width="1669" height="893" alt="öğrenci profile ekranı" src="https://github.com/user-attachments/assets/37947430-0698-4b78-830b-96f3c8049b0e" />
<img width="1673" height="944" alt="kurs ekranı" src="https://github.com/user-attachments/assets/886c26a9-e836-48d9-9581-caeda88a98da" />
<img width="1678" height="921" alt="kurs detay ekranı" src="https://github.com/user-attachments/assets/b00ac26d-4dca-477b-89d0-c5328c1ef31c" />
<img width="1673" height="964" alt="kurs secili ekran" src="https://github.com/user-attachments/assets/b768579f-3d36-40be-8291-438b084c08be" />
<img width="1588" height="502" alt="kurs ekleme ekranı" src="https://github.com/user-attachments/assets/736f0ede-c8de-4c29-9804-d82c4e76b5c1" />
<img width="1668" height="752" alt="eklenen kurs ekranı" src="https://github.com/user-attachments/assets/bb6c90a5-8928-4350-8a38-e60e8d42e528" />
<img width="1676" height="957" alt="profil ekranı" src="https://github.com/user-attachments/assets/2782bb5c-c326-4787-b459-e0c517c39d67" />





