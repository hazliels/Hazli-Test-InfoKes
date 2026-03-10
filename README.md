# Soal 1 - Fundamental QA dan Quality Judgment

## 1. Testing Scope Decision

Dengan kondisi tim yang terbilang kecil (3 Backend Engineer, 2 Frontend Engineer, dan 1 Product Manager), deadline hanya 7 hari, serta environment yang sering tidak sinkron, maka pendekatan testing harus dilakukan secara **risk-based testing**.

Karena keterbatasan waktu dan resource, saya akan memprioritaskan testing pada fitur yang memiliki **dampak terbesar terhadap user dan sistem**.

Dari tiga fitur yang akan dirilis, maka prioritas testing saya adalah sebagai berikut:

---

### Login & Logout (Priority 1)

Login adalah gerbang awal sebuah aplikasi. Jika user gagal melakukan login maka user tidak bisa menggunakan aplikasi sama sekali. Karena itu risiko bisnis dan risiko terhadap pengalaman user sangat tinggi.

Fitur ini juga biasanya berhubungan dengan beberapa komponen penting seperti:

- Authentication
- Session management
- Security validation

Jika ada bug pada fitur ini, dampaknya bisa sangat luas, misalnya:

- User tidak bisa login
- Session tidak tersimpan
- Logout tidak benar-benar mengakhiri session

Karena itu fitur ini akan saya test **secara mendalam**.

---

### Reset Password (Priority 2)

Berbeda dengan Login & Logout yang harus ditest secara mendalam atau Edit Profile yang hanya ditest secara minimal, **Reset Password juga termasuk fitur yang cukup kritikal** karena berkaitan langsung dengan **pemulihan akun pengguna**.

Dalam beberapa kasus, banyak user sering lupa password. Jika fitur reset password bermasalah maka user bisa mengalami beberapa pengalaman seperti:

- Tidak bisa mengakses kembali akun mereka
- Link untuk reset password tidak berjalan
- Password baru tidak bisa digunakan untuk login

Karena fitur ini berkaitan langsung dengan **account recovery**, maka saya juga akan menguji fitur ini secara **cukup mendalam**.

---

### Edit Profile (Priority 3)

Fitur edit profile tentu tetap penting, tetapi dari sisi prioritas risikonya **lebih rendah dibanding Login/Logout dan Reset Password**.

Jika terjadi bug pada edit profile seperti:

- Nama tidak terupdate
- Validation message kurang tepat

User masih tetap bisa:

- Login
- Menggunakan fitur utama aplikasi

Artinya bug di area ini biasanya **tidak langsung memblokir user flow dari penggunaan aplikasi**.

Dengan deadline yang cukup ketat, saya kemungkinan akan melakukan **testing dasar saja**, seperti:

- Update field profile
- Save perubahan
- Cek data tersimpan

Jika ditemukan bug kecil, kemungkinan bug tersebut bisa diperbaiki pada **sprint berikutnya**, sesuai dengan arahan PM bahwa bug minor bisa menyusul.

---

# 2. Prioritization Logic

Jika saya hanya memiliki **1 hari testing efektif**, maka saya harus lebih fokus lagi pada **critical user flow**.

Urutan prioritas testing saya kemungkinan akan seperti berikut:

---

### Login

Login akan menjadi prioritas pertama karena tanpa login user tidak bisa menggunakan aplikasi.

Dalam waktu terbatas saya akan memastikan beberapa skenario penting seperti:

- Login dengan credential benar
- Login dengan credential salah
- Validasi input
- Login setelah logout
- Session behavior

---

### Reset Password

Setelah fokus pada fitur login, saya akan fokus ke fitur **Reset Password**.

Alasannya adalah fitur ini berkaitan dengan **pemulihan akun**. Jika login bermasalah dan reset password juga tidak berjalan, maka user benar-benar tidak memiliki cara lain untuk mengakses kembali akunnya.

---

### Logout

Logout juga penting karena berkaitan dengan **session management**, tetapi secara prioritas biasanya sedikit lebih rendah dibanding login.

Jika logout bermasalah, biasanya user masih bisa:

- menutup browser
- menunggu session expired

---

### Edit Profile

