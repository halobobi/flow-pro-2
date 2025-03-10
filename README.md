# Power Apps Fejlesztési Útmutató

## Bevezetés
A Power Apps egy Microsoft által fejlesztett low-code/no-code platform, amely lehetővé teszi egyedi üzleti alkalmazások gyors és hatékony fejlesztését. Az alábbiakban részletes útmutatót találsz a platform használatához.

## Kezdeti lépések

### Hozzáférés és Környezet
- **Elérés**: https://make.powerapps.com
- **Környezet típusok**:
  - **Solution (Megoldás)**: Komplex alkalmazásokhoz, Dataverse táblákkal
  - **Standalone App**: Egyszerűbb alkalmazásokhoz, külső adatforrásokkal

### Alkalmazás létrehozása
- **Új app létrehozási lehetőségek**:
  - Üres alkalmazás (Blank)
  - Adatforrás alapú
  - Figma design alapú
- **Adattárolási lehetőségek**:
  - SharePoint listák
  - Excel táblák
  - Dataverse (Solution esetén)

## Fejlesztői Felület

### Felület Áttekintés
- **Bal oldali panel**: 
  - Fastruktúra
  - Új elemek hozzáadása
  - Adatok kezelése
  - Változók definiálása
    - Globális változók
    - Környezeti változók
    - Formulák (egyéni függvények)

- **Középső terület (Canvas)**:
  - Vizuális elemek:
    - Gombok
    - Szövegbeviteli mezők
    - Címkék (Label)
    - Legördülő listák (Dropdown)
    - Kombinált listák (Combobox)

### Nyelvi Beállítások
- Alapértelmezetten a szervezeti vagy számítógép nyelvi beállításait követi
- Felül lehet írni a jobb fenti beállításokban

## Fontos Komponensek és Tulajdonságaik

### Gallery (Katalógus)
- Adattáblák megjelenítésére szolgál
- Automatikus listázás az Items tulajdonság beállítása után
- ThisItem: Az aktuális rekordra hivatkozás
- Szűrési lehetőségek beépítve

### Form (Űrlap)
- Új rekord hozzáadása adatbázishoz
- DataSource: adatbázis
- Tulajdonságokban a Mezők résznél adhatunk hozzá vagy törölhetünk mezőket
- Hozzáadás esetén: az Alapértelmezett módot állítsuk Új-ra
- Módosítás és Nézet esetén: állítsuk be a megfelelő értéket az Alapértelmezett módnál, majd az Item tulajdonságnak adjunk egy rekordot, különben nem jelenik meg az űrlap
- Legördülő menü esetén (tehát amikor több táblával, Foreign key-el dolgozunk)
  1. A mezőknél adjuk hozzá a Foreign key-t tartalmazó oszlopot
  2. Ugyanitt gördítsük le a hozzáadott vezérlő kártyáját, majd a Vezérlő típusát állítsuk Megengedett értékek-re
  3. Kapcsoljuk ki a szerkesztési korlátozást az adott vezérlőn (bal oldali sáv, majd jobb gomb Zárolás feloldása)
  4. Ezután adjuk hozzá az új kapcsolatot a dimenzió táblánkhoz
  5. A vezérlő Items tulajdonságának adjuk meg ezt az adatot
  6. A Value alapértelmezetten felveszi a tulajdonságot, de ha mégis az ID jelenne meg: jobb oldali tulajdonságok menü, majd Value beállítása a kívánt oszlopra
  7. Ezután ne a vezérlőre (nem a DataCardValue), hanem magára a kártyára (DataCard) kattintsunk, majd az Update tulajdonságot javítsuk, pl.:     DataCardValue.Selected.Title (az alapértelmezett Value itt egyébként is hibát jelez, az elküldendő oszlopot hivatkozzuk itt meg)
- Sorszám kezelés esetén (Sharepoint lista pl. nem auto incrementel)
  1. Kapcsoljuk ki a zárolást a sorszám vezérlőn
  2. A Default paraméternek adjuk ezt a kódot:
    ```If(!IsBlank(Parent.Default);Parent.Default;Last(Sort(adatforrás;Int(Title);SortOrder.Ascending)).Title+1)
     //Ha üres a vezérlő, tehát új rekordot akarunk hozzáadni, akkor az adatbázisban a következő szabad ID-t adja vissza
     //Ha nem szöveges ID mezőt használunk (a Title mező sajnos mindig szöveg), akkor elég a LookUp(adatforrás;ID=Max(ID),ID)+1 -et használni
     //Ha nem üres a vezérlő azt jelenti, hogy egy meglévő rekordot szerkesztünk, tehát jelenítsük meg az adott rekord ID-ját```
  3. A megjelenítési módnál állítsuk View-ra az értéket, hogy megakadályozzuk a felhasználót a manuális értékbeviteltől

