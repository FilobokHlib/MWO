



## Diagramy przypadków użycia

### Szybki wybór rodzaju biletu

```mermaid
flowchart TD
    A[Wybór języka]

    A --> |Include| B[Ustawienie domyślnego języka]
    A --> |Include| C[Anulowanie transakcji]

    %% Relacja Extend (opcjonalna)
    A -.->|Extend| F[Wyświetlenie listy popularnych języków]
```
