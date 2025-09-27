# LmsProject

### Kullanılan Teknolojiler

Bu proje aşağıdaki temel teknolojiler kullanılarak geliştirilmiştir:

-   Angular
-   HTML / SCSS
-   JSON Server (backend verisi için)

---

### Gereksinimler

Projenin başarıyla kurulup çalıştırılması için sisteminizde aşağıdaki yazılımların yüklü olması gerekmektedir:

-   **Node.js**
-   **Angular CLI**
-   **JSON Server**
-   **npm-run-all**: JSON Server ve Angular'ı birlikte başlatmak için `npm install --save-dev npm-run-all` komutu ile kurulmalıdır.

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





