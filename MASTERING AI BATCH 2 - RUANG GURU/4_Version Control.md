# Version Control dengan Git 🚀

## 📌 Object-Oriented Programming (OOP) - Recap Singkat

### Kenapa Butuh OOP?

Sebelumnya kita punya beberapa cara untuk menyimpan data student:
```python
# Cara 1: Variabel terpisah (ribet!)
student_name = "Levi"
student_age = 14
student_grade = 9

# Cara 2: Tuple (gak jelas!)
student = ("Levi", 14, 9)

# Cara 3: List (sama aja gak jelas!)
student = ["Levi", 14, 9]

# Cara 4: Dictionary (lumayan!)
student = {
    "name": "Levi",
    "age": 14,
    "grades": {"English": 9, "Math": 8, "Science": 7}
}

# Cara 5: OOP (TERBAIK!) 🎯
```

### Konsep OOP

**Class** = Blueprint/cetak biru
**Object** = Hasil jadi dari blueprint

```python
class Student:
    def __init__(self, name, age, grades):
        self.name = name
        self.age = age
        self.grades = grades
    
    def calculate_gpa(self):
        total = sum(self.grades.values())
        count = len(self.grades)
        return total / count
    
    def display_grades(self):
        print(f"Nilai {self.name}:")
        for subject, grade in self.grades.items():
            print(f"\t{subject}\t\t{grade}")

# Cara pakai:
student1 = Student("Levi", 14, {"English": 9, "Math": 8, "Science": 7})
student1.display_grades()
gpa = student1.calculate_gpa()
```

### Komponen Penting OOP

1. **`__init__`** = Constructor, dipanggil pertama kali saat bikin object
2. **`self`** = Referring ke diri sendiri, untuk akses data internal
3. **Method** = Function yang ada di dalam class
4. **Attributes** = Data yang dimiliki object (contoh: `self.name`, `self.age`)

### Keuntungan OOP vs Functional

**Functional Programming:**
- Data dan behavior terpisah
- Function menerima data sebagai parameter
- Contoh: `display_grades(student1)`

**OOP:**
- Data dan behavior jadi satu paket! 📦
- Lebih terproteksi (enkapsulasi)
- Lebih mudah dipahami
- Contoh: `student1.display_grades()`

```python
# Contoh enkapsulasi:
# Kalau functional, bisa sembarangan diubah:
def increase_grade(student):
    student["grades"]["Math"] += 10  # Bahaya! Bisa curang!

# Kalau OOP, lebih terkontrol
```

### Magic Methods

- `__init__`: Constructor
- `__str__`: Mengubah object jadi string saat di-print
- `__repr__`: Representasi object

```python
class Pawn:
    def __init__(self, color, x, y):
        self.color = color
        self.x = x
        self.y = y
    
    def __str__(self):
        return "P"  # Tampil sebagai "P" saat di-print
```

### Private vs Public

- **Public**: Method/attribute yang bisa diakses dari luar
- **Private**: Pakai underscore (`_method_name`) supaya "tersembunyi"

```python
class Board:
    def print_board(self):  # Public
        self._draw_grid()   # Private (helper)
    
    def _draw_grid(self):   # Tidak perlu diketahui user
        pass
```

---

## 🎯 Version Control dengan Git

### Masalah Tanpa Version Control

Pernah ngalamin ini? 😅

```
skripsi.docx
skripsi_final.docx
skripsi_final_revisi.docx
skripsi_final_revisi2.docx
skripsi_final_revisi_dosen.docx
skripsi_final_ACC.docx
skripsi_final_ACC_revisi.docx
```

**Masalahnya:**
1. File jadi banyak banget!
2. Bingung mana yang terbaru
3. Kalau komputer rusak/hilang, semuanya hilang!
4. Susah kolaborasi
5. Gak tahu siapa yang ngubah apa

### Solusi: Git!

Git adalah **distributed version control system** yang:
- ✅ Menyimpan semua history perubahan
- ✅ Bisa balik ke versi lama (time machine!)
- ✅ Gampang kolaborasi
- ✅ Aman dari kehilangan data
- ✅ Tahu siapa yang ngubah apa dan kapan

---

## 📖 Konsep Dasar Git

### 1. Repository

**Repository** = Tempat penyimpanan semua file + history-nya

Cara bikin:
- Buka folder project di VS Code
- Klik icon Source Control (Git icon)
- Klik "Initialize Repository"

