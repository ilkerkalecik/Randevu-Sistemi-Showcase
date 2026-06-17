📌 Uygulama Genel Özeti

Geliştirilen sistem; berber, avukatlık bürosu, klinik, güzellik merkezi gibi hizmet odaklı işletmelerin randevu süreçlerini dijital ortamda yönetmesini sağlayan, modern ve ölçeklenebilir bir randevu yönetim platformudur.

Sistem; kullanıcıların kolayca randevu oluşturabildiği, işletmelerin ise tüm operasyonlarını merkezi bir panel üzerinden yönetebildiği, çok arayüzlü (multi-UI) ve tek backend mimarisine sahip bir yapı üzerine kurulmuştur.

🧠 Temel Amaç

Bu projenin temel amacı:

İşletmelerin randevu süreçlerini dijitalleştirmek
Müşteri deneyimini kolaylaştırmak
Telefon trafiğini azaltmak
Manuel hataları minimize etmek
Küçük ve orta ölçekli işletmelere uygun maliyetli bir çözüm sunmak
⚙️ Sistem Mimarisi

Uygulama aşağıdaki mimari yapı ile tasarlanmıştır:

Tek Backend (API)
Tüm iş mantığı merkezi bir API üzerinden yürütülür.
Birden Fazla Frontend (Next.js)
Her işletme için ayrı arayüz (UI) bulunur.
Aynı Kod Tabanı
Tüm işletmeler aynı backend ve frontend kodunu kullanır.
Konfigürasyon Bazlı Özelleştirme
Her işletme:
kendi domaini
kendi tema rengi
kendi logo ve içerikleri
ile sistemde özelleştirilir.
Veri İzolasyonu
Her işletme:
ayrı veritabanı kullanabilir
veya
tek veritabanında businessId ile ayrıştırılabilir.
👤 Kullanıcı Tarafı (Public)

Son kullanıcılar:

İşletme bilgilerini görüntüleyebilir
Hizmetleri inceleyebilir
Çalışan seçebilir
Müsait saatleri görebilir
Randevu oluşturabilir
SMS doğrulama (OTP) ile randevuyu onaylayabilir
Telefon numarası ile randevularını sorgulayabilir
Randevularını iptal edebilir
🏢 İşletme Tarafı (Admin Panel)

İşletmeler için yönetim paneli sayesinde:

Hizmet ekleme / düzenleme
Çalışan yönetimi
Çalışma saatleri belirleme
Kapalı gün tanımlama
Randevuları görüntüleme ve yönetme
Randevu oluşturma / iptal etme
Günlük, haftalık ve aylık istatistikleri görüntüleme
Galeri ve içerik yönetimi

gibi tüm işlemler kolayca yapılabilir.

🔐 Güvenlik ve Doğrulama

Sistem güvenliği için:

JWT tabanlı admin authentication
SMS OTP doğrulama sistemi
Randevu oluşturma ve iptal işlemlerinde çift doğrulama
Rate limit ve spam koruması (OTP abuse önleme)

mekanizmaları kullanılır.

📱 Esneklik ve Uyarlanabilirlik

Sistem farklı sektörlere kolayca uyarlanabilir:

💈 Berber / Kuaför
⚖️ Avukatlık Bürosu
🏥 Klinik / Sağlık Merkezi
💅 Güzellik Merkezi
🦷 Diş Kliniği

İş modeline göre sadece UI ve bazı alanlar değiştirilerek kullanılabilir.

🚀 Ölçeklenebilirlik

Sistem iki farklı şekilde büyüyebilir:

1. Ayrı Sistem Modeli

Her işletme:

ayrı domain
ayrı backend deploy
ayrı database 2. SaaS Modeli (Gelecek Planı)
Tek backend
Tek database
businessId ile ayrım

Bu sayede sistem başlangıçta basit, ileride güçlü hale gelir.

💡 Katma Değer

Bu uygulama işletmelere:

Profesyonel dijital görünüm
7/24 randevu alma imkanı
Operasyonel verimlilik
Müşteri memnuniyeti artışı
Düşük maliyetli yazılım çözümü

