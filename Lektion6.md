# Överblick
Repetition
Vyer
Slow-queries: att hitta långsamma queries (vad segar ned allt?)
Lite "versionshantering"
Plan är föreläs fram till lunch, kanske lite efter. 
# Repetition SQL
SQL är nästan samma överallt. Det finns variationer på syntax mellan olika databasmotorer, ex SQL server MySQL. 

Om man använder limit behöver man också använda offset om man vill bläddra mellan resultat. 
# View
Kan användas för att kolla andra saker
```mysql
create view ny_namn AS
SELECT kolumn FROM tabell_namn WHERE villkor;

-- ex
CREATE VIEW felTelefonNummer AS SELECT id, number FROM person_phone WHERE number LIKE '%-%';

-- för att göra som ovan med en extra rad med fullständiga mängden med number
SELECT COUNT(*) from feltelefonnummer UNION SELECT COUNT(*) from person_phone;

-- för att joina alla felaktiga nummer med personer:
select * from feltelefonnummer AS a 
INNER JOIN person as b on (b.fk_phone = a.id or b.fk_mobile = a.id);

-- för att göra en vy baserad på en vy
CREATE VIEW felTelefonummerMedPerson AS (

SELECT a.id, a.number, b.firstname, b.lastname FROM feltelefonnummer AS a
    INNER JOIN person AS b 
    ON (b.fk_phone = a.id OR b.fk_mobile = a.id)
);

CREATE VIEW VY_Medlemmar AS (
    SELECT firstname, lastname FROM `person` ORDER BY firstname DESC
);
-- för att uppdatera senaste vyn
CREATE OR REPLACE VIEW VY_Medlemmar AS (
    SELECT firstname, lastname FROM `person` ORDER BY firstname DESC LIMIT 3
);

```
# Aggregering
Om vi vill veta hur många som har ett visst telefonnummer:
```mysql
SELECT fk_phone, COUNT(*) AS person_count FROM person GROUP BY fk_phone;
```
(bra för bokhandlen)
För att räkna hur många som har samma efternamn: 
```mysql
SELECT lastname, COUNT(*) AS antal FROM person GROUP BY lastname ORDER BY antal desc;
```
# Maths
```mysql
SELECT YEAR(CURRENT_TIMESTAMP), YEAR('1987-03-15'), (YEAR(CURRENT_TIMESTAMP) - YEAR('1987-03-15'));
```

# Koppla till databas från applikation
- skapa connection med inloggning, skicka sql, få tillbaka data
- tänk på att skapa lämpliga användare med lämpliga behörigheter
- Genom att använda prepared statements kan man undvika sql-injections

 sträng för anslutning börjar med mysq (eller mots) localhost:3306/namnpådatabasen.
 ```mysql
select * from employees where lastname =? and firstname like?"; prepStmt = 

$title = $_GET["title"];
myspli_exec($m, "select * from bnooks where title=.$title.";");
-- exempel på url:

```
Var noga med att använda språk som fortfarande är moderna och underhållna. Använd även prepared statements för att undvika att folk kan påverka vad som händer. Vi bör även överväga vilka rättigheter databasanvändarna har. Skilj kritiska system från produktanvändarna. 

# export
backup vs export: export innehåller inte nödvändigtvis all information. Lock tables ser till att ingen skriver till tabellen medans man paketerar ihop den. 

# Versionshantering
Data: informationen i databasen (records). Behöver göras backups på ofta för att skydda. 

Utöver det behöver man ha versionshantering på strukturen på databasen (tables, views, keys). Det bör hållas separat från data-backupen.

Man kan köra en full backup där både struktur och data ingår, men det blir jobbigt.

Var noga med att förvara data separat från systemet som den tas från. 

Om man tar backuper ofta utan att ta bort äldre backuper så finns en risk att utrymme på diskar tar slut för att ta nya backuper. Glöm inte bort att sätta det i system också.

Glöm inte heller bort att ha lika hög eller högre säkerhet på backupen som databasens system. 
Med MySQL workbench installerat kan man köra följande rader i terminalen för att få en backup till pathen i slutet av koden.
```terminal
cd "C:\Program Files\MySQL\MySQL Workbench 8.0 CE" 

.\mysqldump.exe -u root -p --databases persons > backup.sql
```

# Slowquery
För att hitta vad som är långsamt i en databas kan man skapa en loggfil som lagrar det som är långsamt. 
```mysql
SET GLOBAL slow_query_log=1; 
SET GLOBAL slow_query_log_file="C:/xampp/mysql/data/latest-slow-query.log"; 
SET GLOBAL long_query_time=1;
-- för att kontrollera att de är aktiverade
SELECT @@slow_query_log;
SELECT @@slow_query_log_file;
SELECT @@datadir;
```
När man sedan öppnar filen listas de query som tagit lång tid. 
