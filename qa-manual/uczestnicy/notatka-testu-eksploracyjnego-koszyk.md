# Notatka z testu eksploracyjnego: koszyk, rabat i dostawa

## Charter sesji

**Eksploruj** koszyk zalogowanego klienta  
**używając** produktu `Espresso Blend 500 g`, kodu `KAWA10` i dostawy standardowej  
**aby odkryć** czy zmiana ilości, zastosowanie rabatu, wybór dostawy i podsumowanie zamówienia są zrozumiałe oraz poprawnie przeliczane.

- Data obserwacji: 03.10.2026
- Źródło obserwacji: pierwszy dostarczony zrzut ekranu
- Drugi zrzut nie był dostępny do odczytu w tej sesji, dlatego notatka nie opisuje działań, których nie widać na pierwszym ekranie.

## Działania widoczne na ekranie

1. Zalogowany klient `Anna Nowak` otworzył koszyk.
2. W koszyku znajduje się jedna sztuka produktu **Espresso Blend 500 g** w cenie **54,99 zł**.
3. Ilość produktu pozostawiono na poziomie **1**.
4. W polu kodu rabatowego użyto kodu `KAWA10` i wyświetlono komunikat **„Kod został zastosowany”**.
5. Wybrano dostawę **Kurier standard**.
6. Przedstawiono podsumowanie:
   - wartość produktów: **54,99 zł**,
   - rabat `KAWA10`: **-5,50 zł**,
   - dostawa: **14,99 zł**,
   - kwota do zapłaty: **64,48 zł**.
7. Na ekranie są również dostępne akcje **Usuń**, **Usuń kod** i **Złóż zamówienie**, ale na zrzucie nie widać ich użycia.

## Obserwacje i ocena

| Obszar | Obserwacja | Ocena |
|---|---|---|
| Ilość produktu | Pole ilości pokazuje `1`, czyli poprawną minimalną ilość. | Zgodne z BR-03 dla zaobserwowanej wartości |
| Kod rabatowy | `KAWA10` obniża 54,99 zł o 5,50 zł. | Zgodne z BR-06 |
| Dostawa | Dla wartości po rabacie 49,49 zł dostawa standardowa kosztuje 14,99 zł. | Zgodne z BR-04 |
| Suma | 54,99 - 5,50 + 14,99 = 64,48 zł. | Zgodne z BR-08 |
| Informacja zwrotna | Komunikat potwierdza zastosowanie kodu. | Pozytywna obserwacja |

## Zakres niepokryty przez zrzut

- Nie sprawdzono zmiany ilości, usunięcia produktu ani usunięcia kodu.
- Nie sprawdzono kodu wygasłego, kodu z warunkiem minimalnej kwoty ani zastępowania aktywnego kodu innym kodem.
- Nie sprawdzono dostawy express ani progu darmowej dostawy **200,00 zł**.
- Nie widać rezultatu kliknięcia **„Złóż zamówienie”**, więc nie potwierdzono utworzenia zamówienia.
- Nie oceniono dostępności formularza dla czytnika ekranu na podstawie samego obrazu.

## Wniosek

Na podstawie dostępnego zrzutu ścieżka prezentuje prawidłowe przeliczenie koszyka z kodem `KAWA10` i dostawą standardową. Jest to obserwacja jednego scenariusza; do pełnej oceny koszyka trzeba wykonać przypadki graniczne i negatywne wymienione powyżej.

## Źródła

- `docs/wymagania.md`: BR-03, BR-04, BR-05, BR-06, BR-08.
- `src/store.ts`: produkt `Espresso Blend 500 g` i cena 54,99 zł.
- `src/domain/pricing.ts`: `shippingCost()` i `priceCart()`.
- `src/routes/cart.ts`: obsługa kodu rabatowego i wyboru dostawy.
- Pierwszy dostarczony zrzut ekranu koszyka.