sağlar.

🎯 Kısa Özet (Sunumluk)

Bu proje, hizmet veren işletmeler için geliştirilen; randevu oluşturma, yönetme ve takip süreçlerini dijitalleştiren, tek backend ve çoklu arayüz mimarisiyle çalışan, ölçeklenebilir bir randevu yönetim platformudur.

# Randevu Sistemi API Kontratı v1

Base URL:

```
/api/v1
```

Standart response:

```
{
  "success":true,
  "message":"İşlem başarılı",
  "data": {},
  "errors": []
}
```

Standart error:

```
{
  "success":false,
  "message":"Validasyon hatası",
  "data":null,
  "errors": [
    {
      "field":"phoneNumber",
      "message":"Telefon numarası zorunludur."
    }
  ]
}
```

---

# 1. Genel Enumlar

```
BusinessType:
BARBER
LAWYER
CLINIC
BEAUTY_CENTER
DENTIST
OTHER

AppointmentStatus:
PENDING_OTP
CONFIRMED
CANCELLED
COMPLETED
NO_SHOW

OtpPurpose:
APPOINTMENT_CONFIRM
APPOINTMENT_CANCEL
PHONE_VERIFY

DayOfWeek:
MONDAY
TUESDAY
WEDNESDAY
THURSDAY
FRIDAY
SATURDAY
SUNDAY
```

---

# 2. Public İşletme Bilgisi

## İşletme profili

```
GET /business/profile
```

Response:

```
{
  "id":1,
  "businessName":"Elite Berber",
  "businessType":"BARBER",
  "description":"Profesyonel berber hizmetleri",
  "phoneNumber":"+905555555555",
  "email":"info@eliteberber.com",
  "address":"Malatya Merkez",
  "mapUrl":"https://maps.google.com/...",
  "instagramUrl":"https://instagram.com/eliteberber",
  "whatsappUrl":"https://wa.me/905555555555",
  "logoUrl":"https://cdn.example.com/logo.png",
  "coverImageUrl":"https://cdn.example.com/cover.png",
  "themeColor":"#111111",
  "currency":"TRY"
}
```

---

# 3. Public Hizmetler

## Aktif hizmetleri getir

```
GET /services
```

Response:

```
[
  {
    "id":1,
    "name":"Saç Kesimi",
    "description":"Profesyonel saç kesimi",
    "price":300,
    "durationMinutes":30,
    "isActive":true
  }
]
```

## Hizmet detayı

```
GET /services/{serviceId}
```

Response:

```
{
  "id":1,
  "name":"Saç Kesimi",
  "description":"Profesyonel saç kesimi",
  "price":300,
  "durationMinutes":30,
  "isActive":true
}
```

---

# 4. Public Çalışanlar

## Aktif çalışanları getir

```
GET /employees
```

Response:

```
[
  {
    "id":1,
    "fullName":"Ahmet Yılmaz",
    "label":"Uzman Berber",
    "phoneNumber":"+905555555555",
    "email":"ahmet@example.com",
    "imageUrl":"https://cdn.example.com/ahmet.jpg",
    "isActive":true
  }
]
```

## Hizmete göre çalışan getir

```
GET /employees?serviceId=1
```

Response aynı.

---

# 5. Public Galeri

```
GET /gallery
```

Response:

```
[
  {
    "id":1,
    "imageUrl":"https://cdn.example.com/gallery-1.jpg",
    "title":"Saç Kesimi",
    "description":"Örnek çalışma",
    "displayOrder":1
  }
]
```

---

# 6. Müsait Saatler

## Müsait slot getir

```
GET /appointments/available-slots?date=2026-04-27&serviceId=1&employeeId=1
```

`employeeId` opsiyonel olabilir.

Response:

```
[
  {
    "startTime":"10:00",
    "endTime":"10:30",
    "isAvailable":true
  },
  {
    "startTime":"10:30",
    "endTime":"11:00",
    "isAvailable":false
  }
]
```

---

# 7. Randevu Oluşturma Akışı

## 7.1 Randevu ön kaydı

