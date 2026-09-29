# Pogoda

ApexGPS może pokazać aktualne warunki, prognozę godzinową i 7-dniową dla dowolnego punktu, który Cię interesuje.

## Co zobaczysz

Pogoda jest **domyślnie włączona** od wersji 1.32.2. Gdy tylko masz pozycję GPS:

- Mały chip pojawia się nad paskiem statystyk i pokazuje warunki w Twojej aktualnej pozycji.
- Stuknięcie punktu pokazuje wiersz „Pogoda tutaj" w panelu punktu.
- Stuknięcie któregokolwiek otwiera arkusz z pełnym rozkładem.

Gdy pogoda jest włączona, Twoja szerokość/długość jest wysyłana do Open-Meteo (darmowe publiczne API pogodowe) przy każdym zapytaniu. Jeśli wolisz, by aplikacja nie wykonywała połączeń sieciowych, otwórz **Ustawienia → Pogoda** i wyłącz **Pokaż pogodę** — chip i wiersz „Pogoda tutaj" znikną, a ApexGPS przestanie się łączyć z Open-Meteo.

## Co pokazuje chip

Chip rotuje pomiędzy kilkoma stanami:

| Stan | Wygląda jak | Znaczenie |
|---|---|---|
| Świeży | ikona pogody, a po niej `24° · 12 km/h NE` | Aktualizacja w ciągu ostatnich 15 minut. |
| Starzejący się | `… · 32 min temu` | Ponad 15 minut, ale prawdopodobnie nadal trafny. |
| Przestarzały / offline | `⚠ … · 2h temu` (szary) | Ponad godzina lub brak połączenia. Stuknij chip, aby ręcznie odświeżyć. |

Chip nie znika, gdy pogoda jest włączona — zmienia tylko wygląd, by pokazać, jak wiarygodne są dane.

## Arkusz prognozy

Stuknij chip (lub wiersz „Pogoda tutaj" przy waypoincie), by otworzyć pełny arkusz. Z góry na dół:

- **Teraz** — duża ikona pogody + temperatura, „odczuwalna" oraz po jednym wierszu na wiatr / wilgotność + maks. opady / punkt rosy + UV / ciśnienie + zachód.
- **Pasek prognozy z przełącznikiem 24h / 8h / 2h** — osiem ikon (wariant dzienny lub nocny zależnie od lokalnego wschodu / zachodu dla danego kroku) z temperaturą, a pod spodem **prawdopodobieństwo opadów**. Wartość procentowa pojawia się dopiero od 20 %, aby sucha prognoza pozostała czytelna. Mały przełącznik nad paskiem zmienia zakres:
  - **24h** — cały dzień na jeden rzut oka, jedna komórka co 3 godziny.
  - **8h** — najbliższe osiem godzin, jedna komórka na godzinę.
  - **2h** — najbliższe dwie godziny w krokach 15-minutowych, aby wychwycić nadchodzący przelotny deszcz lub burzę (nawet ~45 minut wcześniej niż widok godzinowy). Opcja **2h** pojawia się tam, gdzie dostępne są dane 15-minutowe (większa część Europy i Ameryki Północnej); gdzie indziej zobaczysz tylko **24h** i **8h**.
- **Trend ciśnienia** (zielony) — wykres liniowy 24-godzinny, przydatny do wykrywania zbliżającego się frontu.
- **Trend wilgotności** (jasnoniebieski) — wykres liniowy 24-godzinny.
- **Następne 7 dni** — rząd ikon dni tygodnia z maksymalną / minimalną temperaturą oraz prawdopodobieństwem opadów, gdy wynosi ono 20 % lub więcej.

W nagłówku arkusza jest przycisk odświeżania (↻), który pomija cache i pobiera świeże dane. Offline poprzednie dane pozostają na ekranie, a wskaźnik „przestarzałe" chipa pozostaje widoczny.

## Prognozy świadome wysokości na szczytach

Jeśli waypoint ma zapisaną wysokość (wpisaną ręcznie, ustawioną z GPS lub zaimportowaną z GPX z `<ele>`), ta wysokość jest wysyłana do Open-Meteo, by model wiedział, że to szczyt, a nie punkt doliny przy tych samych współrzędnych. To samo dla chipa: używa Twojej wysokości GPS. Na szczycie 1500 m zwykle koryguje to prognozowaną temperaturę o ~9 °C w porównaniu z zapytaniem tylko po współrzędnych.

## Auto-odświeżanie

Gdy pogoda jest włączona, chip aktualizuje się co 15 minut, dopóki aplikacja jest otwarta i online. W trybie samolotowym chip zachowuje ostatnie znane dane i wskaźnik „przestarzałe", aż połączysz się ponownie.

## Jak czytać ikony

Symbole pogody rysuje sam ApexGPS, więc wyglądają **dokładnie tak samo na każdym telefonie**. Znaczenie niesie kolor:

| Kolor | Znaczenie |
|---|---|
| Niebieski | Opady — mżawka, deszcz, deszcz ze śniegiem lub śnieg |
| Bursztynowy | Czyste niebo (słońce; nocą czyste niebo pokazuje zwykły szary księżyc) |
| Szary | Chmury lub mgła |
| Czerwony | Burza |

Szczegółowość ikon odpowiada prognozie: słaby deszcz, deszcz i silny deszcz to trzy różne symbole, podobnie śnieg,
silny śnieg i deszcz ze śniegiem. Ponieważ prawdopodobieństwo opadów jest też podane liczbą, nigdy nie musisz polegać
wyłącznie na odczytaniu małego symbolu.

Do wersji 1.47.1 symbole te były emoji dostarczanymi przez sam telefon, przez co dwa telefony mogły pokazywać różne
obrazki — albo żaden — dla tej samej prognozy. Naprawione od 1.48.0.

## Źródła danych

Prognozy pochodzą z **[Open-Meteo](https://open-meteo.com)**, darmowego publicznego API łączącego ECMWF, GFS, ICON i inne globalne modele najwyższej klasy. Darmowe dla użytku osobistego, bez konta, bez klucza API.

## Ograniczenia

- **Konwekcyjny deszcz w regionach suchych** jest trudny do prognozowania dla każdego modelu. Spodziewaj się okazjonalnych pomyłek przy nagłych burzach w miejscach takich jak góry Hajar w ZEA. Liczba prawdopodobieństwa jest co do tego szczera, ale lokalne zjawiska mogą wypaść poza siatkę.
- **Ostrzeżenia o zjawiskach groźnych** nie są częścią tej funkcji. ApexGPS nie wysyła powiadomień push przy zbliżających się burzach.
- **Pogoda wzdłuż trasy** nie jest wspierana — prognozy są punktowe, a nie „jaka będzie pogoda na tym szlaku za 2 godziny". Możesz stuknąć poszczególne waypointy wzdłuż trasy, by sprawdzać punktowo.