Edit profile akan menjadi prioritas terakhir karena tidak mempengaruhi akses user terhadap aplikasi.

Jika ada bug kecil pada fitur ini, biasanya masih bisa diterima untuk rilis awal selama tidak merusak data secara serius.

---

# 3. Handling Conflict

Jika developer mengatakan bahwa behavior tersebut **expected**, tetapi menurut saya berpotensi memberikan **pengalaman buruk bagi user**, saya biasanya tidak langsung menyimpulkan bahwa itu bug.

Langkah pertama yang saya lakukan adalah mencoba **memahami konteksnya terlebih dahulu**.

Saya akan mengecek kembali:

- Requirement
- Dokumentasi
- Acceptance Criteria

Kadang perbedaan terjadi karena requirement tidak cukup jelas atau masing-masing pihak memiliki interpretasi yang berbeda.

Jika setelah dicek ternyata behavior tersebut memang sesuai implementasi developer, saya akan mencoba menjelaskan dari **sudut pandang user experience**.

Jika diskusi dengan developer masih belum menemukan titik terang, langkah berikutnya adalah **melibatkan Product Manager**, karena pada akhirnya keputusan apakah suatu behavior dapat diterima atau tidak biasanya merupakan **product decision**, bukan hanya technical decision.

Dalam situasi tersebut saya akan menjelaskan:

- Apa behavior yang terjadi
- Bagaimana user kemungkinan akan melihatnya
- Apa potensi dampaknya

Apabila Product Manager tetap memutuskan behavior tersebut dapat diterima untuk rilis, saya akan memastikan bahwa issue tersebut tetap **didokumentasikan dengan baik** sebagai referensi untuk perbaikan di masa depan.

---

# 4. Quality Risk Awareness

Jika rilis tetap dilakukan dengan waktu testing terbatas, maka ada beberapa risiko yang perlu diperhatikan.

---

### Risiko 1 – User tidak bisa login

Ini adalah risiko terbesar karena login merupakan **gerbang utama untuk menggunakan aplikasi**.

Jika saya menemukan bug yang berpotensi menyebabkan user tidak bisa login, maka saya akan **mengeskalasi issue tersebut kepada developer dan product manager sebelum rilis dilakukan**.

Bug pada area ini biasanya termasuk **release blocker**.

---

### Risiko 2 – Bug kecil pada Edit Profile

Risiko lain yang mungkin terjadi adalah bug pada fitur seperti edit profile.

Contohnya:

- Perubahan profile tidak langsung muncul
- Data tidak langsung terupdate di UI

Bug seperti ini tetap perlu diperbaiki, tetapi biasanya **tidak langsung menghalangi user menggunakan aplikasi**.

Karena itu, dalam kondisi deadline yang ketat, saya masih bisa menerima risiko ini untuk rilis awal dengan catatan:

- Bug sudah didokumentasikan
- Dibuatkan ticket untuk perbaikan
- Dijadwalkan untuk diperbaiki pada sprint berikutnya

# Soal 2 - Automation UI dan Test Engineering Judgment

## Part A - Analisa Automation Existing

Jika saya bergabung dalam tim yang sudah memiliki automation tetapi hasilnya sering flaky, hal pertama yang saya lakukan adalah mencoba memahami mengapa test automation tersebut bisa gagal secara flaky. Berdasarkan pengalaman saya, ada beberapa penyebab mengapa automation UI bisa menjadi flaky.

### 1. Masalah timing issue antara test dan aplikasi
Automation sangat bergantung pada kondisi halaman, terkadang aplikasi membutuhkan waktu untuk memproses data, menampilkan element, gambar, atau menyelesaikan request API. Jika script automation langsung mencoba klik atau validasi sebelum elemen tersebut benar-benar muncul, maka test bisa gagal walaupun sebenarnya aplikasi tidak bermasalah. Hal ini biasanya terjadi karena script automationnya tidak menggunakan mekanisme wait sama sekali. Dari pengalaman saya, ini adalah penyebab flaky test yang paling sering saya temui.

