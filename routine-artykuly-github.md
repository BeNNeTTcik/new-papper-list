# Routine: codzienny research artykułów → GitHub

Instrukcja jak skonfigurować **Claude Routine** (funkcja Claude Code, w tej chwili w wersji research preview), która codziennie:

1. wyszukuje w internecie aktualne artykuły na wybrany temat,
2. wybiera z nich najciekawsze / najbardziej wartościowe,
3. zapisuje je jako osobny plik `.md` w repo na GitHubie, w formacie „lista do przeczytania na dany dzień” (lub „ciekawostki” — patrz warianty niżej).

---

## 1. Wymagania wstępne

- Konto Claude z dostępem do **Claude Code on the web** (plan Pro / Max / Team / Enterprise).
- Repozytorium na GitHubie, do którego chcesz wrzucać pliki (np. `daily-reads` albo folder `reading-list/` w istniejącym repo).
- Zainstalowana **Claude GitHub App** na tym repo — jeśli jeszcze jej nie masz, Claude poprosi o instalację przy tworzeniu routine'a z poziomu przeglądarki.
- Dostęp do `claude.ai/code/routines`.

> Routines uruchamiają się w chmurze Anthropica, więc nie potrzebujesz włączonego komputera. Każde uruchomienie klonuje repo, wykonuje polecenie z promptu i (w naszym przypadku) commituje nowy plik.

---

## 2. Jak utworzyć routine (krok po kroku)

