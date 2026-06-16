#  DVD Rental Data Analizi, Excel Hesabatı & Statistik Modelləşdirmə

Bu layihədə məşhur **DVD Rental** (PostgreSQL) verilənlər bazası üzərində SQL inteqrasiyası, Python (Jupyter Notebook) vasitəsilə qabaqcıl statistik analizlər və **Excel** mühitində interaktiv biznes hesabatlığı (Reporting) tətbiq olunmuşdur.

##  İstifadə Olunan Texnologiyalar və Alətlər
* **Verilənlər Bazası:** PostgreSQL (SQLAlchemy & Psycopg2)
* **Data Analiz (Python):** `pandas`, `numpy`, `scipy.stats`, `statsmodels`, `matplotlib`
* **Biznes Hesabatlığı:** Microsoft Excel (Pivot Tables, Advanced Formulas, Data Modeling)

---

##  1. Python ilə Statistik Analizlər və Hipotez Testləri

### Kirayə Qiymətinin Real Saxlanma Müddətinə Təsiri
* **Metodologiya:** Müştərilərin filmləri evdə real saxlama müddətləri hesablanmış və fərqli qiymət kateqoriyalarına ($0.99, $2.99, $4.99) görə qruplaşdırılmışdır. Paylanma normal olmadığı üçün (KS Testi, $p < 0.05$) non-parametrik **Kruskal-Wallis** testi icra edilmişdir.
* **Nəticə:** $p\text{-value} = 0.9879 > 0.05$ olduğu üçün qiymət qrupları arasında müştərinin filmi saxlama müddəti baxımından statistik olaraq əhəmiyyətli bir fərq tapılmamışdır.

### Janrlar Üzrə Gəlir Müqayisəsi (Action vs. Children)
* **Metodologiya:** Hər iki janrın gəlir paylanması **Shapiro-Wilk** testi ilə yoxlanılmış və normal olmadığı üçün **Mann-Whitney U** testi tətbiq olunmuşdur.
* **Nəticə:** $p\text{-value} = 0.8051 > 0.05$ nəticəsi ilə mağazanın hər iki janrdan statistik olaraq eyni dərəcədə gəlir əldə etdiyi sübut olunmuşdur.

### Ekonometrik Modelləşdirmə: OLS Reqressiyası
Günlük ümumi gəlirə (`daily_gross_income`) təsir edən faktorları öyrənmək üçün **OLS (Ordinary Least Squares)** reqressiya modeli qurulmuşdur.
* **R-squared:** `0.958` (Model günlük gəlirdəki dəyişikliyin **95.8%-ni** uğurla izah edir).
* **Müştəri Sıxlığı (`unique_customers`):** Statistik olaraq olduqca əhəmiyyətlidir ($p = 0.000$). Müştəri sayının 1 vahid artması günlük gəliri orta hesabla **$6.42** artırır.

---

##  2. Excel Biznes Hesabatlığı və Data Analizi (Reporting)

Layihənin bu hissəsində verilənlər bazasından çəkilən xam məlumatlar menecerlər və qərarverici şəxslər (stakeholders) üçün **Excel** mühitində analiz edilmiş və vizuallaşdırılmışdır:

* **Pivot Cədvəllər (Pivot Tables):** Müştəri seqmentasiyası, ödəniş trendləri və ən çox gəlir gətirən kateqoriyalar Pivot vasitəsilə qruplaşdırılmışdır.
* **Təkmil Düsturlar:** Tarixlər arası fərqlərin, cərimə faizlərinin və şərti şərtlərin hesablanması üçün Excel funksiyalarından istifadə olunmuşdur.
* **Biznes Tapıntıları:** Mağazanın günlük satış dinamikası, pik günləri və müştəri loyallığı Excel hesabatında aydın şəkildə strukturlaşdırılmışdır.

### Hesabatın Ekran Görüntüsü:
*(Hesabatın interaktiv quruluşunu aşağıdakı qrafikdən baxa bilərsiniz)*

<img width="1876" height="732" alt="image" src="https://github.com/user-attachments/assets/2283e2f7-1255-41c3-8a93-441c78440ead" />


---




