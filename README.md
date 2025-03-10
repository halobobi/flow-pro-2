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
- Az Alapértelmezett módot állítsuk Új-ra, enélkül nem jelenik meg a Form

## Adatkezelés és Kapcsolatok

### SharePoint Integráció
- Külön hozzáadás szükséges az "Adatok" fülön
- Oszlopokra hivatkozás esetenként field_1 formátumban

### Office 365 Integráció
- Felhasználói adatok lekérése
- Email és szervezeti információk elérése
- Profilképek kezelése

## Függvények és Képletek

### Változók Kezelése
```
Set(változónév, érték)
UpdateContext({változó: érték})
```

### Navigáció
```
Navigate(képernyő, NavigationType, {paraméterek})
```

### Feltételes Műveletek
```
If(feltétel, igen_ág, nem_ág)
Switch(kifejezés, eset1, eredmény1, eset2, eredmény2, alapértelmezett)
```

### Adatszűrés és Kezelés
```
Filter(tábla, feltétel1[, feltétel2])
Sort(tábla, oszlop[, növekvő])
FirstN(tábla, n)
LastN(tábla, n)
```

### Collection Műveletek
```
Collect(collection_neve, rekord)
Clear(collection_neve)
ClearCollect(collection_neve, rekordok)
Remove(collection, rekord)
RemoveIf(collection, feltétel)
```

### Rekord Műveletek
```
Collect(adatforrás, rekord)
Patch(adatforrás, rekord, változtatások)
Update(collection, régi_rekord, új_rekord)
UpdateIf(collection, feltétel, új_értékek)
```

### Validációs Függvények
```
IsBlank(érték)
IsEmpty(collection)
CountRows(tábla)
CountIf(tábla, feltétel)
```

### Dátum és Idő Függvények
```
Now()  // Aktuális dátum és idő
Today()  // Mai dátum
DateAdd(dátum, napok, egység)
DateDiff(dátum1, dátum2, egység)
```

### Vezérlési Struktúrák
```
// Sequence - hasonló a Python range függvényhez
Sequence(rekordok_száma;kezdőérték;lépés))

//Számok generálása 00-59 között ForAll segítségével
ForAll(Sequence(60;0);Text(Value;"00"))
```
## Speciális Technikák

### Felhasználó által definiált függvények

```
// Példák függvény definiálása
FuggvenyNeve1(valtozo1:Number;valtozo2:Number):Number= valtozo1*valtozo2;;

FuggvenyNeve2():Void={Notify();;Set()};;
```

### LoadingSpinner Használata
```
// LoadingSpinner alapvető implementáció
Set(isLoading, true)  // Loading indítása
LoadingSpinner1.Visible = isLoading
LoadingSpinner1.Color = RGBA(98, 100, 167, 1)

// Használat példa
Button1.OnSelect = (
    Set(isLoading; true);;
    // Hosszabb művelet
    Patch('Tasks'; Defaults('Tasks'); {Title: TextInput1.Text});;
    Set(isLoading; false)
)
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
