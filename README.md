# Przewidywanie cukrzycy — Klasyfikacja binarna

Projekt zaliczeniowy z przedmiotu: Programowanie 2. 

Celem projektu jest implementacja i porównanie modeli uczenia maszynowego
do przewidywania wystąpienia cukrzycy typu 2 na podstawie danych medycznych.


## Zbiór danych

Nazwa: Pima Indians Diabetes Database
Źródło: Kaggle / UCI Machine Learning Repository
Rozmiar: 768 obserwacji, 9 kolumn
Populacja: Kobiety z grupy etnicznej Pima Indian, wiek minimum 21 lat
KolumnaOpisPregnanciesLiczba ciążGlucoseStężenie glukozy w osoczu (mg/dL)BloodPressureRozkurczowe ciśnienie krwi (mm Hg)SkinThicknessGrubość fałdu skórnego tricepsa (mm)InsulinStężenie insuliny w surowicy (uU/mL)BMIWskaźnik masy ciała (kg/m2)DiabetesPedigreeFunctionGenetyczne ryzyko cukrzycyAgeWiek w latachOutcomeZmienna docelowa: 1 = cukrzyca, 0 = brak cukrzycy
Cel analizy
Projekt rozwiązuje problem klasyfikacji binarnej — na podstawie pomiarów
medycznych model przewiduje, czy u danej pacjentki występuje cukrzyca.
Główną metryką oceny jest recall dla klasy pozytywnej (cukrzyca), ponieważ
w zastosowaniach medycznych przeoczenie chorego pacjenta niesie wyższe
konsekwencje niż fałszywy alarm. Dodatkowo raportowane są: accuracy,
precision, F1-score oraz ROC-AUC.
Struktura projektu
.
├── README.md
└── Przewidywanie_cukrzycy.ipynb
Notebook zawiera:

eksploracyjną analizę danych (EDA)
analizę korelacji między cechami
czyszczenie danych i obsługę brakujących wartości
budowę pipeline'u ML z użyciem scikit-learn
porównanie trzech modeli klasyfikacji z identycznym preprocessingiem
strojenie hiperparametrów Random Forest metodą GridSearchCV
wybór modelu końcowego na podstawie wyników cross-validation
ocenę finalnego modelu na zbiorze testowym
wizualizacje: macierz konfuzji, krzywa ROC, współczynniki modelu
przykładowe predykcje dla nowych danych z dwoma progami decyzyjnymi

Metodologia
Dane zawierają ukryte brakujące wartości — biologicznie niemożliwe zera
w kolumnach takich jak Glucose czy BMI. Zastąpiono je medianą kolumny
przy użyciu SimpleImputer, który jest częścią Pipeline i działa wyłącznie
na danych treningowych, co eliminuje ryzyko data leakage.
Podział danych: 80% trening / 20% test z parametrem stratify, który
gwarantuje zachowanie proporcji klas w obu zbiorach.
Wszystkie trzy modele zbudowano w Pipeline z identycznymi krokami
przetwarzania: SimpleImputer (mediana) oraz StandardScaler. Porównanie
przeprowadzono przy użyciu StratifiedKFold (k=5) z metryką ROC-AUC.
Random Forest poddano dodatkowo strojeniu hiperparametrów przez GridSearchCV
z rozszerzoną siatką parametrów. Model końcowy wybrano na podstawie
najwyższego ROC-AUC w cross-validation i oceniono jednorazowo
na zbiorze testowym.
Wyniki
Porównanie modeli w walidacji krzyżowej:
ModelROC-AUC (CV)Logistic Regression0.8434Random Forest0.8154Random Forest (po strojeniu GridSearchCV)0.8406SVM0.8333
Najlepsze wyniki osiągnęła Logistic Regression — nawet po strojeniu
hiperparametrów Random Forest nie zdołał jej przegonić. Logistic Regression
została wybrana jako model końcowy. Wyniki na zbiorze testowym:
MetrykaWartośćAccuracy0.71ROC-AUC0.81Precision (cukrzyca)0.60Recall (cukrzyca)0.50F1-score (cukrzyca)0.55
Najważniejsze cechy według współczynników modelu: Glucose (1.18),
BMI (0.69) i liczba ciąż (0.38) — wszystkie zwiększają ryzyko cukrzycy,
co jest zgodne z wiedzą medyczną.
Technologie

Python 3
Google Colab
pandas, numpy
matplotlib, seaborn
scikit-learn

Uruchomienie
Notebook pobiera dane automatycznie z publicznego repozytorium GitHub,
więc nie wymaga ręcznego pobierania plików. Wystarczy otworzyć go
w Google Colab i uruchomić wszystkie komórki.
Literatura
Geron A., Hands-On Machine Learning with Scikit-Learn, Keras and TensorFlow, O'Reilly, 2019.
