# Klasyfikacja Obrazów MNIST i PneumoniaMNIST przy Użyciu Sieci Neuronowych

**Autor:** Marcin Przybylski
**Data:** 29 kwietnia 2025

## Opis Projektu

Niniejszy projekt koncentruje się na zastosowaniu głębokiego uczenia do problemów klasyfikacji obrazów. Analizie poddano dwa zbiory danych: MNIST, zawierający ręcznie pisane cyfry, oraz PneumoniaMNIST, składający się z obrazów rentgenowskich klatki piersiowej do diagnozy zapalenia płuc. Głównym celem było zbudowanie, wytrenowanie i porównanie dwóch architektur sieci neuronowych: w pełni połączonej sieci neuronowej (FCN) oraz konwolucyjnej sieci neuronowej (CNN). Projekt demonstruje proces przygotowania danych, treningu, oceny modeli oraz analizuje przewagi i ograniczenia każdej z architektur w kontekście zadań klasyfikacji obrazów.

## 📂 Struktura Projektu

Projekt obejmuje analizę dwóch zbiorów danych przy użyciu dwóch różnych architektur sieci neuronowych. Prace zostały zrealizowane w środowisku Google Colab.

## ⚙️ Metodologia

### 1. Wczytywanie i Przygotowanie Danych

Niezależnie dla obu zbiorów danych (MNIST i PneumoniaMNIST) przeprowadzono następujące kroki:

* **Zbiór MNIST:**
    * Wczytanie danych (60 000 obrazów treningowych, 10 000 testowych, 28x28 pikseli, skala szarości) z biblioteki Keras.
    * Podział zbioru treningowego na właściwy zbiór treningowy (85%) i walidacyjny (15%).
    * Normalizacja wartości pikseli do zakresu [0, 1].
    * Spłaszczenie obrazów ($28 \times 28$ do wektorów 784) dla FCN.
    * Zachowanie struktury 2D (28, 28, 1) dla CNN.
    * Kodowanie "one-hot" etykiet (0-9).
* **Zbiór PneumoniaMNIST:**
    * Wczytanie danych (obrazy RTG klatki piersiowej, klasy: Normal vs. Pneumonia) przy użyciu `tensorflow_datasets`.
    * Zmiana rozmiaru obrazów do $28 \times 28$ pikseli i konwersja do skali szarości.
    * Normalizacja wartości pikseli do zakresu [0, 1].
    * Spłaszczenie obrazów (28x28x1 do wektorów 784) dla FCN.
    * Zachowanie struktury 2D (28, 28, 1) dla CNN.
    * Kodowanie "one-hot" etykiet binarnych ([1, 0], [0, 1]).

### 2. Architektury Modeli

* **W Pełni Połączona Sieć Neuronowa (FCN):**
    * Warstwa wejściowa (784 neurony).
    * Dwie ukryte warstwy gęste (Dense) po 512 neuronów z aktywacją ReLU i Dropout (0.2, 0.1/0.2).
    * Warstwa wyjściowa Dense (10 neuronów dla MNIST, 2 dla PneumoniaMNIST) z aktywacją Softmax.
* **Konwolucyjna Sieć Neuronowa (CNN):**
    * Dwa bloki konwolucyjne (Conv2D 3x3 ReLU + MaxPooling 2x2; 32 filtry w bloku 1, 64 w bloku 2).
    * Dodatkowo Batch Normalization w modelu PneumoniaMNIST.
    * Część FCN (Flatten, Dense 512 ReLU, Dropout 0.2, Dense wyjściowe z Softmax).

### 3. Proces Treningu

* Kompilacja modeli z funkcją kosztu `categorical_crossentropy`, optymalizatorem `Adam` (lub `SGD` dla FCN MNIST) i metryką `accuracy`.
* Trening przez określoną liczbę epok z ustalonym `batch_size`.
* Zastosowanie augmentacji danych dla CNN PneumoniaMNIST.
* Monitorowanie postępu za pomocą `livelossplot`.

### 4. Ewaluacja Modeli

* Ocena wydajności na zbiorach testowych.
* Wykorzystane metryki: dokładność (accuracy), strata (loss).
* Analiza macierzy pomyłek i przykładów błędnych klasyfikacji.

## 🛠️ Wykorzystane Narzędzia i Technologie

* **Język programowania:** Python
* **Biblioteki:**
    * TensorFlow z Keras API (budowa i trening modeli)
    * TensorFlow Datasets (wczytanie danych PneumoniaMNIST)
    * NumPy (operacje na danych)
    * Matplotlib i Seaborn (wizualizacja)
    * Scikit-learn (podział danych, metryki)
    * Livelossplot (monitorowanie treningu)
