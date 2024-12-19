Om man ska nå Nevena, kontakta henne på hennes contactus. Hon använder också teams flititgt. Om det är en fråga från basgruppen kan man använda basgruppens kanal. 

Inga krav på litteratur. Microsoft learn och GeeksForGeeks är poppis.

Hon använder classpoint (https://www.classpoint.app/) med kod SYNE24L
## Kursintro
Kursmoment 1: Intro till IT och modellering, SQL frågesproåk
Kursmoment 2 tar Emanuel.

Vi ska kunna tillräckligt om de olika typerna av databaserna så att vi kan motivera val av typ av databas. 
### Examinerande moment
- Laboration datamodellering (start 20/12) 
	- Databas görs tillsammans
	- Frågor besvaras enskilt
	- Vad lämnas in? .bak fil (backup) och dokument med svar på frågor
	- Inlämning 6/1
- Laboration ORM 
- Laboration NoSQL 
- Muntlig redovisning, individuell (max VG)

## Informationsteori
### Data och Databas
En hemsida visas i en klient (webbläsare). Den skickar förfrågor i form av webbside-anrop till en webbserver. Den har information om hur sidan ser ut, men den har inte alltid all information om stor data. Det skickar SQL anrop till en databas.

Vad är skillnaden mellan data och information?
Data: 10, 39
Information: Klockan är 10, 39 kr är ett belopp.

Skillnaden mellan Information och kunskap? 
Klockan är 10, då är det mellis.
Slutsatser man kan dra från information.
#### Data
en samling med värderingar, fakta, siffror som har samlats in
En databas är ett bibliotek eller lagringsområde. Dubbelriktad (kan importera in data och returnera informationen). Det finns många olika typer av databaser.
#### Relationsdatabaser
- Relationsdatabaser är vanligast (80%). 
- strukturerad data
- standardiserade frågespråk
#### NoSQL
- NOSQL är senast, används främst för väldigt stora mängder data (ex twitter, facebook). Namnet är missvisande (använder SQL), är inte strukturerad.
- stora ängder ostrukturerad data
- kräver hög skalbarhet
- hög flexibilitet
#### Varför Databas?
Möjliggör att kunna göra dynamiska hemsidor (produkter på en hemsida)
- Skapa ett inlägg på en blogg
- Boka en biobiljett
- etc...


### Relationsdatabas
En databas som lagrar information och organiserar den som en samling tabeller med relaterade punkter. Kan visas som en eller flera tabeller. Relationer kan finnas, men är inte ett krav. 
Ofta kan man skapa flera tabeller som bara innehåller data som är relaterad till ett visst avseende. Ex en tabell för adress, en för medlemsstatus, etc.

Lucidchart är ett exempel på verktyg som kan användas för att rita upp datamodeller. 
#### Tabeller
Exempel på tabeller för en butik:
- Kundtabell
- Tabell över lagervaror
- Ordertabell
- Fakturatabell
- Betalningstabell
#### Fields
Kolumnerna. Styrs av syftet med datan. 
Vanligtvis finns det ett fält som används som nyckelfält (key column). Unik identifierare.
#### Records
Varje rad. Kan även kallas poster eller tuples. 
#### Databasscheman
 Skiss över en logisk vy på hur datan organiseras.
#### Views
Att kunna visa upp enbart datan som är relevant för ett specifikt syfte utan att behöva skapa nya tabeller. Sammanställs bara när man gör anrop till vyn. Kan innehålla info från en eller flera tabeller.

### Databashanterare
en applikation som lagrar och hanterar data. Database Management System (DBMS). Vi kommer att arbeta med det utifrån visual studio. 
Exempel som MongoDB, MySQL, Oracle, Microsoft SQL Server osv.
Det är verktyg för att hantera databaser, inte själva databaserna.

#### SQL
Standardiserat frågespråk. Relativt nära vanligt tal. ex
```sql
// Hämta alla studenter som tillhör kursen databaser
SELECT * FROM Studenter WHERE Course ='databaser'
FROM Medlem INNER JOIN Avgift
ON Medlem.Mednr=Avgift.Mednr
WHERE Mednr=550
ORDER BY Betalt DESC

INSERT INTO Avgift (Mednr,Betalt,Avgift)
VALUES (550,'2008-11-05', 150)

UPDATE Medlem SET Telefon='xxxx'
WHERE Mednr=550

```
Select: hämta
Where: Begränsar, 
From: vilken tabell
I entity framework kan man använda select som en metod.  (Select(vad man vill välja)).

ORDER BY: Standard är asc (ascending), annars desc (descending)

### SQL Server
En databasmotor som bearbetar transaktioner för att skapa databaser, skapa och manipulera tabeller och hantera data som finns i dem. 
Den säkerställer även att databasen är
- Säker
- Presterar bra
- Säkerställer integritet
- Säkerställer datatillförlitlighet
- Tillåter flera samtidiga åtkomster till databasen
## Datamodellering
1. Analys för behov, processer som berörs, intervjuer med kund
2. Datamodeller skapas, skapa diagram
3. Skapa underlag för att skapa databasen
### Vad är datamodellering
Att utforma databaser.

Ex att man vill ha en databas för att hålla koll på vilka som registrera sig på vilka kurser. Du vill kunna undvika att vara tvungen att lägga till massor med kolumner. Smart att skapa flera olika databaser som är kopplade till varandra med KeyID. Att upprepa väldigt mycket information många gånger ska undvikas.

Man vill utgå från vilken fråga som ställs oftast. 

Datamodellering är i stort sett att göra en plan för hur databasen ska fungera och vilka tabeller ska ha relationer med varandra och hur. 

En bra databasstruktur
- är lätt att skala
- dubbellagrar inte
- är lagom stor och inte för komplicerad
### Livscykelmodellen
(finns bra bild i presentationen)
Datamodellering ska påbörjas i mitten av analysen av systemet. 
### Datamodellering
Se utvecklingsprocessen. Glöm inte bort att dokumentera ;)
1. Identifiera objekt, attribut och relationer (en skola har många utbildningar. en skola har många elever)
2. Avbilda verkligheten (enkel bild för att kunna diskutera)
3. Rita en datamodell
4. Skapa tabeller (fyll i testdata, så man ser att det funkar som man har tänkt sig. Man brukar inte ha jättelånga namn på kolumnerna)

