# Przepływ od dodania produktu do złożenia zamówienia

```mermaid
flowchart TD
    A[Klient wybiera produkt] --> B[POST /api/cart/items<br/>productId, quantity]
    B --> C{Produkt istnieje?}
    C -- Nie --> C1[Błąd 404<br/>Produkt nie istnieje]
    C -- Tak --> D{Ilość 1–10<br/>i nie większa niż stan?}
    D -- Nie --> D1[Błąd walidacji lub OUT_OF_STOCK]
    D -- Tak --> E[Produkt dodany do koszyka]

    E --> F{Dodać kolejny produkt?}
    F -- Tak --> B
    F -- Nie --> G[GET /api/cart<br/>Podgląd pozycji i summary]

    G --> H{Zastosować kod rabatowy?}
    H -- Tak --> I[POST /api/cart/discount]
    I --> J{Kod poprawny<br/>i spełnia warunki?}
    J -- Nie --> J1[Błąd 422<br/>kod nieznany, wygasły<br/>lub za niska wartość]
    J -- Tak --> K[Summary przeliczone po rabacie]
    J1 --> H
    H -- Nie --> K

    K --> L[PUT /api/cart/shipping<br/>STANDARD lub EXPRESS]
    L --> M[Summary przeliczone<br/>z kosztem dostawy]
    M --> N{Koszyk nie jest pusty<br/>i produkty są dostępne?}
    N -- Nie --> N1[Błąd 400 EMPTY_CART<br/>lub 409 OUT_OF_STOCK]
    N -- Tak --> O[POST /api/orders]
    O --> P[Zamówienie utworzone<br/>status NEW]
    P --> Q[Towar zdjęty ze stanu]
    Q --> R[Koszyk wyczyszczony]
```

## Źródła

- `docs/architektura.md`: opis przepływu składania zamówienia.
- `docs/api.md`: endpointy koszyka, rabatu, dostawy i zamówień.
- `src/routes/cart.ts`: walidacja pozycji, rabatu i dostawy.
- `src/routes/orders.ts`, funkcja obsługi `POST /api/orders`: kontrola pustego koszyka i stanu magazynowego oraz utworzenie zamówienia.
- `docs/wymagania.md`: BR-03 (ilość i dostępność), BR-04 (dostawa), BR-05–BR-08 (rabaty i kwoty).

**MOŻLIWY BŁĄD (BR-03):** wymaganie mówi o ilości od 1 do 10 sztuk. W `src/routes/cart.ts`, w schemacie `updateItem`, ustawiono tylko limit maksymalny (`max(10)`), bez minimum `1`; diagram pokazuje zachowanie wymagane biznesowo.