* **Środowisko:** Google Colab

## 📊 Wyniki i Interpretacja

### Klasyfikacja MNIST

* **Model FCN:**
    * Dokładność na zbiorze testowym: 94.74%
    * Strata na zbiorze testowym: 0.1844
    * Poprawnie sklasyfikowano 9474 obrazy, błędnie 526.
* **Model CNN:**
    * Dokładność na zbiorze testowym: 99.23%
    * Strata na zbiorze testowym: 0.0225
    * Poprawnie sklasyfikowano 9923 cyfry, błędnie 77.

### Klasyfikacja PneumoniaMNIST

* **Model FCN:**
    * Dokładność na zbiorze testowym: 80.13%
    * Strata na zbiorze testowym: 0.8064
    * Poprawnie sklasyfikowano 500 obrazów, błędnie 124.
* **Model CNN:**
    * Dokładność na zbiorze testowym: 86.38%
    * Strata na zbiorze testowym: 0.4459
    * Poprawnie sklasyfikowano 539 przypadków, myląc się w 85.

### Ogólna Interpretacja

Wyniki jednoznacznie potwierdzają, że architektury CNN są znacznie lepiej przystosowane do zadań klasyfikacji obrazów niż standardowe sieci FCN. Wykorzystanie cech przestrzennych przez warstwy konwolucyjne i pooling prowadzi do wyższej dokładności, szczególnie na bardziej złożonym zbiorze PneumoniaMNIST. Modele często mylą klasy wizualnie podobne lub przypadki graniczne.

## 🏁 Podsumowanie i Wnioski

1.  **Skuteczność CNN:** Konwolucyjne sieci neuronowe wykazały znaczną przewagę nad w pełni połączonymi sieciami w obu zadaniach klasyfikacji obrazów.
2.  **Znaczenie Przetwarzania Danych:** Odpowiednie przygotowanie danych (normalizacja, formatowanie, kodowanie etykiet) jest kluczowe dla sukcesu modeli. Augmentacja danych może poprawić generalizację.
3.  **Interpretowalność Modeli:** Analiza metryk, macierzy pomyłek i przykładów błędów dostarcza cennych informacji o wydajności i ograniczeniach modeli.
4.  **Praktyczne Zastosowanie Narzędzi:** Projekt zademonstrował praktyczne wykorzystanie popularnych bibliotek do realizacji zadań uczenia maszynowego.

## 💡 Odpowiedzi na Kluczowe Pytania (FAQ)

* **Czym jest zbiór MNIST?** Standardowa baza danych ręcznie pisanych cyfr (0-9), 28x28 pikseli, używana do testowania algorytmów rozpoznawania obrazów.
* **Co to jest format One-Hot?** Metoda kodowania kategorii jako wektorów binarnych, gdzie tylko jedna pozycja ma wartość 1. Np. cyfra '3' to `[0,0,0,1,0,0,0,0,0,0]`.
* **Który neuron odpowiada za cyfrę '0' w modelu MNIST?** W 10-klasowym modelu z kodowaniem one-hot, neuron na pierwszej pozycji (indeks 0) odpowiada cyfrze '0'.
* **Różnice między funkcjami kosztu Categorical Cross-Entropy a Binary Cross-Entropy:** Categorical jest używana dla wielu klas (>2) z etykietami one-hot. Binary jest dla dwóch klas, często z pojedynczym neuronem wyjściowym (sigmoid).
* **Wpływ GPU na przyspieszenie treningu:** Użycie GPU znacząco przyspiesza trening (zwłaszcza CNN), często kilkukrotnie lub kilkudziesięciokrotnie w porównaniu do CPU.
* **Czy sieci neuronowe popełniają błędy podobne do ludzkich?** Tak, sieci (szczególnie FCN) mogą mylić wizualnie podobne cyfry (np. 1 i 7), co widać w macierzach pomyłek.
* **Wpływ rozmiaru Batch Size na trening:**
    * **Duży (np. 10000):** Skraca czas epoki, ale może pogorszyć generalizację.
    * **Mały (np. 32):** Wydłuża czas epoki, ale często poprawia generalizację.
* **Porównanie FCN vs CNN dla rozpoznawania cyfr:** CNN są znacznie lepsze, ponieważ wykorzystują informacje o przestrzennej strukturze pikseli.

---
