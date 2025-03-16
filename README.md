# 1. alkalom feladatai

## Közös feladatok

### Adatbázis létrehozása
1. Keressük fel a Lists oldalát
    - A listákat a saját listák közzé hozzuk létre
3. Nyissuk meg a mintaadatokat tartalmazó Excel file-t
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
    1. ID oszlop megjelenítése, Title oszlop elrejtése
    2. ```StorageName``` szöveges oszlop hozzáadása
    3. ```StorageLocation``` szöveges oszlop hozzáadása
    4. Adatok hozzáadása Excel-ből
     
#### Ténytábla hozzadása
5. Adjunk hozzá új listát ```Device_FactDevice``` néven
6. ID, Title oszlopok kezelése
7. DeviceName szöveges, AqDate dátum típusú oszlop hozzáadása
8. Foreign key oszlopok hozzáadása
    1. LookUp (Keresés) típusú oszlopok hozzáadása ```StatusID``` és ```StorageID``` néven
    2. Megfelelő listák és értékek beállítása (hozzáadás után nem lehet módosítani)
9. Adatok hozzáadása Excel-ből

