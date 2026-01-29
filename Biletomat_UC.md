## Diagramy przypadków użycia

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
    
    %% Связь с внешней системой, которая дает подтверждение оплаты
    UC1 --- ST
    
    UC3 -.->|«extend»| UC1
```
