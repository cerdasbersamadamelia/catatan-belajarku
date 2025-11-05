# Pandas - Python Data Analysis Library

## Apa itu Pandas?
**Pandas** = **P**ython for **D**ata **A**nalysis

- Library Python untuk analisis dan manipulasi data
- Salah satu library paling penting untuk Data Science & AI Engineering
- Dibangun dengan NumPy (Numerical Python) - keduanya selalu dipakai bareng!
- Dibuat tahun 2008, open source sejak 2009

### Kenapa Pakai Pandas?
✅ Manipulasi data cepat dan efisien  
✅ Handle berbagai format data (CSV, Excel, SQL, JSON, dll)  
✅ Terintegrasi dengan library ML dan visualisasi  
✅ Perfect untuk eksplorasi data sebelum modeling  

---

## Instalasi Pandas

```python
# Di Jupyter Notebook / Google Colab
!pip install pandas

# Di Terminal/Anaconda
pip install pandas
# atau
conda install pandas
```

**Good news:** Google Colab sudah include Pandas by default!

```python
import pandas as pd
import numpy as np
```

---

## Data Structure di Pandas

Pandas punya 2 struktur data utama:

### 1. Series (1 Dimensi)
- Hanya ada **baris** (row)
- Mirip array/list tapi lebih powerful

### 2. DataFrame (2 Dimensi)
- Ada **baris** DAN **kolom**
- Mirip tabel Excel/spreadsheet
- Paling sering dipakai!

---

## Membuat DataFrame

### Dari Dictionary
```python
data = {
    'name': ['John', 'Anna', 'Peter'],
    'age': [25, 23, 28],
    'house': [10, 11, 12]  # jarak rumah dari sekolah (km)
}

df = pd.DataFrame(data)
```

**Output:**
```
    name  age  house
0   John   25     10
1   Anna   23     11
2  Peter   28     12
```

💡 **Key → Kolom**, **Value → Baris**

### Menambah Kolom Baru
```python
df['score'] = [85, 90, 88]
```

⚠️ **Penting:** Jumlah data baru harus sama dengan jumlah baris!

---

## Series

### Membuat Series

#### 1. Dari Dictionary
```python
s = pd.Series({'a': 1, 'b': 2, 'c': 3})
# Key jadi index, Value jadi nilai
```

#### 2. Dari List
```python
s = pd.Series([1, 2, 3, 4])
# Index otomatis: 0, 1, 2, 3
```

#### 3. Dari Skalar
```python
s = pd.Series(5, index=[0, 1, 2, 3])
# Semua nilai = 5
```

### Mengambil Kolom = Series
```python
names = df['name']  # Ini jadi Series!
type(names)  # pandas.core.series.Series
```

---

## Indexing & Slicing

### Custom Index
```python
df = pd.DataFrame(data, index=['A', 'B', 'C'])
```

**Output:**
```
   name  age
A  John   25
B  Anna   23
C Peter   28
```

### Reset Index
```python
# Dengan kolom index lama
df.reset_index()

# Hapus kolom index lama
df.reset_index(drop=True)
```

---

## Indexing Methods

### 1. `.loc` - Label Based
Pakai **nama** index/kolom

```python
# Single row
df.loc['A']

# Multiple rows
df.loc['A':'C']  # Include C!

# Row + kolom spesifik
df.loc['A', 'name']
df.loc['A':'B', 'age']

# Semua row, kolom tertentu
df.loc[:, 'name']
```

### 2. `.iloc` - Integer Based
Pakai **angka** index

```python
# First row
df.iloc[0]

# First 2 rows (exclude index 2!)
df.iloc[0:2]

# Semua row, 2 kolom pertama
df.iloc[:, 0:2]

# Row & kolom spesifik
df.iloc[0:2, 0:2]
```

**Perbedaan Penting:**
- `.loc['A':'C']` → Include 'C' ✅
- `.iloc[0:2]` → Exclude index 2 ❌

---

## Filtering Data

### Filter Sederhana
```python
# Nama = John
df[df['name'] == 'John']

# Umur > 24
df[df['age'] > 24]
```

### Multiple Conditions
```python
# Umur > 25 DAN country = UK
df[(df['age'] > 25) & (df['country'] == 'UK')]

# Umur > 25 ATAU country = UK
df[(df['age'] > 25) | (df['country'] == 'UK')]
```

