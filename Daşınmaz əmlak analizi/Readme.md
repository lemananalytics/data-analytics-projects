# 🏢 Bakı Daşınmaz Əmlak Bazarının Təhlili və Power BI Dashboard-u

Bu layihədə Bakı şəhəri üzrə daşınmaz əmlak (mənzil) elanları əsasında məlumatların təmizlənməsi, statistik təhlili, Power Query/Excel vasitəsilə yeni göstəricilərin (DAX/Calculated columns) hesabatlılığa əlavə edilməsi və **Power BI** mühitində 3 səhifəlik interaktiv dashboard-un qurulması həyata keçirilmişdir.

---

## 📌 1. Məlumat Mənbəyi (Data Source)
Məlumatlar [Kaggle](https://www.kaggle.com/) platformasından əldə edilmişdir. Dataset Bakı şəhərinin müxtəlif rayonları üzrə 60 mənzil elanının texniki göstəricilərini və qiymət parametrini əks etdirir.

---

## 📊 2. Statistik Analiz və Məlumatların Təmizlənməsi (Python / Excel)

Data mühəndisliyi və analizi mərhələsində aşağıdakı statistik təhlillər aparılmışdır:

* **Çatışmayan və Təkrar Məlumatlar:** Dataset-də boş (null) dəyərlər və dublikat `Elan_ID`-lər yoxlanılmış, datanın tam bütöv olduğu təsdiqlənmişdir.
* **Təsviri Statistika (Descriptive Statistics):** Metraya məsafə, bina yaşı, otaq sayı, sahə, $m^2$ qiyməti, satış və kirayə qiymətləri üzrə `mean`, `std`, `median (50%)`, `IQR`, `skewness` (asimmetriya) və `kurtosis` göstəriciləri hesablanmışdır.
* **Kənar Dəyərlərin Təyini (Outliers Detection):** $1.5 \times IQR$ qaydası tətbiq olunaraq mənzil sahəsi ($m^2$), satış və kirayə qiymətləri üzrə kənar dəyərlər (outlier-lər) müəyyən edilmişdir.
* **Kategorik Analiz:** Rayonlar, bina növü (Yeni/Köhnə tikili), təmir növü, çıxarış (kupça) və dayanacaq varlığı üzrə paylanmalar analiz edilmişdir.

---

## 🛠️ 3. Excel və Power Query-də Yaradılmış Yeni Sütunlar (Feature Engineering)

Data modeli daha dərin vizuallaşdırmaq və biznes metrikalarını hesablamaq üçün Excel / Power Query (DAX) vasitəsilə aşağıdakı yeni sütunlar və hesablamalar əlavə edilmişdir:

1. **`Bina_Yaşı`:** Binanın tikinti ilinə əsasən yaşının hesablanması.
2. **`Qiymət_m2_AZN`:** Mənzilin ümumi satış qiymətinin sahəyə ($m^2$) nisbəti.
3. **`Gəlirlilik Qazancları / ROI Metrikaları`:** Kirayə qiyməti və kommunal xərclər əsasında mənzilin illik gəlirliliyinin satış qiymətinə nisbəti hesablamaq üçün köməkçi sütunlar.
4. **`Mərtəbə Kateqoriyası`:** Mənzilin yerləşdiyi mərtəbənin binanın ümumi mərtəbə sayına nisbətinə görə qruplaşdırılması (ilk, orta, son mərtəbə).

---

## 📈 4. Power BI Dashboard Səhifələri

Dashboard 3 əsas interaktiv səhifədən ibarətdir:

### 🔹 Səhifə 1: Ümumi İcmal (Executive Overview)
* Bakı əmlak bazarının ümumi göstəriciləri (Ümumi elan sayı, orta $m^2$ qiyməti, orta satış qiyməti).
* Rayonlar üzrə mənzil paylanması və qiymət fərqləri.

<img width="1311" height="737" alt="image" src="https://github.com/user-attachments/assets/7641e69d-75ed-467d-899c-6df951b61e6c" />


---

### 🔹 Səhifə 2: Qiymət və Rayon Analizi (Price & Location Analysis)
* Metro stansiyalarına məsafənin və bina yaşının qiymətə təsiri.
* Təmir növü və çıxarışın (kupça) satış qiymətinə və satışda qalma gününə təsiri.

<img width="1311" height="740" alt="image" src="https://github.com/user-attachments/assets/19ffa964-d43f-421c-81ef-c08eeb9a7fc5" />


---

### 🔹 Səhifə 3: İnvestisiya və Kirayə Təhlili (Investment & Rental Yield)
* Mənzillərin satış qiyməti ilə kirayə qiymətləri arasındakı korrelyasiya.
* Yeni və köhnə tikililər üzrə gəlirlilik müqayisəsi.

<img width="1310" height="737" alt="image" src="https://github.com/user-attachments/assets/dc1a6b55-76cc-46f0-b332-4c16bc110363" />


---

## 📁 Fayl Strukturu

```text
├── baki_emlak_data.xlsx         # Əsas istifadə olunan data faylı
├── baki_emlak_data_PowerBI.xlsx # Hesablanmış yeni sütunlar olan Excel faylı
├── Report.pbix                  # Power BI Hesabat faylı
├── screens/                     # Dashboard skrinşotları
│   ├── page1.png
│   ├── page2.png
│   └── page3.png
└── README.md                    # Layihə haqqında etrafli məlumat
