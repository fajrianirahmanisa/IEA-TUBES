# 📚 Fine System – Library Microservices (Enterprise Application Integration)
Fine System adalah Sistem Denda Perpustakaan berbasis Microservices yang dikembangkan sebagai bagian dari Tugas Besar Integrasi Aplikasi Enterprise.
Sistem ini menangani perhitungan denda, aturan denda, pembayaran, serta laporan keuangan, dan terintegrasi lintas kelompok dengan Library System menggunakan GraphQL.
# 🧩 Arsitektur Sistem

Fine System menerapkan Microservices Architecture dengan prinsip:

Database-per-Service

Service Autonomy

API-First Design menggunakan GraphQL

Containerized Deployment (Docker & Docker Compose)

# Komponen Microservices
Service	Port	Fungsi
Fine Service	5001	Perhitungan denda & integrasi utama
Fine Rule Service	5002	Manajemen aturan denda
Fine Payment Service	5003	Proses pembayaran denda
Report Service	5004	Laporan & analitik keuangan

Setiap service memiliki database MySQL 8.0 terpisah.

# 🔗 Integrasi Lintas Kelompok (Wajib)

Fine System berperan ganda sebagai:

✅ Provider GraphQL

Digunakan oleh Library System (kelompok lain), khususnya:

Return Service → cek & hitung denda

Validasi status pembayaran anggota

Contoh query:

query {
  getMemberFines(memberId: "123") {
    fineId
    amount
    status
    dueDate
  }
}

✅ Consumer GraphQL

Fine System mengkonsumsi:

Member Service (Library System) → data anggota

Return Service → data pengembalian buku

# 🧪 Tech Stack

Backend: Node.js, Express

API: Apollo GraphQL Server

Database: MySQL 8.0

ORM: Sequelize

Deployment: Docker, Docker Compose

Security: JWT (RS256 – Stateless Verification)

# 📁 Struktur Repository
FINE-SYSTEMS/
│
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
1️⃣ Prasyarat
Docker
Docker Compose
Git

2️⃣ Clone Repository
git clone https://github.com/username/FINE-SYSTEMS.git
cd FINE-SYSTEMS

3️⃣ Jalankan Semua Service
docker-compose up --build

4️⃣ Akses GraphQL Playground
Service	URL
Fine Service	http://localhost:5001/graphql

Fine Rule Service	http://localhost:5002/graphql

Fine Payment Service	http://localhost:5003/graphql

Report Service	http://localhost:5004/graphql
# 🔐 Keamanan (JWT)

Autentikasi menggunakan JWT RS256

Token diverifikasi secara stateless di tiap service

Tidak ada penyimpanan data user/password di service bisnis

Response error standar:

401 Unauthorized

403 Forbidden

# 🧪 Contoh Alur Pengujian

Login → mendapatkan JWT

Return Service memanggil Fine Service

Fine Service:

Ambil aturan dari Fine Rule Service

Hitung denda

Payment Service memproses pembayaran

Report Service menghasilkan laporan

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
