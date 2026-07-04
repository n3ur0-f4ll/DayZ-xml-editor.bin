<p align="center">
  <img src="img/logo.png" alt="DayZ XML Editor" width="140">
</p>

<h1 align="center">DayZ XML Editor</h1>

<p align="center">
  Wieloplatformowy edytor plików XML ekonomii (Central Economy) serwera DayZ.<br>
  Zakładki dla <b>types</b>, <b>events</b>, <b>cfgeventspawns</b>,
  <b>cfgeventgroups</b>, <b>cfgplayerspawnpoints</b>, <b>cfgspawnabletypes</b>,
  <b>cfgrandompresets</b>, <b>cfglimitsdefinition</b>, <b>globals</b> i
  <b>cfgignorelist</b> — z trybem misji, edycją masową, walidacją między
  plikami, cofaniem zmian i ciemnym/jasnym motywem.
</p>

<p align="center">
  <img src="img/screenshot.png" alt="Zrzut ekranu" width="860">
</p>

---

## ⬇️ Pobieranie (dla użytkowników)

Nie musisz instalować Pythona ani niczego dodatkowego — pobierasz gotowy plik.

1. Wejdź do zakładki **[Releases](../../releases)** tego repozytorium.
2. Pobierz plik dla swojego systemu:
   - **Windows** → `DayZEditor.exe`
   - **Linux** → `DayZEditor-x86_64.AppImage`
3. Uruchom:
   - Windows: dwuklik w `DayZEditor.exe`.
   - Linux: nadaj prawo wykonania (`chmod +x DayZEditor-x86_64.AppImage`) i uruchom.

> **Windows SmartScreen** przy pierwszym uruchomieniu może pokazać niebieskie
> okno „Windows chronił Twój komputer". To normalne dla niepodpisanego programu —
> kliknij **Więcej informacji → Uruchom mimo to**.

## ✨ Funkcje

- **Tryb misji** (`File → Open Mission Folder`) — wskaż folder misji, a narzędzie
  samo wczyta `db/types.xml`, `db/events.xml`, `cfgeventspawns.xml`,
  `cfgeventgroups.xml`, `cfgplayerspawnpoints.xml`, `cfgspawnabletypes.xml`,
  `cfgrandompresets.xml`, `cfglimitsdefinition.xml`, `db/globals.xml` i
  `cfgignorelist.xml` do właściwych zakładek. **Save All** (`Ctrl+Shift+S`)
  zapisuje wszystkie zmienione pliki jednym skrótem, a **File → Open Recent
  Mission** pamięta ostatnio otwierane foldery. Rozmiar okna i położenie suwaka
  są zapamiętywane między uruchomieniami.
- **Dziesięć edytorów w zakładkach** — types, events, event spawns, event groups,
  player spawns, spawnabletypes, random presets, definitions, globals, ignore
  list; każda z własną historią zmian i znacznikiem `*`.
- **Tabela wszystkich wpisów** z sortowaniem po kolumnach; flagi klikasz
  bezpośrednio w tabeli (kolumny checkbox).
- **Wyszukiwanie po nazwie** oraz filtry (np. `category`/`usage`/`value`)
  łączone warunkiem I.
