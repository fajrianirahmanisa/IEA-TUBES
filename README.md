# 📚 Fine System – Library Microservices (Enterprise Application Integration)
Fine System merupakan sistem pengelolaan denda perpustakaan yang dikembangkan menggunakan arsitektur microservices sebagai bagian dari Tugas Besar Mata Kuliah Integrasi Aplikasi Enterprise.
- Sistem ini berfokus pada pengelolaan:
- Perhitungan denda keterlambatan
- Manajemen aturan denda
- Proses pembayaran denda (simulasi)
- Laporan dan analitik keuangan
Fine System terintegrasi lintas kelompok dengan Library System menggunakan GraphQL over HTTP, serta dideploy menggunakan Docker & Docker Compose.
# 🎯 Tujuan Pengembangan
- Menerapkan konsep Enterprise Application Integration
- Mengimplementasikan GraphQL sebagai mekanisme komunikasi antar service
- Menerapkan Microservices Architecture
- Menggunakan Docker untuk containerization
- Membuktikan integrasi lintas kelompok (provider & consumer)
# 🧩 Arsitektur Sistem
- Fine System menerapkan prinsip:
- Separation of Concerns
- Database per Service
- Service Autonomy
- API-First Design
- Containerized Deployment
# Microservices yang Dikembangkan

| Service              | Port | Deskripsi                           |
| -------------------- | ---- | ----------------------------------- |
| Fine Service         | 5001 | Perhitungan denda & integrasi utama |
| Fine Rule Service    | 5002 | Manajemen aturan denda              |
| Fine Payment Service | 5003 | Proses pembayaran denda             |
| Report Service       | 5004 | Laporan & analitik                  |

Setiap service memiliki database MySQL 8.0 terpisah.
# 🔗 Integrasi Lintas Kelompok (Wajib)

Fine System berperan ganda sebagai:
Fine System sebagai Provider
Digunakan oleh Library System (kelompok lain):
- Return Service → hitung denda
- Validasi status pembayaran anggota
Contoh query:
query {
  getMemberFines(memberId: "123") {
    fineId
    amount
    status
    dueDate
  }
}
Fine System sebagai Consumer
Mengambil data dari Library System:
- Member Service → data anggota
- Return Service → data pengembalian
## 📁 Struktur Repository
FINE-SYSTEMS/
├── fine-service/
│   ├── src/

│   ├── schemas/

│   ├── Dockerfile

│

├── fine-rule-service/

│   ├── src/

│   ├── schemas/

│   ├── Dockerfile

│

├── fine-payment-service/

│   ├── src/

│   ├── schemas/

│   ├── Dockerfile

│

├── report-service/

│   ├── src/

│   ├── schemas/

│   ├── Dockerfile

│

├── docker-compose.yml

└── README.md


# 🚀 Cara Menjalankan Sistem
1. Clone Repository
git clone https://github.com/username/FINE-SYSTEMS.git
cd FINE-SYSTEMS
2. Jalankan Docker
docker-compose up --build
3. Akses GraphQL Playground

| Service              | URL                                                            |
| -------------------- | -------------------------------------------------------------- |
| Fine Service         | [http://localhost:5001/graphql](http://localhost:5001/graphql) |
| Fine Rule Service    | [http://localhost:5002/graphql](http://localhost:5002/graphql) |
| Fine Payment Service | [http://localhost:5003/graphql](http://localhost:5003/graphql) |
| Report Service       | [http://localhost:5004/graphql](http://localhost:5004/graphql) |
# 🔐 Keamanan (JWT)

- Autentikasi menggunakan JWT RS256
- Verifikasi token dilakukan secara stateless
- Tidak ada penyimpanan user/password di service bisnis
- Standar error:
  401 Unauthorized
  403 Forbidden

# 🧪 Contoh Alur Pengujian
Pengujian dilakukan dengan:
- GraphQL Playground
- Request antar service melalui Docker Network
- Simulasi integrasi lintas kelompok
# 🛠️ Troubleshooting

❌ Container tidak bisa connect ke database
Solusi:
Pastikan service dan database berada di network Docker yang sama
Gunakan service name, bukan localhost
DB_HOST=fine-db

❌ Error ECONNREFUSED antar service
Solusi:
Pastikan semua container berjalan:
docker ps
Pastikan dependency service sudah depends_on di docker-compose.yml

❌ GraphQL Schema tidak terbaca
Solusi:
Pastikan file .graphql dimuat di Apollo Server
Restart container setelah perubahan schema

❌ Token JWT tidak valid
Solusi:
Pastikan public key JWT benar
Cek expired time token (15–30 menit)

# 👥 Tim Pengembang
| Nama                   | Role                             | Service              |
| ---------------------- | -------------------------------- | -------------------- |
| Evi Nirmalasari        | Fine Calculation Engineer        | Fine Service         |
| Meisya Ayu Sashi P.    | Rule Management Specialist       | Fine Rule Service    |
| Fikri Surya Prayoga    | Payment Engineer                 | Fine Payment Service |
|   Fajriani Rahmanisa   | Analytics & Reporting Specialist | Report Service       |
