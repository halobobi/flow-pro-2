# 1. alkalom feladatai

## Közös feladatok

### Adatbázis létrehozása
1. Keressük fel a Lists oldalát
    - A listákat a saját listák közzé hozzuk létre
2. Nyissuk meg a mintaadatokat tartalmazó Excel file-t
#### Dimenziótáblák létrehozása
3. Adjunk hozzá új listát ```Device_DimStatus``` néven
    1. Új oszlop hozzáadása
        1. Oszlopok elrejtése/megjelenítése:
            - ```ID``` oszlop megjelenítése
            - ```Title``` oszlop elrejtése
        2. ```StatusName``` szöveges oszlop hozzáadása
    2. Adatok hozzáadása Excel-ből
        1. Rácsnézet
        2. Excel-ből az adatok másolása fejléc nélkül a megfelelő munkalapról
        3. Kattintás az új elem sorba egyszer (figyeljünk arra, hogy az oszlopok azonos sorrendben szerepeljenek)
        4. Beillesztés (CTRL+V)

4. Adjunk hozzá új listát ```Device_DimStorage``` néven
    1. ```ID``` oszlop megjelenítése, ```Title``` oszlop elrejtése
    2. ```StorageName``` szöveges oszlop hozzáadása
    3. ```StorageLocation``` szöveges oszlop hozzáadása
    4. Adatok hozzáadása Excel-ből
     
#### Ténytábla hozzadása
5. Adjunk hozzá új listát ```Device_FactDevice``` néven
6. ```ID```, ```Title``` oszlopok kezelése
7. ```DeviceName``` szöveges, ```AqDate``` dátum típusú oszlop hozzáadása
8. Foreign key oszlopok hozzáadása
    1. LookUp (Keresés) típusú oszlopok hozzáadása ```StatusID``` és ```StorageID``` néven
    2. Megfelelő listák és értékek beállítása (hozzáadás után nem lehet módosítani)
9. Adatok hozzáadása Excel-ből

### Irány a Power Apps

0. Nyelv beállítása angolra a böngészőben

1. Új üres Canvas app létrehozása, telefonra
    - Lehet reszponzív, tablet, telefon mód, lehet létrehozni design-t adatvezérelten, illetve akár Figmából is
2. Körbetekintés
    - Baloldali sávon:
        - Fastruktúra, itt találjuk az app komponenseket, új képernyő hozzáadása
        - Új elem hozzáadása
        - Adatok: új adatkapcsolat létrehozása
        - Környezet, globális változók, saját függvények
        - Egyéb: keresés, média hozzáadása, flow-k és tesztek
    - Középen: Canvas, behúzhatóak az új elemek, itt kiválasztva azokat szerkeszthetők a tulajdonságaik
    - Jobboldali sáv: részletes tulajdonságok
    - Felül:
        - baloldalt legördülőben is elérhetőek a tulajdonságok
        - középen lévő sávba írva tudunk a tulajdonságoknak értéket adni (viselkedési (esemény) tulajdonságnak függvényt, nem viselkedésinek csak adatot (kivéve LookUp, If, stb.)
        - feljebb egyéb szöveges formázások
        - jobbra megosztás, futtatás, mentés, publikálás (ez kell ahhoz, hogy nyílvánosan mindenkinek elérhető legyen az app)
3. Adatkapcsolatok kialakítása
    - SharePoint listák hozzáadása a ```https://bcecid-my.sharepoint.com/personal/{saját e-mail cím}/_layouts/15/lists.aspx``` site-ról
    - Office365Users hozzáadása
5. Üdvözlő felület:
    1. Adjunk hozzá egy horizontális konténert felülre, majd a label-t és képet ide tegyük
        - Justify: space between
        - Align: center
        - Size: 640 x 140
        - Padding: ```20``` minden oldalon
        - Shadow: ```None```
        - A szín valamilyen nagyon világos szín legyen: ```RGBA(241, 244, 249, 1)```
        - Border radius: ```50```, de alulról utólag töröljük a spec. tulajdonságoknál
        - Ha nem szeretnénk a rendezéssel, méretezéssel szenvedni, akkor elég egy sima konténer is, vagy kihagyhatjuk 
    2. Adjunk hozzá labelt-t balra
        - Text: ```Concatenate("Üdv ",Office365Users.UserProfile(User().Email).DisplayName,"!")```
        - Height: ```70```
        - Width: ```490```
        - Font weight: ```Bold```
        - Ha nem zavar a rossz lokalizáció: ```User().FullName```
    3. Adjunk hozzá egy képet jobbra
        - Image: ```If(Office365Users.UserPhotoMetadata(User().Email).HasPhoto,Office365Users.UserPhotoV2(User().Email),SampleImage)```
        - Ha lusták vagyunk, és nem kezelünk hibát: ```User().Image```
        - Size: 100 x 100
        - Border radius: ```90```, így kör alakú lesz a kép
    4. Adjunk hozzá újabb konténert
        - Simát, ami az egész képernyőt lefedi a fejlécen kívül, illetve az alsó vezérlősávon kívül (később gombot tudunk ide tenni)
        - Legyen ugyanaz a színe, mint a fejlécnek
    5. Adjunk hozzá vertikális galériát
        - Adatforrásnak állítsuk be a ```Device_FactDevice``` listát (ha valakinek nem sikerült saját adatbázist létrehozni, a ```https://bcecid.sharepoint.com/sites/bit-bce-hq``` site-ról hozzáfér egy előre elkészítetthez

