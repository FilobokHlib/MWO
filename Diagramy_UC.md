## Diagramy przypadków użycia

### Wybór języka

```mermaid
flowchart TD
    N[Uzytkownik] --- A[Wybór języka]

    A --> |Include| B[Ustawienie domyślnego języka]
    A --> |Include| C[Anulowanie transakcji]

    %% Relacja Extend (opcjonalna)
    A -.->|Extend| F[Wyświetlenie listy popularnych języków]
```


### Szybki wybór rodzaju biletu

```mermaid
flowchart TD
    N[Uzytkownik] --- A[Szybki wybór rodzaju biletu]

    A --> |Include| B[Weryfikacja dostępnych biletów]
    A --> |Include| C[Anulowanie transakcji]

    %% Relacja Extend (opcjonalna)
    A -.->|Extend| F[ Wyświetlenie podpowiedzi ]
```
### Zarządzanie taryfami biletowymi

```mermaid
flowchart TD
    N[Administrator systemu] 
    A[Zarządzanie taryfami biletowymi]
    B[Zapis taryf]
    C[Powiadomienie o błędach taryf]

    N --- A
    A -->|Include| B
    A -.->|Extend| C
```

### Zarządzanie dostępnością biletów
```mermaid
flowchart TD
    N[Administrator systemu]
    A[Zarządzanie dostępnością biletów]
    B[Synchronizacja danych]
    C[Powiadomienie o problemach synchronizacji]

    N --- A
    A -->|Include| B
    A -.->|Extend| C
```

## Diagramy sekwencji
### Scenariusz
Szybki wybór rodzaju biletu Opis krokowy: 
1. Użytkownik podchodzi do biletomatu (Rozpoczęcie interakcji).
2. Użytkownik wybiera kategorię biletu (np. jednorazowe, okresowe) (Wybór kategorii).
3. Użytkownik wybiera konkretny bilet z dostępnej listy (Wybór biletu).
4. System wyświetla podsumowanie wyboru (Wyświetlenie podsumowania).
5. Użytkownik przechodzi do realizacji transakcji (Potwierdzenie wyboru).
6. Użytkownik w dowolnym momencie może anulować proces (Anulowanie transakcji).
Scenariusz alternatywny 1:
Płatność za bilet Opis krokowy:
1. Użytkownik wybiera metodę płatności (karta, gotówka, telefon) (Wybór metody płatności).
2. System weryfikuje dostępność wybranej metody (Weryfikacja metody płatności).
3. Użytkownik dokonuje płatności (np. wprowadza kartę, gotówkę, korzysta z NFC) (Realizacja płatności).
4. System potwierdza zakończenie transakcji (Potwierdzenie transakcji).
5. Użytkownik w dowolnym momencie może anulować proces (Anulowanie transakcji).
Scenariusz alternatywny 2:
Anulowanie transakcji Opis krokowy:
1. Użytkownik rozpoczyna proces zakupu biletu (Rozpoczęcie interakcji).
2. W dowolnym momencie użytkownik wybiera opcję "Anuluj" (Wybranie opcji anulowania).
3. System wyświetla komunikat potwierdzający anulowanie transakcji (Komunikat o anulowaniu).
4. System resetuje interfejs do ekranu głównego (Reset interfejsu).

### Szybki wybór rodzaju biletu

```mermaid
sequenceDiagram
    actor Użytkownik
    participant Biletomat as System Biletomatu

    %% Scenariusz główny – Szybki wybór rodzaju biletu
    Użytkownik ->> Biletomat: Rozpoczęcie interakcji
    Użytkownik ->> Biletomat: Wybór kategorii biletu
    Biletomat -->> Użytkownik: Wyświetlenie listy biletów
    Użytkownik ->> Biletomat: Wybór konkretnego biletu
    Biletomat -->> Użytkownik: Wyświetlenie podsumowania
    Użytkownik ->> Biletomat: Potwierdzenie wyboru

    alt Scenariusz alternatywny 1: Płatność za bilet
        Użytkownik ->> Biletomat: Wybór metody płatności
        Biletomat ->> Biletomat: Weryfikacja metody płatności
        Użytkownik ->> Biletomat: Realizacja płatności
        Biletomat -->> Użytkownik: Potwierdzenie transakcji
    else Scenariusz alternatywny 2: Anulowanie transakcji
        Użytkownik ->> Biletomat: Wybranie opcji "Anuluj"
        Biletomat -->> Użytkownik: Komunikat o anulowaniu
        Biletomat ->> Biletomat: Reset interfejsu do ekranu głównego
    end

    opt Anulowanie w dowolnym momencie
        Użytkownik ->> Biletomat: Anuluj
        Biletomat -->> Użytkownik: Proces anulowany
        Biletomat ->> Biletomat: Reset interfejsu
    end
```

### Scenariusz
Wybór języka
Opis krokowy:
1. Użytkownik uruchamia biletomat (Rozpoczęcie interakcji).
2. System wyświetla ekran powitalny z opcjami wyboru języka (Wyświetlenie opcji
języka).
3. Użytkownik wybiera preferowany język (Wybór języka).
4. System dostosowuje interfejs do wybranego języka (Dostosowanie interfejsu).
5. Użytkownik w dowolnym momencie może anulować proces (Anulowanie
transakcji).
Scenariusz alternatywny:
Anulowanie transakcji
Opis krokowy:
1. Użytkownik rozpoczyna proces zakupu biletu (Rozpoczęcie interakcji).
2. W dowolnym momencie użytkownik wybiera opcję "Anuluj" (Wybranie opcji
anulowania).
3. System wyświetla komunikat potwierdzający anulowanie transakcji (Komunikat
o anulowaniu).
4. System resetuje interfejs do ekranu głównego (Reset interfejsu)

### Wybór języka
```mermaid
sequenceDiagram
    actor Użytkownik
    participant Biletomat as System Biletomatu

    %% Scenariusz główny – Wybór języka
    Użytkownik ->> Biletomat: Uruchomienie biletomatu
    Biletomat -->> Użytkownik: Ekran powitalny\n(opcje wyboru języka)
    Użytkownik ->> Biletomat: Wybór preferowanego języka
    Biletomat ->> Biletomat: Dostosowanie interfejsu\n do wybranego języka
    Biletomat -->> Użytkownik: Interfejs w wybranym języku

    %% Scenariusz alternatywny – Anulowanie transakcji
    alt Anulowanie transakcji
        Użytkownik ->> Biletomat: Wybranie opcji "Anuluj"
        Biletomat -->> Użytkownik: Komunikat o anulowaniu
        Biletomat ->> Biletomat: Reset interfejsu\n do ekranu głównego
    end
```









