# S-AES Simulator — Web Simulasi Simplified AES

> **Tugas Individu — Mata Kuliah Kriptografi**  
> Pembangunan Web Simulasi Simplified Advanced Encryption Standard (S-AES)

---

## 🔗 Links

| | Link |
|---|---|
| 🌐 Aplikasi Web | [[URL .my.id — isi setelah deploy]](#) |
| 📹 Video YouTube | [[URL YouTube — isi setelah upload]](#) |
| 📄 Laporan | Lihat file `Laporan_Project_SAES.docx` |

---

## 📋 Deskripsi

Aplikasi web simulasi **S-AES (Simplified Advanced Encryption Standard)** yang dibangun sebagai tugas mata kuliah Kriptografi. Aplikasi menampilkan proses enkripsi dan dekripsi S-AES secara lengkap dengan visualisasi step-by-step setiap transformasi.

**S-AES** adalah versi penyederhanaan AES (Schaefer & Waidner, 1998) yang dirancang untuk keperluan pembelajaran, dengan parameter:

| Parameter | Nilai |
|---|---|
| Ukuran Blok | 16 bit |
| Ukuran Kunci | 16 bit |
| Jumlah Ronde | 2 |
| Field Aritmetika | GF(2⁴) dengan polinomial x⁴+x+1 |

---

## ✨ Fitur Aplikasi

- **Enkripsi & Dekripsi** — Input plaintext/ciphertext dan kunci 16-bit biner
- **Validasi Input** — Hanya menerima 16 karakter biner (0 dan 1)
- **Output Lengkap** — Hasil ditampilkan dalam format biner dan heksadesimal
- **Step-by-Step Detail** — Setiap transformasi ditampilkan secara rinci:
  - Key Expansion (w0–w5, K0, K1, K2)
  - Initial AddRoundKey
  - Round 1: SubNibbles, ShiftRows, MixColumns, AddRoundKey
  - Round 2: SubNibbles, ShiftRows, AddRoundKey
  - (Dekripsi) Inverse dari semua transformasi di atas
- **Visualisasi Matriks** — State 2×2 ditampilkan dalam kotak dengan nilai hex dan biner
- **Detail GF(2⁴)** — Setiap perkalian GF(2⁴) pada MixColumns ditampilkan
- **Tabel Referensi** — S-Box, Inverse S-Box, MixColumns, RCON
- **Responsif** — Dapat digunakan di desktop maupun mobile

---

## 🚀 Cara Menggunakan

### Online
Akses langsung di: **[URL .my.id]**

### Lokal
```bash
# Clone repository
git clone https://github.com/[username]/[repo-name].git
cd [repo-name]

# Buka langsung di browser (tidak perlu server)
xdg-open saes-simulator.html     # Linux
open saes-simulator.html          # macOS
start saes-simulator.html         # Windows
```

---

## 🔬 Contoh Pengujian

| Input | Nilai | Hex |
|---|---|---|
| Plaintext | `0110 1111 0110 1011` | `0x6F6B` |
| Kunci | `1010 0111 0011 1011` | `0xA73B` |
| **Ciphertext** | `0000 0111 0011 1000` | **`0x0738`** |

**Key Expansion** untuk kunci `0xA73B`:
```
w0 = 0xA7  (1010 0111)
w1 = 0x3B  (0011 1011)
w2 = 0x1C  (0001 1100)  → K1 high byte
w3 = 0x27  (0010 0111)  → K1 low byte
w4 = 0x76  (0111 0110)  → K2 high byte
w5 = 0x51  (0101 0001)  → K2 low byte
K0 = 0xA73B | K1 = 0x1C27 | K2 = 0x7651
```

---

## 📐 Implementasi Teknis

### Operasi GF(2⁴)
```javascript
function gfMul(a, b) {
  let r = 0;
  for (let i = 0; i < 4; i++) {
    if (b & 1) r ^= a;
    const hb = a & 0x8;
    a = (a << 1) & 0xF;
    if (hb) a ^= 0x3;  // reduksi: x^4 ≡ x+1 (mod x^4+x+1)
    b >>= 1;
  }
  return r;
}
```

### Struktur Kode
```
saes-simulator.html
├── CSS (dark theme, responsive)
├── HTML (header, form, ref tables, result, steps)
└── JavaScript
    ├── Konstanta: SBOX, INV_SBOX, MIXCOL, INV_MIXCOL, RCON
    ├── GF(2⁴): gfMul()
    ├── Key Expansion: expandKey(), rotWord(), subWord()
    ├── Transformasi: ark(), subNibs(), shiftRows(), mixColumns()
    ├── Enkripsi: encrypt() + step logging
    ├── Dekripsi: decrypt() + step logging
    └── UI Render: renderMat(), renderSnContent(), renderMcContent() dst.
```

---

## 📚 Referensi

- Stallings, W. (2017). *Cryptography and Network Security: Principles and Practice* (7th ed.). Pearson Education.
- Schaefer, E. F., & Waidner, M. (1998). A Simplified AES Algorithm and Its Linear and Differential Cryptanalysis. *Cryptologia, 22*(4), 148–177.
- NIST. (2001). *FIPS PUB 197: Advanced Encryption Standard (AES)*.

---

## 📁 Isi Repository

```
.
├── saes-simulator.html       ← Aplikasi utama (single file)
├── Laporan_Project_SAES.docx ← Laporan lengkap
├── Skrip_Video_SAES.md       ← Skrip presentasi video
└── README.md                 ← File ini
```

---

## ⚖️ Ketentuan Akademik

Proyek ini dikerjakan secara individu sebagai tugas mata kuliah Kriptografi.  
Implementasi algoritma merupakan hasil pekerjaan sendiri berdasarkan referensi literatur di atas.
