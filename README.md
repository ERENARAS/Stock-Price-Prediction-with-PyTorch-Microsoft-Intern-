# Hisse Senedi Fiyat Tahmini

Bu proje, **PyTorch** kullanılarak geliştirilmiş **tek ve çok değişkenli LSTM (Long Short-Term Memory)** ve **GRU (Gated Recurrent Unit)** modelleri ile hisse senedi fiyat tahmini yapmayı amaçlamaktadır.

---

## Proje Hakkında

Amacımız, modern derin öğrenme mimarilerini kullanarak finansal zaman serilerini (hisse senedi fiyatları) analiz etmek ve gelecekteki fiyatlarını yüksek doğrulukla tahmin etmektir.  

Bu kapsamda:  
- **Tek değişkenli modeller**: Sadece *kapanış fiyatı* kullanılarak.  
- **Çok değişkenli modeller**: Kapanış fiyatına ek olarak **işlem hacmi (Volume)**, **EMA**, **RSI** gibi teknik göstergeler eklenerek.  

Projede hem **LSTM** hem de **GRU** mimarileri karşılaştırılmıştır.

---

## Veri Seti ve Hazırlık Süreci

- **Kaynak**: [Yahoo Finance](https://finance.yahoo.com/) → `yfinance` kütüphanesi ile çekildi.  
- **Şirket**: Google (GOOGL)  
- **Zaman Aralığı**: `01.01.2010 - 01.01.2025` (Günlük veriler)

### Veri Ön İşleme
- **Tek değişkenli**: Yalnızca `Close` fiyatı.  
- **Çok değişkenli**: `Close`, `Volume`, `EMA_14`, `RSI_14` → `pandas_ta` ile hesaplandı.  
- **Normalizasyon**: `MinMaxScaler` (0-1 arası ölçekleme).  
- **Veri Ayırma**: %80 Eğitim - %20 Test (kronolojik).  
- **Sliding Window**: `time_step = 60` gün → Son 60 gün → 61. gün tahmini.

---

##  Model Mimarisi

### Tek Değişkenli
- **Input**: 1 özellik (`Close`)  
- **Hidden Units**: 50  
- **Modeller**: LSTM & GRU  

### Çok Değişkenli
- **Input**: 4 özellik (`Close`, `Volume`, `EMA_14`, `RSI_14`)  
- **Hidden Units**: 128  
- **Modeller**: LSTM & GRU  

Her modelde son katman: **Linear → Tek değer (tahmini fiyat)**  

---

##  Model Eğitimi ve Optimizasyon

- **Loss Function**: MSE (Mean Squared Error)  
- **Optimizer**: Adam (`lr=0.001`)  
- **Epochs**: 200  

---

## Değerlendirme ve Sonuçlar

Performans metriği: **RMSE (Root Mean Squared Error)**  

| Model | Veri Yaklaşımı | RMSE (USD) |
|-------|----------------|------------|
| LSTM  | Tek Değişkenli (Close) | **8.08** |
| GRU   | Tek Değişkenli (Close) | **4.09** |
| LSTM  | Çok Değişkenli (4 Özellik) | **4.62** |
| GRU   | Çok Değişkenli (4 Özellik) | **3.60** |

---

## Çıkarımlar

- **GRU > LSTM** → Her iki yaklaşımda da GRU daha düşük hata verdi.  
- **Özellik mühendisliği önemli** → Ek göstergeler (EMA, RSI, Volume) tahmin gücünü artırdı.  
- **En iyi model**: Çok değişkenli GRU → **RMSE: 3.60 USD**

---

## Görsel Sonuçlar

- **LSTM - Tek Değişkenli**: Genel trendi yakalasa da yüksek hata (RMSE=8.08).  
- **GRU - Tek Değişkenli**: Daha yakın tahminler (RMSE=4.09).  
- **LSTM - Çok Değişkenli**: İyileştirilmiş performans (RMSE=4.62).  
- **GRU - Çok Değişkenli**: En başarılı model (RMSE=3.60).  

*(Grafikler proje dosyaları içinde yer almaktadır.)*

---

## Proje Sürecinde Kazanımlar

- Veri biliminin **döngüsel doğası**  
- Veri ön işlemenin **kritik önemi**  
- Özellik mühendisliği ile **performans artışı**  
- LSTM & GRU karşılaştırması  
- **PyTorch** ile derin öğrenme uygulamaları  
- Hata analizi ve model iyileştirme deneyimi  

---

## Kullanılan Teknolojiler

- Python  
- PyTorch  
- pandas, numpy, matplotlib, seaborn  
- pandas_ta (teknik indikatörler)  
- scikit-learn (veri ölçekleme)  
- yfinance (veri çekme)

---
