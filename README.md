# Triki Wand: silnik sterowania kursorem

## Cel
Aplikacja webowa i silnik, który zamienia ruch kontrolera Triki (Żabka) w precyzyjne sterowanie kursorem. Kursor ma reagować na kierunek wskazywania kapslem (tryb różdżki), a nie na samo obracanie go wokół osi, jak w obecnej myszce"https://github.com/kub0vvik/TrikiAirMouse". Wykorzystujemy wszystkie dostępne dane: żyroskop, akcelerometr i przycisk.

## Założenia
- **Dane:** czujnik LSM6DSL, ramki co ok. 104 Hz: gyro X/Y/Z, accel X/Y/Z, plus stan przycisku. Czas liczymy z odbioru ramek.
- **Chwyt:** między kciukiem a palcem wskazującym, jak pilot. Kciuk naciska przycisk.
- **Orientacja:** filtr łączący żyroskop z akcelerometrem. Grawitacja pokazuje "dół", więc kapsel działa tą samą stroną zieloną i srebrną (do sprawdzenia).
- **Kalibracja:** pomiar szumu w bezruchu, kilka machnięć w lewo/prawo i góra/dół (silnik sam znajduje osie), wskazanie środka ekranu i opcjonalnie rogów.
- **Mapowanie:** podstawa to ruch względny z żyroskopu (odporny na dryf), z miękkim dociąganiem do absolutnego kierunku. Dochodząc do krawędzi, silnik przesuwa punkt zerowy razem z ręką.
- **Obróbka:** adaptacyjne wygładzanie (mocne przy drżeniu, słabe przy szybkim ruchu), miękki próg, krzywa przyspieszenia.
- **Klik:** bufor ok. 150 ms ruchu, po wykryciu naciśnięcia kursor wraca do pozycji sprzed niego. Sprzęgło ("podniesienie myszki") do wstrzymywania wskazywania.
- **Miejsce na rozszerzenia:** silnik ma od początku osobne wejście na dodatkowe zdarzenia z akcelerometru (pukanie, wstrząsy, szarpnięcia, wykrywanie leżenia na stole). Ich późniejsze dodanie nie może wymagać przebudowy rdzenia.

## Tryby (kolejność)
1. Różdżka (domyślny, budujemy pierwszy).
2. Stół: mysz inercyjna z zerowaniem prędkości w bezruchu.
3. Gesty ręki: pochylenia i szarpnięcia względem pozycji neutralnej.

## Proces
1. Wizualizator i nagrywanie surowych 6 kanałów.
2. Eksperymenty: czy znak osi odróżnia zieloną i srebrną stronę, dryf w bezruchu, przycinanie żyroskopu przy szybkich ruchach, wpływ naciśnięcia przycisku na ruch.
3. Silnik różdżki: kalibracja, mapowanie, obróbka ruchu.
4. Klik z cofaniem i sprzęgło.
5. Dostrajanie na nagraniach, odtwarzanych offline z różnymi ustawieniami.
6. Później: pukanie, wstrząsy, tryby stołu i gestów.

## Testy
- Cele na ekranie o malejącej wielkości i kreślenie okręgu.
- Bezruch przez minutę (dryf i drgania kursora).
- Mierzymy: odsetek trafień, czas dojścia do celu, błąd okręgu, ucieczkę kursora w spoczynku.
- Poprzeczkę (np. trafianie w przycisk o zwykłej wielkości) ustalamy po pierwszych pomiarach.

## Otwarte pytania
- Czy znak jednej osi akcelerometru odróżnia obie strony kapsla?
- Co zmienia ostatni bajt komendy startowej (`01` w jednym projekcie, `03` w drugim)?
- Czy puknięcie jest widoczne przy 104 Hz?
- Jak często szybki ruch przekracza zakres żyroskopu (±250°/s)?