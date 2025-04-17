# 01. Čísla a číselné algoritmy

## Obsah
- Reprezentace celých a reálných čísel v paměti
- Implementace čísel o velkém počtu číslic
- Algoritmy převodu mezi soustavami
- Hornerovo schéma
- Testy prvočíselnosti
- Eratosthenovo síto
- Testy dělitelnosti
- Eukleidův algoritmus
- Rozklad na prvočísla
- Bitové operace

## Reprezentace celých a reálných čísel v paměti

### Celá čísla
- `byte`: 8 bitů, rozsah -128 až 127
- `short`: 16 bitů, rozsah -32,768 až 32,767
- `int`: 32 bitů, rozsah -2^31 až 2^31-1
- `long`: 64 bitů, rozsah -2^63 až 2^63-1

Celá čísla jsou v paměti uložena ve dvojkovém doplňkovém kódu pro reprezentaci záporných čísel.

### Racionální čísla
- Racionální čísla lze zapsat jako zlomek dvou celých čísel:
  ```
  a / b, kde a ∈ ℤ, b ∈ ℤ a b ≠ 0
  ```
- Příklady:
  - `0.5 = 1 / 2`
  - `-3 = -3 / 1`
  - `7.25 = 29 / 4`

## Ukládání čísel v počítači
- Počítače používají **dvojkovou (binární) soustavu**
- Desetinná čísla jsou reprezentována pomocí **plovoucí řádové čárky**

## Vědecký zápis čísel
- V běžné matematice (desítkově):
  ```
  1.55 × 10ⁿ
  ```
- V počítači (binárně):
  ```
  1.1001... × 2ⁿ
  ```
  - `1.1001...` je **mantisa** (v binární podobě)
  - `2` je základ soustavy
  - `n` je **exponent** (také v binární podobě)
  - `2ⁿ` znamená posun desetinné (resp. binární) čárky o n pozic vlevo/vpravo
  - `n` je celé číslo (kladné nebo záporné), uložené v binárním formátu

Reprezentace celých čísel
1. Bez znaménka (Unsigned Integer)
	•	Používá všechny bity k reprezentaci kladných čísel (včetně nuly)
	•	Např. 8bitové číslo může nabývat hodnot 0 až 255 (2⁸ = 256 možných hodnot)
2. Se znaménkem (Signed Integer)
	•	Používá jeden bit (nejvyšší bit, tzv. MSB – Most Significant Bit) jako znaménkový bit:
	◦	0 znamená kladné číslo
	◦	1 znamená záporné číslo
	▪	nejpoužívanější – dvojkový doplněk => první číslo v dvojkové soustavě posloupnosti značí, že výsledné číslo v desítkové soustavě bude záporné, viz obrázek dole


## Shrnutí
- Racionální čísla lze převést na binární zlomek
- V počítači jsou uložena jako:
  ```
  mantisa × 2^exponent
  ```
  
### Čísla s velkým počtem číslic
Pro práci s čísly o velkém počtu číslic:
- `BigInteger`: pro celá čísla neomezené velikosti
- `BigDecimal`: pro desetinná čísla s libovolnou přesností

## Algoritmy převodu mezi soustavami

### Převod z desítkové do jiné soustavy
Algoritmus:
```java
// Převod z desítkové soustavy do trojkové soustavy
String s = "";
while(x > 0) {
   //s += x % 3;
   s = "" + (x % 3) + s; //takhle přidám na začátek
   x /= 3;
}
System.out.println(s);
```

### Hornerovo schéma
Slouží k efektivnímu vyhodnocení polynomu nebo převodu mezi soustavami.

