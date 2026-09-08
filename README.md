# Odoo CRM Pipeline Lifecycle & Automation (P0 - P5)

Dokumentasi dan spesifikasi aturan resmi otomatisasi pergeseran stage CRM Pipeline untuk tim Sales Telemarketing B2B / Pengadaan Pemerintah PT. Aston Graphindo Indonesia.

---

## 📌 Definisi & Matriks Aturan Stage (P0 - P5)

| Stage | Nama Stage | Definisi & Karakteristik | Syarat Sah Masuk Stage (*Evidence*) | Batas Waktu (*SLA*) | Aksi Saat Expired (*Macet*) |
|---|---|---|---|---|---|
| **P4** | **Data Mentah (*Raw Leads*)** | Data leads yang hanya memiliki nama instansi saja (hasil import / scrap SIRUP). | Belum ada nomor kantor tersambung & belum ada nama PIC. | **14 Hari** | **Auto-Archive** (Disimpan/diarsipkan agar papan Kanban bersih). |
| **P3** | **Aware (*Mulai Tertarik*)** | Customer sudah mengenal produk & ada minat awal, namun belum ada rencana pengadaan waktu dekat. | Ada Nama PIC + Nomor HP/WA tersambung + Minimal 1 log interaksi valid. | **30 Hari** | **Turun ke P5** (Data Tidak Bergerak). |
| **P2** | **Interest (*Ada Penawaran*)** | Kontak valid & ada rencana pengadaan 1–2 bulan ke depan. | **WAJIB terbit Surat Penawaran Harga resmi (SPH)** via modul generator penawaran. | **45 Hari** | **Turun ke P5** jika masa berlaku penawaran habis. |
| **P1** | **Desire (*Hot Lead Bulan Ini*)** | Prioritas sangat tinggi, harga & spesifikasi disetujui, siap eksekusi pembelian. | **WAJIB dibuatkan Form Purchase Order (PO) Customer** di Odoo. | **15 Hari** | **Turun ke P2 atau P5** jika lewat bulan berjalan tanpa deal. |
| **P0** | **Deal (*Won / Hubungan Relasi*)** | Transaksi berhasil (Deal), Form Purchase Order (PO) Customer sudah dikonfirmasi (Confirmed / Ditandatangani). | **Form PO Customer status `Confirmed`**. | **60 Hari** | **Otomatis turun ke P3** untuk pemeliharaan relasi & penjajakan repeat order (Bukan diarsipkan). |
| **P5** | **Data Tidak Bergerak (*Stagnant*)** | Kontak & PIC lengkap, berasal dari P3/P2/P1 yang macet / tidak ada respon. | Nama PIC + Nomor HP valid. | **90 Hari** | Data dingin. Otomatis **naik kembali ke P3** jika ada respon/interaksi baru. |

---

## ⚙️ Dokumen Pemicu Otomatis (*Triggers*)
1. **Trigger Naik ke P2 (Interest):** Terbit Dokumen **Surat Penawaran Harga (SPH)** resmi.
2. **Trigger Naik ke P1 (Desire / Hot):** Dibuatkan Dokumen **Form Purchase Order (PO) Customer** (Draft).
3. **Trigger Naik ke P0 (Deal / Won):** Dokumen **Form Purchase Order (PO) Customer** diubah statusnya menjadi **Confirmed / Disetujui**.
4. **Trigger Turun ke P3 (Aware):** Setelah 60 hari masa transaksi P0 selesai tanpa order baru, lead otomatis digeser ke P3 untuk relasi berkala.
5. **Trigger Turun ke P5 (Tidak Bergerak):** Melewati batas waktu aktivitas (*Last Activity Inactivity*) tanpa interaksi baru.

---

## 📂 Pengaturan Admin
- Lokasi Menu: **CRM > Konfigurasi > Aturan Stage Pipeline (P0-P5)**
- Admin dapat mengubah angka batas waktu (hari), syarat sah, serta aksi tujuan langsung dari antarmuka Odoo.
