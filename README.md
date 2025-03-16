# Flow Pro II: Power Apps

## Bevezetés
A Power Apps egy Microsoft által fejlesztett low-code/no-code platform, amely lehetővé teszi egyedi üzleti alkalmazások gyors és hatékony fejlesztését. Az alábbiakban részletes útmutatót találsz a platform használatához.

### Adatbázis
#### Microsoft Dataverse adatbázis: [<ins>https://dataverse.microsoft.com</ins>](https://dataverse.microsoft.com)
- Alapvetően fizetős
- Saját solution-ben, és saját könyezetben (environment) használható az appok mellé ingyenesen is
  - Mivel csak lokálisan hozzáférhető a solution-ön belül, nem lehet közös adatbázisként használni

#### Microsoft Lists adatbázis: [<ins>https://m365.cloud.microsoft</ins>](https://m365.cloud.microsoft) -> Bento menü -> Lists
- Ingyenes, SharePoint alapú megoldás
- Bármely Teams csoporton belül használhatjuk közös adatbázisnak
- Adatbázis működés eléréséhez:
  - Auto-increment ID oszlop:
1. Új oszlop hozzáadása
2. Oszlop elrejtése / megjelenítése
3. ID oszlop kiválasztása
  - Táblakapcsolatok kezelése (Foreign key egyszerű kezelése):
1. Új oszlop hozzáadása
2. Oszlop típus: Lookup (Keresés)
3. A kiválasztandó a lista valamely dimenzió tábla (pl.: DimStorage)
4. Az megjelenítendő oszlop pedig kívánt érték a dimenzióból (pl.: Raktárnév)
  - Használhatjuk az alapértelmezett Title oszlopot is adatok tárolására, de a későbbi felhasználás miatt egyszerűbb elrejteni (törölni nem lehet), majd egy értelemszerű néven új oszlopot hozzáadni
  - Trükk a lista feltöltésére akár több sorral:
1. Másoljuk csak az adatokat, fejléc nélkül Excel-ből
2. A Lists felületén váltsunk rácsnézetbe
3. Új elem hozzáadása, egyszer kattintsunk a sorba, ne kezdjük el szerkeszteni
4. Majd CTRL+V-vel beillesztés
#### Excel file-ok
- Adatbázisként nem igazán szokták használni (sajnos néha mégis előfordul)
- De egy Excel táblázat "megszépítésére", bizonyos funkciók korlátozása miatt, vagy felhasználóbaráttá tétel céljával építhető Power Apps alkalmazás

## Kezdeti lépések

