# Repetition
### Datamodellering
- Planera in en databas av olika modeller
	- Konceptuell modell
		- modellering av tabeller och relationer (3 olika typer, kan skrivas på fler sätt)
			- 1:1
			- 1:n
			- n:1
			- n:m
		- Beståndsdelar i databaser:
			- Tabell
			- Fields
			- records
			- views 
			- schema
			- procedure
	- Logisk modell
		- Ta relationer och översätt det till objekt eller relationella objekt
			- n:m görs om till relationsobjekt (kopplingstabell)
		- Normalisering
			- 3 normala former som bör implementeras, med start i första normala formen. Man kan fortsätta in i flera former i prod, man slutar ofta i tredje vid utbildningar.
			- 1a: kräver unik nyckel och atomära fält (fälten kan inte växa på bredden, ex flera telefonnummer)
			- 2a: 1NF + Alla icke nyckelfält ska vara beroende av hela nyckeln
			- 3e: 1NF + 2NF + Det får inte finnas beroenden mellan icke-nyckelfält.
		- Ibland är det bra att normalisera men man väljer ibland att inte göra det. Beror på hur databasen ska användas etc. Men man *bör* alltid använda 1NF.
		- Om man har icke-nnormaliserad databas kan det bli problem (hämta olika data med samma nyckel om flera fält delar nyckel ex). Det blir inte lika tillförlitligt.
# Intro
Vi har skapat en fakturamodell tidigare. Nevena har en övning med data med exempeldata som publiceras efter genomgången. 

Om vi är klara med den logiska modellen kan man ibland införa den rakt av i den fysiska modellen. Ofta hittar man dock saker som man vill förbättra innan man gör det. Det finns olika typer av optimering och anpassningar som man kan göra.
## Generalisering
Säg att man har många till många relationer som trots allt delar en urtyp, ex lärare, studenter och styrelsedeltagare har urtypen deltagare, kan man minimera mängden med många till många relationer. Det blir fortfarande relationsobjekt, men de blir färre. Ex blir det en till många relation mellan kategori till personal. En person har bara en kategori men flera personer ingår i samma kategori. 

Kom ihåg att inte skapa olika tabeller för olika typer av fordon (båt, motorcykel, bil), utan gör en för fordon.
## Optimering
### Kolumnvis delning
*Kolumnvis delning (dekomponering)*
Om man har information som man inte använder i varje fall (bokbeskrivning ex) kan man lägga det i en separat tabell. Kan även vara bra för något som behöver hårdare säkerhet. Mycket lättare att säkerställa att man har en säker tabell istället för en säker kolumn.
### Radvis delning
*Kallas även segmentering eller partitionering*
Oftast gör man det för att arkivera data. Samma tabell delas upp i kortare tabeller. 
**Fördelar**
- Kortare tabeller - går snabbare att läsa in
- Lätt att organisera arkivering av gammal data
**Nackdelar**
- Måste byta tabeller när man läser in data.
- Svårare att föra statistik över flera år, eftersom man behöver läsa in allt gammalt först.

### Kolumnvis sammanslagning
Om det visar sig att man har skapat onödiga tabeller, ex man skapar en tabell som bara har två värden (ex för distriktsnamn syd och nord) så kan det vara värt att slå ihop den igen. 
Det kan bryta mot 2NF eller 3NF, men det kan vara värt det ändå.
**Fördelar**
- Snabbare access - behöver färre joins
**Nackdelar**
-  Kan bli problem om ytterligare uppdelningar sker behöver man ändra på många ställen.
- Det man hade separat blir nu beroende av det man slår ihop det med (ex distrikt beroende av kunder)
- Man måste validera datan ytterligare innan man hämtar/använder den
### Redundanta tabeller
Variant av denormalisering. Frekventa omfattande frågor lagras i en tabell som kan läsas snabbt. Måste genereras om ifall tabeller ändras. 

Skillnaden mellan det och en view är att det en view är virtuell och en det här skapar en fysisk tabell. 
## Vad är index?
Ett annat namn på sorteringsordning. De primära är indexerade. Håller man inte datan sorterad så blir det mycket svårt att hitta rätt info.
### Olika typer av index
- Heap (oindexerad). Det finns ingen ordning på hur datan är organiserad. Sökning sker sekventiellt (man går genom varje rad för att kontrollera att det man söker finns med).
- Sorted/Hash (indexerad). Snabbare, eftersom alla rader som har med varandra att göra är nära varandra och man inte nödvändigtvis behöver söka genom hela tabellen.
### Index ... ISAM
Index-sekventiell access metod
Fysisk ordning: det som införs i kronologisk ordning.
Index på efternamn kan vara en variant av en tabell som sorterar namn i ordning och ger namnets index. 

