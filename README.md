# LmsProject

Modern Angular (v19) Standalone mimarisiyle geliştirilen bir öğrenme yönetim sistemi (LMS) örneği. Arka uç olarak `json-server-auth` ile JWT tabanlı sahte kimlik doğrulama ve veri kaynağı kullanılır. Öğrenci ve Eğitmen rollerine göre kurs görüntüleme, kayıt, yorum ve eğitmen tarafında kurs CRUD akışlarını içerir.

---

### Kullanılan Teknolojiler

- Angular 19 (Standalone Components, `bootstrapApplication`)
- TypeScript
- HTML / CSS
- JSON Server + json-server-auth (JWT tabanlı mock backend)
- Concurrently (ön-uç ve mock backend’i aynı anda çalıştırma)
- RxJS

---

### Mimari Genel Bakış

- `src/main.ts`: Uygulama `bootstrapApplication(AppComponent, appConfig)` ile başlatılır.
- `src/app/app.config.ts`:
  - `provideZoneChangeDetection({ eventCoalescing: true })` ile olay birleştirme (performans için küçük bir iyileştirme)
  - `provideRouter(routes)` ve `provideHttpClient()`
- `src/app/app.routes.ts`: Ana sayfa, giriş/kayıt, kurs listesi, kurs detayı ve profil rotaları. `authGuard` ile korunan sayfalar (`/courses`, `/courses/:id`, `/profile`).
- `src/app/app.component.html`: `tokenStatus` true ise navbar görünür; değilse gizlenir. Durum `NavigationEnd` olaylarında kontrol edilerek güncellenir.
- Standalone bileşenler: `Home`, `Login`, `Register`, `Courses`, `CoursesItem`, `Profile`, `Navbar` vb.

---

### Veri ve API

- `src/app/utils/apiUrl.ts`:
  - `baseURL`: `http://localhost:3001/`
  - Endpoint’ler: `login`, `register`, `users`, `courses`, `logout`, `profile`
- `db.json`:
  - `users`: E-posta, hash’lenmiş şifre, ad, soyad, rol (`student`/`instructor`), `id`
  - `courses`: Kurs başlığı, ikon, açıklama, eğitmen adı/ID, yıldız ve yorum listesi
- `ApiService`:
  - Auth: `userLogin`, `userRegister`, `userLogout`, `getCurrentUser`
  - Kurs: `allCourses`, `getCourseById`, `createCourse`, `updateCourse`, `deleteCourse`
  - Öğrenci: `getStudentCourses` (localStorage’daki `enrolledCourses` üzerinden)
  - Eğitmen: `getInstructorCourses`
  - Arama: `searchCourses`
  - Yorum: `addComment`, `getCourseComments`

---

### Kimlik Doğrulama ve Guard

- Giriş: `userLogin(email, password)` çağrısı ile `json-server-auth`’tan gelen `accessToken` `localStorage`’a kaydedilir.
- Kullanıcı bilgisi: `getCurrentUser()` JWT payload’dan `sub` okunarak `/users/:id` isteği yapılır.
- Guard: `authGuard` `localStorage`’da `accessToken` yoksa `window.location.replace('/')` ile ana sayfaya yönlendirir (bloklama yerine yönlendirme yaklaşımı).

---

### Özellikler ve Akışlar

- Öğrenci:
  - Kursları listeleme ve arama (`search` query param’ı ile navbar araması senkron)
  - Kursa kayıt (localStorage: `enrolledCourses`)
  - Kurstan çıkma
  - Kurslara yorum ekleme (kurs kaydı `PUT` ile güncellenir)
- Eğitmen:
  - Yeni kurs ekleme
  - Kurs düzenleme
  - Kurs silme
- Navbar:
  - Kullanıcı adı/soyadı gösterimi (JWT ile `getCurrentUser`)
  - `NavigationEnd` ile URL query `search` senkronizasyonu

---

### Validasyon ve Yardımcılar (utils/valid.ts)

- `emailValid`, `passwordValid` regex kontrolleri
- `nameValid`, `surnameValid`: Çok kelimeli ad/soyad için TR yerelleştirme ile ilk harfi büyütme (`firstCharUpper`)
- JWT yardımcıları: `getCurrentUser`, `isUserLoggedIn`, `logout`

---

### Kurulum ve Çalıştırma

Ön koşullar:
- Node.js ve Angular CLI
- `json-server-auth` ve `concurrently` (devDependencies içinde mevcut)

Kurulum:
- `npm install`

Geliştirme sırasında çalıştırma seçenekleri:
- Tek komutla her iki servis: `npm start`
  - Arka uç: `http://localhost:3001/`
  - Ön uç: `http://localhost:4200/`
- Ayrı ayrı:
  - Sadece Angular: `npm run start:angular`
  - Sadece backend: `npm run start:server`

Test:
- `npm test`

---

### Teknik Notlar (Trick’ler)

- Zone event coalescing: `provideZoneChangeDetection({ eventCoalescing: true })`
- Guard yönlendirmesi: Token yoksa `window.location.replace('/')`
- Kayıtlı kurslar localStorage’da tutulur; öğrenci kursları bu ID’lerden REST çağrılarıyla toplanır
- Navbar arama alanı URL `search` param’ı ile senkronize olur (router events)

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





