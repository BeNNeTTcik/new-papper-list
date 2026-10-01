# Pomiary do zrozumienia tempa rozwoju AI wewnątrz laboratoriów pierwszej linii (Measuring the pace of AI development)

Źródło: https://www.anthropic.com/institute/measuring-pace-of-ai-development
Data opracowania: 2026-10-01

## Abstract
Anthropic proponuje trzy mierzalne wskaźniki tempa rozwoju AI w laboratoriach frontier: stopień automatyzacji prac R&D przez AI, zdolność nadzoru nad agentami oraz podział mocy obliczeniowej między rozwój możliwości a bezpieczeństwo. Publikuje pierwsze liczby z własnej firmy – m.in. że Claude „prowadzi” 26% prac R&D nad AI – i postuluje niezależną weryfikację takich pomiarów.

## Problem
Wiedza o tym, jak szybko i jak bezpiecznie rozwija się AI, jest skoncentrowana wewnątrz laboratoriów; społeczeństwo i regulatorzy mają niewiele twardych danych. Bez wspólnych, weryfikowalnych metryk trudno ocenić ryzyko rekurencyjnego samoulepszania, skuteczność nadzoru nad coraz bardziej autonomicznymi agentami ani rzeczywiste nakłady na bezpieczeństwo, a tym samym trudno projektować polityki „spowolnienia” rozwoju.

## Metoda
(1) AI-led R&D: skatalogowano zadania R&D w Anthropic na podstawie Slacka i dokumentacji, tworząc drzewo 542 węzłów; agenci Claude oceniali poziom automatyzacji (skala AL0–AL5), z wagami według czasu pracy ludzi; niezależni recenzenci weryfikowali oceny bez wglądu w uzasadnienia modelu. (2) Nadzór: nadawanie agentom indywidualnych tożsamości i używanie otwartych kanałów komunikacji zamiast prywatnych, co daje audytowalne ślady; działają monitory online (blokujące) i offline (analiza transkryptów). (3) Compute: klasyfikacja obciążeń jako „safety” lub „capability” na podstawie infrastruktury monitorującej, z próbą ok. 14% przebiegów treningowych, ważoną ku największym zadaniom.

## Wyniki
Claude prowadzi 26% prac R&D (z <1% w lutym 2026); ponad 90% pracy jest na poziomie „AI współpracuje” lub wyższym. Monitory online blokują ok. 0,002% akcji agentów (1 na 47 000), offline oznaczają 1–2 na tysiąc transkryptów. Ok. 6% mocy obliczeniowej R&D idzie na bezpieczeństwo (12% w przypadku R&D prowadzonego przez AI).

## Wnioski
Wartość publikacji leży w zaproponowaniu konkretnych, powtarzalnych metryk i pokazaniu pierwszych liczb; to krok ku transparentności. Ograniczenia, które sami autorzy wskazują, są istotne: oceny automatyzacji zależą od modelu-sędziego, a zamrożony koszyk zadań nie uchwyci nowych typów pracy; system nadzoru działa dopiero od miesięcy, a przy milionach agentów rzadkie incydenty będą częste; dane o compute to jednorazowa, nieweryfikowana technicznie migawka z subiektywnym podziałem safety/capability. Dane pochodzą od samej firmy, więc kluczowa jest zapowiadana weryfikacja przez strony trzecie. Najbardziej użyteczny jako punkt wyjścia do standardów branżowych, zwłaszcza compute jako najłatwiej audytowalny wskaźnik.
