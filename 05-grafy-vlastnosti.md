# 05. Grafy – vlastnosti

## Obsah
- Základní pojmy
- Typy grafů
- Cesta a souvislost grafu
- Komponenta grafu
- Kostra grafu
- Stromy a binární stromy
- Topologické uspořádání

**Graf** je matematická struktura tvořená množinou vrcholů (uzlů) a množinou hran, které propojují některé dvojice vrcholů.

### Dělení grafů 
**Ohodnocení**
- Neohodnocené
- Ohodnocené
  - Hranově(délka/ nosnost/ ... silnice)
  - Vrcholově(poplatky na letištích)


**Orientace**
- neorientované
- orientované

### Další typy grafů
- **Strom** - souvislý graf bez cyklů
- **Les** - všechny jeho komponenty souvislosti jsou stromy (v lese jsou stromy 💀)
- **Úplný graf** - každý vrchol je spojen hranou s každým jiným vrcholem
  <pre> ``` n(n - 1)/2 //počet hran``` </pre>
  
- **Bipartitní graf** - Pojmem bipartitní graf nebo sudý graf se v teorii grafů označuje takový graf, jehož množinu vrcholů je možné rozdělit na dvě disjunktní množiny tak, že žádné dva vrcholy ze stejné množiny nejsou spojeny hranou.
![Bipart Graph](https://labuladong.online/algo/images/algo4/1-en.jpg)

- **Acyklický graf** - graf neobsahující žádný cyklus
  
- **Multigraf** - graf umožňující více hran mezi stejnými vrcholy(Multihrany)
- **Pseudograf** - graf umožňující smyčky a násobné hrany
  - pseudograph is a multigraph that is permitted to have loops
![MultiGraf](https://drek4537l1klr.cloudfront.net/malak/Figures/03fig12.jpg)
## Cesta a souvislost grafu

### Cesta
**Cesta** v grafu je posloupnost vrcholů a hran, kde každá hrana spojuje předchozí a následující vrchol v posloupnosti.

Typy cest:
- **Jednoduchá cesta** - žádný vrchol se v cestě neopakuje
- **Kružnice (cyklus)** - cesta, která začíná a končí ve stejném vrcholu
- **Hamiltonovská cesta** - cesta, která prochází všemi vrcholy grafu právě jednou
- **Eulerova cesta** - cesta, která prochází každou hranou grafu právě jednou

### Souvislost grafu
- **Souvislý graf** - existuje cesta mezi libovolnými dvěma vrcholy
- **Nesouvislý graf** - existují vrcholy, mezi kterými nevede žádná cesta
- **Silně souvislý orientovaný graf** - existuje orientovaná cesta mezi libovolnými dvěma vrcholy v obou směrech
- **Slabě souvislý orientovaný graf** - graf je souvislý, pokud ignorujeme orientaci hran

## Komponenta grafu

**Komponenta grafu** je maximální souvislý podgraf původního grafu. To znamená, že:
- Každé dva vrcholy v komponentě jsou spojeny cestou
- Neexistuje další vrchol mimo komponentu, který by byl spojen cestou s některým vrcholem komponenty

Nesouvislý graf má více než jednu komponentu. Každá komponenta představuje izolovanou část grafu.

U orientovaných grafů rozlišujeme:
- **Silně souvislé komponenty** - maximální podgrafy, v nichž existuje orientovaná cesta mezi libovolnými dvěma vrcholy
- **Slabě souvislé komponenty** - maximální podgrafy, které by byly souvislé, kdyby se ignorovala orientace hran

## Kostra grafu

**Kostra grafu** je podgraf, který:
- Obsahuje všechny vrcholy původního grafu
- Je souvislý
- Nemá cykly (je to strom)
- Má minimální možný počet hran potřebných k propojení všech vrcholů (n-1 hran pro n vrcholů)

Vlastnosti kostry:
- Graf může mít více různých koster
- Kostra existuje pouze pro souvislé grafy
- Kostra je minimální souvislý podgraf obsahující všechny vrcholy

**Minimální kostra grafu** je kostra ohodnoceného grafu s minimálním součtem vah hran. Pro její nalezení se používají algoritmy:
- Kruskalův algoritmus
- Primův algoritmus
- Borůvkův algoritmus

## Stromy a binární stromy

### Strom
**Strom** je souvislý acyklický graf. Má následující vlastnosti:
- Mezi libovolnými dvěma vrcholy existuje právě jedna cesta
- Má n-1 hran pro n vrcholů
- Přidání jakékoliv hrany vytvoří cyklus
- Odstranění jakékoliv hrany rozdělí graf na dvě komponenty

**Kořenový strom** je strom s jedním speciálním vrcholem označeným jako kořen. Pro kořenový strom definujeme:
- **Kořen** - speciální vrchol, ze kterého vycházíme
- **List** - vrchol stupně 1 (kromě kořene)
- **Vnitřní vrchol** - vrchol, který není list
- **Potomek** - vrchol přímo spojený s daným vrcholem směrem od kořene
- **Rodič** - vrchol přímo spojený s daným vrcholem směrem ke kořeni
- **Hloubka vrcholu** - vzdálenost (počet hran) od kořene k danému vrcholu
- **Výška stromu** - maximální hloubka listu

### Binární strom
**Binární strom** je kořenový strom, ve kterém má každý vrchol nejvýše dva potomky, které rozlišujeme jako levého a pravého potomka.

Typy binárních stromů:
- **Úplný binární strom** - každý vnitřní vrchol má přesně dva potomky
- **Vyvážený binární strom** - rozdíl výšek levého a pravého podstromu je u každého vrcholu maximálně 1
- **Binární vyhledávací strom** - pro každý vrchol platí, že všechny hodnoty v levém podstromu jsou menší než hodnota vrcholu a všechny hodnoty v pravém podstromu jsou větší

## Topologické uspořádání

**Topologické uspořádání** je lineární uspořádání vrcholů orientovaného acyklického grafu (DAG) takové, že pokud existuje hrana z vrcholu u do vrcholu v, pak u předchází v v tomto uspořádání.

Vlastnosti:
- Existuje pouze pro orientované acyklické grafy (DAG)
- Graf může mít více různých topologických uspořádání
- Používá se k plánování úloh s precedenčními omezeními

Algoritmy pro topologické uspořádání:
- Algoritmus založený na odstranění zdrojových vrcholů (Kahn)
- Algoritmus založený na prohledávání do hloubky (DFS)

Využití topologického uspořádání:
- Plánování projektů (PERT, kritická cesta)
- Kompilace závislostí (make, Maven)
- Kurz studia (předpoklady pro předměty)
- Vyhodnocení výrazů v kompilátorech

## Příklady využití grafů
- **Mapy a navigace** - města jako vrcholy, silnice jako hrany
- **Sociální sítě** - lidé jako vrcholy, vztahy jako hrany
- **Počítačové sítě** - zařízení jako vrcholy, spojení jako hrany
- **Plánování projektů** - úkoly jako vrcholy, závislosti jako hrany
- **Chemické sloučeniny** - atomy jako vrcholy, vazby jako hrany
- **Webové stránky** - stránky jako vrcholy, hyperlinky jako hrany
- **Logistika** - sklady jako vrcholy, dopravní trasy jako hrany

## Základní operace s grafy

- **Procházení grafu** - algoritmy BFS (prohledávání do šířky) a DFS (prohledávání do hloubky)
- **Hledání nejkratší cesty** - Dijkstrův algoritmus, Bellman-Fordův algoritmus, Floyd-Warshallův algoritmus
- **Hledání minimální kostry** - Kruskalův a Primův algoritmus
- **Hledání komponent** - pomocí BFS nebo DFS
- **Detekce cyklů** - pomocí DFS nebo disjunktních množin
