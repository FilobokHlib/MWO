## DIAGRAMY PRZYPADKÓW UŻYCIA
### SZYBKI WYBÓR RODZAJU BILETU

```mermaid
flowchart TD
    N[Administrator systemu] 
    A[Zarządzanie taryfami]
    B[Zapis taryf]
    C[Powiadomienie o błędach taryf]

    N --> A
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



