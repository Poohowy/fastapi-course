# FastAPI – Status codes w endpointach GET



Notatka z lekcji Udemy

FastAPI



## 1. Domyślny status code dla GET

W FastAPI każde zapytanie HTTP typu `GET` zwraca domyślnie status 200 OK, jeżeli operacja zakończy się powodzeniem.

Możemy jednak jawnie określić oczekiwany status odpowiedzi, korzystając z parametru `status_code` w dekoratorze endpointu.

```
@app.get("/", status_code=200)
def root():
    return {"message": "Welcome to Rent Room"}
```

- `@app.get()` – definiuje endpoint obsługujący metodę GET.
- `status_code=200` – określa status HTTP zwracany w przypadku powodzenia operacji.

Ważne: Jawne podanie `status_code=200` nie jest konieczne, ponieważ FastAPI ustawia ten status automatycznie dla endpointów GET. Jest to jednak sposób na zwiększenie czytelności kodu.

## 2. Korzystanie z modułu `status`

Zamiast wpisywać numery statusów HTTP bezpośrednio w kodzie, możemy skorzystać z gotowych stałych dostępnych w module `status`.

Import:

```
from fastapi import status
```

Następnie zamiast:

```
@app.get("/", status_code=200)
```

możemy napisać:

```
@app.get("/", status_code=status.HTTP_200_OK)
```

Oba zapisy oznaczają dokładnie to samo.

Dlaczego warto korzystać z `status`?

- Kod jest bardziej czytelny.
- Nazwa stałej wyjaśnia znaczenie konkretnego statusu.
- Nie musimy zapamiętywać wszystkich numerów statusów HTTP.
- Zmniejszamy ryzyko pomyłek przy wpisywaniu kodów.

Moduł `status` pochodzi z biblioteki Starlette, która jest jedną z podstawowych zależności FastAPI.

Przykładowe stałe:

| Stała                                   | Znaczenie                                             |
| --------------------------------------- | ----------------------------------------------------- |
| `status.HTTP_200_OK`                    | Żądanie zakończone powodzeniem                        |
| `status.HTTP_201_CREATED`               | Zasób został utworzony                                |
| `status.HTTP_204_NO_CONTENT`            | Żądanie zakończone powodzeniem, bez treści odpowiedzi |
| `status.HTTP_400_BAD_REQUEST`           | Nieprawidłowe żądanie                                 |
| `status.HTTP_404_NOT_FOUND`             | Nie znaleziono zasobu                                 |
| `status.HTTP_500_INTERNAL_SERVER_ERROR` | Wewnętrzny błąd serwera                               |

## 3. Obsługa błędów i słowo kluczowe `raise`

W Pythonie możemy celowo wywołać wyjątek za pomocą słowa kluczowego `raise`.

Przykład:

```
@app.get("/")def root():    raise ValueError()
```

Co się tutaj dzieje?

1. Wywołujemy endpoint GET.
2. Funkcja `root()` rozpoczyna wykonywanie.
3. Instrukcja `raise` celowo zgłasza wyjątek `ValueError`.
4. Wykonanie funkcji zostaje przerwane.
5. FastAPI zwraca odpowiedź HTTP `500 Internal Server Error`.

500 Internal Server Error

Oznacza to, że wystąpił nieobsłużony błąd po stronie serwera.

W rzeczywistych aplikacjach może to być spowodowane np.:

- błędem logicznym w kodzie,
- odwołaniem do nieistniejącej zmiennej,
- literówką,
- nieoczekiwanym błędem podczas wykonywania operacji.

Uwaga: `ValueError` jest tutaj przykładem celowo wywołanego wyjątku. Nie oznacza to, że każdy `ValueError` powinien kończyć się statusem 500. Właściwa obsługa wyjątków pozwala zwracać odpowiednie kody HTTP.

## 4. Rola parametru `status_code`

To najważniejsza rzecz do zapamiętania z tej lekcji.

Parametr `status_code` w dekoratorze endpointu określa status HTTP zwracany w przypadku pomyślnego wykonania operacji.

Przykładowo:

```
@app.get("/", status_code=status.HTTP_200_OK)
def root():
    return {"message": "Welcome to Rent Room"}
```

Nie oznacza to, że endpoint zawsze zwróci 200.

Jeżeli podczas wykonywania funkcji wystąpi nieobsłużony wyjątek, FastAPI może zwrócić np. 500.

Nie musimy więc definiować wszystkich możliwych błędów za pomocą parametru `status_code`.

## 5. Dokumentacja Swagger

FastAPI automatycznie generuje dokumentację API na podstawie definicji endpointów.

Informacje o oczekiwanym statusie odpowiedzi również trafiają do wygenerowanej dokumentacji Swagger.

Dzięki temu osoba korzystająca z API może zobaczyć, jaki status HTTP jest przewidziany dla poprawnego wykonania danego endpointu.

## Najważniejsze rzeczy do zapamiętania

1. GET domyślnie zwraca 200 OK – nie musimy jawnie ustawiać tego statusu.
2. `status_code` – pozwala określić oczekiwany status odpowiedzi w przypadku sukcesu.
3. `from fastapi import status` – importuje moduł zawierający gotowe stałe statusów HTTP.
4. `status.HTTP_200_OK` – czytelniejsza alternatywa dla wpisania liczby `200`.
5. `raise` – służy do ręcznego zgłaszania wyjątków w Pythonie.
6. 500 Internal Server Error – oznacza nieobsłużony błąd po stronie serwera.
7. Swagger – automatycznie uwzględnia informacje o statusach odpowiedzi.
Moja wskazówka: Szczególnie zapamiętaj różnicę między `status_code` a `raise`. Pierwszy określa oczekiwany status odpowiedzi przy sukcesie, a drugi służy do zgłaszania wyjątków. To dwa różne mechanizmy, które będą często pojawiać się podczas budowania API w FastAPI.
