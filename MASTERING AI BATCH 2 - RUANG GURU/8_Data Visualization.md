# Data Visualization 📊

## Kenapa Perlu Data Visualization?

Data mentah dalam bentuk angka dan tabel itu susah dipahami. Makanya kita butuh visualisasi buat:

- **Simplify Complex Data** - Gampang nangkep pola dari gambar daripada tabel Excel
- **Faster Decision Making** - Lihat grafik langsung paham tren, lebih cepat bikin keputusan
- **Identifying Pattern & Relationship** - Bisa deteksi pola naik/turun, hubungan antar kolom
- **Data Storytelling** - Presentasi lebih mudah dicerna kalau pakai gambar

> Otak manusia lebih mudah mencerna informasi dalam bentuk visual!

---

## Library untuk Visualisasi

### 1. Matplotlib
Library paling basic untuk bikin grafik. Cara install:
```python
pip install matplotlib
```

Import biasanya:
```python
import matplotlib.pyplot as plt
```

### 2. Seaborn
Library yang lebih cantik tampilannya, biasanya dipakai bareng Matplotlib.
```python
import seaborn as sns
```

### 3. Plotly
Library untuk visualisasi interaktif (bisa hover, zoom, download).
```python
import plotly.express as px
```

> **Tips**: Ketiga library ini saling melengkapi, gak ada yang lebih baik. Biasanya dipakai bersamaan!

---

## Basic Plotting dengan Matplotlib

### Line Plot
Cocok untuk:
- Data kategorik (sumbu X) vs numerik (sumbu Y)
- **Time series data** (ada unsur waktu seperti tahun, jam)

```python
# Contoh line plot
plt.plot(x, y)
plt.xlabel('Construction Year')
plt.ylabel('Price')
plt.title('Price vs Year')
plt.show()
```

**Tips penting:**
- Gunakan `plt.xlabel()` dan `plt.ylabel()` untuk label sumbu
- Gunakan `plt.title()` untuk judul
- Chart yang bagus HARUS punya: X Label, Y Label, dan Title!

**Cara hapus output log:**
- Tambahin titik koma di akhir: `plt.plot(x, y);`
- Atau gunakan `plt.show()`

---

### Scatter Plot (Diagram Sebar)
Cocok untuk:
- Sumbu X: data numerik continuous
- Sumbu Y: data numerik continuous
- Melihat **korelasi** antara dua variabel

```python
plt.scatter(x, y)
plt.xlabel('Number of Reviews')
plt.ylabel('Price')
plt.show()
```

**Korelasi:**
- Nilai korelasi: -1 sampai +1
- **Positif (mendekati +1)**: Semakin tinggi X, semakin tinggi Y
- **Negatif (mendekati -1)**: Semakin tinggi X, semakin rendah Y
- **0**: Tidak ada hubungan linear

---

### Bar Chart
Cocok untuk:
- Sumbu X: data kategorik
- Sumbu Y: data numerik continuous

```python
plt.bar(categories, values)
plt.xlabel('Room Type')
plt.ylabel('Average Price')
plt.show()
```

> **Warning**: Jangan gunakan kategori yang terlalu banyak (misalnya 100+ kategori). Grafiknya jadi berantakan!

**Solusi kalau kategori terlalu banyak:**
- Ambil top 5-10 kategori, sisanya masukkan ke "Others"
- Atau gunakan grouping yang lebih sederhana

---

### Histogram
Untuk melihat distribusi data numerik.

```python
plt.hist(data, bins='auto')
plt.xlabel('Price')
plt.ylabel('Frequency')
plt.show()
```

**Bins**: Jumlah kotak/interval
- `bins='auto'` → sistem otomatis tentukan
- `bins=10` → 10 kotak
- `bins=100` → 100 kotak (terlalu detail, kurang optimal)

---

### Pie Chart
Untuk melihat **proporsi** dari data kategorik.

```python
plt.pie(values, labels=categories)
plt.title('Room Type Distribution')
plt.show()
```

> **Warning**: Jangan pakai pie chart kalau kategorinya terlalu banyak (>5)! Malah jadi susah dibaca.

---

## Integrasi dengan Pandas

Pandas sudah terintegrasi dengan Matplotlib, jadi bisa langsung plot dari DataFrame!

```python
# Langsung plot dari Pandas
df['price'].plot(kind='hist')
df['price'].plot(kind='line')
```

---

## Customization

### 1. Warna (Colors)
```python
# Common color names
plt.plot(x, y, color='green')

# Hex color
plt.plot(x, y, color='#FF5733')

# RGB tuples (0-1)
plt.plot(x, y, color=(0.1, 0.2, 0.5))
```

### 2. Marker
Penanda untuk setiap data point.

```python
plt.plot(x, y, marker='o')   # Lingkaran
plt.plot(x, y, marker='s')   # Square/kotak
plt.plot(x, y, marker='D')   # Diamond
plt.plot(x, y, marker='+')   # Plus
plt.plot(x, y, marker='x')   # X
```