Indexering har störst nytta när det är 10000 rader. Då är man glad att inte behöva söka genom varje rad.
#### Indexbalanserade träd
Typ av indexsökning. Det är en algoritm som redan finns och är lätt att införa. 
Man kollar om index är större eller mindre än halva index i flera steg. (större eller mindre än sju? större eller mindre än 4?)
Fungerar bara om man har en numerisk primärnyckel. Men man har oftast numerisk primärnyckel.

## Repetition Nycklar
Primary key - primär nyckel. Unik. Den primära sorteringsordningen.
Foreign Key - Främmande nyckel. *Kan vara unik*. Används för att koppla samman två tabeller. *Bör vara indexerat vid många poster*. 
Index - Anger en sortering. Kan vara unikt. Underlättar sökning.
Secondary Key - sekundär nyckel. Namn för andra än PK.
Alternate key / Candidate Key - Alternativ primärnyckel. 

Primära nycklar och unika kolumner indexerar vanligtvis automatiskt.
## För/nackdelar med index
**Fördelar**
- Det går snabbare att visa en sorterad en lista om det finns index.
- Det går snabbare att hitta en post i stora tabeller om det finns index.
- Om man indexerar den främmande nyckeln går det snabbare att hitta relaterade poster. 
	- (SELECT Enamn WHERE Enamn = 'Berg')