### 2. Commit

**Commit** = Menyimpan snapshot/foto kondisi project di satu waktu

Analogi: Kayak save game di video game, nanti bisa load lagi!

**Cara commit:**
1. Ubah file
2. File akan muncul di Source Control dengan tanda **U** (Untracked) atau **M** (Modified)
3. Review perubahan (hijau = tambah, merah = hapus)
4. Klik **+** untuk stage changes
5. Tulis commit message (jelas dan singkat!)
6. Klik **✓** untuk commit

**Tips Commit Message:**
- ✅ "Add introduction section"
- ✅ "Fix typo in pendahuluan"
- ✅ "Refactor calculate_gpa function"
- ❌ "update" (gak jelas!)
- ❌ "asdfgh" (ngawur!)

### 3. Commit History

**Commit History** = Daftar semua commit yang pernah dilakukan

Bisa lihat:
- Siapa yang bikin perubahan
- Kapan perubahannya
- Apa yang diubah
- Kode sebelum vs sesudah

### 4. Checkout

**Checkout** = Berpindah ke commit tertentu (time travel!)

Cara:
- Buka Git History
- Klik kanan pada commit yang diinginkan
- Pilih "Switch to Commit"

**Kegunaan:**
- Balik ke versi yang stabil kalau versi baru ada bug
- Lihat kode versi lama
- Research kapan bug mulai muncul

---

## 🌐 Kolaborasi dengan GitHub

### Kenapa GitHub?

Git itu **distributed**, tapi tetap butuh tempat "central" buat kolaborasi:

```
Komputer Levi ←→ GitHub ←→ Komputer Rahmat
```

Tanpa GitHub:
- Susah kolaborasi (harus kirim file manual)
- Gak ada backup online
- Gak bisa buka dari komputer lain

### Workflow Dasar

**1. Setup Awal (Sekali Aja)**

```bash
# Di GitHub: Create repository
# Di terminal:
git clone <url-repository>
```

**2. Workflow Sehari-hari**

```bash
# 1. Ambil update terbaru
git pull

# 2. Kerja, ubah-ubah file
# ...

# 3. Commit changes (bisa berkali-kali)
git add .
git commit -m "Add feature X"

# 4. Upload ke GitHub
git push
```

### Perintah Git Penting

| Perintah | Fungsi |
|----------|--------|
| `git clone <url>` | Download repository (sekali di awal) |
| `git pull` | Ambil update terbaru dari GitHub |
| `git add .` | Stage semua perubahan |
| `git commit -m "..."` | Simpan snapshot dengan pesan |
| `git push` | Upload ke GitHub |
| `git status` | Cek status file |
| `git log` | Lihat history commit |

---

## 🔥 Konsep Lanjutan

### Branching (Cabang)

**Branch** = Versi parallel dari project

Contoh skenario:
- `main` branch: versi stabil
- `feature-login` branch: lagi develop fitur login
- `bugfix-typo` branch: perbaiki typo

Keuntungan:
- Gak ganggu versi stabil
- Bisa eksperimen bebas
- Bisa kerja di fitur berbeda secara bersamaan

### Pull Request (PR)

**Pull Request** = Minta code kamu di-review sebelum digabung

Workflow:
1. Bikin branch baru
2. Commit changes di branch itu
3. Push ke GitHub
4. Buat Pull Request
5. Teman/dosen review code kamu
6. Kasih comment/saran
7. Kalau sudah OK, di-merge ke branch utama

### Conflict (Konflik)

**Conflict** = Dua orang ngubah bagian yang sama

Contoh:
- Levi ngubah baris 10: "Halo"
- Rahmat ngubah baris 10: "Hello"
- Git bingung: mau pakai yang mana? 🤔

**Cara resolve:**
1. Git akan tandai bagian yang konflik
2. Pilih mau pakai punya siapa (atau gabung keduanya)
3. Commit hasil resolve-nya

**Konflik itu NORMAL!** Terjadi sehari-hari di dunia kerja.

### Git Blame

**Git Blame** = Tahu siapa yang nulis setiap baris kode

Kegunaan:
- "Eh, siapa yang nulis kode ini?"
- "Kok ada bug di sini, siapa yang bikin?"
- Bukan buat nyalahin, tapi buat tanya/diskusi! 😊

### Git Bisect

**Git Bisect** = Cari kapan bug mulai muncul (binary search!)