- **Edycja pojedyncza** — panel ze wszystkimi polami wpisu:
  - *types*: skalary, flagi oraz listy `category/usage/value/tag` (z podpowiedziami
    z cfglimitsdefinition);
  - *events*: skalary, `position`/`limit`, flagi oraz **tabela `children`**;
  - *spawnabletypes*: **drzewo `attachments`/`cargo`** z itemami i szansami
    (pole Name podpowiada classname'y z types.xml);
  - *event spawns*: **tabela pozycji** `<pos x z a>` dla eventu
    (pole Name podpowiada nazwy z events.xml);
  - *player spawns*: grupy punktów spawnu graczy (fresh/hop/travel) z tabelą
    pozycji;
  - *random presets*: presety `cargo`/`attachments` z polem `chance` i tabelą
    itemów (pole Preset w spawnabletypes podpowiada nazwy presetów);
  - *event groups*: grupy obiektów (wraki, konwoje) z tabelą `<child>`;
  - *globals*: zmienne strojenia CE — edytowalne kolumny `type`/`value`;
  - *ignore list*: klasy ignorowane przez sprzątanie CE
    (pole Name podpowiada classname'y z types.xml).
- **Edycja masowa** — zaznacz wiele wpisów (lub ustaw filtr) i zmień je naraz:
  ustaw / dodaj / pomnóż wartości liczbowe, włącz/wyłącz flagi, dodaj/usuń/zamień
  wartości list.
- **Duplikowanie** wpisu (`Ctrl+D`); **`Ctrl+N`** — nowy wpis.
- **Kopiuj/wklej wpisy** (`Ctrl+C`/`Ctrl+V`, gdy tabela ma fokus) — także
  **między misjami**: skopiuj wpisy, otwórz inną misję (`Open Recent`), wklej.
  Wklejanie tylko do zakładki tego samego typu; przy kolizji nazw dopisek `_copy`.
- **Walidacja + synergia między plikami** — duplikaty, złe zakresy, eventy bez
  `children`; wartości `usage/value/category/tag` spoza cfglimitsdefinition
  (uwzględnia też grupy z `cfglimitsdefinitionuser.xml`); wpisy spawnabletypes
  bez `<type>` w types.xml **lub z `preset` spoza cfgrandompresets.xml**; pozycje
  eventów bez `<event>` w events.xml; klasy ignore list spoza types.xml; złe
  `chance` w presetach oraz `type`/`value` w globals.
  **Validate All** (`Ctrl+Alt+V`) sprawdza wszystkie zakładki naraz, z filtrem
  Errors/Warnings i licznikiem problemów na etykiecie zakładki; kliknięcie
  problemu przeskakuje do właściwej zakładki i zaznacza wpis.
- **Cofnij / Ponów** (`Ctrl+Z` / `Ctrl+Y`) dla każdej zmiany.
- **Ciemny / jasny motyw** — przełącznik w menu **View**, zapamiętywany między
  uruchomieniami.
- **Bezpieczny zapis** — zachowuje komentarze i formatowanie pliku; zmiana
  jednego pola daje jednoliniowy diff, w pełni czytelny dla silnika DayZ.

## 📚 Który plik do czego? (i jak się łączą)

Wszystkie te pliki obsługuje **Central Economy (CE)** — silnik DayZ, który
decyduje, *co*, *gdzie*, *ile* i *jak długo* pojawia się na mapie (loot,
eventy, pojazdy, zombie/zwierzęta). Poniżej co robi każda zakładka i jak wpływa
na grę.

| Zakładka | Plik | Za co odpowiada | Wpływ na grę |
|---|---|---|---|
| **Types** | `db/types.xml` | Główna baza przedmiotów: dla każdej klasy `nominal` (ile ma być), `min`, `lifetime` (jak długo leży), `restock`, `cost` + flagi (`count_in_map`, `deloot`…) i etykiety `usage/value/tag/category`. | Ustala, które przedmioty w ogóle istnieją w ekonomii i ich bazowe ilości/czas życia. To „serce" lootu. |
| **Events** | `db/events.xml` | Dynamiczne eventy (wraki helikopterów, skażone strefy, konwoje, dostawy): budżet (`nominal/min/max`), `lifetime`, `restock` i tabela `<children>` z klasami do zrodzenia. | Definiuje *co i ile* pojawia się w ramach eventów — poza normalnym lootem budynkowym. |
| **Event spawns** | `cfgeventspawns.xml` | Współrzędne `<pos x z a>` (i strefy) dla każdego eventu z `events.xml`. | Mówi silnikowi *gdzie* na mapie dany event może się pojawić. |
| **Event groups** | `cfgeventgroups.xml` | Nazwane, gotowe układy obiektów (`<group>` z `<child>` o offsetach): np. cały konwój wraków, obstawa obozu. | Pozwala jednym eventem postawić całą scenę z wielu obiektów naraz, zamiast pojedynczych sztuk. |
| **Player spawns** | `cfgplayerspawnpoints.xml` | Punkty spawnu graczy (świeży start / hop / travel) z listą pozycji. | Decyduje, *gdzie* w świat wchodzą nowi i odradzający się gracze. |
| **Spawnable** | `cfgspawnabletypes.xml` | Dla przedmiotów z `types.xml`: co ma w sobie (`cargo`), co ma zamontowane (`attachments`), losowe damage/ilości. | Sprawia, że np. zrodzona broń ma magazynek/celownik, a plecak — zawartość. |
| **Random presets** | `cfgrandompresets.xml` | Wielokrotnego użytku, ważone (`chance`) zestawy `cargo`/`attachments`. | Daje różnorodność wyposażenia bez wpisywania tego samego w każdym `cfgspawnabletypes`. |
| **Definitions** | `cfglimitsdefinition.xml` | Słownik dozwolonych wartości `usage / value / tag / category` (+ aliasów z `cfglimitsdefinitionuser.xml`). | Definiuje „język", którym `types.xml` opisuje, *gdzie* i *w jakim tierze* rzadkości rzecz może się pojawić. |
| **Globals** | `db/globals.xml` | Zmienne strojenia CE dla całego serwera: limity (`AnimalMaxCount`, `ZombieMaxCount`), timery sprzątania (`CleanupLifetime*`), `InitialSpawn`, `RespawnLimit`. | Ustawia globalne ramy ekonomii — wspólne dla wszystkich graczy naraz. |
| **Ignore list** | `cfgignorelist.xml` | Klasy, które CE ma **pomijać** (nie liczyć, nie sprzątać, nie traktować jak loot). | Wyłącza pojedyncze klasy z ekonomii bez kasowania ich z `types.xml`. |

### Jak te pliki się ze sobą łączą

- **Definitions → Types → mapa.** `cfglimitsdefinition.xml` (+ `…user.xml`)
  ustala słownik `usage/value/tag/category`; `types.xml` przypina te etykiety do
  przedmiotów; punkty lootu na mapie mają te **same** etykiety, więc rzecz spawnuje
  się tam, gdzie się „zgadza" (np. `usage=Military`, `value=Tier3`). Wpisanie
  etykiety spoza słownika = przedmiot się nie pojawi tam, gdzie chcesz.
- **Types → Spawnable → Random presets.** `types.xml` to lista klas; gdy
  przedmiot się rodzi, `cfgspawnabletypes.xml` dokłada mu `cargo`/`attachments`,
  a przez `preset="…"` sięga do wspólnego zestawu z `cfgrandompresets.xml`.
- **Events → Event spawns → Event groups.** `events.xml` mówi *co i ile*,
  `cfgeventspawns.xml` *gdzie*, a `cfgeventgroups.xml` dostarcza *gotowy układ
  wielu obiektów* dla eventów obiektowych.
- **Globals** nakłada globalne limity i timery na wszystko powyższe;
  **Ignore list** wypina wybrane klasy z całego mechanizmu.

> 💡 Program **pilnuje właśnie tych powiązań**: ostrzega, gdy `types.xml` używa
> etykiety spoza `cfglimitsdefinition`, gdy `cfgspawnabletypes` odwołuje się do
> nieistniejącego `<type>` lub `preset`, gdy event nie ma pozycji, albo gdy klasa
> z ignore list nie istnieje w `types.xml` — a pola z nazwami podpowiada
> automatycznie między plikami.

---

## 🖱️ Szybki start

1. **File → Open Mission Folder** i wskaż folder misji
   (np. `mpmissions/dayzOffline.chernarusplus/`) — zakładki wypełnią się same.
   Alternatywnie **File → Open** wczytuje pojedynczy plik do aktywnej zakładki.
2. Edytuj pojedynczo (panel po prawej → **Apply changes**) lub masowo
   (**Bulk edit…**).
3. **Validate**, żeby sprawdzić poprawność (w tym niespójności między plikami).
4. **File → Save** (`Ctrl+S`) zapisuje aktywną zakładkę, a **Save All**
   (`Ctrl+Shift+S`) — wszystkie zmienione naraz. Tytuł okna i zakładka pokazują
   `*`, gdy są niezapisane zmiany.

---

## 📄 Licencja

Oprogramowanie **własnościowe** (proprietary) — **nie** jest to projekt
open-source. Możesz **pobierać i używać** programu za darmo do własnych,
prywatnych celów, ale **nie wolno** go modyfikować, redystrybuować ani poddawać
inżynierii wstecznej. **Użycie komercyjne wymaga pisemnej zgody autora.**

Pełne warunki: [LICENSE](LICENSE). Komponenty firm trzecich (PySide6/Qt, lxml,
Python, PyInstaller) pozostają pod własnymi licencjami — zob.
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

© 2026 Johny Python (JPython). Wszelkie prawa zastrzeżone.
DayZ jest znakiem towarowym Bohemia Interactive a.s.; to narzędzie jest
niezależne i niepowiązane z Bohemia Interactive.
