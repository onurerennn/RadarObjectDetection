# 🎯 RADAR VERİSİ ANALİZİ VE OBJECT DETECTION

Bu proje, otomotiv radar verilerini analiz ederek object detection ve tracking işlemleri gerçekleştirir. FMCW (Frequency Modulated Continuous Wave) radar sensörlerinden gelen dual-channel veri ile gelişmiş görselleştirme ve makine öğrenmesi algoritmaları kullanılır.

## 📋 Proje Özeti

### 🎯 Amaç
- Radar verilerinin kapsamlı analizi
- Object detection ve sınıflandırma
- Real-time tracking simülasyonu
- Gelişmiş görselleştirme teknikleri

### 🛡️ Radar Sistemi
- **Tip:** FMCW (77 GHz) Otomotiv Radarı
- **Kanallar:** Dual-channel (Right/Left)
- **Parametreler:** Range, Velocity, Angle
- **Uygulama:** Autonomous Vehicle/ADAS

## 📊 Veri Yapısı

### Veri Formatı (8 Sütun)
```
[Sensor] [Raw_Range] [Raw_Velocity] [Raw_Angle] [Timestamp] [Proc_Range] [Proc_Velocity] [Proc_Angle] [Status]
```

| Sütun | Açıklama | Birim | Örnek |
|-------|----------|-------|-------|
| 1 | Sensor Channel (R/L) | - | R |
| 2 | Ham Mesafe | metre | 8.46642 |
| 3 | Ham Hız | m/s | 0.0287602 |
| 4 | Ham Açı | radyan | -3.04035 |
| 5 | Zaman Damgası | mikrosaniye | 1477010443399637 |
| 6 | İşlenmiş Mesafe | metre | 8.6 |
| 7 | İşlenmiş Hız | m/s | 0.25 |
| 8 | İşlenmiş Açı | radyan | -3.00029 |
| 9 | Obje Sınıfı/Durum | - | 0 |

## 🚀 Kurulum

### Gereksinimler
```bash
Python >= 3.8
```

### Kütüphaneler
```bash
pip install pandas numpy matplotlib seaborn scikit-learn plotly scipy
```

### Dosya Yapısı
```
radar/
├── data-1.txt          # Radar verisi
├── radar.ipynb         # Ana analiz notebook'u
└── README.md           # Bu dosya
```

## 🔧 Kullanım

### 1. Notebook'u Başlatma
```bash
jupyter notebook radar.ipynb
```

### 2. Adım Adım Çalıştırma
1. **Kütüphane Kurulumu** - Gerekli importlar
2. **Veri Yükleme** - Radar verisini okuma
3. **Veri Temizleme** - Preprocessing işlemleri
4. **İstatistiksel Analiz** - Temel istatistikler
5. **Görselleştirme** - Çeşitli plot türleri
6. **Object Detection** - Clustering algoritmaları
7. **Real-time Simulation** - Tracking simülasyonu

## 📊 Analiz Özellikleri

### 🔍 Veri Analizi
- **Dual-channel** radar verisi işleme
- **Zaman serisi** analizi
- **İstatistiksel** özellik çıkarımı
- **Kategori** dağılımları (hız, mesafe)

### 🎨 Görselleştirmeler
- **Histogram** dağılımları
- **Polar koordinat** görünümleri
- **Range-Doppler** haritaları
- **3D scatter** plotlar
- **Trajectory** analizi
- **Zaman serisi** grafikleri

### 🤖 Object Detection
- **DBSCAN Clustering** algoritması
- **Özellik normalizasyonu** (StandardScaler)
- **Obje sınıflandırması:**
  - 🏢 Statik Objeler
  - 🚗 Yaklaşan Araçlar
  - 🚙 Uzaklaşan Araçlar
  - 🚶 Yavaş Objeler

### 🚀 Real-Time Detection
- **Sliding window** yöntemi
- **Güven skoru** hesaplama
- **Detection timeline** görselleştirme
- **Eşik tabanlı** filtreleme

## 📈 Çıktılar