⚠️ **Wajib pakai `&` (and) dan `|` (or)**, bukan `and` / `or`!

### Negasi Filter
```python
# Kebalikan filter
df_new = df[~(df['age'] > 25)]
```

---

## Read & Write Data

### Read CSV
```python
df = pd.read_csv('file.csv')
df = pd.read_csv('https://url-to-data.csv')  # Bisa dari URL!
```

**Delimiter custom:**
```python
df = pd.read_csv('file.txt', delimiter=';')
```

### Write CSV
```python
df.to_csv('output.csv', index=False)
```

💡 `index=False` → Jangan simpan kolom index 0,1,2...

### Format Lain
Pandas support banyak format:
- Excel: `pd.read_excel()`, `df.to_excel()`
- JSON: `pd.read_json()`, `df.to_json()`
- SQL: `pd.read_sql()`
- HTML, XML, Google BigQuery, dll!

---

## Eksplorasi Data Cepat

```python
# 5 data teratas
df.head()
df.head(10)  # 10 teratas

# 5 data terbawah
df.tail()

# Ukuran data (rows, columns)
df.shape  # (20, 5)

# Info lengkap
df.info()
# - Jumlah data
# - Tipe data tiap kolom
# - Non-null count
# - Memory usage

# Statistik deskriptif
df.describe()
# - count, mean, std
# - min, 25%, 50%, 75%, max
```

---

## Data Manipulation

### Menambah Kolom

#### 1. Nilai Skalar (Sama Semua)
```python
df['discount'] = 0.15
```

#### 2. Dari Series
```python
new_values = pd.Series([100, 200, 300, 400, 500])
df['bonus'] = new_values
# Sisanya jadi NaN kalau jumlah beda!
```

#### 3. Hasil Perhitungan
```python
df['total_price'] = df['price'] - (df['price'] * df['discount'])
```

### Update Kolom
```python
# Ganti nilai
df['discount'] = 0.25

# Update dengan calculation
df['total_price'] = df['price'] - (df['price'] * 0.25)
```

### Mapping (Apply Nilai Berbeda per Kategori)
```python
# Buat mapping dictionary
discount_map = {
    'First': 0.15,
    'Second': 0.20,
    'Third': 0.30
}

# Apply mapping
df['discount'] = df['class'].map(discount_map)
```

### Rename Kolom
```python
# Single
df.rename(columns={'sex': 'gender'}, inplace=True)

# Multiple
df.rename(columns={
    'sex': 'gender',
    'passenger_id': 'id'
}, inplace=True)
```

💡 `inplace=True` → Langsung ubah, gak perlu simpan ke variabel baru

---

## Operasi Baris (Row)

### Menambah Row Baru

#### Cara 1: Loc (Harus sesuai jumlah kolom!)
```python
new_row = [21, 'Tom', 30, 'USA']
df.loc[20] = new_row  # Index 20
```

#### Cara 2: Concat (Recommended!)
```python
new_df = pd.DataFrame([new_row], columns=df.columns)
df = pd.concat([df, new_df], ignore_index=True)
```

### Update Row
```python
df.loc[0, ['passenger_id', 'cabin']] = [24, 'C1']
```

### Hapus Row
```python
# Hapus index 2, 3, 4
df.drop([2, 3, 4], inplace=True)
df.reset_index(drop=True, inplace=True)
```

### Hapus Kolom
```python
df.drop(columns=['class', 'gender'], inplace=True)
```

---

## Shift (Nilai Sebelum/Sesudah)

```python
# Ambil nilai baris sebelumnya
df['prev_value'] = df['value'].shift(1)

# Ambil nilai baris sesudahnya
df['next_value'] = df['value'].shift(-1)
```

---

## Sorting

```python
# Sort 1 kolom
df.sort_values(by='age', ascending=True).reset_index(drop=True)

# Sort multiple kolom
df.sort_values(
    by=['class', 'cabin'],
    ascending=[True, False]  # Class naik, Cabin turun
).reset_index(drop=True)
```

---

## Agregasi Functions

