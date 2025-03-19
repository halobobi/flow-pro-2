# 2. alkalom feladatai

## Közös feladatok



## Önálló feladatok

### 1. feladat

1. Hozzunk létre egy új Canvas app-ot
    - Tetszőlegesen lehet reszponzív, tablet vagy telefon méret
2. A felhasználandó adatbázis: ```https://bcecid.sharepoint.com/sites/bit-bce-hq/Lists/Flow%20Pro%202_Kzs%20adatbzis/AllItems.aspx```
3. A felhasználónak jelenítsük meg az adatbázis összes rekordját, mindegyik oszlop adatait, tetszőleges formában
4. A sorban megjelenő nyíl megnyomásával törölhessük az adott rekordot
    - Állítsunk az esemény jellegéhez megfelelő ikont a gombnak
5. A galériából egy másik nyíl segítségével nyíljon új oldal, ahol az adott rekordot tudjuk módosítani
    - Form segítségével valósítsuk ezt meg
    - A beviteli mezőknek adjunk saját nevet: StatusID helyett Státusz, stb.
	- Szabadon lehessen navigálni a két oldal között, újraindítás nélkül
6. Legyen szűrhető és rendezhető galéria: egy tetszőleges (legördülő menüből választható) oszlop tartalma alapján lehessen szűrni, és sorbarendezni
	- Ha nincs találat a szűrésre, értesítésben jelezzük a felhasználónak
	- Opcionális: csökkenő, növekvő sorrend választására is legyen lehetőség
### Szorgalmi feladat
7. A főoldalról (Screen1) egy gomb segítségével nyíljon egy másik oldal, ahol új rekordot tudjunk hozzáadni:
    - Form segítségével lehessen az új rekord értékeit megadni
	- Szabadon lehessen navigálni a két oldal között, újraindítás nélkül
	- Opcionális: tetszőleges oszlopérték alapján oldjuk meg, hogy ha már létezik ilyen érték az adatbázisban, ne lehessen a rekordot hozzáadni