**Nackdelar**
- Innebär att databasen blir större (tänk på att inte indexera allt i onödan)
- Om något ändras så blir det mer arbete eftersom indexeringen behöver göras om. Inte super-viktigt med kraftfulla moderna maskiner.
# Fysisk modell
Direkt underlag till hur man bygger databasen. Ska vara en del av dokumentationen. Ska innehålla
- Tabeller
- Fält med fältnamn och datatyper
- Pk, Fk, Index
- Beroenden
- etc..
Får inte skilja sig från den fysiska modellen!
Exempel: ![Pasted image 20241227100819](https://github.com/user-attachments/assets/0e931105-5a61-4203-9675-f1734a820d35)

PK är skriven i fetstil. 

## RI - Referentiell Integritet
Om man är intresserad kan man ta sig in i Referentiell Integritet
Detta är inte superviktigt för kursen, men om man är intresserad. 

No Action/Restrict: Kan inte ändra/ta bort föräldrar om det finns barnposter 
Cascade: Tillåter att föräldrar ändras om det finns barn. Om föräldern tas bort tas även barnet bort (om du tar bort kund tar du även bort dess telefonnummer ex)
Set Default: Fungerar liknande som cascade, men istället för att det resulterar i tomma rader så får raden ett default värde som då behöver anges. 
Set null: Samma som Set Default, men man sätter null istället. Fälten måste tillåtas innehålla null.

No action och cascade är absolut vanligast.
I vissa program tillåter att man inte har någon RI. Det kan vara smart medans man håller på att bygga databasen så man inte drabbas av dessa beroenden. När man är säker på att man har fått strukturen rätt så kan man lägga till RI. 

### Allmänt
Steg 5, 6, 7 har inte lika strikt ordning, även om allt behöver göras. (volymberäkningar, tester, databaskonstruktion, dokumentation etc)
## Volymberäkning
När man skapar en databas behöver man ofta ange hur mycket utrymme den kommer behöva. Har man inte data blir man tvungen att beräkna det.

Man behöver även identifiera vilka tabeller som kan ge kapacitetsproblem (belastningsproblem). 

![Pasted image 20241227102234](https://github.com/user-attachments/assets/66fe8644-a559-492f-8ca6-6d7fe70690de)
När vi började bygga denna fysiska modell så började vi med kund för att den har 2 1:n relationer. Det är även här man börjar med volymberäkning. 
Kund - Telefon: 
0/2/5 -> min/medelvärde(eller förväntat värde)/max värde
Kund - Faktura:
1/5/20 (minst 1 faktura per kund, normalt 5 men kan variera kraftigt beroende på kravspecifikationen, 20 är max). Detta är antaget per år. 
### Beräkningsdags
Först gör man volymberäkning för ett år för att sedan kunna extrapolera ut hur mycket som behövs över tid. 

Utifrån kravspecifikationen bestäm ett antal kunder. Antag 500 kunder. Med medel antal telefoner 2 -> 1000 telefoner. 
Med medel 5 antal fakturor per kund: 5x500 = 2500 fakturor.
Fakturarader i snitt 5: 5 x 2500 = 12500 fakturarader (växer väldigt mycket!)

Man behöver inte alla tabeller. Exempelvis behöver man inte beräkna teltyp och moms, eftersom de snarare är hjälptabeller och inte är beroende av kund. 

Det ger antalet poster som ska ingå i tabellerna för första året. Den största tabellen är dimensionerande. 
### Tillväxt under 5 år
Gör antaganden
Vilka tabeller behåller samma antal poster?
	Typtabeller (moms)
I vilken takt ökar tabeller?
	Kunderlaget ökar med 10% per år
	Faktura, telefon, fakturarader ökar beroende på kund
	Artiklar kan också öka oberoende, men i detta fall antag 10%
EX: Enkel beräkning i excel
![Pasted image 20241227105503](https://github.com/user-attachments/assets/88076c51-ed8b-4a80-888a-764d02922e13)
Kom ihåg att det är ackumulerad tillväxt (dvs beräkna år 3 på år 2). Värdena för år 1 är tagna från beräkningen vi gjorde ovan.

Observera faktura som går från 2500 till 5250 på ett år, eftersom det även summeras. Den ökar kraftigt, och ännu mer ökar fakturarader. 

Faktura och fakturrader beräknas beroende på kundantal. Även om det finns ett beroende från artiklar från fakturarader så utgår från man kund. Det är huvudtabellen. 

För att kunna beräkna hur mycket diskutrymme man behöver måste man kunna beräkna hur mycket data varje tabell kräver. 

![Pasted image 20241227110528](https://github.com/user-attachments/assets/4ce99634-7da2-45bf-bfa3-ab2dc5333ec8)
Man utgår från hur mycket utrymme varje datatyp i en tabell behöver. Det summeras, för att sedan kunna multiplicerad det med antal rader från tidigare. 

Ex: Teletyp: 14 tecken, 5 rader = 70 bytes. Förblir likadan år 1 & 5.
Ex: Kund: 175 tecken, 500 rader första året = 87 500 bytes
	År 5: 128 109 bytes.
![Pasted image 20241227110654](https://github.com/user-attachments/assets/6f27d3eb-8421-4f19-b75d-8de7f19fab70)

Notera att fakturarader är väääääldigt mycket större än alla andra efter 5 år, eftersom det är en stor mängd som växer exponentiellt.

Faktura och faktura-rad kanske inte är intressant att behålla det efter massa år, med tanke på hur mycket utrymme det kommer ta. Ifall man vill arkivera bör man ta det med kunden för systemet. 
### Historik
Skapa två tabeller som är precis likandana som faktura (fakturaH) och fakturarad (fakturaradH)som vi flyttar över historisk information till regelbundet. 
![Pasted image 20241227111405](https://github.com/user-attachments/assets/144e1edc-b151-440c-904d-9c81dc0eb524)
Anledningen till att vi inte frikopplar FakturaH från kund är att vi vill fortfarande veta info om de äldre fakturorna. 

Det är väldigt viktigt att man tänker på hur historik ska hanteras när man bygger databaser, eftersom annars kommer systemet bli långsammare och långsammare med tiden. 

Inte ett krav för kursen, men det är bra att vara bekant.

## Sammanfattning Fysisk modell
- **Optimeringar**
	- Radvis, kolumnvis delning
	- Denormalisering
	- Sammanslagning av kolumner
	- Redundanta tabeller, vyer
- Indexering
- **Volymberäkning** efter fysiska modellen
	- Tillväxtplanering

# Testdata
På learnpoint, lektion 3 finns adventure works som är ett populärt dataset att använda till databaser.

# Testa själv
>Sample database - AdventureWorks  
> Skapa katalog Samples på C partition i din dator och extrahera denna .zip inuti.  
> Starta VS utan kod och välj File -> Open... -> File.  
> Öppna fil som heter **instawdb** inuti den extraherade katalog (AdventureWorks).  
Kör skriptet mot din sql server.  
(skriptet innehåller path till filen i C:/Samples/AdventureWorks, om din path är annorlunda då behöver du uppdatera skriptet.

# SQL
## Begrepp
- DML - Data Manipulating Language (SELECT, DELETE, UPDATE, INSERT)
- DDL - Data Definition Language (CREATE, ALTER, DROP)
- DCL - Data Control Language (grant, revoke) (inte jätteviktigt i kursen)
- TCL - Transactional Control Language (Begin, commit, rollback) (versionshantering)

Man kan göra mer än bara select osv med SQL, men det är det vanligaste man använder DML och DDL. 

## SQL DML
SELECT - Läser ut data
## SQL DDL
CREATE: skapa ny databas, tabell index
ALTER: Ändra existerande objekt
DROP: Radera tabell, databas, index
## SQL DCL
Grant: tilldela rättighet
Revoke: ta bort rättighet
## SQL TCL
Begin: Börja transaktion
Commit: Avsluta transaktion
Rollback: Backa transaktion
#### Övrig info
Lunch mellan 11:30 och 12:30. 
Därefter SQL syntax, sedan grupparbete. Ingen återsamling efter det. När vi känner oss nöjda får vi hoppa ut. Nevena tillgänglig till 16. 
