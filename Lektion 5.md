# Övningar
mysql compass och finns mysql workbench är alternativ till klient till det vi använder.  

För att ta bort en databas:
```sql
	DROP DATABASE kunder;
```

```sql
CREATE DATABASE medlemsregister;
USE medlemsregister;
CREATE TABLE medlem (
    id int AUTO_INCREMENT PRIMARY KEY,
	forNamn varchar(128),
    efterNamn varchar(128),
    betalt tinyint DEFAULT false
);
```

Tinyint motsvarar bool.... men inte enligt andra hemsidor. Verkar dock funka. Men det finns boolean datatyp också.

För att lägga till rader i tabellen medlem
```sql
INSERT INTO medlem (forNamn, efterNamn) VALUES ("rolf", "svensson");
INSERT INTO medlem (forNamn, efterNamn) VALUES ("julia", "evertsson");
INSERT INTO medlem (forNamn, efterNamn) VALUES ("adam", "albertsson");
INSERT INTO medlem (forNamn, efterNamn, betalt) VALUES ("eva", "albertsson", TRUE);
-- enbart eva har betalat
```

För att få fram unika namn:
```sql
SELECT DISTINCT efterNamn FROM `medlem`;
```
Det ger 3 rader från datan ovan.

Man kan exportera ut tabeller till sql-filer, och sen importera dem för att skapa nya tabeller. Nedan exempel är gjorda med en tabell som Emanuel la ut i discord.

```sql
-- Ändra en persons förnamn till Hulda
UPDATE medlemmar SET fornamn = "Hulda" WHERE id = 2;
-- Sätt deras betal-status till sann
UPDATE medlemmar set betalt= true where fornamn = "Hulda";
```

Vi vill lägga till en lista med län i en separat tabell. 
```sql
CREATE table lan (
	id int UNIQUE AUTO_INCREMENT,
    name varchar(32)
);

INSERT into lan (name) VALUES("Östergötland");
INSERT into lan (name) VALUES("Småland");
```

Visa listan med användare sorterat på om de har betalat eller inte:
```sql
SELECT * FROM `medlemmar` ORDER BY betalt desc;
```
Man kan även ändra på hur namnen på kolumnerna visas.
```sql
SELECT id, fornamn as "För namn", efternamn, betalt FROM `medlemmar` ORDER BY betalt desc;
```

För att komma åt en tabell i en annan databas:
```mysql
DESCRIBE database.table;
```

```mysql
-- för att visa hur många medlemmar som är med i gruppen
SELECT COUNT(*) as Medlemmar FROM `medlemmar`;
-- för att visa hur många som heter svensson i efternamn
SELECT COUNT(*) as Medlemmar FROM `medlemmar` WHERE efternamn = 'svensson';

-- ger medlemmar som heter jansson eller svensson
-- alternativ till en or operation
SELECT * FROM medlemmar where efternamn IN ('jansson', 'svensson')

-- för att söka case sensitive: 
SELECT * FROM medlemmar WHERE fornamn = BINARY('Hulda');
```

För att lägga till en kolumn för tidsstämplar
```mysql
ALTER TABLE medlemmar ADD COLUMN skapad TIMESTAMP DEFAULT CURRENT_TIMESTAMP AFTER betalt;
```

```sql
ALTER TABLE medlemmar
	ADD COLUMN fk_lan int default 0
    REFERENCES lan(id)
```

# Repetition
Man skapar en databas med create database namn.
Use database_namn för att slippa skriva ut allt igen och igen. 
Skapa tabell med create table...
Drop table för att slänga tabeller. 
För att ändra på en tabell, ex att lägga till kolumn Alter table.

# Joining time
Left, right, inner är de olika typerna. 
## Left Join
```mysql
SELECT person.firstname, person.lastname, person_phone.numbner
FROM person LEFT JOIN person_phone ON person.fk_phone = person_phone.id
```
En left join returnerar alla rader från den vänstra tabellen även mo det inte finns en match i den högra tabellen. 

## Right Join
En right join returnerar alla rader från den högra tabellen .... (hann inte anteckna)

## Inner Join
Returnerar bara de rader där det finns en match mellan båda tabellerna. 
Lista giltiga personer och tillhörande 
## Sammanfattning
Left: alla personer, även de utan telefonnummer
right: alla telefonnummer, även de utan person
inner: endast personer med telefonnummer. (OR)
Tips https://www.geeksforgeeks.org/sql-join-set-1-inner-left-right-and-full-joins/

# Övning igen
Nu vill vi ha ett id på alla rader i kolumnen fk_lan.
```sql
update medlemmar set fk_lan = 1 where id IS Null;
-- joining time
SELECT * FROM `medlemmar`
	inner join lan 
    	ON fk_lan = lan.id;

```

Det går köra joins mellan tabeller inom samma databas. Mellan databaser behöver man göra något annat. 

sista övning innan lunch lets go
på databasen kunder
