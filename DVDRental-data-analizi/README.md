#  DVD Rental Data Analizi & Statistik Modelləşdirmə

Bu layihədə məşhur **DVD Rental** (PostgreSQL) verilənlər bazası üzərində SQL inteqrasiyası, geniş statistik analizlər, fərziyyə testləri və ekonometrik modelləşdirmə (OLS Reqressiyası) tətbiq olunmuşdur.

##  İstifadə Olunan Texnologiyalar
* **Verilənlər Bazası:** PostgreSQL (SQLAlchemy & Psycopg2)
* **Data Manipulyasiyası:** `pandas`, `numpy`
* **Statistik Analiz:** `scipy.stats`, `statsmodels`
* **Vizuallaşdırma:** `matplotlib`

---

##  Statistik Analizlər və Hipotez Testləri

### 1. Kirayə Qiymətinin Real Saxlanma Müddətinə Təsiri
* **Metodologiya:** Müştərilərin filmləri evdə real saxlama müddətləri hesablanmış və fərqli qiymət kateqoriyalarına ($0.99, $2.99, $4.99) görə qruplaşdırılmışdır. Paylanma normal olmadığı üçün (KS Testi, $p < 0.05$) non-parametrik **Kruskal-Wallis** testi icra edilmişdir.
* **Nəticə:** $p\text{-value} = 0.9879 > 0.05$ olduğu üçün qiymət qrupları arasında müştərinin filmi saxlama müddəti baxımından statistik olaraq əhəmiyyətli bir fərq tapılmamışdır. Qiymət müştəri davranışına bu kontekstdə təsir etmir.

### 2. Janrlar Üzrə Gəlir Müqayisəsi (Action vs. Children)
* **Metodologiya:** Hər iki janrın gəlir paylanması **Shapiro-Wilk** testi ilə yoxlanılmış və normal olmadığı üçün **Mann-Whitney U** testi tətbiq olunmuşdur.
* **Nəticə:** $p\text{-value} = 0.8051 > 0.05$ nəticəsi ilə mağazanın hər iki janrdan statistik olaraq eyni dərəcədə gəlir əldə etdiyi sübut olunmuşdur.

### 3. Film Müddəti (Length) və Kirayə Qiyməti (Rental Rate)
* **Nəticə:** Korrelyasiya analizi ($r \approx 0.03$) göstərir ki, filmin uzunluğu ilə qiyməti arasında hər hansı bir asılılıq yoxdur. Qiymət siyasətində film müddəti nəzərə alınmayıb.

### 4. Rəsmi Kirayə Müddəti və Cərimə Məbləği Arasındakı Əlaqə
* **Nəticə:** Yalnız cərimə ödəyən müştərilər analiz edildikdə, rəsmi kirayə müddəti ilə cərimə məbləği arasında mənfi korrelyasiya ($r \approx -0.44$) tapılmışdır. Mağazanın təyin etdiyi rəsmi limit qısa olduqca, müştərilərin limiti aşma və daha çox cərimə ödəmə ehtimalı yüksəlir.

---

##  Ekonometrik Modelləşdirmə: OLS Reqressiyası

Günlük ümumi gəlirə (`daily_gross_income`) təsir edən faktorları öyrənmək üçün **OLS (Ordinary Least Squares)** reqressiya modeli qurulmuşdur.

* **Asılı Dəyişən (Y):** `daily_gross_income`
* **Müstəqil Dəyişənlər (X):** `staff_id`, `is_weekend`, `unique_customers`

### Modelin Nəticələri (Model Summary)
* **R-squared:** `0.958` (Model günlük gəlirdəki dəyişikliyin **95.8%-ni** uğurla izah edir).
* **Müştəri Sıxlığı (`unique_customers`):** Statistik olaraq olduqca əhəmiyyətlidir ($p = 0.000$). Müştəri sayının 1 vahid artması günlük gəliri orta hesabla **$6.42** artırır.
* **İşçi və Həftəsonu Faktoru:** `staff_id` və `is_weekend` dəyişənlərinin $p\text{-value}$ göstəriciləri $0.05$-dən böyük olduğu üçün günlük gəlirə birbaşa təsir göstərmirlər.
* **Konstant (const):** Müştəri sayı sıfır olduğu halda mağazanın sabit xərclərdən dolayı mənfiyə düşməsini real olaraq ifadə edir.

---