### Basic Stats
```python
df['age'].mean()     # Rata-rata
df['age'].median()   # Median
df['age'].min()      # Minimum
df['age'].max()      # Maximum
df['age'].std()      # Standard Deviasi
df['age'].count()    # Jumlah data non-null
df['age'].sum()      # Total
```

### Group By + Aggregation
```python
# Rata-rata umur per kelas
df.groupby('class')['age'].mean()

# Multiple aggregations
df.groupby('class').agg({
    'age': 'mean',
    'fare': ['min', 'max', 'mean']
})
```

---

## Apply Functions

### Lambda Function
```python
# Tambah 5 ke setiap passenger_id
df['passenger_id'].apply(lambda x: x + 5)
```

### Custom Function
```python
def calculate_total(price, discount):
    return price - (price * discount)

# Apply ke DataFrame (axis=1 = per row)
df['total'] = df.apply(
    lambda row: calculate_total(row['price'], row['discount']),
    axis=1
)
```

**Axis:**
- `axis=0` → Per kolom
- `axis=1` → Per baris

---

## Data Cleaning 🧹

### 1. Handling Missing Values

#### Deteksi Missing
```python
# Cek mana yang null (True/False)
df.isnull()

# Hitung jumlah null per kolom
df.isnull().sum()

# Persentase missing
missing_pct = (df.isnull().sum() / len(df)) * 100
```

#### Drop Missing Values
```python
# Drop kolom dengan banyak missing
df.drop(columns=['license'], inplace=True)

# Drop baris yang ada missing di kolom tertentu
df.dropna(subset=['id', 'name', 'host_id'], inplace=True)

# Drop SEMUA baris dengan missing value
df.dropna(inplace=True)
```

**Rule of Thumb:**
- Missing > 20% → **Drop kolom**
- Missing < 20% → **Fill/Impute**

#### Fill Missing Values
```python
# Fill dengan nilai tertentu
df['column'].fillna('Unknown', inplace=True)

# Fill dengan mean
df['age'].fillna(df['age'].mean(), inplace=True)

# Fill dengan median (lebih robust ke outliers!)
df['price'].fillna(df['price'].median(), inplace=True)

# Fill dengan modus (paling sering muncul)
df['category'].fillna(df['category'].mode()[0], inplace=True)
```

**Kapan pakai apa?**
- **Mean**: Data numerik tanpa outliers
- **Median**: Data numerik dengan outliers (recommended!)
- **Mode**: Data kategorikal
- **Unknown/0**: Case-by-case

---

### 2. Remove Duplicates

```python
# Cek duplikat
df.duplicated().sum()

# Lihat data duplikat
df[df.duplicated()]

# Hapus duplikat
df.drop_duplicates(inplace=True)
df.reset_index(drop=True, inplace=True)
```

---

### 3. Convert Data Type

```python
# Cek tipe data
df.dtypes

# Convert float ke integer
df['id'] = df['id'].fillna(0).astype(int)

# Convert ke string
df['code'] = df['code'].astype(str)
```

⚠️ **Hati-hati:** Kolom dengan NaN otomatis jadi float!  
**Solusi:** Fill NaN dulu, baru convert!

---

### 4. Outlier Detection

#### Visualisasi dengan Boxplot
```python
import seaborn as sns

sns.boxplot(x=df['number_of_reviews'])
```

Data di luar "whisker" = Outlier!

#### Hitung dengan IQR (Interquartile Range)
```python
# Step 1: Hitung Q1 & Q3
Q1 = df['column'].quantile(0.25)
Q3 = df['column'].quantile(0.75)

# Step 2: IQR
IQR = Q3 - Q1

# Step 3: Batas bawah & atas
lower_bound = Q1 - (1.5 * IQR)
upper_bound = Q3 + (1.5 * IQR)

# Step 4: Filter outliers
outliers = df[(df['column'] < lower_bound) | (df['column'] > upper_bound)]
```

#### Handling Outliers

**Pilihan:**
1. **Biarkan** → Tetap ada info, coba eksperimen with/without
2. **Hapus** → Kehilangan data/informasi
3. **Capping** → Ganti dengan min/max (kurang recommended)
4. **Advanced**: Pakai Isolation Forest (ML-based)

**Tips:**
- Untuk ML: **Coba eksperimen!**
  - Model dengan outliers
  - Model tanpa outliers
  - Pilih yang performa terbaik
