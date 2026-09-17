# 🏢 Employee Turnover Prediction: A Reality Check in HR Analytics

## 📌 Projenin Amacı
Bu proje, yaklaşık 2 milyon satırlık devasa bir İnsan Kaynakları veri seti kullanılarak çalışanların **istifa etme (Resigned)** olasılıklarını tahmin etmek amacıyla geliştirilmiştir. 

Projenin temel felsefesi "en yüksek doğruluk oranını (Accuracy) bulmak" değil; **verideki mantıksızlıkları iş kurallarıyla (Business Rules) temizlemek, veri sızıntılarını (Data Leakage) engellemek ve dengesiz sınıf (Class Imbalance) problemlerini çözerek "gerçekçi ve işe yarar" bir model kurmaktır.**

## 🛠️ Kullanılan Teknolojiler
* **Dil:** Python
* **Kütüphaneler:** Pandas, NumPy, Scikit-Learn
* **Makine Öğrenmesi Modeli:** Random Forest Classifier
* **Teknikler:** Rule-Based Data Cleaning, One-Hot/Ordinal Encoding, Undersampling, Feature Importance Analysis

## 🚀 Proje Adımları ve "Senior" Yaklaşımları

### 1. Mantıksız Verilerin Avlanması (Sanity Checks)
Makine öğrenmesi modelleri sadece onlara verilen veriyi işler. Ancak İK verilerinde sıkça rastlanan mantık hataları (örneğin; "25 yaşında, 15 yıl deneyimli Direktör" gibi) modelin kafasını karıştırır.
* Üniversite mezuniyet yaşı taban alınarak (Min: 21 yaş) deneyim yılı ile yaş arasındaki doğrusal ilişki (Multicollinearity) test edildi.
* Yaklaşık **50.000 adet mantıksız kayıt** tespit edildi ve iş mantığına uygun olarak dinamik bir şekilde düzeltildi.

### 2. Doğruluk Paradoksunu (Accuracy Paradox) Yıkmak
İlk kurulan model **%92** başarı oranı (Accuracy) gösterdi. Ancak veri setindeki çalışanların %92'si zaten "Active" (Çalışan) statüsündeydi. Model, azınlıkta olan istifaları (%8) bulmak yerine herkese "Kalır" diyerek ezber (Overfitting) yapıyordu.
* **Çözüm:** Hedef kitleyi saptıran "Kovulanlar" ve "Emekliler" veriden çıkarıldı.
* **Undersampling (Alt Örnekleme):** 1.7 milyonluk Active çalışan verisi, Resigned (İstifa eden) sayısı kadar (yaklaşık 160.000) rastgele seçilerek %50-%50 dengeli ve adil bir öğrenme ortamı (Arena) yaratıldı.

### 3. Veri Sızıntısının (Data Leakage) Tespiti
Modelin Feature Importance (Özellik Önemi) sonuçları incelendiğinde, modelin istifaları tahmin ederken `%12.6` oranında `Year` (Yıl) sütununa güvendiği görüldü. Geleceği tahmin eden bir modelin geçmiş yıllara ait trendleri ezberlemesi bir veri sızıntısıdır. `Year` sütunu derhal sistemden atıldı.

## 📊 Model Sonuçları ve İş Çıktısı (Business Value)

Adil şartlarda, veri sızıntısı olmadan eğitilen modifiye edilmiş Random Forest modelinin nihai sonuçları:
* **Accuracy & F1-Score:** %61
* **Recall (İstifa Edenleri Yakalama Oranı):** %61

**Yorum:** %61 ilk bakışta düşük gibi görünse de, bu oran "Ekonometrik ve İstatistiksel Gerçekliktir". İnsanların istifa kararını sadece *Yaş, Maaş ve Departman* gibi kısıtlı özelliklerle %100 açıklamak imkansızdır (Omitted Variable Bias). 
Ancak İK departmanı açısından bu sonuç büyük bir zaferdir: **İstifa edecek her 100 kişiden 61'i, henüz istifa etmeden aylar önce tespit edilebilir ve elde tutma (Retention) stratejileri uygulanabilir.**