#### Namngivning av objekt
- **Varje objekt måste ha ett eget unikt namn**. 
- Namn ska vara singular (kund, inte kunder). 
- Använd helst engelskt alfabet och engelska ord. Om man ska använda svenska tecken behöver man sätta klamrar runt. Värdet i objekten kan vara vad som helst. 
- Använd helst inte mellanslag i namn
- Namnet bör beskriva innehållet.
#### Några symboler och begrepp
(symboler finns i presentationen)
### Nycklar
Primärnyckel är unika för varje rad. 
Främmande nyckel är en sekundär nyckel i en tabell som ofta är primär i sin ursprungliga tabell.
#### Primärnyckel
Består av ett eller flera fält (ex namn och födelsedag tillsammans som nyckel).
Man brukar markera den med en linje över namnet i bilder på tabeller.
#### Främmande nyckel
Används för att skapa relationer mellan tabeller. Den måste ha samma datatyp mellan olika tabeller när man skapar dem. 
## Objekttyper
### Självständiga objekt
- Oberoedne av andra objekt. 
### Beroendeobjekt
- Ägs och är beroende av att det finns ett föräldraobjekt (varför skulle du lagra telefonnummer utan att koppla det till en person)
### Relationer
- 1-1 En till en
- 1 - n En till många
- n - 1 många till en
- n - m många till många
Kan ha max och min regler. Ex att en skola kan ha flera, men minst en utbildning. 

1:1 relation och n:1
Onödigt att ladda in all information som kanske inte behövs varje gång. Ex bokbeskrivning som är ganska lång vill man inte ha med varje gång man ska kolla upp något om alla böcker. 
#### Många till många relationer
En bil kan ägas av flera personer. Flera personer kan äga samma bil. Det kan leda till ett många-till-många relation. Det kräver en extra tabell som visar relationerna, en kopplingstabell. 

## Data modellerings exempel
En person köper en eller flera bilar bid olika datum. En bil kan köpas av flera personer vid olika datum. Ett visst datum kan det ske ett eller flera bilköp.
1. Identifiera vilka de olika objekten är (kund, datum, bil)
2. Skapa en konceptuell datamodell med objekten som uppfyller kraven ovan 
3. Eftersom vi får många till många relationer måste vi använda oss av relationsobjekt som kopplingstabeller
	- Eftersom varje datum kan det ske flera köp (som kräver info om bil och kunder) så måste relationspunkten hamna mellan Datum och Person + Bil
## Tid för frågor och funderingar, miljö
## Installation

SQL Command Line Utilities
SQL Server Data Tools
SQL Server 

Typ alla utom preview

sätt nyckel i registry för att tillåta större partition för sqlserver
om sql browser inte dyker upp i "tjänster" så kan det bero på ovan. 

Fix med server
Om man skapar en egen server på en separat dator kan man koppla sig till på nätverket om man lägger upp den öppet på lokala nätverket.
Varianten som ingår i VS kan man bara komma åt från VS.


# Fix med övning
För att skapa ny databas: Högerklicka på servern.

För att lägga till stored procedure:
I databasen, Programmability. Högerklicka på Stored Procedure och lägg till en ny.
