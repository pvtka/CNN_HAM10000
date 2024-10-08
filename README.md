# Analiza Danych HAM10000

## Opis projektu
Celem niniejszej pracy jest zbadanie możliwości zastosowania uczenia maszynowego, a w szczególności konwolucyjnych sieci neuronowych, w diagnozowaniu zmian skórnych na podstawie zbioru danych "Human Against Machine with 10000 training images" (HAM10000), który został opublikowany na platformie Harvard Dataverse. Zbiór ten zawiera obrazy zmian skórnych, takich jak znamiona, czerniaki oraz inne typy nowotworów skóry, które zostały dokładnie opisane i sklasyfikowane przez specjalistów. 

## Problem badawczy
Problem badawczy koncentruje się na ocenie skuteczności algorytmów uczenia maszynowego, ze szczególnym uwzględnieniem konwolucyjnych sieci neuronowych, w automatycznym rozpoznawaniu i klasyfikacji nowotworowych zmian skórnych na podstawie obrazów dermoskopowych. Zagadnienie dotyczy tego, w jakim stopniu algorytmy te mogą wspierać lub zastępować tradycyjne metody diagnostyczne w rozpoznawaniu zmian skórnych, szczególnie tych o charakterze nowotworowym. Istotnym wyzwaniem jest osiągnięcie wysokiej dokładności predykcji oraz zapewnienie, że modele będą skuteczne w różnych warunkach klinicznych, uwzględniając takie parametry jak precyzja, czułość, specyficzność oraz zdolność do generalizacji na nowych, niewidzianych wcześniej danych.

## Funkcje projektu

### 1. **Wczytywanie i wstępna obróbka danych**
   - **Wczytywanie danych**: Projekt wczytuje dane z pliku CSV, który zawiera metadane dotyczące obrazów dermatologicznych.
   - **Czyszczenie danych**: Usuwanie wierszy z brakującymi wartościami oraz duplikatami w kolumnach `image_id` i `dx`.

### 2. **Analiza danych**
   - **Statystyki opisowe**: Generowanie statystyk opisowych dla zmiennych numerycznych oraz rozkładów dla zmiennych kategorycznych.
   - **Wizualizacja**: Wykresy przedstawiające rozkłady zmiennych, takich jak wiek, płeć, diagnoza i lokalizacja zmian.
   - **Histogramy**: Analiza histogramów intensywności kolorów obrazów oraz ich średnich wartości.

### 3. **Wykrywanie i usuwanie duplikatów**
   - Sprawdzanie duplikatów obrazów na podstawie hashu i usuwanie ich, aby zapewnić unikalność danych.

### 4. **Testy statystyczne**
   - **Test Shapiro-Wilka**: Ocena normalności rozkładu zmiennej `age`.
   - **Test Kolmogorova-Smirnova**: Ocena, czy wiek pacjentów różni się w zależności od diagnozy.
   - **Test Kruskala-Wallisa**: Porównanie rozkładów wieku między różnymi diagnozami.
   - **Test chi-kwadrat**: Analiza zależności między płcią, lokalizacją a diagnozą.

### 5. **Przetwarzanie obrazów**
   - **Zmiana rozmiaru obrazów**: Przetwarzanie obrazów poprzez zmianę ich rozmiaru do 128x128 pikseli.
   - **Normalizacja**: Normalizacja obrazów, aby wartości pikseli mieściły się w zakresie [0, 1].

### 6. **Modelowanie**
   - **Tworzenie modelu**: Definiowanie architektury modelu CNN (Convolutional Neural Network) do klasyfikacji obrazów na podstawie metadanych i obrazów.
   - **Wizualizacja wyników**: Wyświetlanie wyników klasyfikacji oraz wizualizacja skuteczności modelu.

## Ocena Skuteczności Modelu

Ostateczny model oparty na architekturze ResNet50 został oceniony na zbiorze testowym, obejmującym 1952 obrazy dermatoskopowe. Poniżej przedstawiono kluczowe miary efektywności modelu:

