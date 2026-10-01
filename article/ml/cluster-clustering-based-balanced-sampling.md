# Clustering-Based Balanced Sampling and Allocation with Data Parallelism for High-Performance Fine-Tuning (CluSTER)

Źródło: https://arxiv.org/abs/2609.12584
Data opracowania: 2026-10-01

> Uwaga: pełny tekst (arxiv.org) był niedostępny z poziomu środowiska (blokada sieciowa). Streszczenie opiera się na abstrakcie i fragmentach/streszczeniach dostępnych w wynikach wyszukiwania, więc brakuje szczegółowych tabel wyników.

## Abstract
Autorzy proponują CluSTER – framework redukcji danych do fine-tuningu instrukcyjnego LLM w środowisku data parallelism (DP). Dane są klastrowane w przestrzeni gradientów, następnie próbkowane w sposób zbalansowany względem klastrów i workerów, a aktualizacja gradientu jest ważona tak, by zachować oryginalny rozkład danych. Według autorów skraca to czas treningu nawet o 69,6% prawie bez utraty jakości.

## Problem
Zbiory do instruction tuningu są duże, redundantne i niezbalansowane. Naiwny trening na dużych batchach wielokrotnie uwzględnia nadreprezentowane grupy próbek, a słabo pokrywa rzadsze, ale informatywne przykłady – zwłaszcza gdy dane są rozdzielane między wiele GPU. Istniejące metody selekcji danych zwykle ignorują aspekt równoległości (jak próbki trafiają do poszczególnych workerów), więc oszczędność danych nie przekłada się w pełni na stabilność i wydajność treningu.

## Metoda
Framework ma trzy etapy: (1) klastrowanie oparte na gradientach – próbki grupowane według reprezentacji w przestrzeni gradientów; (2) klastrowe, zbalansowane próbkowanie – rozmiary klastrów są wyrównywane (parametr pojemności α, domyślnie 1,5), a ze zbioru wybierana jest frakcja r; domyślnie liczba klastrów K równa się liczbie workerów DP, co daje dwupoziomowe pokrycie (klaster + worker); (3) ważona aktualizacja gradientu – klaster c dostaje wagę w_c = |C_c|·K/N, proporcjonalną do jego oryginalnego rozmiaru, co przywraca wkład klastra do globalnego gradientu i utrzymuje spójność z fine-tuningiem na pełnym zbiorze, zachowując redukcję wariancji dzięki zbalansowanemu próbkowaniu. Nowością jest połączenie selekcji danych z alokacją do workerów DP oraz korekta wagami.

## Wyniki
Na kilku zbiorach instruction tuningu CluSTER skraca czas treningu do 69,6% względem pełnego treningu, przy niemal braku spadku dokładności, i wypada lepiej niż wcześniejsze metody próbkowania/redukcji danych; poprawia też stabilność treningu. Szczegółowych liczb dla poszczególnych benchmarków, modeli i baseline'ów nie udało się zweryfikować w dostępnych źródłach.

## Wnioski
Podejście jest praktyczne tam, gdzie fine-tuning dużych zbiorów instrukcji na wielu GPU jest kosztowny, a dane są redundantne – dodatkowy koszt to obliczenie gradientów do klastrowania, który trzeba porównać z uzyskaną oszczędnością. Ważone aktualizacje to rozsądny sposób na uniknięcie biasu wynikającego z balansowania. Ograniczenia (moja ocena): zależność od hiperparametrów K, α, r; koszt wstępnego etapu; brak weryfikacji na bardzo dużych modelach i innych zadaniach niż instruction tuning, o ile artykuł ich nie pokazuje. Warto sprawdzić kod autorów i zreplikować wyniki przed adopcją.