## Adatkezelés és Kapcsolat

### SharePoint Integráció
- Külön hozzáadás szükséges az "Adatok" fülön: "SharePoint"
- Oszlopokra hivatkozás esetenként field_1 formátumban

### Office 365 Integráció
- Felhasználói adatok lekérése
- Email és szervezeti információk elérése
- Profilképek kezelése
- Külön hozzáadás szükséges az "Adatok" fülön: "Office365Users"

## Függvények és Képletek

### Változók Kezelése
```
Set(változónév; érték) //Változó értékének beállítása
UpdateContext({változó: érték}) //Környezeti változó értékének frissítése
```

### Navigáció
```
Navigate(képernyő; NavigationType; {context}) //Képernyők közötti navigáció
```

### Feltételes Műveletek
```
If(feltétel; igen_ág; nem_ág) //Egyszerű feltételes elágazás
Switch(kifejezés; eset1; eredmény1; alapértelmezett1; eset2; eredmény2; alapértelmezett2) //Többágú feltételes elágazás
```

### Adatszűrés és Kezelés
```
Filter(tábla; feltétel1[; feltétel2]) //Rekordok szűrése feltételek alapján
Sort(tábla; oszlop[; növekvő]) //Rekordok rendezése
FirstN(tábla; n) //Első N rekord kiválasztása
LastN(tábla; n) //Utolsó N rekord kiválasztása
```

### Collection Műveletek
```
Collect(collection_neve; rekord) //Új rekord hozzáadása a collection-hez
Clear(collection_neve) //Collection törlése
ClearCollect(collection_neve; rekordok) //Collection törlése és új rekordok hozzáadása
Remove(collection; rekord) //Rekord törlése a collection-ből
RemoveIf(collection; feltétel) //Feltételes rekord törlés
```

### Rekord Műveletek
```
Collect(adatforrás; rekord) //Új rekord hozzáadása az adatforráshoz
Patch(adatforrás; rekord; változtatások) //Rekord módosítása az adatforrásban
Update(collection; régi_rekord; új_rekord) //Rekord módosítása a collection-ben
UpdateIf(collection; feltétel; új_értékek) //Feltételes rekord módosítás
```

### Validációs Függvények
```
IsBlank(érték) //Vezérlő üres-e
IsEmpty(collection) //Collection üres-e
CountRows(tábla) //Sorok számának meghatározása
CountIf(tábla; feltétel) //Feltételes számlálás
```

### Dátum és Idő Függvények
```
Now()  //Aktuális dátum és idő lekérése
Today()  //Mai dátum lekérése
DateAdd(dátum; napok; egység) //Dátum eltolása megadott időegységgel
DateDiff(dátum1; dátum2; egység) //Két dátum közötti különbség számítása
```

### Vezérlési Struktúrák
```
// Számok generálása 00-59 között ForAll segítségével
ForAll(Sequence(60;0);Text(Value;"00")) //00-tól 59-ig számok generálása két számjeggyel
```

## Speciális Technikák

### Felhasználó által definiált függvények
```
// Példák függvény definiálása
FuggvenyNeve1(valtozo1:Number;valtozo2:Number):Number = valtozo1*valtozo2;; //Számok szorzása
FuggvenyNeve2():Void={Notify();;Set()};; //Értesítés küldése és változó beállítása
```

### LoadingSpinner Használata
```
// LoadingSpinner alapvető implementáció
Set(isLoading; true)  //Loading indítása
LoadingSpinner1.Visible = isLoading //LoadingSpinner megjelenítése
LoadingSpinner1.Color = RGBA(98; 100; 167; 1) //LoadingSpinner színének beállítása

// Használat példa
//Button1 OnSelect tulajdonságba
Set(isLoading; true);;
Patch('Tasks'; Defaults('Tasks'); {Title: TextInput1.Text});;
Set(isLoading; false)
```

### Képernyők Közötti Adatátadás
- Context használata navigációnál
- Globális változók alkalmazása
- Collections ideiglenes tárolásra

### Teljesítmény Optimalizálás
- Delegálható műveletek előnyben részesítése
- Képletek egyszerűsítése
- Timer komponens használata időzített műveletekhez

### Biztonság és Jogosultságkezelés
- User().Email alapján jogosultságok kezelése
- Szerepkörök definiálása

## Hasznos Tippek
1. Magyar nyelvű környezetben a függvények paramétereit pontosvesszővel (;) kell elválasztani
2. Összetett feltételeknél használj zárójeleket
3. A ThisItem mindig az aktuális Gallery elemre vonatkozik
4. Használj beszédes változóneveket 
