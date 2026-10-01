# Daily Research → GitHub Pipeline (Claude Routines)

Szablon dwóch **Claude Routines** do ponownego wdrożenia na nowy temat / nowe repo. Pipeline:

```
┌────────────────────────┐        ┌───────────────────────┐        ┌────────────────────────┐
│  ROUTINE A             │        |  {LISTA_ZBIORCZA}     │        │  ROUTINE B             │
│  Daily Digest          │──────▶|  (ml_list.md itp.)     │──────▶│  Deep Dive             |
│  codziennie o {GODZ}   │ append │  rosnąca tabela       │  czyta │  codziennie, {GODZ_B}  │
│                        │        │  Tytuł│Link│Plik│     │        │                        │
│  szuka artykułów →     │        │  Ocena│Opracowano     │        │  szuka Ocena=X         │
│  pisze plik dzienny →  │        │                       │        │  i puste Opracowano →  │
│  dopisuje nowe wiersze │        │  Ty ręcznie wpisujesz │        │  pisze pełne streszcz. │
│  do listy zbiorczej    │        │  "X" w kolumnie Ocena │        │  do folderu article/   │
└────────────────────────┘        └───────────────────────┘        └────────────────────────┘
```

Jeden HITL: **Ty oznaczasz `X`** w kolumnie "Ocena" przy artykułach, które Cię zainteresowały. Routine B dogrywa dla nich pełne, ustrukturyzowane streszczenie — bez ręcznego kopiowania czegokolwiek.

---

## Wymagania wstępne

- Konto Claude z dostępem do **Claude Code on the web** (Pro / Max / Team / Enterprise).
- Repozytorium GitHub z zainstalowaną **Claude GitHub App**.
- Dostęp do `claude.ai/code/routines`.
- Branch `main` (lub domyślny) **nieoznaczony jako protected** — oba routine'y commitują bezpośrednio do niego, bez PR. Jeśli branch jest chroniony, GitHub i tak odrzuci bezpośredni push i routine wróci do brancha `claude/...` + PR niezależnie od treści promptu.

---

## Placeholdery do podmiany przy wdrożeniu

| Placeholder           | Co to jest                    | Przykład                                                       |
| --------------------- | ----------------------------- | -------------------------------------------------------------- |
| `{REPO}`              | repo docelowe                 | `BeNNeTTcik/new-papper-list`                                   |
| `{TEMAT}`             | temat/tematy do śledzenia     | `clustering, oversampling, undersampling, imbalanced datasets` |
| `{FOLDER}`            | folder na dzienne pliki       | `ml`                                                           |
| `{LISTA_ZBIORCZA}`    | plik zbiorczy z tabelą        | `ml_list.md`                                                   |
| `{ARTICLE_FOLDER}`    | folder na pełne streszczenia  | `article`                                                      |
| `{GODZ}` / `{GODZ_B}` | godziny uruchomienia (B po A) | `07:00` / `08:00`                                              |

Dla każdego nowego tematu powtarzasz całą parę routine'ów z nowym kompletem placeholderów (osobny `{FOLDER}` i `{LISTA_ZBIORCZA}` na temat, mogą współdzielić to samo `{REPO}`).

---

## Jak utworzyć routine (ogólne kroki, dla obu)