```
POST /appointments/request
```

Request:

```
{
  "customerFirstName":"Ceren",
  "customerLastName":"Fırat",
  "phoneNumber":"+905555555555",
  "serviceId":1,
  "employeeId":1,
  "appointmentDate":"2026-04-27",
  "startTime":"10:00",
  "note":"İlk randevum"
}
```

Response:

```
{
  "appointmentRequestId":"req_123456",
  "otpRequired":true,
  "expiresAt":"2026-04-27T15:10:00+03:00",
  "message":"Telefonunuza doğrulama kodu gönderildi."
}
```

---

## 7.2 OTP ile randevu onaylama

```
POST /appointments/confirm
```

Request:

```
{
  "appointmentRequestId":"req_123456",
  "otpCode":"482913"
}
```

Response:

```
{
  "appointmentId":15,
  "status":"CONFIRMED",
  "message":"Randevunuz başarıyla oluşturuldu."
}
```

---

# 8. Randevu Sorgulama

```
POST /appointments/search-by-phone
```

Request:

```
{
  "phoneNumber":"+905555555555"
}
```

Response:

```
[
  {
    "id":15,
    "customerFullName":"Ceren Fırat",
    "phoneNumber": "+905555555555",
    "serviceName":"Saç Kesimi",
    "employeeName":"Ahmet Yılmaz",
    "appointmentDate": "2026-04-27",
    "startTime":"10:00",
    "endTime":"10:30",
    "status":"CONFIRMED"
  }
]
```

---

# 9. Randevu İptal Akışı

## 9.1 İptal OTP isteği

```
POST /appointments/{appointmentId}/cancel-request
```

Request:

```
{
  "phoneNumber":"+905555555555"
}
```

Response:

```
{
  "cancelRequestId":"cancel_123456",
  "otpRequired":true,
  "expiresAt":"2026-04-27T15:20:00+03:00"
}
```

---

## 9.2 OTP ile iptal onayı

```
POST /appointments/cancel-confirm
```

Request:

```
{
  "cancelRequestId":"cancel_123456",
  "otpCode":"928411"
}
```

Response:

```
{
  "appointmentId":15,
  "status":"CANCELLED",
  "message":"Randevunuz iptal edildi."
}
```

---

# 10. Chatbot API

## Chatbot başlangıç

```
POST /chatbot/start
```

Request:

```
{
  "phoneNumber":"+905555555555"
}
```

Response:

```
{
  "hasActiveAppointment":true,
  "appointments": [
    {
      "id":15,
      "serviceName":"Saç Kesimi",
      "employeeName":"Ahmet Yılmaz",
      "appointmentDate":"2026-04-27",
      "startTime":"10:00",
      "status":"CONFIRMED"
    }
  ],
  "actions": [
"CREATE_APPOINTMENT",
"QUERY_APPOINTMENT",
"CANCEL_APPOINTMENT"
  ]
}
```

---

# 11. Admin Auth

## Admin login

```
POST /admin/auth/login
```

Request:

```
{
  "email":"admin@example.com",
  "password":"123456"
}
```

Response:

```
{
  "accessToken":"jwt-token",
  "refreshToken":"refresh-token",
  "expiresAt":"2026-04-27T18:00:00+03:00",
  "admin": {
    "id":1,
    "fullName":"Admin Kullanıcı",
    "email":"admin@example.com",
    "role":"OWNER"
  }
}
```

---

## Refresh token

```
POST /admin/auth/refresh-token
```

Request:

```
{
  "refreshToken":"refresh-token"
}
```

Response:

```
{
  "accessToken":"new-jwt-token",
  "refreshToken":"new-refresh-token",
  "expiresAt":"2026-04-27T19:00:00+03:00"
}
```

---

## Logout

```
POST /admin/auth/logout
```

Request:

```
{
  "refreshToken":"refresh-token"
}
```

Response:

```
{
  "message":"Çıkış yapıldı."
}
```

---

# 12. Admin Dashboard

```
GET /admin/dashboard/summary
```

Response:

