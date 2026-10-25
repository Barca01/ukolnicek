Implement this design in this codebase:
https://claude.ai/code/artifact/5e80a4b1-4630-4b12-89a4-c17362f8979e

Read the design's files from that link. Match how it looks, using this codebase's own components and conventions.

Vytvoř jednoduchou webovou aplikaci "úkolníček" jako jediný soubor index.html (HTML, CSS a vanilla JavaScript, bez externích knihoven, bez serveru a bez internetu při běhu). Rozhraní je v češtině, datumy ve formátu "so 24. 10." Aplikace je určená pro jednu uživatelku na iPhonu (Safari, později přidaná na plochu), takže navrhuj mobile-first: velké dotykové plochy (min. 44 px), žádné hover efekty, respektuj safe-area, přidej meta tagy pro spuštění z plochy jako samostatná aplikace (apple-mobile-web-app-capable, název, viewport). Vzhled obrazovek přesně následuj přiloženými screenshoty návrhu a referenčním obrázkem koťátka (viz sekce PŘÍLOHY).

PRINCIP
Každý den má svůj seznam úkolů. Uživatelka je den předem nebo ráno téhož dne projde, určí pořadí a nepotřebné úkoly přesune na jiný den. Teprve potom vytvoří diagram a začne úkoly odškrtávat. Do seznamu se může vrátit kdykoliv.

DVA REŽIMY DNE

1. SEZNAM (plánování)
- Nahoře je přepínač dnů: šipky předchozí/další den, zobrazené datum (den v týdnu, datum) a tlačítko "Dnes". Jde se libovolně do minulosti i budoucnosti.
- Přidání úkolu: pod přepínačem je vstupní pole "Přidat úkol…" a vedle něj čtvercové tlačítko +. Klepnutí na + nebo Enter na klávesnici přidá úkol na úplný konec seznamu daného dne (i za splněné úkoly) s dalším pořadovým číslem. Text se ořízne o mezery na krajích. Pole se po přidání vyprázdní a zůstane aktivní (klávesnice se nezavře), aby šlo hned psát další úkol. Seznam se posune tak, aby byl nový úkol vidět. Při prázdném poli (nebo jen z mezer) se po klepnutí na + nestane nic a tlačítko + je v tomto stavu bledé. Pole pro přidání je jen v režimu Seznam.
- Každý úkol je karta s: úchytem k přetažení, kroužkem s pořadovým číslem, textem a tlačítkem "Přesunout na jiný den" (ikona kalendáře se šipkou; otevře nativní výběr data, úkol se přesune na zvolený den a zachová se mu stav a pozice se zařadí na konec cílového dne). Pořadí jde měnit přetažením za úchyt, plus tlačítka nahoru/dolů jako záloha pro dotyk.
- Klepnutí na text úkolu otevře úpravu: změna textu, volitelný "Krátký název pro diagram" (max. cca 30 znaků) a tlačítko Smazat (s potvrzením).
- Splněné úkoly jsou v seznamu vidět odškrtnuté (přeškrtnutý text barvou škrtu, tlumené pozadí, odškrtnutý kroužek) a nejdou přesunout na jiný den.
- Seznam se při větším počtu úkolů posouvá (scroll), zatímco pole pro přidání nahoře a tlačítko dole zůstávají na místě.
- Velké tlačítko "Vytvořit diagram" dole (aktivní jen když je v seznamu aspoň jeden úkol) přepne do režimu Diagram.
- Prázdný den: jen prázdný seznam s polem pro přidání úkolu, bez dalšího textu a ilustrací.