### Görsel Çıktılar
1. **Temel Dağılımlar** - Mesafe, hız, açı histogramları
2. **Zaman Serileri** - Trend analizi
3. **Polar Görünümler** - Radar tarama simülasyonu
4. **Range-Doppler Maps** - 2D yoğunluk haritaları
5. **3D Analiz** - Spatial visualization
6. **Clustering Sonuçları** - Obje gruplandırma
7. **Detection Timeline** - Real-time sonuçlar

### Analitik Çıktılar
- Tespit edilen obje sayısı ve türleri
- Ortalama mesafe, hız değerleri
- Clustering metrikleri (gürültü oranı)
- Detection güven skorları

## 🔧 Konfigürasyon

### DBSCAN Parametreleri
```python
eps = 0.5           # Neighborhood radius
min_samples = 15    # Minimum cluster size
```

### Detection Eşikleri
```python
range_threshold = 1.0    # Mesafe değişim eşiği
velocity_threshold = 0.3 # Hız eşiği
window_size = 50         # Sliding window boyutu
```

## 📝 Algoritma Detayları

### 1. Veri Preprocessing
```python
# Timestamp dönüşümü
df['datetime'] = pd.to_datetime(df['timestamp'], unit='us')

# Açı dönüşümü (radyan → derece)
df['angle_deg'] = np.degrees(df['raw_angle'])

# Polar → Kartezyen koordinat
x = range * cos(angle)
y = range * sin(angle)
```

### 2. DBSCAN Clustering
```python
# Özellik normalizasyonu
scaler = StandardScaler()
features_scaled = scaler.fit_transform([x, y, velocity])

# Clustering
dbscan = DBSCAN(eps=0.5, min_samples=15)
clusters = dbscan.fit_predict(features_scaled)
```

### 3. Real-Time Detection
```python
# Sliding window analizi
for window in sliding_windows:
    range_change = max(range) - min(range)
    velocity_avg = mean(velocity)
    
    if range_change > threshold or abs(velocity_avg) > threshold:
        # Detection event
        confidence = calculate_confidence(range_change, velocity_avg)
```

## 🎯 Use Cases

### Autonomous Vehicles
- **Obstacle Detection** - Statik ve dinamik engeller
- **Vehicle Tracking** - Diğer araçları takip
- **Lane Change** - Şerit değişimi desteği
- **Emergency Braking** - Acil fren sistemi

### ADAS Systems
- **Adaptive Cruise Control** - Uyarlanabilir hız kontrolü
- **Blind Spot Detection** - Kör nokta uyarısı
- **Parking Assistance** - Park yardımı
- **Cross Traffic Alert** - Çapraz trafik uyarısı

## 🔬 Gelişmiş Özellikler

### Multi-Object Tracking
- Kalman Filter implementasyonu
- Object ID assignment
- Trajectory prediction

### Machine Learning
- Feature engineering
- Classification algorithms
- Neural network integration

### Signal Processing
- FFT analysis
- Noise filtering
- Signal enhancement

## 📊 Performance Metrikleri

- **Detection Rate:** Doğru tespit oranı
- **False Positive Rate:** Yanlış alarm oranı
- **Processing Time:** İşlem süresi
- **Memory Usage:** Hafıza kullanımı

## 🛠️ Troubleshooting

### Yaygın Sorunlar
1. **ImportError:** Kütüphane eksikliği
   ```bash
   pip install --upgrade pandas matplotlib seaborn
   ```

2. **Memory Error:** Büyük veri seti
   ```python
   # Batch processing kullanın
   chunk_size = 1000
   ```

3. **Plot görünmüyor:** Backend sorunu
   ```python
   %matplotlib inline
   ```

## 📚 Referanslar

- [FMCW Radar Principles](https://en.wikipedia.org/wiki/Continuous-wave_radar)
- [DBSCAN Algorithm](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html)
- [Automotive Radar Systems](https://ieeexplore.ieee.org/)

## 👥 Katkıda Bulunma

1. Fork the repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Create Pull Request

## 📄 Lisans

Bu proje MIT lisansı altında lisanslanmıştır.

## 📞 İletişim

Sorularınız için:
- 📧 Email: [onurerenejder36@gmail.com]


---

**🎯 Happy Radar Analysis!** 🚗📡 