#### Pro vyhodnocení polynomu:
Mějme polynom libovolného stupně: P(x) = aₙxⁿ + ... + a₂x² + a₁x + a₀(např. 4x² + 2x + 6)
a hodnotu x, pro kterou chceme daný polynom vypočítat(např. x = 3)
Algoritmus je založen na tom, že je polynom zapsaný v nořené formě(vytýkám x):
Polynom P(x) = a0 + a1 * x + a2 * x^2 + ... + an * x^n se postupním vytýkáním x dá zapsat jako: 
P(x) = a0 + x * (a1 + x * (a2 + ... + x * (a(n-1) + an * x)...)). 
Začínám od koeficientu a0 ale pro lepší představu můžu začít od koeficentu aₙ.
Mějme polynom 4x² + 2x + 6, který lze zapsat jako 6 + x(2 + 4x) a x = 2. 
Začnu tím, že do proměnné výsledek uložím
koeficent aₙ tedy 4. To je inicializace proměnné. 
Poté výsledek vynásobím x (2) a přičtu koeficent o an-1 (2). 
V dalším kroku výsledek vynásobím x (2) a přičtu koeficient an-2 (6) a program skončí. 
Algoritmus funguje jako by postupoval člověk, 
který začne v nejíc zanořené závorce koeficient vynásobí x a přičte koeficent an-1, 
který je v závorce. Následně si výsledek pamatuje, zapíše si ho a pokračuje dál tím, 
že výsledek vynásobí x a přičte koeficent v závorce, dokud nedojde na konec. 
Stejně funguje algoritmus:
```java
// Hornerovo schéma pro vyhodnocení polynomu
double[] coefficients = {3, -2, 5, -1, 7};  // Coefficients: a4 = 3, a3 = -2, a2 = 5, a1 = -1, a0 = 7
double x = 2;  // Value of x for which to evaluate the polynomial
public static double evaluatePolynomialFromAn(double[] coefficients, double x) {
    double result = coefficients[coefficients.length - 1];  // Start with the highest degree coefficient an
    
    // Loop through each coefficient starting from an-1 to a0
    for (int i = coefficients.length - 2; i >= 0; i--) {
        result = result * x + coefficients[i];  // Horner's method step
    }
    
    return result;  // Final result is the value of the polynomial at x
}
```

Složitost: O(n), kde n je stupeň polynomu.

## Eratosthenovo síto
Efektivní algoritmus pro nalezení všech prvočísel do určitého limitu.
- Začnu procházet boolean pole, kde ze začátku jsou všechna čísla potencionálně prvočísla. Když příjdu k číslu (začátek 2), které dosud nebylo vyškrtnutp(hodnota by byla false), zapíšu si toto číslo jako prvočíslo a vyškrtám všechny jeho násobky. Algoritmus pokračuje dál (číslo 3) zapíše si ho jako prvočíslo a všechny jeho násobky vyškrtá(nastaví na false). To je základní princip síta. Jsou tu dvě zásadní optimalizace:
  1) Vždy když začínám vyškrtávat násobky daného prvočísla nemusím začínat od prvního násobku ale od druhé mocniny daného prvočísla. Např. pro číslo 3 bych mohl začít vyšktávat 6, 9, ... . Všimněme si ale, že můžu začít vyškrtávat od 9 (3 2), protože 6 byla již vyškrtnutamenším menším prvočíslem(v tomto případě číslem 2).
  2) Když dojdu k číslu 11 a n = 100, pak 11 2 je 121. 121 > 100, takže nemá smysl pokračovat. Ani u vyškrtnuté desítky již nemá smysl pokračovat. Proto nechodím ve for cyklu do n ale do odmocniny z n(místo i < n použiji i*i < n). Protože, když je n např 100, tak u čísla 10 už mám všechny čísla co nejsou prvočísla nastavené na false a všechna prvočísla na true a můžu vypsat pole, kde je hodnota true, čímž jsem získal všechny provčísla pro n = 100.

```java
// Implementace Eratosthenova síta
boolean[] eratosthenovoSito(int n) {
    boolean[] jePrvocislo = new boolean[n + 1];
    
    // Inicializace - všechna čísla jsou potenciálně prvočísla
    for (int i = 2; i <= n; i++) {
        jePrvocislo[i] = true;
    }
    
    // Hlavní průchod sítem
    for (int p = 2; p * p <= n; p++) {
        // Pokud p je prvočíslo, škrtáme jeho násobky
        if (jePrvocislo[p]) {
            // Začneme od p*p, protože menší násobky už byly škrtnuty
            for (int i = p * p; i <= n; i += p) {
                jePrvocislo[i] = false;
            }
        }
    }
    
    return jePrvocislo;
}
```
![Eratosthenovo sito animace](https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif)
Složitost: O(n log log n)
Paměťová: O(n)

Výhody:
- Velmi efektivní pro hledání prvočísel do velkého limitu
- Jednoduchá implementace

Nevýhody:
- Paměťově náročné pro velká čísla

## Eukleidův algoritmus
Algoritmus pro nalezení největšího společného dělitele (NSD) dvou čísel.
Algoritmus vychází z tohoto? NSD(150, 15) = NSD(15, 150 - qv) = NSD(15, 150 - 10*15) = NSD(15, 0) = 15

