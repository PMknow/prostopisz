# prostopisz
Asystent AI dla NGO automatyzujący analizę regulaminów grantowych i pre-audyt ryzyka (PoC). Narzędzie pomocnicze – nie stanowi porady prawnej. #TechForGood #LegalTech

Otwartoźródłowy asystent analityczny wspierający organizacje pozarządowe (NGO), grupy nieformalne oraz osoby przygotowujące wnioski grantowe.

Projekt automatyzuje analizę regulaminów konkursowych, ekstrakcję wymagań formalnych oraz identyfikację ryzyk przy wykorzystaniu wieloagentowej architektury AI opartej na platformie Dify i modelach Google Gemini.

Głównym celem projektu jest ograniczenie ryzyka błędów formalnych podczas przygotowywania wniosków grantowych poprzez dostarczanie:

- zwięzłych podsumowań regulaminów,
- checklist formalnych,
- zestawień kluczowych terminów i limitów,
- identyfikacji potencjalnych niespójności i niejednoznaczności w dokumentacji konkursowej,
- raportów przygotowanych zgodnie z zasadami prostego języka.

> Projekt został zaprojektowany i przetestowany wyłącznie dla dokumentów w języku polskim. Skuteczność działania dla dokumentów w innych językach nie była przedmiotem walidacji.

---

## Ważna informacja prawna

Prostopisz jest eksperymentalnym narzędziem analitycznym wykorzystującym generatywną sztuczną inteligencję.

Projekt znajduje się na etapie Proof of Concept (PoC) i może generować odpowiedzi niepełne, nieaktualne lub błędne.

Wszystkie raporty, checklisty, podsumowania i rekomendacje mają charakter wyłącznie informacyjny oraz pomocniczy.

Prostopisz:

- nie świadczy usług prawnych,
- nie świadczy usług doradztwa podatkowego ani finansowego,
- nie stanowi oficjalnej interpretacji regulaminu,
- nie zastępuje konsultacji z ekspertem,
- nie gwarantuje kompletności lub poprawności wyników.

Użytkownik ponosi pełną odpowiedzialność za wykorzystanie wygenerowanych materiałów.

Przed złożeniem wniosku grantowego lub podjęciem decyzji należy każdorazowo zweryfikować informacje z aktualną dokumentacją konkursową oraz oficjalnymi komunikatami organizatora naboru.

W przypadku rozbieżności pomiędzy raportem wygenerowanym przez Prostopisz a dokumentacją źródłową nadrzędne znaczenie ma dokumentacja opublikowana przez organizatora programu.

---

## Kluczowe funkcjonalności

### Analiza dokumentów konkursowych

Obsługiwane formaty wejściowe:

- PDF zawierający warstwę tekstową,
- DOCX,
- TXT.

### Generowanie checklist formalnych

System automatycznie identyfikuje między innymi:

- warunki udziału,
- wymagane załączniki,
- kryteria kwalifikowalności,
- terminy,
- ograniczenia budżetowe,
- obowiązki formalne.

### Audyt danych krytycznych

System weryfikuje i porządkuje:

- kwoty finansowania,
- limity kosztów,
- terminy,
- okresy realizacji,
- wymagania dla różnych typów wnioskodawców.

### Identyfikacja niespójności

Raport może wskazywać potencjalne:

- niespójności pomiędzy zapisami regulaminu,
- omyłki redakcyjne,
- niejednoznaczne wymagania,
- braki informacyjne.

---

## Dwuetapowa architektura analizy i audytu

Projekt wykorzystuje dwa niezależne etapy przetwarzania dokumentu.

```text
Dokument źródłowy
        │
        ▼
┌────────────────────┐
│ Ekstraktor         │
│ dokumentów         │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Redaktor           │
│ Gemini Flash       │
└─────────┬──────────┘
          ▼
      Szkic
          ▼
┌────────────────────┐
│ Audytor            │
│ Gemini Argon       │
└─────────┬──────────┘
          ▼
 Zweryfikowany raport
```

### Etap 1. Redaktor

Model przygotowuje:

- podsumowanie dokumentu,
- szkic checklisty formalnej,
- opis programu w prostym języku.

Na tym etapie nie wpisuje dat, kwot, limitów ani progów punktowych. W ich miejsce pozostawia znaczniki do późniejszej weryfikacji.

Przykład:

```text
[DO UZUPEŁNIENIA PRZEZ AUDYTORA]
```

### Etap 2. Audytor

Model porównuje szkic z dokumentem źródłowym i:

- uzupełnia daty, kwoty i limity,
- wyszukuje pominięte wymagania,
- rozdziela ścieżki dla różnych typów podmiotów,
- dodaje odwołania do odpowiednich sekcji dokumentu,
- identyfikuje potencjalne niespójności i niejednoznaczności.

Takie rozdzielenie syntezy treści od weryfikacji danych ogranicza ryzyko błędów w raportach generowanych przez AI.

### Mechanizmy bezpieczeństwa

