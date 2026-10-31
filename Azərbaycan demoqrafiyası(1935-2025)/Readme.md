# Azərbaycanın Demoqrafik Göstəricilərinin Statistik Təhlili (1935–2025)

Bu layihədə Azərbaycanın 90 illik (1935–2025) demoqrafik göstəriciləri Python ilə yeddi mərhələdə təhlil olunub: trend analizi, reqressiya, müqayisəli testlər, korrelyasiya, böyümə sürəti, klasterləşmə (dövrləşdirmə) və əhali proqnozu. Nəticələr yekun PDF hesabatda ümumiləşdirilib.

## Layihənin məqsədi

- Əhalinin, doğum və ölüm əmsallarının, təbii artımın, miqrasiyanın və ümumi doğuşluq əmsalının (TFR) uzunmüddətli dinamikasını öyrənmək
- Tarixi dövrlər (Sovet və müstəqillik, müharibə, pandemiya) arasındakı fərqlərin statistik cəhətdən mənalı olub-olmadığını yoxlamaq
- Oxşar demoqrafik xüsusiyyətli illəri klasterləşdirərək dövrləri data əsasında müəyyən etmək
- Əhalinin 2026–2030 üçün proqnozunu vermək və modeli backtest ilə yoxlamaq

## Data

Data 1935–2025-ci illəri əhatə edən 91 illik sətirdən və 10 sütundan ibarətdir (`azerbaycan_demoqrafiya_1935_2025.csv`). Göstəricilər Azərbaycan Dövlət Statistika Komitəsinin məlumatlarına əsaslanır və Wikipedia-nın "Demographics of Azerbaijan" səhifəsindən toplanıb.

| Sütun | Mənası | Mövcud illər |
|---|---|---|
| `Il` | İl | 1935–2025 |
| `Ortalama_Ehali` | Orta illik əhali (nəfər) | 1935–2025 |
| `Diri_Dogumlar`, `Olumler`, `Tebii_Artim` | Mütləq say | 1935–2025 |
| `Dogum_Emsali_1000de`, `Olum_Emsali_1000de`, `Tebii_Artim_Emsali_1000de` | Min nəfərə düşən əmsal (‰) | 1935–2025 |
| `Migrasiya_Emsali_1000de` | Xalis miqrasiya əmsalı (‰) | 1936–2025 |
| `Umumi_Dogusluq_Emsali` | TFR (bir qadına düşən orta uşaq sayı) | 1961–2025 |

**Boş dəyərlər:** TFR 1935–1960 üçün, miqrasiya isə 1935 üçün mövcud deyil. Bu sütunlarla aparılan hesablamalarda boş sətirlər `dropna` ilə çıxarılıb və nəticələr yalnız mövcud illər üçün keçərlidir.

## Qovluğun strukturu

```
Azerbaycan-demoqrafiyasi-1935-2025/
├── Azerbaycan_demoqrafiya_1935_2025.ipynb      # Analiz kodu (Jupyter Notebook)
├── Azerbaycan_demoqrafiya_yekun_hesabat.pdf    # Yekun hesabat
├── azerbaycan_demoqrafiya_1935_2025.csv        # Data
└── Readme.md
```

## Analiz mərhələləri

### 1. Trend analizi
5 illik hərəkətli orta ilə əhali, doğum və ölüm əmsalları, təbii artım və TFR trendləri. İllik artım tempi `pct_change()` ilə hesablanıb.
- Əhali 2 933 800 nəfərdən 10 243 600 nəfərə çatıb (ümumi artım 249,16%, orta illik artım tempi 1,41%)
- Ən sürətli artım 1961-ci ildə (3,88%), ən kəskin düşüş 1943-cü ildə (−7,57%)

### 2. Reqressiya analizi
OLS (`statsmodels`) və polinomial reqressiya (`numpy.polyfit`).

| Model | R² | Nəticə |
|---|---|---|
| İl → doğum əmsalı (1935–2025) | 0,435 | Meyl −0,2221 ‰/il, düz xətt tam dövr üçün zəif uyğundur |
| İl → doğum əmsalı (1996–2025) | 0,215 | Meyl −0,1428 ‰/il, zəif uyğunluq |
| TFR, polinomial (dərəcə 3) | 0,949 | Güclü uyğunluq, diapazondan kənar ekstrapolyasiya riski var |
| Təbii artım ~ doğum + ölüm + miqrasiya | 1,000 | Riyazi eyniyyətdir (təbii artım = doğum − ölüm), analitik dəyəri yoxdur |

### 3. Müqayisəli testlər
Test seçilməzdən əvvəl normallıq (Shapiro-Wilk) və dispersiyaların bərabərliyi (Levene) yoxlanılıb. Dispersiyalar fərqli olduğu üçün Welch t-testi istifadə olunub.
- Doğum əmsalı, Sovet dövrü (1935–1990) və müstəqillik dövrü (1991–2025): 29,41‰ və 16,29‰ (t = 11,24, p < 0,001)
- Ölüm əmsalı, müharibə illəri (1941–1945) və ətraf sülh illəri: 14,90‰ və 10,42‰ (p = 0,0013)
- Ölüm əmsalı, COVID (2020–2021) və 2015–2019: 7,55‰ və 5,74‰. Qruplarda cəmi 2 və 5 müşahidə olduğu üçün nəticə ehtiyatla şərh olunmalıdır.