```
{
  "todayAppointmentCount":8,
  "tomorrowAppointmentCount":5,
  "weeklyAppointmentCount":32,
  "monthlyAppointmentCount":120,
  "monthlyRevenue":24500,
  "cancelledAppointmentCount":4,
  "completedAppointmentCount":65
}
```

---

# 13. Admin Hizmet Yönetimi

```
GET /admin/services
POST /admin/services
PUT /admin/services/{serviceId}
PATCH /admin/services/{serviceId}/status
DELETE /admin/services/{serviceId}
```

Create request:

```
{
  "name":"Saç Kesimi",
  "description":"Profesyonel saç kesimi",
  "price":300,
  "durationMinutes":30,
  "isActive":true
}
```

Status request:

```
{
  "isActive":false
}
```

---

# 14. Admin Çalışan Yönetimi

```
GET /admin/employees
POST /admin/employees
PUT /admin/employees/{employeeId}
PATCH /admin/employees/{employeeId}/status
DELETE /admin/employees/{employeeId}
```

Create request:

```
{
  "firstName":"Ahmet",
  "lastName":"Yılmaz",
  "phoneNumber":"+905555555555",
  "email":"ahmet@example.com",
  "label":"Uzman Berber",
  "imageUrl":"https://cdn.example.com/ahmet.jpg",
  "isActive":true,
  "serviceIds": [1,2]
}
```

---

# 15. Admin Çalışma Saatleri

## Genel çalışma saatleri

```
GET /admin/working-hours
PUT /admin/working-hours
```

Request:

```
[
  {
    "dayOfWeek":"MONDAY",
    "isWorkingDay":true,
    "startTime":"09:00",
    "endTime":"18:00",
    "breakStartTime":"12:00",
    "breakEndTime":"13:00"
  }
]
```

---

## Çalışana özel çalışma saatleri

```
GET /admin/employees/{employeeId}/working-hours
PUT /admin/employees/{employeeId}/working-hours
```

Request:

```
[
  {
    "dayOfWeek":"MONDAY",
    "isWorkingDay":true,
    "startTime":"10:00",
    "endTime":"17:00",
    "breakStartTime":"13:00",
    "breakEndTime":"14:00"
  }
]
```

---

# 16. Admin Kapalı Günler

```
GET /admin/closed-days
POST /admin/closed-days
DELETE /admin/closed-days/{closedDayId}
```

Create request:

```
{
  "date":"2026-05-01",
  "reason":"Resmi tatil",
  "employeeId":null
}
```

`employeeId: null` ise tüm işletme kapalı.

`employeeId: 1` ise sadece o çalışan kapalı.

---

# 17. Admin Randevu Yönetimi

## Listeleme

```
GET /admin/appointments?range=today
GET /admin/appointments?range=tomorrow
GET /admin/appointments?range=this-week
GET /admin/appointments?date=2026-04-27
GET /admin/appointments?status=CONFIRMED
```

Response:

```
[
  {
    "id":15,
    "customerFullName":"Ceren Fırat",
    "phoneNumber":"+905555555555",
    "serviceName":"Saç Kesimi",
    "employeeName":"Ahmet Yılmaz",
    "appointmentDate":"2026-04-27",
    "startTime":"10:00",
    "endTime":"10:30",
    "status":"CONFIRMED",
    "price":300,
    "note":"İlk randevum"
  }
]
```

---

## Admin randevu oluşturur

```
POST /admin/appointments
```

Request:

```
{
  "customerFirstName":"Ceren",
  "customerLastName":"Fırat",
  "phoneNumber":"+905555555555",
  "serviceId":1,
  "employeeId":1,
  "appointmentDate":"2026-04-27",
  "startTime":"10:00",
  "note":"Admin tarafından oluşturuldu"
}
```

Response:

```
{
  "appointmentId":15,
  "status":"CONFIRMED"
}
```

---

## Randevu güncelle

```
PUT /admin/appointments/{appointmentId}
```

Request:

```
{
  "serviceId":1,
  "employeeId":1,
  "appointmentDate":"2026-04-28",
  "startTime":"11:00",
  "note":"Saat değiştirildi"
}
```

