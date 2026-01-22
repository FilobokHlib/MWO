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

### DIAGRAM SEKWENCJI DLA PRZYPADKU UŻYCIA GENEROWANIA POTWIERDZENIA ZAKUPU
- AKTOR: BILETOMAT
- OBIEKTY: SYSTEM TRANSAKCYJNY, MODUŁ DRUKOWANIA, INTERFEJS UŻYTKOWNIKA, UŻYTKOWNIK
- KOLEJNOŚĆ KOMUNIKATÓW (SCENARIUSZ GŁÓWNY):
 
  o SYSTEM TRANSAKCYJNY WYSYŁA POTWIERDZENIE TRANSAKCJI DO BILETOMATU.
  
  o BILETOMAT PRZEKAZUJE ŻĄDANIE DO MODUŁU DRUKOWANIA.
  
  o MODUŁ DRUKOWANIA GENERUJE BILET I ZWRACA STATUS DO BILETOMATU.
  
  o BILETOMAT INFORMUJE UŻYTKOWNIKA PRZEZ INTERFEJS: "ODBIERZ BILET".
  
  o UŻYTKOWNIK ODBIERA BILET.
  
  o BILETOMAT POTWIERDZA ZAKOŃCZENIE DO SYSTEMU TRANSAKCYJNEGO.
  
- SCENARIUSZ ALTERNATYWNY 1 (BŁĄD GENEROWANIA):
  
  o MODUŁ DRUKOWANIA WYKRYWA BŁĄD (BRAK PAPIERU/AWARIA).
  
  o MODUŁ DRUKOWANIA ZWRACA STATUS BŁĘDU DO BILETOMATU.
  
  o BILETOMAT WYŚWIETLA UŻYTKOWNIKOWI: "BŁĄD. SKONTAKTUJ SIĘ Z OBSŁUGĄ".
  
  o BILETOMAT WYSYŁA POWIADOMIENIE O BŁĘDZIE DO SYSTEMU TRANSAKCYJNEGO.

```mermaid
sequenceDiagram
    participant ST as System Transakcyjny
    participant B as Biletomat
    participant MD as Moduł Drukowania
    participant IU as Interfejs Użytkownika
    participant U as Użytkownik

    ST->>B: Potwierdzenie transakcji
    B->>MD: Żądanie generowania biletu
    
    alt Scenariusz główny
        MD-->>B: Bilet gotowy
        B->>IU: Komunikat o odbiorze
        IU->>U: "Odbierz bilet"
        U-->>U: Odbiór biletu
        IU-->>B: Potwierdzenie
        B->>ST: Transakcja zakończona
        ST-->>B: OK
    else Błąd generowania
        MD-->>B: Status błędu
        B->>IU: Komunikat o błędzie
        IU->>U: "Błąd. Skontaktuj się z obsługą"
        B->>ST: Powiadomienie o błędzie
        ST-->>B: OK
    end
  ```