Raport końcowy zawiera przypomnienie o pomocniczym charakterze analizy oraz konieczności weryfikacji dokumentacji źródłowej.

---

## Struktura repozytorium

```text
prostopisz/
├── prostopisz-v0.2-workflow.yml
├── LICENSE
└── README.md
```

Przykładowe zanonimizowane analizy mogą zostać opublikowane w kolejnych wersjach projektu.

---

## Uruchomienie workflow w Dify

### Wymagania

- Dify 0.7.0 lub nowszy,
- skonfigurowana wtyczka Google Gemini (`langgenius/gemini`),
- własny klucz API Google AI Studio lub Google Cloud Vertex AI,
- aktywne konto rozliczeniowe zgodne z zasadami dostawcy modeli.

> Workflow nie zawiera własnych zasobów obliczeniowych ani dostępu do modeli AI. Użytkownik musi samodzielnie skonfigurować i rozliczać wykorzystanie usług Google Gemini.

### Instalacja

1. Zaloguj się do swojej instancji Dify (Cloud lub Self-Hosted).

2. Wybierz **Import DSL File** i zaimportuj plik:

```text
prostopisz-v0.2-workflow.yml
```

3. W ustawieniach dostawców modeli skonfiguruj wtyczkę:

```text
langgenius/gemini
```

4. Uruchom workflow i prześlij dokument źródłowy.

5. Po zakończeniu analizy system wygeneruje raport zawierający:

   - podsumowanie dokumentu,
   - zestawienie kluczowych terminów i limitów,
   - checklistę formalną,
   - uwagi audytorskie.

---

## Znane ograniczenia

Wersja v0.2 została przetestowana głównie na dokumentach zawierających poprawnie rozpoznawalną warstwę tekstową.

Nie przeprowadzono jeszcze systematycznych testów dla:

- skanowanych dokumentów PDF pozbawionych warstwy tekstowej,
- dokumentów wymagających zewnętrznego OCR,
- materiałów o niskiej jakości skanu,
- dokumentów zawierających istotne informacje w postaci obrazów,
- rozbudowanych tabel, których układ ma znaczenie interpretacyjne,
- dokumentów, w których kluczowe warunki zostały zapisane wyłącznie w tabelach,
- dokumentów zawierających kluczowe informacje wyłącznie w załącznikach,
- dokumentów silnie odwołujących się do innych dokumentów zewnętrznych.

W takich przypadkach system może nie rozpoznać wszystkich informacji lub nieprawidłowo odtworzyć strukturę dokumentu.

---

## Status projektu

Prostopisz znajduje się obecnie na etapie Proof of Concept (v0.2).

Projekt powstał w celu sprawdzenia skuteczności dwuetapowej architektury analizy i audytu w analizie regulaminów grantowych oraz tworzeniu checklist formalnych.

Dalszy rozwój projektu będzie zależał od wyników testów, dostępności czasu oraz zainteresowania społeczności.

---

## Możliwe kierunki rozwoju

Poniższa lista przedstawia rozważane kierunki rozwoju projektu. Nie stanowi deklaracji realizacji ani harmonogramu prac.

| Wersja | Kamień milowy | Funkcjonalności i architektura | Walidacja |
|---------|---------|---------|---------|
| **v0.2** (obecna) | Silnik audytu (PoC) | Dwuetapowy łańcuch analityczny w Dify, podział ścieżek podmiotowych, ekstrakcja limitów 1:1, identyfikacja potencjalnych niespójności i niejednoznaczności. | Weryfikacja ekspercka poprzez konfrontację raportów z dokumentami źródłowymi. |
| **v0.3** | Interfejs i eksport raportów | Automatyczny eksport raportów do formatu DOCX, rozwój interfejsu oraz publikacja zanonimizowanych przykładów analiz. | Wywiady pogłębione i testy zadaniowe z przedstawicielami NGO. |
| **v0.4** | Interaktywny doradca grantowy | Rozszerzenie projektu o tryb konwersacyjny z pamięcią sesji i możliwością zadawania pytań dotyczących analizowanego regulaminu. | Testy użyteczności oraz analiza potrzeb użytkowników. |
| **v0.5** | Weryfikacja wniosków i OCR | Automatyczna analiza gotowych wniosków grantowych oraz integracja rozwiązań OCR dla dokumentów skanowanych. | Testy na zanonimizowanych danych historycznych i benchmarking skuteczności. |

---

## Współpraca

Sugestie, zgłoszenia błędów i propozycje usprawnień są mile widziane.

Możesz skorzystać z:

- Issues,
- Pull Requests,
- Discussions.

---

## Licencja

Projekt jest udostępniany na licencji MIT.

Pełna treść licencji znajduje się w pliku `LICENSE`.

Copyright (c) 2026 Katarzyna Nowakowska

---

## Misja projektu

Ułatwianie dostępu do finansowania organizacjom społecznym poprzez automatyzację analizy regulaminów grantowych oraz ograniczanie ryzyka błędów formalnych przy wykorzystaniu odpowiedzialnie zaprojektowanej sztucznej inteligencji.
