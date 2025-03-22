# 2. alkalom feladatai

## Közös feladatok

### Projekt létrehozása
1. Hozzunk létre egy üres app-ot, tablet méretben

### Adatbázis kapcsolatok kialakítása
2. Data -> SharePoint connector -> Device_DimStatus, Device_DimStorage, Device_FactDevice

### Kezdőlap
3. Adjunk hozzá egy galériát
	- Items: ```Device_FactDevice```
4. Legyen a galéria sorbarendezhető tetszőleges oszlop alapján, az irányt is lehessen megadni
	- Legdördülő menüket használjunk
	- Dropdown1.Items: ```["ID","DeviceName","Created"]```
 		- A lookup típusú oszlopokra a Sort segítségével szűrhetünk, pl.: ```StatusID.Value```
   		- Ezt sajnos nem tudjuk legördülő menükkel együtt használni, csak hard code-olni lehet
	- Dropdown2.Items: ```[{Value:"Növekvő",Action:SortOrder.Ascending},{Value:"Csökkenő",Action:SortOrder.Descending}]```
	- Gallery1.Items: ```SortByColumns(Device_FactDevice,Dropdown1.Selected.Value,Dropdown2.Selected.Action)```
5. A galéria soraiban a nyílra kattintva szerkeszthessük az adott rekordot
	- OnSelect: ```Navigate(Screen2,ScreenTransition.Cover,{item:ThisItem})```
### Screen2: Rekord módosítás
6. A Screen2-n adjunk hozzá egy Űrlap elemet
	- Item: ```item```
	- DataSource: ```Device_FactDevice```
	- Az ```Title```, ```ID``` és ```Created``` (esetlegesen a ```Tartalomtípus```) mezőket rejtsük el, ezeket nem szerkeszthetjük
	- Default layout: ```Edit```
7. Adjunk hozzá egy Vissza gombot, ikonnal valósítsuk ezt meg
	- OnSelect: ```Back(ScreenTransition.UnCoverRight)```
9. Adjunk hozzá egy gombot, amivel elküldhető az űrlap
	- Sikeres küldéskor értesítsük a felhasználót, majd lépjünk vissza a főoldalra
	- OnSelect: ```SubmitForm(Form1);ResetForm(Form1);Notify("Sikeres mentés!",NotificationType.Success);Back(ScreenTransition.UnCoverRight) //reset: ha Edit módban volt Edit-re, stb. new: mindig New módra állítja```
### Screen3: Új rekord hozzáadása
9. A kezdőlapon egy gomb nyomás után űrlap segítségével adhassunk új rekordot az adatbázishoz
	- OnSelect: ```Navigate(Screen3)```
  	- DataSource: ```Device_FactDevice```
	- Az ```Title```, ```ID``` és ```Created``` mezőket rejtsük el, ezeket nem szerkeszthetjük
	- Default layout: ```New```
10. Adjunk hozzá egy gombot, amivel elküldhető az űrlap
	- OnSelect: ```SubmitForm(Form1);ResetForm(Form1);Notify("Sikeres hozzáadás!",NotificationType.Success);Back(ScreenTransition.UnCoverRight)```
11. Az új rekord adatait küldjük el Teams-en egy tetszőleges felhasználónak (pl. magunknak)
	- ... -> Power Automate -> Create new flow
 	- Választhatunk a template-ek közül, vagy készíthetünk manuálisan
  	- Advanced módban szerkeszthetjük is a sablont
12. Adjunk hozzá egy üres flow-t 
    	- Adjunk hozzá egy ```ItemID``` nevű text és egy ```Email``` nevű email input-ot a trigger-hez
    	- Adjunk hozzá egy SharepPoint action-t: ```Get item```
    		- Site Address: a saját Lists oldalunk
    		- List Name: ```Device_FactDevice```
    		- Id: ```@{triggerBody()['text']} //Itt nem mindig engedi dynamic context-ként hozzáadni az ItemID bemenetet, ezért szükséges ez a kód```
    	- Adjunk hozzá új Teams action-t: ```Post message in a chat or channel```
    		- Post as: ```Flow bot```
    		- Post in: ```Chat with Flow bot```
    		- Recipient: ```Email``` bemenet
    		- Message: ```<p class="editor-paragraph">A létrehozott @{triggerBody()['text']}. számú elem elérhető: <a href="@{outputs('Get_item')?['body/{Link}']}">@{outputs('Get_item')?['body/DeviceName']}</a></p>```

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
6. A szerkesztési oldalon egy gomb segítségével tudjuk Excel-be menteni az adott rekordot
7. Legyen szűrhető és rendezhető galéria: egy tetszőleges (legördülő menüből választható) oszlop tartalma alapján lehessen szűrni, és sorbarendezni
	- Ha nincs találat a szűrésre, értesítésben jelezzük a felhasználónak
	- Opcionális: csökkenő, növekvő sorrend választására is legyen lehetőség
### Szorgalmi feladat
8. A főoldalról (Screen1) egy gomb segítségével nyíljon egy másik oldal, ahol új rekordot tudjunk hozzáadni:
    	- Form segítségével lehessen az új rekord értékeit megadni
		- Szabadon lehessen navigálni a két oldal között, újraindítás nélkül
		- Opcionális: tetszőleges oszlopérték alapján oldjuk meg, hogy ha már létezik ilyen érték az adatbázisban, ne lehessen a rekordot hozzáadni