Skenario:
- Ada 1000 commit
- Tahu commit #1 gak ada bug
- Tahu commit #1000 ada bug
- Gak tahu kapan mulainya

Bisect pakai binary search:
- Cek commit #500 → ada bug
- Cek commit #250 → gak ada bug
- Cek commit #375 → ada bug
- Dan seterusnya...

Cuma butuh **log₂(1000) ≈ 10 kali** cek! 🚀

---

## 💡 Tips & Best Practices

### Do's ✅

1. **Commit sering!** Jangan nunggu selesai semua
2. **Commit message jelas** - tulis apa yang diubah
3. **Pull sebelum push** - hindari konflik
4. **Jangan commit file besar** (video, dataset raksasa)
5. **Gunakan .gitignore** untuk file yang gak perlu di-track
6. **Review sebelum commit** - pastikan gak ada yang salah

### Don'ts ❌

1. ❌ Commit password/API key
2. ❌ Commit file temporary (`.pyc`, `__pycache__`)
3. ❌ Commit message gak jelas ("update", "fix", "asdf")
4. ❌ Langsung push tanpa test
5. ❌ Edit commit history yang sudah di-push (kecuali terpaksa)

### File .gitignore

Contoh `.gitignore` untuk Python:

```gitignore
# Python
*.pyc
__pycache__/
venv/
.env

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db

# Data
*.csv
data/
```

---

## 🎓 Latihan

### Latihan 1: Setup Git

1. Install Git di komputer
2. Buat akun GitHub
3. Setup nama dan email:
   ```bash
   git config --global user.name "Nama Kamu"
   git config --global user.email "email@example.com"
   ```

### Latihan 2: Bikin Repository

1. Buat folder baru untuk latihan
2. Initialize Git repository
3. Bikin file `README.md`
4. Commit dengan message "Initial commit"

### Latihan 3: Push ke GitHub

1. Buat repository di GitHub
2. Connect local repository ke GitHub
3. Push commit pertama
4. Cek di GitHub, apakah file sudah muncul?

### Latihan 4: Kolaborasi

1. Ajak teman untuk collaborate
2. Teman clone repository kamu
3. Teman buat perubahan dan push
4. Kamu pull perubahan teman
5. Coba buat konflik (ubah file yang sama)
6. Resolve konflik bersama

---

## 📚 Resources

### Extension VS Code

1. **Git Lens** - Lihat history dan blame dengan mudah
2. **Git Graph** - Visualisasi branch dan commit
3. **GitHub Pull Requests** - Review PR di VS Code

### Learning Resources

- [GitHub Learning Lab](https://lab.github.com/)
- [Git Documentation](https://git-scm.com/doc)
- [Oh My Git!](https://ohmygit.org/) - Game buat belajar Git!
- [Visualizing Git](https://git-school.github.io/visualizing-git/) - Visualisasi interaktif

### Cheat Sheet

```bash
# Setup
git init                    # Inisialisasi repository
git clone <url>             # Clone dari GitHub

# Basic
git status                  # Cek status
git add <file>              # Stage file
git add .                   # Stage semua
git commit -m "message"     # Commit
git push                    # Upload ke GitHub
git pull                    # Download dari GitHub

# History
git log                     # Lihat history
git log --oneline           # History ringkas
git diff                    # Lihat perubahan

# Branch
git branch                  # Lihat branch
git branch <name>           # Bikin branch baru
git checkout <name>         # Pindah branch
git merge <name>            # Gabung branch

# Undo
git restore <file>          # Buang perubahan (belum commit)
git reset HEAD~1            # Undo commit terakhir
git revert <commit>         # Bikin commit baru yang undo
```

---

## 🎯 Kesimpulan

**Git itu WAJIB untuk:**
- Software Engineer
- Data Scientist
- Bikin skripsi/thesis
- Kerja tim
- Backup code

**Key Takeaways:**
1. Git = Time machine untuk code 🕐
2. Commit sering, commit dengan message jelas
3. GitHub = Tempat backup + kolaborasi
4. Conflict itu normal, jangan takut!
5. Practice makes perfect - pakai Git untuk semua project!

**Next Steps:**
1. Pakai Git untuk semua project mulai sekarang
2. Bikin GitHub profile yang keren
3. Kontribusi ke open source
4. Latihan resolve conflict

---

**Happy Coding! 🚀**

*"The best time to start using Git was yesterday. The second best time is now."*