1. Wejdź na **[claude.ai/code/routines](https://claude.ai/code/routines)** → **New routine**.
2. **Nazwa routine'a**, np. `Daily Reading List – {TEMAT}`.
3. **Prompt** — wklej treść z sekcji [3. Gotowy prompt](#3-gotowy-prompt) poniżej, podmieniając placeholdery `{...}`.
4. **Repozytoria** — dodaj repo, do którego mają trafiać pliki.
5. **Środowisko (Environment)** — zostaw domyślne (`Default`, `Trusted`), chyba że wyszukiwanie ma iść przez konkretne API/serwisy spoza domyślnej allowlisty — wtedy trzeba dodać domenę w ustawieniach środowiska.
6. **Trigger** → wybierz **Schedule** → **Daily**, ustaw godzinę (np. 7:00 rano czasu lokalnego).
7. **Connectors** — routine automatycznie dołączy Twoje podpięte connectory. Jeśli nie potrzebujesz np. Slacka czy Gmaila do tego zadania, możesz je odznaczyć — zasada ograniczonych uprawnień.
8. Kliknij **Create**. Możesz od razu przetestować przyciskiem **Run now**.

Alternatywnie z CLI: `/schedule` i opisz zadanie w naturalnym języku, np.:
```
/schedule codziennie o 7:00, wyszukaj artykuły o {TEMAT} i wrzuć jako plik do repo {REPO}
```
Claude przeprowadzi Cię przez konfigurację konwersacyjnie.

---

## 3. Gotowy prompt

Podmień `{TEMAT}`, `{REPO}`, `{FOLDER}` na swoje wartości i wklej to jako **Instructions / Prompt** routine'a.

```
Jesteś moim codziennym researcherem. Twoje zadanie, wykonywane raz dziennie:

1. Wyszukaj w internecie 5–8 aktualnych, wartościowych artykułów na temat: {TEMAT}.
   - Priorytet dla treści opublikowanych w ciągu ostatnich 24–48 godzin.
   - Preferuj źródła oryginalne (blogi firmowe, media branżowe, publikacje naukowe) nad agregatorami.
   - Pomiń duplikaty tematyczne (jeśli 3 źródła piszą o tym samym wydarzeniu, wybierz najlepsze jedno).

2. Dla każdego artykułu przygotuj:
   - Tytuł (link do oryginału)
   - 2–3 zdania streszczenia własnymi słowami (bez cytowania dłuższych fragmentów)
   - Jedno zdanie: dlaczego warto to przeczytać / co z tego wynika

3. Sklonuj repozytorium {REPO}. W folderze {FOLDER} utwórz NOWY plik markdown
   o nazwie w formacie: YYYY-MM-DD.md (użyj bieżącej daty, np. 2026-09-17.md).
   Nie nadpisuj istniejących plików z innych dni.

4. Plik powinien mieć strukturę:

   # Lista do przeczytania — {data}
   Temat: {TEMAT}

   ## 1. [Tytuł artykułu](link)
   Streszczenie...
   **Dlaczego warto:** ...

   (kolejne pozycje w tym samym formacie)

5. Zacommituj plik na branchu `claude/daily-reads-{data}` i otwórz Pull Requesta
   do brancha domyślnego z opisem podsumowującym liczbę dodanych artykułów.

6. Jeśli tego dnia nie znajdziesz żadnych sensownych, nowych artykułów na temat
   {TEMAT}, nie twórz pustego pliku — zamiast tego zakończ sesję z krótką notatką
   w podsumowaniu runa, że nic wartościowego nie znaleziono.
```

### Wariant „ciekawostki” zamiast listy do przeczytania

Jeśli wolisz krótsze, bardziej „snackowe” wpisy zamiast pełnej listy artykułów do przeczytania, zamień punkty 2 i 4 na:

```
2. Dla każdego znalezionego artykułu wyciągnij JEDNĄ najciekawszą ciekawostkę /
   fakt / liczbę — coś zaskakującego, nieoczywistego lub istotnego. Podaj ją
   w 1–2 zdaniach własnymi słowami + link do źródła.

4. Plik:
   # Ciekawostki dnia — {data}
   Temat: {TEMAT}

   - 🔹 Ciekawostka 1 własnymi słowami. ([źródło](link))
   - 🔹 Ciekawostka 2 własnymi słowami. ([źródło](link))
   ...
```

---

## 4. Uwagi i dobre praktyki

- **Nazwa pliku = data** — dzięki temu masz naturalne, chronologiczne archiwum bez nadpisywania.
- **PR zamiast bezpośredniego commita na main** — bezpieczniejsze, dajesz sobie szansę przejrzenia przed mergem; jeśli wolisz w pełni automatyczny flow, poproś w prompcie o commit bezpośrednio do brancha głównego (o ile nie jest chroniony).
- **Kontrola kosztów/limitów** — routines liczą się do dziennego limitu uruchomień konta; jeden routine dziennie to niewielkie obciążenie.
- **Strefa czasowa** — godzina w harmonogramie jest ustawiana w Twojej lokalnej strefie i automatycznie konwertowana.
- **Minimalny interwał** harmonogramu to 1 godzina, więc „codziennie” nie jest problemem, ale częstsze odświeżanie (np. co godzinę) też jest możliwe, gdybyś chciał zamienić to w „na bieżąco”.
- Routines są obecnie w **research preview** — zachowanie i limity mogą się zmieniać.

---

## 5. Czego jeszcze potrzebuję od Ciebie, żeby to dopracować

Zostawiłem placeholdery `{TEMAT}`, `{REPO}`, `{FOLDER}` — mogę je od razu wypełnić, jeśli powiesz mi:

1. **Jaki dokładnie temat/tematy** ma śledzić routine (np. "AI i LLM-y", "polska gospodarka", "nowości Kubernetes")? Czy to ma być jeden stały temat, czy chcesz mieć kilka osobnych routine'ów na różne tematy?
2. **Nazwa repo i folderu** docelowego na GitHubie (np. `janek/daily-reads`, folder `lista/` czy `ciekawostki/`)?
3. Czy wolisz wersję **"lista do przeczytania"**, **"ciekawostki"**, czy **obie jednocześnie** (osobne pliki każdego dnia)?
4. Czy artykuły mają iść od razu na main, czy wolisz PR do akceptacji (opisane wyżej)?
5. Czy chcesz też powiadomienie (np. na Slacka) gdy nowy plik się pojawi?

Daj znać, a zaktualizuję plik z konkretnymi wartościami zamiast placeholderów.