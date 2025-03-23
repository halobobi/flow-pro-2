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
### Teams flow

11. Az új rekordról küldjünk üzenetet Teams-en egy tetszőleges felhasználónak (pl. magunknak)
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

### Diagram

13. Készítsünk egy kördiagramot, ami a StatusID-k megoszlását mutatja
	- Items: ```Device_FactDevice //Nem működik, az ID-k értékét számolja```
 	- Készítenünk kell egy segédkimutatást
  	- OnVisible:
	```
   	Collect(pivot,
    	{Title:"Új",Value:CountIf(Device_FactDevice,StatusID.Value="Új")},
    	{Title:"Használt",Value:CountIf(Device_FactDevice,StatusID.Value="Használt")},
    	{Title:"Selejt",Value:CountIf(Device_FactDevice,StatusID.Value="Selejt")}
	)
	```
 	- Items: ```pivot```
  	- Az ItemColorSet paramétert érdemes üresre állítani, hogy elkülönülő színek jelenjenek meg
### Saját függvény

14. Gombnyomásra tudjuk frissíteni a galériát
	- Ehhez adjunk hozzá saját függvényt ```Reload``` néven
 	- Ez a függvény töltse újra az adatbázis kapcsolatot, illetve a diagram adatforrását is frissítse
  	- Bal lent beállítások -> Updates -> Experimental -> User-defined functions engedélyezése
  	- Variables -> New -> Named formula -> Formulas:
	```
 	Reload():Void={Refresh(Device_FactDevice);
		ClearCollect( //Ha nem Clear lenne, a 3 új rekord mindig hozzáadódna
    		pivot,
    		{Title:"Új",Value:CountIf(Device_FactDevice,StatusID.Value="Új")},
    		{Title:"Használt",Value:CountIf(Device_FactDevice,StatusID.Value="Használt")},
    		{Title:"Selejt",Value:CountIf(Device_FactDevice,StatusID.Value="Selejt")}
		)
	};
 	```

### CSV export flow

14. Hozzunk létre egy új üres flow-t
	- Bemeneti paraméterek a trigger-be: ```JSON```, szöveges
 	- Data Operation: ```Parse JSON```
  		- Content: ```JSON``` bemenet
    		- Schema:
		```{ "type": "array", "items": { "type": "object", "properties": { "Created": { "type": "string" }, "DeviceName": { "type": "string" }, "ID": { "type": "integer" }, "StatusID": { "type": "object", "properties": { "Id": { "type": "integer" }, "Value": { "type": "string" } } }, "StorageID": { "type": "object", "properties": { "Id": { "type": "integer" }, "Value": { "type": "string" } } } }, "required": [ "Created", "DeviceName", "ID", "StatusID", "StorageID" ] } }```
 	- Data Operation: ```Create CSV table``` action
  		- From: Parse JSON: ```Body```
 	- Adjunk hozzá egy SharePoint ```Create file``` action-t
  		- Site Address: ```https://bcecid.sharepoint.com/sites/bit-bce-hq```
   		- Folder Path: ```/HQ/Szakmai Képzéseink/Project Management Office/2024_2025_2/Flow Pro II_Power Apps/2. alkalom/temp```
     		- File Name: ```export_<saját név>_@{utcNow()}.csv```
    		- File Content: ```@{concat(decodeUriComponent('%EF%BB%BF'),body('Create_CSV_table'))} //UTF-8 encoding miatt kell: "BOM is 3 characters (EF BB BF) to mark the file is encoded as UTF-8."```
      	- SharePoint: ```Get file properties``` action
      		- Site Address: ```https://bcecid.sharepoint.com/sites/bit-bce-hq```
   		- Library Name: ```HQ```
     		- Id: ```@{outputs('Create_file')?['body/ItemId']}```
      	- Adjunk hozzá egy ```Respond to a Power App or flow``` action-t
      		- Paraméterek: output, ```@{outputs('Get_file_properties')?['body/{Link}']}```
      	- A Lookup típusú oszlopok objektumként jelennek meg, a kurzusban ezt nem kezeljük (Power Automate-ben pl. Apply to each ciklus segítségével lehet a Value értéket kiemelni)
   
16. Adjunk hozzá egy Mentés gombot
	- OnSelect:
	```
 	Download(
		ExportData.Run(
        		JSON(
            			ShowColumns(Gallery1.AllItems,
                			ID,DeviceName,StatusID,StorageID,Created
                		)
            		)
 		).output
    	)
 	```
    
## Önálló feladatok

### 1. feladat

1. Hozzunk létre egy új Canvas app-ot
	- Tetszőlegesen lehet reszponzív, tablet vagy telefon méret
2. A felhasználandó adatbázis: ```https://bcecid.sharepoint.com/sites/bit-bce-hq/Lists/Flow%20Pro%202_Kzs%20adatbzis/AllItems.aspx```
3. A felhasználónak jelenítsük meg az adatbázis összes rekordját, mindegyik oszlop adatait, tetszőleges formában
4. A sorban megjelenő törlés gomb megnyomásával törölhessük az adott rekordot
	- Állítsunk az esemény jellegéhez megfelelő ikont a gombnak
5. A galériából egy másik nyíl segítségével nyíljon új oldal, ahol az adott rekordot tudjuk módosítani
	- Form segítségével valósítsuk ezt meg
	- A beviteli mezőknek adjunk saját nevet: StatusID helyett Státusz, stb.
		- Szabadon lehessen navigálni a két oldal között, újraindítás nélkül
6. Értesítsünk egy tetszőleges felhasználót Teams-en a változásról, adjuk meg az üzenetben az elem elérési útját
### Szorgalmi feladat

7. Legyen szűrhető és rendezhető galéria: egy tetszőleges (legördülő menüből választható) oszlop tartalma alapján lehessen szűrni, és sorbarendezni
	- Ha nincs találat a szűrésre, értesítésben jelezzük a felhasználónak
	- Csökkenő, növekvő sorrend választására is legyen lehetőség
8. A főoldalról (Screen1) egy gomb segítségével nyíljon egy másik oldal, ahol új rekordot tudjunk hozzáadni:
	- Form segítségével lehessen az új rekord értékeit megadni
		- Szabadon lehessen navigálni a két oldal között, újraindítás nélkül
		- Opcionális: tetszőleges oszlopérték alapján oldjuk meg, hogy ha már létezik ilyen érték az adatbázisban, ne lehessen a rekordot hozzáadni
9. A szerkesztési oldalon egy gomb segítségével tudjuk CSV-be menteni az adott rekordot
	- Opcionális: Excel-be mentsük a rekordot (Segítség: OneDrive-ra fájl létrehozása -> Új sor, vagy SharePoint esetén: https://www.matthewdevaney.com/create-an-excel-file-and-add-rows-using-power-automate/)
