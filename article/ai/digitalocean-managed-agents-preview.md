# Managed Agents public preview — DigitalOcean

Źródło: https://digitalocean.com/blog/managed-agents-public-preview
Data opracowania: 2026-10-08

> Uwaga: strona źródłowa była niedostępna (blokada sieciowa). Streszczenie opiera się na wynikach wyszukiwania cytujących dokumentację DigitalOcean (docs.digitalocean.com), notę wydania oraz doniesienia prasowe (m.in. InfoQ, Forkast). Źródła różnią się co do daty (21–23 września 2026) i nazwy drugiego komponentu.

## Abstract
DigitalOcean udostępnił w publicznym podglądzie (public preview) usługę Managed Agents – zarządzaną platformę do uruchamiania agentów AI. Każdy agent dostaje własne izolowane środowisko wykonawcze (microVM) oraz kontrolowany dostęp do narzędzi przez jeden zarządzany endpoint MCP.

## Problem
Wdrażanie agentów AI w produkcji wymaga bezpiecznego sandboxa na wykonywanie kodu, zarządzania sesjami i uprawnieniami oraz integracji z dziesiątkami zewnętrznych usług. Zespoły budują to samodzielnie: izolację, przechowywanie poświadczeń, podłączanie narzędzi. Wycieki kluczy do modelu lub sandboxa oraz brak izolacji między agentami to realne ryzyka.

## Metoda
Platforma składa się z dwóch głównych części: Harness Runtime (warstwa wykonawcza) i Action Gateway (dostęp do narzędzi). Każda sesja działa w osobnej microVM Firecracker z własnym CPU i systemem plików; sesje można wstrzymywać, wznawiać i rozwidlać (fork). Można uruchamiać własne harnessy, np. Claude Code, agentów LangGraph czy Hermes, z konsoli lub CLI. Action Gateway udostępnia ponad 16 000 narzędzi od ponad 500 dostawców (GitHub, Jira, Stripe, PagerDuty, Supabase) przez pojedynczy zarządzany endpoint MCP; poświadczenia są wstrzykiwane w momencie wykonania i niewidoczne dla modelu i sandboxa. Możliwe jest użycie własnych obrazów sandboxa (BYOT). Część źródeł wspomina też komponent inferencji do modeli open-weight i własnościowych.

## Wyniki
To ogłoszenie produktowe – brak benchmarków i wyników eksperymentalnych. Głównym ustaleniem jest zakres oferty: izolacja per agent, katalog narzędzi, zarządzanie poświadczeniami i sesjami. Liczby (16 000 narzędzi, 500 dostawców) pochodzą od dostawcy i nie zostały niezależnie zweryfikowane.

## Wnioski
Oferta ma sens dla zespołów, które chcą szybko wdrożyć agentów bez budowania infrastruktury sandboxów i integracji; separacja poświadczeń od modelu jest istotnym atutem bezpieczeństwa. Ograniczenia: status preview (brak gwarancji SLA i stabilności API), ryzyko vendor lock-in, nieznane ceny i limity, niezweryfikowana jakość tak szerokiego katalogu narzędzi. Warto porównać z podobnymi ofertami (zarządzane sandboxy agentów u innych dostawców chmurowych) i przetestować na niekrytycznym przypadku użycia.
