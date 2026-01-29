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

    N --> A
    A -->|Include| B
    A -.->|Extend| C
```

### Wyświetlenie dostępnych biletów

```mermaid
flowchart TD
    B[Biletomat]
    SC[System Centralny]

    UC1(Wyświetlenie dostępnych biletów)
    UC2(Pobranie listy biletów)
    UC3(Ostrzeżenie o braku danych)

    B --- UC1
    UC1 -->|«include»| UC2
    UC2 --- SC
    
    UC3 -.->|«extend»| UC1
```

### Generowanie potwierdzenia zakupu

```mermaid
flowchart TD
    B[Biletomat]
    ST[System Transakcyjny]

    UC1(Generowanie potwierdzenia zakupu)
    UC2(Generowanie biletu)
    UC3(Błąd generowania)

    B --- UC1
    UC1 -->|«include»| UC2
    
    UC1 --- ST
    
    UC3 -.->|«extend»| UC1
```

## DIAGRAMY SEKWENCJI

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








### Zarządzanie dostępnością biletów

#### Scenariusz do diagramu 
Aktor: Administrator
Obiekty: Interfejs zarządzania taryfami, Serwer aplikacji, Biletomaty

Scenariusz podstawowy
1. Administrator loguje się do systemu zarządzania taryfami.
2. Interfejs przesyła dane logowania do serwera.
3. Serwer przyznaje dostęp administratorowi.
4. Administrator wprowadza zmiany w taryfach biletowych.
5. Interfejs przesyła zmiany do serwera.
6. Serwer zapisuje zmiany i aktualizuje taryfy w biletomatach.
7. System informuje administratora o pomyślnym zakończeniu operacji.
Scenariusz alternatywny 1 – błąd aktualizacji taryf
1. Administrator wprowadza i zatwierdza zmiany w taryfach.
2. Serwer próbuje przesłać zaktualizowane taryfy do biletomatów.
3. Biletomaty nie potwierdzają aktualizacji.

System wyświetla administratorowi komunikat o błędzie aktualizacji taryf.
```mermaid
sequenceDiagram
    participant ADMIN as ADMINISTRATOR
    participant UI as INTERFEJS
    participant SERVER as SERWER
    participant TM as BILETOMATY

    ADMIN->>UI: LOGOWANIE
    UI->>SERVER: DANE LOGOWANIA
    SERVER-->>UI: DOSTĘP

    ADMIN->>UI: ZMIANA TARYF
    UI->>SERVER: ZAPIS TARYF
    SERVER->>TM: AKTUALIZACJA TARYF

    alt AKTUALIZACJA POPRAWNA
        TM-->>SERVER: POTWIERDZENIE
        SERVER-->>UI: SUKCES
        UI-->>ADMIN: KOMUNIKAT O POWODZENIU
    else BŁĄD AKTUALIZACJI
        TM-->>SERVER: BRAK POTWIERDZENIA
        SERVER-->>UI: INFORMACJA O BŁĘDZIE
        UI-->>ADMIN: KOMUNIKAT O BŁĘDZIE
    end
```

#### Scenariusz do diagramu 
Aktor: Administrator
Obiekty: Interfejs zarządzania biletami, Serwer aplikacji, Biletomaty, Aplikacja mobilna

Scenariusz podstawowy
1. Administrator loguje się do panelu zarządzania biletami.
2. Interfejs przesyła dane logowania do serwera aplikacji.
3. Serwer przyznaje administratorowi dostęp do systemu.
4. Administrator edytuje listę dostępnych biletów (dodaje lub usuwa bilety).
5. Interfejs przesyła zmiany do serwera aplikacji.
6. Serwer zapisuje zmiany w systemie centralnym.
7. System synchronizuje zaktualizowaną listę biletów z biletomatami oraz aplikacją mobilną.
8. Administrator otrzymuje informację o pomyślnym zakończeniu operacji.

Scenariusz alternatywny 1 – błąd synchronizacji listy biletów
1. Administrator zapisuje zmiany w liście biletów.
2. System próbuje zsynchronizować dane z biletomatami i aplikacją mobilną.
3. Synchronizacja nie powiodła się.
4. System wyświetla administratorowi komunikat o błędzie synchronizacji.



```mermaid
sequenceDiagram
    participant ADMIN as ADMINISTRATOR
    participant UI as INTERFEJS
    participant SERVER as SERWER
    participant TM as BILETOMATY
    participant APP as APLIKACJA MOBILNA

    ADMIN->>UI: LOGOWANIE
    UI->>SERVER: DANE LOGOWANIA
    SERVER-->>UI: DOSTĘP

    ADMIN->>UI: EDYCJA LISTY BILETÓW
    UI->>SERVER: ZAPIS LISTY BILETÓW
    SERVER->>TM: SYNCHRONIZACJA LISTY
    SERVER->>APP: SYNCHRONIZACJA LISTY

    alt SYNCHRONIZACJA POPRAWNA
        TM-->>SERVER: POTWIERDZENIE
        APP-->>SERVER: POTWIERDZENIE
        SERVER-->>UI: SUKCES
        UI-->>ADMIN: KOMUNIKAT O POWODZENIU
    else BŁĄD SYNCHRONIZACJI
        SERVER-->>UI: INFORMACJA O BŁĘDZIE
        UI-->>ADMIN: KOMUNIKAT O BŁĘDZIE
    end
