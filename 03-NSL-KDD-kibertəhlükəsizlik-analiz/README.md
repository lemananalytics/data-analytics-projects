### NSL-KDD Dataset üzərində Kibertəhlükəsizlik və Anomaliya Analizi

Bu layihə, **NSL-KDD** şəbəkə logları məlumat massivi əsasında anomal və kiber hücum trafiklərinin aşkarlanması, fərqli protokolların analizi, statistik testlərin tətbiqi və maşın öyrənməsi modelləri ilə təsnifləşdirilməsi məqsədilə hazırlanmışdır.

### Layihənin Məqsədi və Domen
Kibertəhlükəsizlik analitikası çərçivəsində reallaşdırılan bu araşdırmanın əsas məqsədləri:
* Şəbəkə paketlərinin xüsusiyyətlərini (`src_bytes`, `count`, `protocol_type` və s.) analiz etmək.
* Spesifik hücum alt tiplərini makro səviyyədə 5 əsas kateqoriyaya (`DoS`, `Probe`, `R2L`, `U2R`, `Normal`) qruplaşdırmaq.
* Hücum tiplərinin şəbəkə bağlantı sayına təsirini riyazi və statistik metodlarla sübut etmək.
* Logistik Reqressiya modeli quraraq legitim və anomal trafikləri yüksək həssaslıqla proqnozlaşdırmaq.

### İstifadə Olunan Texnologiyalar
* **Dil:** Python 3.x
* **Kitabxanalar:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `statsmodels`

 Əsas Analiz Mərhələləri və Nəticələr

### 1. Eksplorativ Data Analizi (EDA) & Korrelyasiya
* **Kənarlaşmalar (Outliers):** Boxplot analizi zamanı `portsweep` (şəbəkə portlarının skan edilməsi) trafiki daxilində ekstremal dərəcədə böyük paket ölçüləri ($src\_bytes$) aşkarlanmışdır.
* **Pearson Korrelyasiyası:** Eyni hosta edilən bağlantı sayısı (`count`) artdıqca, uğurlu girişlərin (`logged_in`) orta dərəcədə mənfi korrelyasiya ($-0.54$) göstərdiyi müəyyən edilmişdir. Bu, legitim istifadəçi davranışının deyil, şəbəkəyə edilən kütləvi hücum cəhdlərinin birbaşa göstəricisidir.

### 2. Normallıq və Post-Hoc Statistik Analizləri
* **Kolmogorov-Smirnov (KS) Testi:** Şəbəkə bağlantı saylarının qruplar üzrə paylanmasının normal paylanmaya (Gaussian) uyğun olmadığı ($p < 0.05$) təsdiqlənmişdir.
* **Tukey HSD Testi:** Normallıq şərti pozulsa da, qruplararası spesifik fərqləri vizual və riyazi olaraq dəqiqləşdirmək üçün aparılan Tukey testi nəticəsində:
  * `DoS` və `Probe` hücumlarının normal trafiklə müqayisədə bağlantı sayını kəskin və statistik olaraq əhəmiyyətli dərəcədə artırdığı ($reject=True$),
  * `R2L` və `U2R` hücumlarının isə normal trafiklə statistik fərq yaratmadığı ($reject=False$), yəni həcm əsaslı deyil, daha gizli struktura malik olduqları sübut edilmişdir.

### 3. Maşın Öyrənməsi Modeli (Logistic Regression)
Şəbəkə trafikinin legitim və ya anomal olduğunu proqnozlaşdırmaq üçün qurulan Logistik Reqressiya modelinin performansı:
* **Ümumi Dəqiqlik (Accuracy):** 90%
* **Həssaslıq (Recall for Anomal):** 92% — Model real olaraq baş verən hücum şəbəkələrinin 92%-ni uğurla aşkarlaya bilir. Kibertəhlükəsizlik analitikasında qaçırılan hücum riskini (False Negative) minimuma endirmək baxımından bu metrik kritik əhəmiyyət daşıyır.

