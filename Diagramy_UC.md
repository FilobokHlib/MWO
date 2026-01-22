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