```

# DIAGRAMY KLAS
## DIAGRAM KLAS Z DIAGRAMU SEKWENCJI "Zarządzanie dostępnością biletów"
```mermaid
classDiagram

class ADMINISTRATOR {
  - STRING LOGIN
  - STRING PASSWORD
  + VOID LOGIN()
  + VOID MODIFYTARIFFS()
  + VOID CONFIRMCHANGES()
}

class INTERFEJS_ZARZADZANIA_TARYFAMI {
  - ADMINISTRATOR CURRENTADMIN
  + VOID SENDLOGINDATA(STRING LOGIN, STRING PASSWORD)
  + VOID SENDTARIFFCHANGES(TARIFFDATA DATA)
  + VOID DISPLAYMESSAGE(STRING MESSAGE)
}

class SERWER_APLIKACJI {
  - BOOLEAN AUTHSTATUS
  + BOOLEAN AUTHENTICATEADMIN(STRING LOGIN, STRING PASSWORD)
  + VOID SAVETARIFFS(TARIFFDATA DATA)
  + BOOLEAN UPDATETICKETMACHINES(TARIFFDATA DATA)
}

class BILETOMAT {
  - STRING MACHINEID
  - BOOLEAN UPDATESTATUS
  + BOOLEAN RECEIVETARIFFUPDATE(TARIFFDATA DATA)
  + VOID CONFIRMUPDATE()
}

class TARIFFDATA {
  - STRING TARIFFID
  - STRING DESCRIPTION
  - FLOAT PRICE
  - DATE VALIDFROM
  - DATE VALIDTO
  + VOID UPDATETARIFF()
}

ADMINISTRATOR --> INTERFEJS_ZARZADZANIA_TARYFAMI : korzysta z
INTERFEJS_ZARZADZANIA_TARYFAMI --> SERWER_APLIKACJI : komunikuje się
SERWER_APLIKACJI --> TARIFFDATA : zarządza
SERWER_APLIKACJI --> BILETOMAT : aktualizuje taryfy
BILETOMAT --> SERWER_APLIKACJI : potwierdza aktualizację

```

## DIAGRAM KLAS Z DIAGRAMU SEKWENCJI "Zarządzanie taryfami biletowymi"
```mermaid
classDiagram

class ADMINISTRATOR {
  - STRING LOGIN
  - STRING PASSWORD
  + VOID LOGIN()
  + VOID EDITTICKETLIST()
  + VOID SAVETICKETCHANGES()
}

class INTERFEJS_ZARZADZANIA_BILETAMI {
  - ADMINISTRATOR CURRENTADMIN
  + VOID SENDLOGINDATA(STRING LOGIN, STRING PASSWORD)
  + VOID SENDTICKETCHANGES(TICKETDATA DATA)
  + VOID DISPLAYMESSAGE(STRING MESSAGE)
}

class SERWER_APLIKACJI {
  - BOOLEAN AUTHSTATUS
  + BOOLEAN AUTHENTICATEADMIN(STRING LOGIN, STRING PASSWORD)
  + VOID SAVETICKETS(TICKETDATA DATA)
  + BOOLEAN SYNCHRONIZEDATASOURCES(TICKETDATA DATA)
}

class BILETOMAT {
  - STRING MACHINEID
  - BOOLEAN SYNCSTATUS
  + BOOLEAN RECEIVETICKETUPDATE(TICKETDATA DATA)
  + VOID CONFIRMSYNCHRONIZATION()
}

class APLIKACJA_MOBILNA {
  - STRING APPLICATIONID
  - BOOLEAN SYNCSTATUS
  + BOOLEAN RECEIVETICKETUPDATE(TICKETDATA DATA)
  + VOID CONFIRMSYNCHRONIZATION()
}

class TICKETDATA {
  - STRING TICKETID
  - STRING NAME
  - FLOAT PRICE
  - BOOLEAN ACTIVE
  + VOID ADDTICKET()
  + VOID REMOVETICKET()
}

ADMINISTRATOR --> INTERFEJS_ZARZADZANIA_BILETAMI : korzysta z
INTERFEJS_ZARZADZANIA_BILETAMI --> SERWER_APLIKACJI : komunikuje się
SERWER_APLIKACJI --> TICKETDATA : zarządza
SERWER_APLIKACJI --> BILETOMAT : synchronizuje
SERWER_APLIKACJI --> APLIKACJA_MOBILNA : synchronizuje
BILETOMAT --> SERWER_APLIKACJI : potwierdza sync
APLIKACJA_MOBILNA --> SERWER_APLIKACJI : potwierdza sync

```