### Hozzáférés és környezet
- **Elérés**: [<ins>https://make.powerapps.com</ins>](https://make.powerapps.com)
- **Környezet típusok**:
  - **Solution (Megoldás)**: Komplex alkalmazásokhoz, Dataverse táblákkal
  - **Standalone App**: Egyszerűbb alkalmazásokhoz, külső adatforrásokkal

### Alkalmazás létrehozása
- **Új app létrehozási lehetőségek**:
  - Üres alkalmazás (Blank)
  - Adatforrás alapú
  - [<ins>Figma design alapú</ins>](https://www.figma.com/community/file/1110934196623232680/microsoft-power-apps-create-apps-from-figma-ui-kit-preview)

## Fejlesztői felület

### Felület áttekintés
- **Bal oldali panel**: 
  - Fastruktúra
  - Új elemek hozzáadása
  - Adatok kezelése
  - Változók
    - Globális változók
    - Környezeti változók
    - Formulák (egyéni függvények)

- **Középső terület (Canvas)**:
  - Vizuális elemek:
    - Gombok
    - Beviteli mezők:
      - Szöveg
      - Dátum
      - Legördülő lista (Dropdown)
      - Kombinált lista (Combobox)
      - Rádiógomb, checkbox, stb.
    - Címkék (Label): szöveg megjelenítése, értékét változón keresztül tudjuk módosítani
    - Katalógus (Gallery): adattáblák megjelenítése

### Nyelvi beállítások
- Alapértelmezetten a szervezeti vagy számítógép nyelvi beállításait követi
- Felül lehet írni a jobb fenti beállításokban

## Függvények és képletek

### Változók kezelése
```
Set(változónév, érték) //Változó értékének beállítása
UpdateContext({változó: érték}) //Környezeti változó értékének frissítése
Int(TextInput.Text) //Beviteli mező számmá alakítása
Text(sorszám) //Szöveggé alakítás
Reset(TextInput) //visszaállítja alapértelmezettre a bevitelő mezőt
```

### Navigáció
```
Navigate(képernyő, NavigationType, {context}) //Képernyők közötti navigáció
```
- Képernyők közötti értékátadás
  - **Context használata navigációnál**
  - Globális változók alkalmazása
  - Collections ideiglenes tárolásra

### Feltételes műveletek
```
If(feltétel, igen_ág, nem_ág) //Egyszerű feltételes elágazás
Switch(kifejezés, eset1, eredmény1, alapértelmezett1, eset2, eredmény2, alapértelmezett2) //Többágú feltételes elágazás
```

### Adatszűrés és kezelés
```
Filter(tábla, feltétel1[, feltétel2]) //Rekordok szűrése feltételek alapján
Sort(tábla, oszlop[, növekvő]) //Rekordok rendezése
FirstN(tábla, n) //Első N rekord kiválasztása
LastN(tábla, n) //Utolsó N rekord kiválasztása
StartsWith(szöveg,minta) //Ellenőrzi, hogy minta változó értékével kezdődik-e szöveg
If(minta in szöveg,true,false) //Tartalmazza-e szöveg minta változó értékét
```

### Collection és rekord műveletek
```
Collect(collection_neve, rekord) //Új rekord hozzáadása a collection-hez
Clear(collection_neve) //Collection törlése
ClearCollect(collection_neve, rekordok) //Collection törlése és új rekordok hozzáadása
Update(collection, régi_rekord, új_rekord) //Rekord módosítása a collection-ben
UpdateIf(collection, feltétel, új_értékek) //Feltételes rekord módosítás
Patch(adatforrás, rekord, változtatások) //Rekord módosítása az adatforrásban
Remove(collection, rekord) //Rekord törlése a collection-ből
RemoveIf(collection, feltétel) //Feltételes rekord törlés
```
```
Collect('test',
  {
    '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedReference", //Amiatt kell, mert nem egyszerű ID-t, hanem Expanded Reference típusú adatot adunk a táblába
    Id: ComboBox1.Selected.EmployeeID, //A rekord összes adatát meg kell adni, ez előnytelen használhatóság a SharePoint részéről
    Value: ComboBox1.Selected.EmployeeName //Alternatívaként egy LookUp lekérdezésből is visszakérhetjük az értékeket az ID alapján
  }
)
```

### Validációs függvények
```
IsBlank(érték) //Vezérlő üres-e
IsEmpty(collection) //Collection üres-e
IsError(művelet) //Hibára fut-e az adott művelet
CountRows(tábla) //Sorok számának meghatározása
CountIf(tábla, feltétel) //Feltételes számlálás
```

### Dátum és idő függvények
```
Now()  //Aktuális dátum és idő lekérése
Today()  //Mai dátum lekérése
DateAdd(dátum, napok, egység) //Dátum eltolása megadott időegységgel
DateDiff(dátum1, dátum2, egység) //Két dátum közötti különbség számítása
```

### Vezérlési struktúrák
```
// Számok generálása 00-59 között ForAll segítségével
ForAll(Sequence(60,0),Text(Value,"00")) //00-tól 59-ig számok generálása két számjeggyel
ThisRecord //ForAll ciklusváltozó
```
## Adatkezelés és kapcsolat

### SharePoint integráció
- Külön hozzáadás szükséges az "Adatok" fülön: "SharePoint"
- Power Automate-hez hasonlóan az oszlopokra hivatkozás az oszlopnév helyett esetenként 'field_1' formátumban (SortByColumns esetén)

### Office 365 integráció
- Felhasználói adatok lekérése
- Email és szervezeti információk elérése
- Profilképek kezelése
- Külön hozzáadás szükséges az "Adatok" fülön: "Office365Users" vagy 'Office365-felhasználók'
  ```
  User().Email //A Power Apps alkalmazásba vagy a web-es felületre bejelentkezett e-mail címet adja vissza

  Office365Users.UserProfile(User().Email) //Az Office365 connection-ön keresztül a profil kérése
    .displayName //Felhasználó teljes neve
    .department //Team
    .jobTitle //Beosztás, pozíció

  Office365Users.UserPhotoV2(email) //Profilkép lekérése

  Office365Users.UserPhotoMetadata(email).HasPhoto //Van-e profilkép beállítva

  If('Office365-felhasználók'.UserPhotoMetadata(User().Email).HasPhoto,'Office365-felhasználók'.UserPhotoV2(User().Email),SampleImage) //Ha nincs beállítva profilkép SampleImage megjelenítése
  ```

## Fontos komponensek és tulajdonságaik

### Gomb
- ```OnSelect```: kattintás után végrehajtandó utasítások
- ```Text```: felirat

### Beviteli mezők
- ```Items```: Adatforrás megadása
- ```Default```: alapértelmezett érték, űrlap esetében fontos, hogy egyből megjelenjen egy rekord adott értéke
- ```DisplayMode```: szerkeszthető vagy nem
- Dropdown esetén ```Selected```: kiválasztott sor
- ComboBox esetén ```SelectedItems```: kiválasztott sor(ok)

### Gallery (Katalógus)
- ```Items```: Adatforrás megadása
- ```ThisItem```: Az aktuális rekordra hivatkozás

### Form (Űrlap)
- Felhasználás: új rekord hozzáadása adatbázishoz, rekord módosítása
- ```DataSource```: Adatforrás megadása
- Tulajdonságokban a Mezők résznél adhatunk hozzá vagy törölhetünk mezőket
- Hozzáadás esetén: az Alapértelmezett módot állítsuk Új-ra
- Módosítás és Nézet esetén: állítsuk be a megfelelő értéket az Alapértelmezett módnál, majd az Item tulajdonságnak adjunk egy rekordot, különben nem jelenik meg az űrlap
- Legördülő menü esetén (tehát amikor több táblával, Foreign key-el dolgozunk):
1. A mezőknél adjuk hozzá a Foreign key-t tartalmazó oszlopot
2. Ugyanitt gördítsük le a hozzáadott vezérlő kártyáját, majd a Vezérlő típusát állítsuk Megengedett értékek-re
3. Kapcsoljuk ki a szerkesztési korlátozást az adott vezérlőn (bal oldali sáv, majd jobb gomb Zárolás feloldása)
4. Ezután adjuk hozzá az új kapcsolatot a dimenzió táblánkhoz
5. A vezérlő Items tulajdonságának adjuk meg ezt az adatot
6. A Value alapértelmezetten felveszi a tulajdonságot, de ha mégis az ID jelenne meg: jobb oldali tulajdonságok menü, majd Value beállítása a kívánt oszlopra
7. Ezután ne a vezérlőre (nem a DataCardValue), hanem magára a kártyára (DataCard) kattintsunk, majd az Update tulajdonságot javítsuk, pl.:     DataCardValue.Selected.Title (az alapértelmezett Value itt egyébként is hibát jelez, az elküldendő oszlopot hivatkozzuk itt meg)
- Sorszám kezelés esetén (Pl. a Sharepoint lista nem auto increment-el):
1. Kapcsoljuk ki a zárolást a sorszám vezérlőn
2. A Default paraméternek adjuk ezt a kódot:
    ```
    If(!IsBlank(Parent.Default),Parent.Default,Last(Sort(adatforrás,Int(Title),SortOrder.Ascending)).Title+1) //Parent (szülő) a container-t jelöli, a Form elemet
    ```
  - Ha üres a vezérlő, tehát új rekordot akarunk hozzáadni, akkor az adatbázisban a következő szabad ID-t adja vissza
  - Ha nem szöveges ID mezőt használunk (a Title mező sajnos mindig szöveg), akkor elég a ```LookUp(adatforrás,ID=Max(ID),ID)+1``` kódot használni
  - Ha nem üres a vezérlő azt jelenti, hogy egy meglévő rekordot szerkesztünk, tehát jelenítsük meg az adott rekord ID-ját
3. A megjelenítési módnál állítsuk View-ra az értéket, hogy megakadályozzuk a felhasználót a manuális értékbeviteltől
  - Hasonlóan az előbbihez, ha dinamikusan akarjuk kezelni, hogy a mező szerkeszthető-e, akkor a ```If(!IsBlank(Parent.Default),Parent.DisplayMode,DisplayMode.View)``` kóddal kezelhetjük, hogy csak szerkesztéskor lehessen módosítani a sorszámot
    - Ez egyébként bad practice, az ID primary key, tehát egyértelműen azonosítja a rekordot, nem érdemes a felhasználóra bízni az értékmegadást
- Parancsok:
  ```
  SubmitForm(form) //Űrlap leadása
  NewForm(form) //üres űrlap betöltése
  ResetForm(form) //bevitt értékek törlése, visszaállítás alapértelmezettre
  ```

## Speciális technikák

### Felugró ablak
- A felhasználó tájékoztatása a felső sávban
- Részletek: [<int>https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-showerror</int>](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-showerror)
```
Notify(szöveg, NotificationType, timeOut)
```

### Felhasználó által definiált függvények
```
// Példák függvény definiálása
FuggvenyNeve1(valtozo1:Number,valtozo2:Number):Number=valtozo1*valtozo2; //Számok szorzása
FuggvenyNeve2():Void={Refresh(adatforrás);Notify();Set()}; //Értesítés küldése és változó beállítása
```

### LoadingSpinner használata
```
// LoadingSpinner alapvető implementáció
Set(isLoading, true)  //Loading indítása
LoadingSpinner1.Visible = isLoading //LoadingSpinner megjelenítése
LoadingSpinner1.Color = RGBA(98, 100, 167, 1) //LoadingSpinner színének beállítása

// Használat példa
//Button1 OnSelect tulajdonságba
Set(isLoading, true);
Patch('Tasks', Defaults('Tasks'), {Title: TextInput1.Text});
Set(isLoading, false)
```

### Teljesítmény optimalizálás
- Delegálás: a Power Apps külső Microsoft / Azure API-kkal futtatja az utasítást, külső kiszolgálóra helyezi a munka nehéz részét