### 3. Line Style
```python
plt.plot(x, y, linestyle='-')     # Solid (default)
plt.plot(x, y, linestyle='--')    # Dashed
plt.plot(x, y, linestyle=':')     # Dotted
plt.plot(x, y, linestyle='-.')    # Dash-dot
```

### 4. Line Width
```python
plt.plot(x, y, linewidth=2)
```

### 5. Kombinasi
```python
plt.plot(x, y, color='green', marker='o', linestyle='--')
```

---

### 6. Ukuran Gambar
```python
plt.figure(figsize=(10, 5))  # (panjang, lebar)
plt.plot(x, y)
plt.show()
```

### 7. Legend (Keterangan)
Penting kalau plot lebih dari 1 line!

```python
plt.plot(x, y1, label='Average Price')
plt.plot(x, y2, label='Median Price')
plt.plot(x, y3, label='Max Price')
plt.legend()  # Tampilkan legend
plt.show()
```

### 8. Horizontal Line (Batas)
```python
# Batas atas
plt.hlines(y=600, xmin=2002, xmax=2023, color='red', label='Upper Limit')

# Batas bawah
plt.hlines(y=100, xmin=2002, xmax=2023, color='orange', label='Lower Limit')

# Target
plt.hlines(y=300, xmin=2002, xmax=2023, color='green', label='Target')
plt.legend()
```

---

### 9. Merapikan Sumbu (MaxNLocator)
Supaya sumbu X tidak ada koma-koma atau pecahan.

```python
from matplotlib.ticker import MaxNLocator

plt.plot(x, y)
plt.gca().xaxis.set_major_locator(MaxNLocator(integer=True))
plt.show()
```

---

### 10. Menyimpan Gambar
```python
plt.savefig('output/plot.jpg', dpi=300, bbox_inches='tight')
```

Parameter:
- `dpi`: Semakin tinggi, semakin clear (default 100)
- `bbox_inches='tight'`: Supaya tidak ada whitespace berlebih

---

## Seaborn - Lebih Cantik!

### Line Plot dengan Seaborn
```python
import seaborn as sns

sns.lineplot(data=df, x='construction_year', y='price')
plt.title('Price vs Year')
plt.show()
```

**Kelebihan Seaborn:**
- Otomatis ada label sumbu X dan Y
- Lebih cantik default-nya
- Bisa dikombinasi dengan Matplotlib

```python
# Kombinasi Seaborn + Matplotlib
sns.lineplot(data=df, x='year', y='price', color='green', marker='o', linestyle='--')
plt.title('Price Trend')
plt.show()
```

---

## Multivariate Visualization

### 1. Pair Plot
Untuk visualisasi banyak kolom sekaligus.

```python
sns.pairplot(df[['price', 'minimum_nights', 'number_of_reviews']])
plt.show()
```

> **Warning**: Jangan masukkan terlalu banyak kolom! Kalau 4 kolom = 16 plot (4x4), makin banyak makin bingung.

**Cara baca:**
- Diagonal: Distribusi masing-masing kolom
- Off-diagonal: Scatter plot antar kolom

---

### 2. Heatmap & Correlation Matrix

**Korelasi** = mengukur hubungan linear antara dua variabel (-1 sampai +1)

```python
# Hitung korelasi
correlation = df.corr()

# Visualisasi dengan heatmap
sns.heatmap(correlation, annot=True, cmap='coolwarm')
plt.title('Correlation Matrix')
plt.show()
```

**Interpretasi warna:**
- **Merah/terang**: Korelasi positif kuat (mendekati +1)
- **Biru/gelap**: Korelasi negatif kuat (mendekati -1)
- **Putih**: Tidak ada korelasi (mendekati 0)

**Contoh:**
- Price vs Service Fee = 0.99 → Korelasi sangat kuat positif
- Number of Reviews vs Minimum Nights = -0.25 → Korelasi negatif lemah

---

### 3. Box Plot
Untuk melihat:
- Distribusi data
- **Outliers** (data yang abnormal)
- Median, quartile

```python
plt.boxplot(data)
plt.show()

# Dengan kategori
sns.boxplot(data=df, x='neighbourhood_group', y='price')
plt.show()
```

**Komponen Box Plot:**
- **Garis tengah**: Median
- **Kotak**: Q1 sampai Q3 (50% data tengah)
- **Whisker**: Batas atas/bawah normal
- **Titik di luar whisker**: Outliers!

---

### 4. Count Plot
Menghitung jumlah data per kategori.

```python
sns.countplot(data=df, x='room_type')
plt.show()
```

**Bedanya Count Plot vs Bar Chart:**
- **Count Plot**: Otomatis hitung jumlah data
- **Bar Chart**: Bisa plot rata-rata, median, dll (tidak hanya count)

---

### 5. Distribution Plot (Displot)
Untuk melihat distribusi data numerik lebih detail.

