# 🚖 Predykcja cen taksówek - Case Study

---

## 📌 Cel i zakres projektu

Głównym celem projektu było zbudowanie i porównanie modeli regresyjnych przewidujących cenę przejazdu taksówką na podstawie zbioru danych `taxi_price`.  

Projekt weryfikował hipotezę badawczą:  
> Czy złożoność obliczeniowa lasu losowego jest uzasadniona, jeśli prostszy model liniowy (Elastic Net) mógłby osiągnąć zbliżone wyniki?

---

## 📊 Analiza danych i preprocessing

**Charakterystyka zbioru:**  
- 1000 obserwacji  
- 11 zmiennych  

**Korelacje:**  
- Najsilniejszym predyktorem ceny jest dystans ($r = 0.86$ dla korelacji Pearsona)

**Braki danych:**  
- Około 5% braków dla niemal każdej zmiennej  
- Łącznie 43.8% wierszy zawierało przynajmniej jedną brakującą wartość  

**Imputacja:**  
- Zastosowano algorytm `missForest` (wykorzystujący lasy losowe)  
- Pozwoliło to uniknąć utraty mocy statystycznej modeli

---

## 🤖 Wykorzystane modele

**1. Random Forest (Model 1)**  
- Wybrany ze względu na świetne radzenie sobie z nieliniowymi zależnościami i brakami danych (poprzez imputację)  

**2. Elastic Net Regression (Model 2)**  
- Model liniowy z regularyzacją L1 i L2  
- Wybrany jako punkt odniesienia ze względu na swoją "przezroczystość" i łatwą interpretację współczynników  

---

## 📈 Wyniki i porównanie

| Metryka | Random Forest | Elastic Net |
|---------|---------------|-------------|
| RMSE    | 10.12         | 16.21       |
| R²      | 0.95          | 0.86        |
| MAE     | 6.29          | 9.54        |
| MAPE    | 13.69%        | 19.94%      |

**Kluczowe obserwacje:**  
- **Jakość dopasowania:** Random Forest wyjaśnia aż 95% zmienności ceny, podczas gdy Elastic Net napotkał barierę na poziomie 86%.  
- **Stabilność:** Niższa wartość RMSE dla Random Forest dowodzi większej odporności na wartości skrajne i nietypowe trasy.  
- **Błędy fizyczne:** Elastic Net generował ujemne wartości ceny (np. -56.34 USD), co jest niedopuszczalne w rozwiązaniach produkcyjnych.

---

## 💡 Wnioski końcowe

Hipoteza o preferencji modelu liniowego została odrzucona. Wyższa złożoność obliczeniowa Random Forest jest w pełni uzasadniona, ponieważ:  
- Model poprawnie odwzorował multiplikatywny charakter taryf (cena jako funkcja dystansu, stawki i czasu)  
- Eliminuje problem nielogicznych, ujemnych prognoz  
- Zapewnia znacznie lepszą stabilność biznesową, mimo mniejszej interpretowalności wag niż w przypadku regresji  

---

## 🛠️ Wykorzystane technologie

- **Język:** R  
- **Biblioteki:** `randomForest`, `glmnet`, `caret`, `missForest`, `ggplot2`, `dplyr`