### 4. Korrelyasiya analizi
Pearson və Spearman əmsalları, korrelyasiya matrisi (heatmap).
- Doğum əmsalı ilə TFR: r = 0,94
- Miqrasiya ilə təbii artım: r = 0,34 (zəif-orta müsbət əlaqə)
- İl ilə TFR: Pearson r = −0,92, Spearman ρ = −0,92

### 5. Böyümə sürəti analizi
Mütləq və nisbi (faiz) illik dəyişmənin müqayisəsi, ən sürətli və ən yavaş artım illəri, dövrlər üzrə orta artım tempi.

| Dövr | Orta illik artım |
|---|---|
| Erkən Sovet (1935–1960) | 1,19% |
| Gec Sovet (1961–1990) | 2,06% |
| Keçid dövrü (1991–2003) | 1,14% |
| Müasir dövr (2004–2025) | 0,96% |

### 6. Dövrləşdirmə (K-means klasterləşmə)
Doğum, ölüm, təbii artım əmsalları və artım tempi standartlaşdırılaraq K-means ilə qruplaşdırılıb. K seçimi inertia və silhouette ilə əsaslandırılıb (K = 4).
- Müharibə tənəzzülü: 1942–1945
- Sürətli artım: 1951–1968 (1953 istisna)
- Orta artım (Sovet): 1936–1941, 1946–1950, 1953, 1969–1993
- Aşağı doğum (müasir): 1994–2025 (32 illin hamısı eyni klasterdə)

### 7. Əhali proqnozu (2026–2030)
Xətti trend və ARIMA 1994–2025 dövrü üzərində müqayisə olunub. Modellər 2020-yə qədər öyrədilib, 2021–2025 üzərində yoxlanılıb (backtest).

| Model | MAPE |
|---|---|
| Xətti trend | 1,54% |
| ARIMA(0, 2, 0) | 0,58% |

ARIMA-ya görə əhali 2030-da təxminən 10,45 mln nəfər olacaq (95% interval: 10,18–10,72 mln). Final modelin konvergensiyası yoxlanılıb.

## Əsas nəticələr

- Əhali 249% artıb, lakin illik artım tempi gec Sovet dövründəki 2,06%-dən müasir dövrdə 0,96%-ə düşüb.
- Doğum əmsalı Sovet dövründən müstəqillik dövrünə statistik cəhətdən mənalı şəkildə azalıb.
- TFR 1996-cı ildə ilk dəfə əvəzetmə həddinin (2,1) altına düşüb, 2005–2015-ci illərdə müvəqqəti olaraq yenidən həddin üzərinə qalxıb və 2016-cı ildən bəri yenidən həddin altındadır.
- Klasterləşmə 1994–2025-ci illəri vahid demoqrafik dövr kimi ayırır.
- Bütün nəticələr statistik əlaqələri təsvir edir, səbəb-nəticə əlaqəsi iddia etmir.

## Metodoloji qeydlər və məhdudiyyətlər

- Data illikdir və cəmi 91 müşahidədən ibarətdir. Backtest 5 nöqtəyə, COVID testi isə 2 nöqtəyə əsaslanır.
- Doğum əmsalı üzrə Model 1 və 2-də qalıqların avtokorrelyasiyası güclüdür (Durbin-Watson ≈ 0,13), ona görə p-dəyərləri optimist ola bilər.
- Sovet dövrü qrupunda normallıq fərziyyəsi pozulub (Shapiro-Wilk p = 0,020). Nəticənin davamlılığı Mann-Whitney testi ilə də yoxlanıla bilər.
- ARIMA-da fərqləmə dərəcəsi (d) AIC ilə seçilib. AIC yalnız eyni d-li modelləri düzgün müqayisə edir, d-nin ADF testi ilə əsaslandırılması növbəti addımdır.
- K-means vaxt ardıcıllığını nəzərə almır, ona görə bir klasterə düşən illər ardıcıl olmaya bilər.
- Data Wikipedia vasitəsilə toplanıb, rəsmi istifadədən əvvəl [stat.gov.az](https://www.stat.gov.az) ilə yoxlanmalıdır.

## İstifadə olunan texnologiyalar

Python, Pandas, NumPy, Matplotlib, Seaborn, SciPy, Statsmodels, Scikit-learn, Jupyter Notebook

## İşə salma

```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn jupyter
jupyter notebook Azerbaycan_demoqrafiya_1935_2025.ipynb
```

Notebook-un data faylını oxuyan sətrində yol qovluqdakı CSV faylına uyğunlaşdırılmalıdır:

```python
df = pd.read_csv("azerbaycan_demoqrafiya_1935_2025.csv")
```

## Yekun hesabat

Bütün mərhələlərin nəticələri, qrafiklər, cədvəllər və məhdudiyyətlər [Azerbaycan_demoqrafiya_yekun_hesabat.pdf](Azerbaycan_demoqrafiya_yekun_hesabat.pdf) faylında verilib.
