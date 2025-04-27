# Analisis-Data-Erupsi-Gunung-Berapi-Global
Proyek ini sebagai pelatihan untuk menganalisis data sebaran erupsi gunung api dunia dalam jangka waktu tahun 2010-2024.

## Tujuan Analisis
- Mengidentifikasi negara dengan jumlah erupsi terbanyak.
- Menganalisis penyebaran lokasi erupsi gunung berapi di dunia.
- Menampilkan visualisasi interaktif lokasi erupsi menggunakan peta.
- Menampilkan distribusi VEI.
- Menampilkan jumlah erupsi berdasarkan tipe gunung api.

## Dataset
Sumber [NOAA](https://www.ngdc.noaa.gov/hazel/view/hazards/volcano/event-data?maxYear=2025&minYear=2010)

## Tools yang digunakan
- Python (Pandas, Matplotlib, Seaborn)
- Folium (untuk peta interaktif)
- Visual Studio Code
- Dataset: CSV 'volcano_eruption'

## Insight & Hasil Visualisasi
### 1. Jumlah Erupsi per Tahun
Grafik yang menunjukkan tahun-tahun dengan jumlah erupsi terbanyak.
```python
plt.figure(figsize=(12,6))
sns.countplot(data=df, x='Year', order=sorted(df['Year'].unique()))
plt.xticks(rotation=45)
plt.title("Jumlah Erupsi Gunung per Tahun")
plt.tight_layout()
plt.show()
```
![Jumlah Erupsi per Tahun](https://github.com/user-attachments/assets/887e110c-fdb2-49b1-bb35-ffee25c5255b)

### 2. Negara dengan Erupsi Terbanyak
Grafik TOP 10 negara dengan jumlah erupsing paling sering terjadi.
```python
plt.figure(figsize=(12,6))
top_countries = df['Country'].value_counts().head(10)
sns.barplot(x=top_countries.index, y=top_countries.values)
plt.title("Top 10 Negara dengan Erupsi Terbanyak")
plt.ylabel("Jumlah Erupsi")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
![Top 10 Negara dengan Erupsi Terbanyak](https://github.com/user-attachments/assets/df1e3431-2f08-421c-9385-e237d61990f5)

### 3. Sebaran Lokasi Erupsi
Visualisasi scatter berdasarkan koordinat (Latitude & Longitude) dan VEI.
```python
df_map = df.dropna(subset=['Latitude', 'Longitude', 'VEI'])
plt.figure(figsize=(10,6))
sns.scatterplot(data=df_map, x='Longitude', y='Latitude', hue='VEI', palette='viridis', size='VEI')
plt.title("Sebaran Lokasi Erupsi Gunung")
plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.legend(bbox_to_anchor=(1.05,1), loc='upper left')
plt.tight_layout()
plt.show()
```
Dibuat dengan folium, menampilkan lokasi erupsi.
![Sebaran Lokasi Erupsi Gunung 2](https://github.com/user-attachments/assets/c4d2b12c-50f6-4d7a-8021-508b5b63cb36)

Silakan buka 'peta_erupsi.htm' di browser. [PETA](file:///C:/Users/User-PC/Downloads/Dataset/peta_erupsi.htm)
![Peta erupsi gunung tampilan webbrowser](https://github.com/user-attachments/assets/f92a772d-e242-41f3-8c66-aaefad6c3ae6)

### 4. Distribusi VEI (Volcanic Explosivity Index)
Grafik distribusi VEI
```python
plt.figure(figsize=(8,5))
sns.histplot(df['VEI'].dropna(), bins=8, kde=True)
plt.title("Distribusi VEI (Volcanic Explosivity Index)")
plt.xlabel("VEI")
plt.ylabel("Frekuensi")
plt.tight_layout()
plt.show()
```
![Distribusi VEI](https://github.com/user-attachments/assets/e70d0fc1-0019-4492-876f-d990ad4652f5)

### 5. Erupsi berdasarkan Tipe Gunung Berapi
Jumlah kejadian erupsi berdasarkan tipe gunung.
```python
# Bersihkan data
df_map = df.dropna(subset=['Latitude', 'Longitude', 'VEI'])

# Peta dasar
peta = folium.Map(location=[0, 120], zoom_start=2)

# Market setiap gunung
for index, row in df_map.iterrows():
    folium.CircleMarker(
        location=[row['Latitude'], row['Longitude']],
        radius=5,
        popup=row['Volcano_Name'],
        color='red',
        fill=True,
        fill_color='red'
    ).add_to(peta)
    
import webbrowser

# Menampilkan peta
peta.save('peta_erupsi.htm')

# Buka otomatis di browser
webbrowser.open('peta_erupsi.htm')

# --- Visualisasi 3: Sebaran Lokasi Erupsi Gunung ---
df_map = df.dropna(subset=['Latitude', 'Longitude', 'VEI'])
plt.figure(figsize=(10,6))
sns.scatterplot(data=df_map, x='Longitude', y='Latitude', hue='VEI', palette='viridis', size='VEI')
plt.title("Sebaran Lokasi Erupsi Gunung")
plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.legend(bbox_to_anchor=(1.05,1), loc='upper left')
plt.tight_layout()
plt.show()
```

### Catatan
- Data mengandung missing value.
- Beberapa kolom memiliki data tidak lengkap
  
---

> Dibuat sebagai latihan menganalisis data bidang geologi.