- Jangan langsung hapus → Outliers bisa jadi informasi penting!

---

## Set Index Custom

```python
# Jadikan kolom sebagai index
df.set_index('class', inplace=True)

# Akses berdasarkan index baru
df.loc['First']  # Semua data kelas First
```

---

## Pandas + NumPy = Best Friends

Pandas dibangun dengan NumPy, jadi keduanya:
- Sangat cepat (optimized)
- Terintegrasi sempurna
- Selalu import bareng!

```python
import pandas as pd
import numpy as np
```

---

## Tips & Best Practices

### 1. Jangan Langsung Overwrite!
```python
# ❌ JANGAN
df.dropna(inplace=True)

# ✅ LAKUKAN (untuk eksplorasi)
df_clean = df.dropna()
# Bisa compare df vs df_clean
```

### 2. Reset Index Setelah Drop/Sort
```python
df = df.drop([1, 3, 5])
df.reset_index(drop=True, inplace=True)
# Index jadi rapi: 0, 1, 2, ...
```

### 3. Chain Operations
```python
df_clean = (df
    .dropna()
    .drop_duplicates()
    .sort_values('age')
    .reset_index(drop=True)
)
```

### 4. Eksplorasi Dulu, Preprocessing Kemudian
Workflow:
1. Load data → `df.head()`, `df.info()`
2. Eksplorasi → Check missing, outliers, distribusi
3. Preprocessing → Clean, transform
4. Eksplorasi lagi → Verify hasilnya

---

## Common Issues & Solutions

### Issue: "KeyError: kolom tidak ada"
```python
# Cek dulu nama kolom
df.columns

# Case sensitive!
df['Name']  # ❌
df['name']  # ✅
```

### Issue: Index berantakan setelah filter
```python
# Selalu reset index
df = df[df['age'] > 25].reset_index(drop=True)
```

### Issue: Kolom jadi float karena NaN
```python
# Fill NaN dulu baru convert
df['id'] = df['id'].fillna(0).astype(int)
```

---

## Dokumentasi & Resources

📚 **Official Docs:** https://pandas.pydata.org/docs/

**Buku Recommended:**
- "Python for Data Analysis" by Wes McKinney (creator Pandas!)

**Cara Belajar:**
- Baca dokumentasi di pandas.pydata.org
- Lihat User Guide → Banyak contoh!
- Practice dengan dataset nyata

---

## Pandas untuk Machine Learning

Pandas adalah **fase eksplorasi & preprocessing**:

1. **Load data** → `pd.read_csv()`
2. **Explore** → `head()`, `info()`, `describe()`
3. **Clean** → Handle missing, duplicates, outliers
4. **Transform** → Feature engineering, encoding
5. **Export** → Siap untuk modeling!

**Flow:**
```
Data mentah → Pandas (clean) → Scikit-learn (model) → Hasil
```

---

## Cheat Sheet Commands

```python
# Load & Save
df = pd.read_csv('file.csv')
df.to_csv('output.csv', index=False)

# Quick View
df.head()           # 5 teratas
df.tail()           # 5 terbawah
df.shape            # (rows, cols)
df.info()           # Info lengkap
df.describe()       # Statistik

# Select
df['col']           # 1 kolom (Series)
df[['c1', 'c2']]    # Multiple kolom (DataFrame)
df.loc[idx]         # By label
df.iloc[0]          # By position

# Filter
df[df['age'] > 25]
df[(df['age'] > 25) & (df['city'] == 'NY')]

# Missing
df.isnull().sum()
df.dropna()
df.fillna(value)

# Add/Update
df['new'] = values
df['col'] = df['col'].map(mapping)

# Delete
df.drop(columns=['col'])
df.drop([0, 1, 2])  # rows

# Sort
df.sort_values('col')

# Group
df.groupby('col')['val'].mean()
```

---

## Next Step: Data Visualization! 📊

Setelah belajar Pandas, selanjutnya:
- **Matplotlib** → Plotting dasar
- **Seaborn** → Statistical visualization
- Visualisasi = Cara terbaik memahami data!

---

**Happy Learning! 🚀**

*Notes: Materi ini dari Webinar #7 - Pandas (29 December 2023)*