- **Dokładność (accuracy)**: 82%
- **Średnia ważona precyzja**: 82%
- **Średnia ważona czułość (recall)**: 82%
- **Średnia ważona miara F1**: 81%

Model osiągnął różne wyniki w zależności od klasy zmiany skórnej:

| Klasa                                          | Precyzja | Czułość  | F1-score | Liczba próbek |
|------------------------------------------------|----------|----------|----------|--------------|
| Znamię melanocytowe                            | 0.71     | 0.39     | 0.50     | 220          |
| Czerniak                                       | 0.56     | 0.75     | 0.64     | 102          |
| Łagodne zmiany przypominające keratozę         | 0.73     | 0.33     | 0.46     | 66           |
| Rak podstawnokomórkowy                         | 0.79     | 0.48     | 0.59     | 23           |
| Rogowacenie słoneczne i rak śródnabłonkowy     | 0.62     | 0.86     | 0.72     | 28           |
| Zmiany naczyniowe                              | 0.90     | 0.94     | 0.92     | 1298         |
| Włókniak twardy                                | 0.60     | 0.71     | 0.65     | 215          |

Najlepsze wyniki model osiągnął dla **zmian naczyniowych**, gdzie precyzja wyniosła 0.90, a czułość 0.94. Dla innych klas, takich jak **czerniak**, model wykazywał dobrą czułość (0.75), co oznacza, że skutecznie wykrywał prawdziwie pozytywne przypadki, jednak miał trudności z unikaniem fałszywych pozytywów (precyzja 0.56).

Model miał najwięcej trudności z klasyfikacją **znamion melanocytowych** oraz **łagodnych zmian przypominających keratozę**, co mogło być spowodowane niewielką liczbą próbek w tych klasach oraz ich podobieństwem do innych zmian skórnych.

## Wnioski i ograniczenia

- Model osiągnął ogólną dokładność 82%, co jest obiecującym wynikiem w klasyfikacji wieloklasowej zmian skórnych.
- Nierównomierność liczby przykładów w klasach (np. duża liczba przypadków zmian naczyniowych) wpłynęła na zdolność modelu do skutecznej klasyfikacji rzadziej występujących przypadków, takich jak **rak podstawnokomórkowy** czy **rogowacenie słoneczne**.
- Pomimo zastosowania technik regularyzacji, takich jak **Dropout**, oraz wstępnie wytrenowanych wag z **ImageNet**, model nadal wykazywał pewne trudności w klasyfikacji bardziej skomplikowanych i rzadszych przypadków.
 
## Źródło danych
Dane pochodzą z zestawu [HAM10000](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DBW86T), który zawiera metadane dotyczące zmian skórnych oraz odpowiadające im obrazy.

## Technologie
- **Python**: Główny język programowania użyty do analizy danych.
- **Jupyter Notebook**: Środowisko używane do rozwijania i uruchamiania kodu.
- **Biblioteki Pythona**:
  - `pandas`: Do manipulacji i analizy danych.
  - `numpy`: Do obliczeń numerycznych.
  - `matplotlib`: Do wizualizacji danych.
  - `scipy`: Do przeprowadzania testów statystycznych.
  - `PIL (Pillow)`: Do przetwarzania obrazów.
  - `seaborn`: Rozszerzenie `matplotlib` ułatwiające tworzenie estetycznych wizualizacji statystycznych.
  - `scikit-learn`: Do uczenia maszynowego, oferująca szeroki zestaw algorytmów do klasyfikacji, regresji i klasteryzacji.
  - `tensorflow`: Do tworzenia i trenowania modeli głębokiego uczenia, z naciskiem na sieci neuronowe.
  - `keras`: Wysokopoziomowa biblioteka do tworzenia modeli głębokiego uczenia, zbudowana na bazie `tensorflow`, ułatwiająca budowę i trenowanie sieci neuronowych.
  - `random`: Generowanie losowych liczb i elementów
  - `glob`: Wyszukiwanie plików.
  - `os`: Interakcja z systemem operacyjnym.