---

## Randevu iptal

```
PATCH /admin/appointments/{appointmentId}/cancel
```

Request:

```
{
  "reason":"Müşteri talebi"
}
```

---

## Randevu tamamla

```
PATCH /admin/appointments/{appointmentId}/complete
```

Response:

```
{
  "appointmentId":15,
  "status":"COMPLETED"
}
```

---

## Gelmedi olarak işaretle

```
PATCH /admin/appointments/{appointmentId}/no-show
```

Response:

```
{
  "appointmentId":15,
  "status":"NO_SHOW"
}
```

---

# 18. Admin İşletme Profili Güncelleme

```
GET /admin/business/profile
PUT /admin/business/profile
```

Update request:

```
{
  "businessName":"Elite Berber",
  "description":"Profesyonel berber hizmetleri",
  "phoneNumber":"+905555555555",
  "email":"info@eliteberber.com",
  "address":"Malatya Merkez",
  "mapUrl":"https://maps.google.com/...",
  "instagramUrl":"https://instagram.com/eliteberber",
  "whatsappUrl":"https://wa.me/905555555555",
  "logoUrl":"https://cdn.example.com/logo.png",
  "coverImageUrl":"https://cdn.example.com/cover.png",
  "themeColor":"#111111"
}
```

---

# 19. Admin Galeri Yönetimi

```
GET /admin/gallery
POST /admin/gallery
PUT /admin/gallery/{imageId}
DELETE /admin/gallery/{imageId}
```

Create request:

```
{
  "imageUrl":"https://cdn.example.com/gallery-1.jpg",
  "title":"Saç Kesimi",
  "description":"Örnek çalışma",
  "displayOrder":1
}
```

---

# 20. Dosya Upload

Logo, çalışan resmi, galeri görseli için:

```
POST /admin/uploads/image
Content-Type: multipart/form-data
```

Form data:

```
file: image.png
folder: gallery
```

Response:

```
{
  "url":"https://cdn.example.com/gallery/image.png"
}
```

---

# 21. Veritabanı Tabloları

Net tablo listesi:

```
Business
Service
Employee
EmployeeService
WorkingHour
EmployeeWorkingHour
ClosedDay
Customer
Appointment
AppointmentRequest
OtpCode
SmsLog
GalleryImage
AdminUser
RefreshToken
```

---

# 22. businessId Kararı

Veritabanında şu tablolarda `businessId` olsun:

```
Service
Employee
WorkingHour
EmployeeWorkingHour
ClosedDay
Customer
Appointment
AppointmentRequest
OtpCode
SmsLog
GalleryImage
AdminUser
RefreshToken
```

Ama frontend şunu göndermeyecek:

```
{
  "businessId":1
}
```

Backend otomatik yapacak:

```
businessId = currentBusinessId
```

Ayrı DB modelinde:

```
currentBusinessId = 1
```

SaaS modele geçince:

```
currentBusinessId = domain, subdomain veya JWT üzerinden çözülür.
```

---

# 23. Frontend İçin Net Kural

Next.js tarafı her işletme için sadece `.env` değiştirsin:

```
NEXT_PUBLIC_API_BASE_URL=https://api.eliteberber.com/api/v1
NEXT_PUBLIC_BUSINESS_TYPE=BARBER
NEXT_PUBLIC_THEME_COLOR=#111111
```

Frontend requestlerinde **businessId yok**.

Doğru:

```
{
  "name":"Saç Kesimi",
  "price":300
}
```

Yanlış:

```
{
  "businessId":1,
  "name":"Saç Kesimi",
  "price":300
}
```

---

# 24. Backend İçin Net Kural

Backend her requestte işletmeyi şöyle çözmeli:

```
1. Eğer ayrı DB ise:
   currentBusinessId = appsettings/env içinden alınır.

2. Eğer tek DB SaaS ise:
   currentBusinessId = domain/subdomain/JWT içinden alınır.
```

Örnek backend config:

```
{
  "Business": {
    "DefaultBusinessId":1,
    "BusinessType":"BARBER"
  }
}
```
