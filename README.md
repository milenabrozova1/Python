 Datová analýza nabídky bytů ve Zlíně (Sreality.cz)

## Cíl projektu
Cílem je získat aktuální inzeráty pronájmů bytů ve Zlíně ze Sreality.cz, data vyčistit (ETL) a odpovědět na analytické otázky pomocí pandas a vizualizací.

## Použité nástroje
- Python
- requests, BeautifulSoup (web scraping)
- pandas (ETL a analýza)
- matplotlib / seaborn (grafy)

## 1) Web scraping
**Zdroj:** Sreality.cz – pronájmy bytů ve Zlíně  
Scraping zahrnoval stránkování (pagination) a u každého inzerátu se ukládalo:
- URL
- rozměry v m²
- dispozice (1+kk, 2+kk, …)
- cena v Kč
- lokace (ulice + město/část)
**Výstup:** `Brozova_Milena_surova_data.csv`

## 2) ETL (Extract, Transform, Load)
### Extract
Načtení surových dat z CSV a kontrola (počet řádků/sloupců, náhled, datové typy).

### Transform (čištění)
- převedení `cena_kc` na číselný typ
- převedení `rozmera_m2` na číselný typ
- normalizace dispozic (sjednocení zápisu)
- rozdělení `lokace` na `ulice` a `mesto`
- zpracování chybějících hodnot (řádky bez ceny/rozměru/dispozice odstraněny, protože by nešly použít v analýze)
- data omezena pouze na město Zlín (pokud se vyskytlo okolí)

### Load
Export vyčištěných dat.
**Výstup:** `Brozova_Milena_zdrojova_data.csv`

## 3) Analýza dat 
### 3.1 Průměrná a mediánová cena
Průměrná cena za pronájem ve výši 13 201 Kč je vyšší než mediánová cena   12 500 Kč.
Medián je odolnější vůči extrémům, průměr může zvyšovat několik dražších bytů.

### 3.2 Průměrná cena podle dispozice
Obecně vyšší dispozice mají vyšší cenu, rozdíly ovlivňuje lokalita/stav/vybavení. 

### 3.3 Průměrná velikost podle dispozice
Větší dispozice mají očekávaně větší plochu.

### 3.4 Dražší byty (nad průměrnou cenou) – top 5 ulic
Definice “dražší” značí situaci, kdy je nájemní cena  vyšší než průměrná cena.  
Vygenerováno 5 TOP ulic s dražším nájmem:
Dlouhá        17000.0
Zálešná I     17000.0
Družstevní    17000.0
L. Váchy      17000.0
J. A. Bati    17000.0

### 3.5 Nejčastější dispozice
Nejčastěji inzerovanými dispozicemi jsou byty 1+kk v rozsahu 23,6% a jako druhé v pořadí v 20,3 % dispozice 2+kk.
Tyto menší dispozice často bývají nabízeny, protože jsou takzv investičními byty právě na pronájem. Dále se může jednat  o byty, z kterých se lidé stěhují buď do větších bytů nebo rodinných domů.

### 3.6 Byty nad 20 000 Kč , zahrnuty 2+1/2+kk
Ve vyčištěných datech se v datasetu nenacházel žádný pronájem vyšší než 20 000 Kč.
Při manuální kontrole webového rozhraní portálu Sreality.cz se však v daném období vyskytují i dražší inzeráty. Tento rozdíl poukazuje na omezení aktuální verze web scrapingu, kdy u některých větších a dražších bytů nebyla cena korektně načtena z detailní stránky inzerátu. V případném opakování procesu by bylo vhodné rozšířit scraping o extrakci ceny přímo z detailu inzerátu.

### 3.7 Min/max cena podle dispozice + největší rozptyl
Větší rozptyl cen je sledován u bytů menších dispozic. Rozptyl se snižuje od dispozice 4+1.

Při analýze dat byla shledána odchylka dat pro pronájem bytu 5+kk, který se fakticky na webu sreality,cz vyskytoval, ale stažená data o něm neodpovídala realitě. Při případném opakování procesu je třeba se na tuto odchylku zaměřit.

I přes případné odchylky lze zhodnotit, že ve městě Zlín se nejvíce nabízejí k pronájmu byty s menší dispozicí a to v průměru za 13 023 Kč až 13 397 Kč. Existují lokality, resp. Ulice, které jsou zřejmě žádanější a proto je jejich nabídková cena k pronájmu vyšší než cena průměrná.

# Python