### 2. Locator atau selector yang berubah-ubah
Dalam automation UI, test biasanya bergantung pada locator seperti id, class, xpath atau css selector untuk menemukan element di halaman. Jika locator yang digunakan terlalu bergantung pada struktur class yang sering berubah, maka test bisa gagal setiap kali ada perubahan pada UI. Misalnya menggunakan xpath yang sangat panjang. Ketika developer melakukan update UI maka automation test yang menggunakan locator tersebut bisa langsung gagal walaupun secara fungsi aplikasi masih berjalan. Oleh karena itu, penting untuk menggunakan locator yang lebih stabil seperti id.

### 3. Data test yang tidak konsisten
Penyebab ketiga adalah data test yang tidak konsisten, terutama karena automation berjalan headless di CI. Kondisi environment bisa berbeda antara server dan lokal. Misalnya, data di database berubah atau environment tidak sepenuhnya sinkron dengan versi aplikasi terbaru. Jika automation test bergantung pada data tertentu yang tidak dikontrol dengan baik, maka test bisa gagal secara flaky.

Dari ketiga penyebab tersebut, yang paling sering saya temui secara pribadi adalah masalah timing issue. Banyak automation UI dibuat dengan asumsi bahwa halaman akan selalu memuat dengan cepat, padahal dalam kenyataannya waktu loading bisa berbeda-beda tergantung kondisi network, server, atau proses lainnya yang terjadi di infrastruktur server.

---

## Part B - Design Automation Test

### Pseudocode Login Automation Flow

#### LoginPage Function
FUNCTION login(username, password)
OPEN browser
MAXIMIZE browser window

NAVIGATE to "URL"

WAIT until username field is visible (max 10 seconds)

INPUT username into username field
INPUT password into password field

CLICK login button
END FUNCTION


#### Test Case Flow
CREATE instance of LoginPage

CALL login function with:
username = "Admin"
password = "admin123"

WAIT until Dashboard title is visible (max 15 seconds)

GET text from Dashboard title

IF Dashboard title equals "Dashboard"
TEST PASS
ELSE
TEST FAIL

CLOSE browser

---

## Part C - Failure Investigation

Jika automation test selalu berhasil di local tetapi gagal di CI, hal pertama yang saya curigakan adalah kemungkinan adanya perbedaan kondisi environment antara local dan CI.

Langkah pertama yang saya lakukan adalah mereview hasil failure di CI secara detail. Saya akan melihat log eksekusi test dan error message yang muncul. Dari log tersebut biasanya sudah bisa terlihat apakah test gagal karena element tidak ditemukan, timeout, assertion gagal, atau dari aplikasi. Ini langkah awal untuk menentukan arah investigasi selanjutnya.

Langkah kedua adalah mencoba mereproduce kondisi CI di local. Kadang automation berjalan dengan versi browser yang berbeda. Karena itu saya akan mencoba menjalankan test di local dengan konfigurasi yang sama dengan CI. Tujuannya untuk memastikan apakah kegagalan tersebut memang hanya terjadi di CI atau sebenarnya bisa direproduksi di kondisi tertentu.

Langkah ketiga adalah memeriksa perbedaan environment antara CI dan local. Beberapa hal yang biasanya di check adalah versi browser, versi driver, konfigurasi environment variable. Dalam beberapa kasus, test gagal karena data di environment CI tidak sama dengan data yang digunakan saat testing di local.

Setelah investigasi dilakukan, saya akan menentukan tindakan terhadap test tersebut berdasarkan hasil temuan. Jika ternyata masalahnya ada pada script automation, misalnya karena mekanisme wait yang kurang tepat atau locator yang tidak stabil, maka test tersebut perlu diperbaiki.

Jika kegagalan disebabkan oleh bug pada aplikasi, maka test tetap dipertahankan dan bug perlu diperbaiki.

Namun jika test tersebut terus gagal karena faktor yang belum bisa diperbaiki dalam waktu dekat, misalnya dependency terhadap service atau environment yang tidak stabil, maka saya bisa mempertimbangkan untuk sementara men-disable test tersebut agar tidak menggangu pipeline CI.

Jika test sudah tidak relevan lagi dengan fitur yang ada atau terlalu mahal untuk dipelihara dibanding manfaatnya, maka opsi terakhir adalah menghapus test tersebut.