2. DIAGRAM
- Vzhled: bílé pozadí jako čtverečkovaný sešit (jemná modrošedá mřížka #DCE6F2, čtverce 16 px). Diagram vypadá jako nakreslený rukou fixkou.
- Bubliny: každý úkol je bublina, tedy skoro kulatý, ale nepravidelný tvar (ne přesná elipsa), s černým okrajem (#1B1B24) jako od fixky. Tvar generuj procedurálně z elipsy s náhodným posunem kontrolních bodů, aby žádné dvě bubliny nevypadaly stejně. Konec čáry mírně přesahuje její začátek, jako když se kruh nakreslí rukou. Seed tvaru ulož u úkolu, aby se tvar po překreslení nezměnil. Všechny nesplněné bubliny mají stejné bílé pozadí.
- Rozmístění: bubliny nejsou přesně pod sebou, ale rozmístěné cik-cak jako na přiloženém screenshotu diagramu: předchozí (splněná) vlevo nahoře, aktuální vpravo uprostřed, další vlevo dole. Rozložení musí zůstat přehledné a vejít se na šířku iPhonu (390 px). Mezi bublinami jsou ručně nakreslené šipky (mírně prohnuté, s hrotem z krátkých čar).
- Zobrazují se jen tři bubliny najednou: poslední splněná před aktuálním úkolem (pokud existuje), aktuální úkol (první nesplněný v pořadí) a úkol po něm (pokud existuje). Po splnění se okno plynule posune dál.
- Aktuální úkol je nejvýraznější: největší bublina (cca 250×170 px), tlustší černý okraj, větší písmo. Další úkol je menší (cca 220×150 px) s tenčím černým okrajem. Šipka z aktuálního k dalšímu je černá a plná, šipka ze splněného k aktuálnímu je šedá a méně výrazná.
- Splněná bublina je barevně tlumená: okraj šedý (#7D8096), text šedý (#4A4D63) zůstává čitelný, přes celou bublinu je nakresleno šrafování fixkou: asi 5 mírně prohnutých, šikmých čar (zleva dole doprava nahoru, tloušťka cca 3,6 px, zakulacené konce), jako by je kreslil člověk. Čáry nejsou dokonale rovné ani rovnoběžné, mají nestejné rozestupy a nestejnou délku. Některé končí kousek před okrajem bubliny, jiné ho lehce přesahují. Barva šrafování se pro každý úkol náhodně vybere z jemné palety (např. #E8836B korálová, #4FB3A5 tyrkysová, #E3B04B žlutá, #8E7CC3 fialová, #5B9BD5 modrá) a uloží se spolu s tvarem čar (seed), aby se po překreslení nezměnila. Šrafování se při splnění plynule "nakreslí".
- Text v bublině se zalamuje. Dlouhé názvy se zkracují lokálním pravidlem bez volání AI: nad cca 40 znaků se zkrátí na hranici slova s "…"; celý název se zobrazí po klepnutí a podržení nebo v popisku pod diagramem. Pokud má úkol vlastní krátký název, použije se ten.
- Splnění úkolu: bublinu je potřeba klepnout DVAKRÁT (dvojité klepnutí), aby ke splnění nedošlo omylem. Jedno klepnutí neudělá nic (pouze lehký vizuální náznak). Pod diagramem je krátký text "Dvojitým klepnutím na bublinu úkol splníš."
- Vrácení splnění: dvojité klepnutí na poslední splněnou bublinu splnění zruší (šrafování zmizí).
- Horní lišta: vlevo tlačítko "Upravit seznam" (vrátí do režimu Seznam), vpravo datum. Pod ní postup "1 z 5 hotovo" s tenkým proužkem. Texty na čtverečkovaném papíru mají bílý podklad, aby je mřížka nerušila.
- Při návratu do Seznamu a dalším vytvoření diagramu zůstávají splněné úkoly splněné (stav patří k úkolu, ne k diagramu). Nové nebo přeskupené úkoly se zařadí podle nového pořadí a aktuální úkol je vždy první nesplněný v pořadí.

ODMĚNY (koťátko)
Postava je originální koťátko nakreslené jako inline SVG, ve stylu moderních animovaných filmů: roztomilé, trochu realistické, chlupaté a huňaté. Použij přiložené soubory kotatko-roztomile.svg, kotatko-smutne.svg a kotatko-stastne.svg jako výchozí grafiku (smíš je vylepšit) a referenční foto koťátka jako vzor vzhledu. Nekopíruj žádnou existující postavu ani maskota. Popis vzhledu: zrzavé mourovaté koťátko s tmavšími pruhy, krémová tlamička a hruď, velké kulaté oči s černými zorničkami a výraznými odlesky, růžový nosík, uvnitř růžové uši, chlupaté kštičky na tvářích, dlouhé vousky, pruhovaný huňatý ocas, jemné stínování bez černých obrysů.
- Roztomilé koťátko: při každém splnění úkolu je 50% šance, že se přes diagram zobrazí asi na 4 sekundy (sedí s packami u brady, velké oči, jemně se pohupuje). Uloží se u úkolu, že odměna byla zobrazena.
- Smutné, vyčítavě se tvářící koťátko: zobrazí se asi na 4 sekundy, když uživatelka zruší splnění úkolu, u kterého předtím roztomilé koťátko viděla (polozavřená víčka, svraštělé obočí, svěšené uši, pokleslá pusa, slza). Pak se u úkolu příznak odměny smaže. Pokud koťátko neviděla, nezobrazí se nic. Při dalším splnění se 50% šance losuje znovu.
- Šťastné koťátko na louce: když je splněn poslední úkol dne, zobrazí se asi na 4 sekundy scéna s šťastným koťátkem (zavřené oči, široký úsměv, skáče), které je na louce kolem řeky, za ním duha. V tomto případě se zobrazí vždy (místo běžného koťátka).
- Všechny odměny se zobrazují jako překryv přes diagram (průsvitný bílý závoj + postava), nezavírají se klepnutím a nejdou vypnout. Překryv nesmí blokovat další ovládání (pointer-events: none) a po uplynutí času sám zmizí.

UKLÁDÁNÍ
- Vše se ukládá do localStorage a musí vydržet minimálně několik týdnů. Ukládej po každé změně.
- Datový model: úkoly mají id, text, volitelný krátký název, datum (YYYY-MM-DD), pořadí v rámci dne, příznak splněno, čas splnění, barvu a seed šrafování, seed tvaru bubliny a příznak "odměna zobrazena". Pro každý den se ukládá, zda je diagram vytvořen.
- Úkoly z minulých dnů a jejich splnění zůstávají zachované a lze je zpětně prohlížet. Nesplněné úkoly se nepřesouvají automaticky, uživatelka je přesune ručně.
- Protože Safari může data webu po delší době nepoužívání smazat, přidej v malém menu (tlačítko se třemi tečkami vpravo nahoře v režimu Seznam) dvě tlačítka: "Zálohovat" (zobrazí/zkopíruje všechna data jako text JSON) a "Obnovit ze zálohy" (vložení tohoto textu). Jinak žádné další nastavení.

DESIGN
- Seznam a obecné ovládací prvky: klidné, nerušivé, světlé pozadí #F6F7FB, jedna hlavní barva indigo #4A4FB8 (tlačítka, aktuální číslo), text #23243A, jemné okraje #E3E5EF, zaoblené rohy 16–18 px, hodně prázdného místa.
- Písmo: zaoblené bezpatkové (Nunito, jinak ui-rounded / systémové), dobrý kontrast, žádné agresivní animace.
- Respektuj prefers-reduced-motion (šrafování se jen zobrazí bez animace, koťátka se zobrazí bez pohybu).
- Plně použitelné s dotykovým ovládáním; základní ovládání i klávesnicí.

PŘÍLOHY (přikládám k promptu)
1. Pět screenshotů návrhu: Seznam, Diagram, Odměna, Vrácení splnění, Vše splněno.
2. Referenční foto koťátka (vzor vzhledu postavy).
3. Soubory kotatko-roztomile.svg, kotatko-smutne.svg, kotatko-stastne.svg.

MIMO ROZSAH (nepřidávej)
Uživatelské účty, přihlášení, kategorie, priority, termíny a notifikace, opakující se úkoly, statistiky, sdílení, zvuky, další nastavení.

VÝSTUP
Dodej kompletní kód v jednom souboru. Na konci stručně napiš, jak aplikaci dostat na iPhone (hostování a přidání na plochu) a jak funguje zálohování.