1. **[claude.ai/code/routines](https://claude.ai/code/routines)** → **New routine**.
2. Nazwa, np. `Daily Digest — {TEMAT}` / `Deep Dive — {TEMAT}`.
3. Prompt — wklej odpowiednią sekcję poniżej, z podmienionymi placeholderami.
4. Repozytoria → dodaj `{REPO}`.
5. Środowisko → domyślne (`Default`, `Trusted`), chyba że research wymaga domen spoza allowlisty.
6. Trigger → **Schedule → Daily**, godzina wg tabeli placeholderów (Routine B startuje **po** Routine A, żeby zdążył się zacommitować).
7. Connectors → odznacz zbędne.
8. **Create**, opcjonalnie **Run now** do testu.

Alternatywnie z CLI: `/schedule` i opis zadania w naturalnym języku — Claude przeprowadzi konfigurację konwersacyjnie.

---

## Routine A — Daily Digest

Codziennie: szuka nowych artykułów na temat, pisze dzienny plik ze streszczeniami, dopisuje nowe wiersze do tabeli zbiorczej (nigdy nie nadpisując istniejących).

```
Jesteś moim codziennym researcherem. Twoje zadanie, wykonywane raz dziennie:

1. Wyszukaj w internecie 5–8 aktualnych, wartościowych artykułów na temat: {TEMAT}.
- Priorytet dla treści opublikowanych w ciągu ostatnich 24–48 godzin.
- Preferuj źródła oryginalne (blogi firmowe, media branżowe, publikacje naukowe) nad agregatorami.
- Pomiń duplikaty tematyczne (jeśli 3 źródła piszą o tym samym wydarzeniu, wybierz najlepsze jedno).


1b. Sprawdź plik({LISTA_ZBIORCZA}) z tabelą linków dla tego samego tematu, generowane przez inny routine w repozytorium "{REPO}"  z tabelą Tytuł | Link | Plik repo) - sprawdź wszystkie pliki z ostatnich 14 dni (patrz po kolumnie "Plik repo". Zbierz z nich listę linków/tytułów, które już się pojawiły.

Dla każdego artykułu znalezionego w kroku 1: jeśli jego link (lub wyraźnie ten sam artykuł pod innym URL) już występuje w tej liście — ODRZUĆ go i znajdź w jego miejsce inny, jeszcze niewystępujący artykuł na ten sam temat. Powtarzaj maksymalnie 3 razy.

Jeśli tego dnia nie znajdziesz żadnych sensownych, nowych artykułów na {TEMAT}, nie twórz pustego pliku — zamiast tego zakończ sesję z krótką notatką w podsumowaniu runa, że nic wartościowego nie znaleziono.aż będziesz mieć 2-5 unikalnych artykułów, których nie było wcześniej w tabeli.

2. Dla każdego artykułu przygotuj:
- Tytuł (link do oryginału)
- 2–3 zdania streszczenia własnymi słowami (bez cytowania dłuższych fragmentów)
- Jedno zdanie: dlaczego warto to przeczytać / co z tego wynika

Oraz dodaj wpis w strukturze (Tytuł | Link | Plik repo) do tabeli w pliku {LISTA_ZBIORCZA}.

3. Sklonuj repozytorium "{REPO}". W folderze "{FOLDER}" utwórz NOWY plik markdown o nazwie w formacie: YYYY-MM-DD.md (użyj bieżącej daty, np. 2026-09-17.md). Nie nadpisuj istniejących plików z innych dni.

4. Plik powinien mieć strukturę:

   # Lista do przeczytania — {data}
   Temat: clustering, oversampling, undersampling, imbalanced datasets

   ## 1. [Tytuł artykułu](link)
   Streszczenie...
   **Dlaczego warto:** ...

   (kolejne pozycje w tym samym formacie)

5. Zacommituj plik bezpośrednio do domyślnego brancha repozytorium (main) — NIE twórz nowego brancha i NIE otwieraj Pull Requesta. Wiadomość commita: krótki opis podsumowujący liczbę dodanych artykułów, np. "Dodano 4 artykuły na temat: clustering, oversampling, undersampling, imbalanced datasets — 2026-09-30".
```

## Routine B — Deep Dive

Codziennie, **po** Routine A: skanuje `{LISTA_ZBIORCZA}` w poszukiwaniu wierszy, które Ty oznaczyłeś `X` w "Ocena", a które nie mają jeszcze `X` w "Opracowano" — i dla nich pisze pełne, ustrukturyzowane streszczenie.

```
Jesteś moim researcherem pogłębionym. Pracujesz na podstawie list, które już istnieją w repozytorium {REPO} — NIE wyszukujesz nowych artykułów, tylko rozwijasz te już ocenione przeze mnie.

1. Sklonuj repozytorium. Otwórz plik {LISTA_ZBIORCZA} w katalogu głównym repo. Ma tabelę z kolumnami: Tytuł | Link | Plik repo | Ocena | Opracowano

2. Znajdź WSZYSTKIE wiersze, w których kolumna "Ocena" zawiera "X" ORAZ kolumna "Opracowano" jest pusta. Jeśli nie ma żadnych takich wierszy, zakończ sesję bez zmian i napisz w podsumowaniu runa, że nic nie było do zrobienia.

3. Dla każdego znalezionego wiersza:
a. Otwórz link z kolumny "Link" i przeczytaj treść artykułu (jeśli strona jest niedostępna, skorzystaj z dostępnych fragmentów/abstraktu — zaznacz to w tekście).
b. Napisz szczegółowe streszczenie w ŚCIŚLE następującej strukturze sekcji (każda jako nagłówek ##, zawsze po polsku, niezależnie od języka źródła):

      ## Abstract
      2-4 zdania — co dokładnie autorzy zrobili i pokazali.

      ## Problem
      Jaki konkretny problem/lukę artykuł adresuje. Jaki jest status quo
      i co w nim nie działa.

      ## Metoda
      Zastosowane podejście opisane konkretnie: kluczowe kroki, założenia,
      na czym polega nowość względem istniejących rozwiązań.

      ## Wyniki
      Najważniejsze wyniki/liczby/benchmarki, jeśli są dostępne. Jeśli
      artykuł nie ma wyników liczbowych, napisz to wprost i opisz główne
      ustalenie zamiast liczb.

      ## Wnioski
      Co z tego wynika, ograniczenia, dla kogo i kiedy to ma sens praktyczny.

      Długość całości: ok. 250-500 słów.
c. Zapisz jako nowy plik w folderze {ARTICLE_FOLDER}/, o nazwie: {ARTICLE_FOLDER}/{FOLDER}/{krotki-slug-tytulu}.md
(małe litery, spacje na myślniki, bez znaków specjalnych, maks. ok. 60 znaków; przy kolizji nazwy dodaj sufiks -2, -3, ...). Plik na górze: tytuł oryginału jako H1, link do źródła, data opracowania, a potem pięć sekcji (Abstract, Problem, Metoda, Wyniki, Wnioski) w tej kolejności.

4. Po opracowaniu wiersza wstaw "X" w kolumnie "Opracowano" w {LISTA_ZBIORCZA} dla tego wiersza. Nie zmieniaj żadnych innych kolumn ani wierszy.

5. Zacommituj WSZYSTKIE zmiany (nowe pliki w {ARTICLE_FOLDER}/ oraz zaktualizowany {LISTA_ZBIORCZA}) jednym commitem, bezpośrednio do domyślnego brancha repozytorium (main) — NIE twórz nowego brancha i NIE otwieraj Pull Requesta. Wiadomość commita: krótkie podsumowanie, np. "Szczegółowe streszczenia: 3 artykuły — {data}".
```

## Dobre praktyki i uwagi do tworzenie ROUTINE

- **Nazwa pliku dziennego = data** — naturalne, chronologiczne archiwum bez nadpisywania.
- **Tabela zbiorcza = tylko dopisywanie** — nowe wiersze lądują na końcu, istniejące nigdy nie są nadpisywane ani kasowane. Dzięki temu ręczne `X` w "Ocena" i automatyczne `X` w "Opracowano" przetrwają każde uruchomienie.
- **Bezpośredni commit do main** — prościej i w pełni automatycznie, ale bez szansy na przegląd przed publikacją. Dla kontroli: zmień prompt na branch + Pull Request.
- **Kolejność uruchomień** — Routine B musi startować _po_ Routine A tego samego dnia, inaczej nie zobaczy najświeższych wierszy.
- **Skąd routine wie, co oznaczyłeś** — musisz wpisać `X` w kolumnie "Ocena" bezpośrednio w pliku na GitHubie (edycja w przeglądarce albo lokalnie + `git push`) _zanim_ Routine B się odpali. Lokalny dysk sam z siebie nie jest widoczny dla routine'a — liczy się tylko to, co jest wypchnięte do repo.
- **Minimalny interwał** harmonogramu to 1 godzina — "codziennie" nie jest problemem.
- Routines są obecnie w **research preview** — zachowanie i limity mogą się zmieniać.

---