### Iterativní varianta
```java
// Iterativní Eukleidův algoritmus
int nsd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

Složitost: O(log(min(a, b)))

První iterace:
    a = 50, b = 30
    a % b = 50 % 30 = 20
    Nové hodnoty: a = 30, b = 20
Druhá iterace:
    a = 30, b = 20
    a % b = 30 % 20 = 10
    Nové hodnoty: a = 20, b = 10
Třetí iterace:
    a = 20, b = 10
    a % b = 20 % 10 = 0
    Nové hodnoty: a = 10, b = 0

Opakovaně prováděné operace nemění hodnotu největšího společného dělitele (neboť NSD(u, v) = NSD(v, u – qv) pro libovolné celé číslo q), ale v každém kroku se hodnota proměnné v sníží, takže je zřejmé, že v konečném počtu kroků se algoritmus ukončí s tím, že v je nulové. Tehdy obsahuje proměnná u největší společný dělitel, neboť NSD(u, 0) = u, což musí být současně největší společný dělitel původních čísel, neboť, jak už bylo uvedeno, hlavní smyčka programu hodnotu největšího společného dělitele nemění. 

## Testy prvočíselnosti

Zde zase stačí jít do odmociny z n. Místo i < odmocnina z n uděláme i*i < n.
### Naivní test
```java
boolean jePrvocislo(int n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    
    // Testujeme dělitelnost pouze lichými čísly většími než 3
    for (int i = 5; i * i <= n; i += 2) {
        if (n % i == 0) return false;
    }
    
    return true;
}
```

Složitost: O(√n)

## Rozklad na prvočísla
Algoritmus pro rozklad čísla na prvočísla.

```java
// Rozklad čísla na prvočísla
void rozkladNaPrvocisla(int n) {
    // Zpracování násobků 2
    while (n % 2 == 0) {
        System.out.print(2 + " ");
        n /= 2;
    }
    
    // Testování lichých čísel od 3
    for (int i = 3; i * i <= n; i += 2) {
        while (n % i == 0) {
            System.out.print(i + " ");
            n /= i;
        }
    }
    
    // Pokud zbylo prvočíslo větší než 2
    if (n > 2) {
        System.out.print(n);
    }
}
```

Složitost: O(√n)

## Bitové operace
Bitové operace pracují s čísly na úrovni jednotlivých bitů:

- `&` (AND): pokud jsou oba bity 1, výsledek je 1, jinak 0
- `|` (OR): pokud je alespoň jeden bit 1, výsledek je 1, jinak 0
- `^` (XOR): pokud jsou bity různé, výsledek je 1, jinak 0
- `~` (NOT): invertuje všechny bity
- `<<` (levý posun): posune bity doleva
- `>>` (pravý posun): posune bity doprava (zachovává znaménko)
- `>>>` (pravý posun bez znaménka): posune bity doprava a doplní nuly

### Příklady použití

1. **Test sudosti/lichosti čísla**:
```java
boolean jeSude(int n) {
    return (n & 1) == 0;
}
```

2. **Výpočet 2^n**:
```java
int mocnina2(int n) {
    return 1 << n;
}
```

3. **Nastavení bitu na určité pozici**:
```java
int nastavBit(int cislo, int pozice) {
    return cislo | (1 << pozice);
}
```

4. **Test, zda je bit na určité pozici nastaven**:
```java
boolean jeBitNastaven(int cislo, int pozice) {
    return (cislo & (1 << pozice)) != 0;
}
```

5. **Přepnutí hodnoty bitu na určité pozici**:
```java
int prepniBit(int cislo, int pozice) {
    return cislo ^ (1 << pozice);
}
```

### Využití bitových operací
- Rychlá matematika: násobení/dělení mocninami 2 pomocí posunů
- Optimalizace paměti: ukládání příznaků do jedné proměnné
- Datové struktury: implementace Eratosthenova síta pomocí bitového pole
- Kryptografie: šifrování dat
- Počítačová grafika: míchání barev, masek apod.
- Hašování a kontrolní součty

## Rozdíl mezi dělením a celočíselným dělením

### Dělení (`/` s reálnými čísly)
- Pracuje s reálnými čísly (float, double)
- Vrací výsledek jako reálné číslo s desetinnou částí
- Příklad: 5.0 / 2.0 = 2.5

### Celočíselné dělení (`/` s celými čísly)
- Pracuje s celými čísly (int, long)
- Vrací pouze celočíselnou část (zaokrouhluje dolů k nule)
- Příklad: 5 / 2 = 2
- Zbytek po dělení se získá operátorem modulo `%`
- Příklad: 5 % 2 = 1

```java
// Ukázka rozdílu
int celočíselnéDělení = 5 / 2;       // Výsledek: 2
double reálnéDělení = 5.0 / 2.0;     // Výsledek: 2.5
int zbytek = 5 % 2;                  // Výsledek: 1
```