```python
# Histogram biasa
sns.displot(df['price'])

# Dengan KDE (Kernel Density Estimation) - smoothing
sns.displot(df['price'], kde=True)

# ECDF (Empirical Cumulative Distribution Function)
sns.displot(df['price'], kind='ecdf')
```

---

## Advanced Plots

### 1. Subplot (Banyak Plot dalam 1 Gambar)
```python
plt.subplot(2, 2, 1)  # (baris, kolom, posisi)
plt.pie(values1, labels=labels1)
plt.title('Room Type')

plt.subplot(2, 2, 2)
plt.pie(values2, labels=labels2)
plt.title('Neighbourhood')

plt.tight_layout()  # Supaya tidak bertabrakan
plt.show()
```

---

### 2. Funnel Chart (Plotly)
Untuk visualisasi funnel (seperti customer journey).

```python
import plotly.express as px

fig = px.funnel(df, x='stage', y='value')
fig.show()
```

**Kelebihan Plotly:**
- Interaktif (bisa hover, zoom, download PNG)
- Keren untuk presentasi

**Kekurangan:**
- Lambat kalau data besar
- Butuh waktu loading lebih lama

---

### 3. 3D Plot (Jarang Dipakai)
```python
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.scatter(x, y, z)
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
plt.show()
```

> **Note**: 3D plot jarang dipakai karena susah dijelaskan dan kompleks interpretasinya.

---

### 4. Violin Plot
Gabungan box plot + distribution plot.

```python
sns.violinplot(data=df, x='room_type', y='price')
plt.show()
```

---

### 5. Swarm Plot
Gabungan scatter + distribution.

```python
sns.swarmplot(data=df, x='room_type', y='price')
plt.show()
```

---

## Kombinasi Bar Chart + Line Chart

```python
# Bar chart
plt.bar(x, y1, label='Count')

# Line chart di atas bar chart
plt.plot(x, y2, color='red', marker='o', label='Trend')

plt.legend()
plt.show()
```

> **Syarat**: Sumbu X harus sama untuk kedua chart!

---

## Tips & Tricks

### 1. Hapus Output Log
```python
plt.plot(x, y);  # Tambahin titik koma
```

### 2. Data Kategorik

**Nominal**: Tidak ada urutan (Jakarta, Bandung, Jogja, nama produk)
**Ordinal**: Ada urutan intrinsik (Tinggi, Sedang, Rendah)

### 3. Data Numerik

**Diskrit**: Tidak bisa pecahan (jumlah orang, jumlah produk)
**Continuous**: Bisa pecahan (harga, berat, tinggi, waktu)

### 4. Chart yang Bagus Harus Punya:
✅ X Label
✅ Y Label  
✅ Title
✅ Legend (kalau lebih dari 1 line/bar)

### 5. Jangan Gunakan:
❌ Kategori terlalu banyak di pie chart atau bar chart
❌ 3D plot untuk hal simpel (overkill)
❌ Plot kompleks untuk audience non-technical

---

## Pandas GroupBy untuk Agregasi

```python
# Grouping satu kolom
df.groupby('neighbourhood_group')['price'].mean()

# Grouping dengan banyak agregasi
df.groupby('neighbourhood_group').agg({
    'price': 'mean',
    'service_fee': 'median'
})
```

---

## Dashboard Tools (Alternatif Python)

Kalau males ngoding, bisa pakai dashboard tools:

1. **Looker Studio** (Google) - Gratis!
2. **Power BI** (Microsoft) - Berbayar, ada versi free terbatas
3. **Tableau** - Berbayar, ada versi free terbatas

---

## Rekomendasi Buku

📚 **"Storytelling with Data"** - Cole Nussbaumer Knaflic
📚 **"Fundamentals of Data Visualization"** - Claus O. Wilke

Kedua buku ini bagus banget untuk belajar data visualization lebih dalam!

---

## Plot yang Sering Dipakai (Prioritas)

1. **Line Plot** - Time series, trend
2. **Bar Chart** - Perbandingan kategori
3. **Scatter Plot** - Melihat korelasi
4. **Box Plot** - Distribusi + outliers
5. **Pie Chart** - Proporsi (max 5 kategori)
6. **Count Plot** - Hitung jumlah per kategori
7. **Heatmap** - Correlation matrix
8. **Distribution Plot** - Distribusi data numerik

Yang lain (violin, swarm, funnel, 3D) → sesuai kebutuhan aja!

---

## Summary

Data visualization itu **wajib** di setiap tahap project data science:
- EDA (Exploratory Data Analysis)
- Feature Engineering
- Model Evaluation
- Presentation ke stakeholder

Gunakan plot yang **simpel** dan **mudah dipahami**. Jangan terlalu kompleks kalau audiencenya non-technical!

**Key libraries:**
- Matplotlib → Basic, flexible
- Seaborn → Cantik, statistik
- Plotly → Interaktif, presentasi

Happy visualizing! 🎨📊
