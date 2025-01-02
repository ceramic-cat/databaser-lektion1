# Planering 
### Kursmoment 3
30/12: Begrepp och syntax i SQL
2/1: Hämta och modifiera data i en relationsdatabas med SQL
3/1: Drift och versionshantering
### Kursmoment 4
13/1: Intro NoSQL
14/1: Bearbeta data i NoSQL

# Databasmotor
Jämför databasmotor med skillnaden mellan office, google suite eller liknande. Man väljer olika beroende på vilka behov man har. Databasmotorer allmänt pratar SQL. En NoSQL-motor pratar något annat. 
Avvägningar: 
- Ska man programmera på databasmotorn? (inte aktuellt för kursen)
- Hur tar man backup? Har de supportavtal?
- mm
Om man skriver ett SQL query kommer man få samma resultat oavsett motor (kan skilja i hastighet etc).
Databasmotorn tar hand om användare och lagring. 

Finns olika typer av NoSQL databaser: dokumentdatabaser, minnesdatabaser, keeperdatabaser, grafdatabaser (kommer senare).

# Databaser
Det är nästan alltid dumt att försöka utveckla en egen databastyp, då det är ett mycket moget tekniskt område.
### SQL tips
- Använd CAPS LOCK till SQL operander (select, insert etc)
- Fält ska döpas till singular
- Ta backup's ofta, det är mycket lätt att råka ta bort mer än man tror
	- Vem bestämmer hur ofta? Fråga den som har ansvar (IT chef etc). Men minst varje gång man gör ändring. Mellan 1-2 ggr per dag brukar vara ok. Ibland 1/hr.
## MySQL
Relationsdatabas
- Snabb och enkel
- OpenSource
## Användare
Notera: Användare för databasen, inte för produkten.
- Backup-användare: behöver aldrig skriva, den läser data från databasen till en fil för att skapa en backup. Ska *inte* kunna göra något i databasen i sig.
- Felsökning: Om flera processer gör saker men det inte är tydligt vilken av dem som har problem, så kan man tilldela dem olika databasanvändare med olika namn för att kunna se vilken av dem som inte funkar som den ska.
- Admin för att administrera användare
- Monitoreringstjänst: ex för att kontrollera att telefonnummer som matats in av användare är korrekt formaterade. 
- 
## Övning 1 MySQL 
- Laddat ner senaste version klient från https://www.apachefriends.org/download.html
- Installerat 
- startat apache och mySQL
- Öppna inställningar för MySQL
- Öppna fliken SQL

 ```sql
CREATE DATABASE mydb;
-- Skapa användare med lösenord ohemlig
CREATE USER myuser1 IDENTIFIED BY "ohemlig";
-- Ge rättigheter till user1
GRANT ALL PRIVILEGES ON mydb.* TO 'myuser1';
-- skapa 
GRANT SELECT ON mydb.* TO 'myuser2';
```

Skapa en tabell
```sql
-- antingen nuse mydb eller klicka in på sql-fältet för mydb
use mydb;
CREATE TABLE users (
    id int AUTO_INCREMENT PRIMARY KEY,
    first_name varchar(60),
    last_name varchar(80),
    email varchar(50)
);
```

För att se strukturen på tabellen: klicka in på mydb och sedan structure.  

## what is it good for?
- automatiska anrop till api'er
- logghantering för senare analys
- automatisering av uppgifter 
- csv filer kan läsas in på flera olika databasmotorer
- BI (business intelligence)
- visualisering av data

## Övning 2
skapa en databas testdb
skapa en användare dbuser1 som har fullständiga rättigheter i databasen testdb1 och får skapa nya användare
låt dbuser skapa testtaabeller

```sql
-- using mydb så det redan finns användare
CREATE TABLE movies(
	-- unique ersätter primary key, har samma funktion
	id int AUTO_INCREMENT UNIQUE, 
    name varchar(80) NOT NULL,
    description text DEFAULT 'beskrivning saknas'
);

```

## Användarrättigheter
Läsa - select
Skriva - insert
Uppdatera - update
Alla - all privileges
finns massor med fler, ex att kunna köra processer etc. men läs på om dem vid behov. 
## Foreign keys
```sql
CREATE TABLE tags (
	id int AUTO_INCREMENT unique,
    tag varchar(40),
    user_id int,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE ratings (
	id int AUTO_INCREMENT UNIQUE,
    rating int,
    user_id int,
    movie_id int,
    FOREIGN KEY (user_id) REFERENCES users(id) ,
	FOREIGN KEY (movie_id) REFERENCES movies(id)
);
```
Observera att man behöver skapa kolumnerna som ska innehålla foreign keys innan. Annars blir de dumma.
Om man redan har en tabell som man ska lägga till foreign keys till:
```sql
ALTER TABLE ratings ADD FOREIGN KEY (user_id) REFERENCES users(id)
```

```sql
-- för att lägga till en record i ratings
-- först med kolumnnamn, sedan värden som matchar ordningen
INSERT INTO RATINGS (rating, user_id, movie_id) VALUES (9, 12, 13)

```

Om man råkar missa att lägga till increment på primary key behöver samtliga värden vara null för att man ska kunna lägga till auto-increment.


## Övning 3 
```sql
-- Skapa tabeller
CREATE TABLE person (
    id INT AUTO_INCREMENT PRIMARY KEY,
    firstname VARCHAR(100),
    lastname VARCHAR(100),
    fk_phone INT,
    fk_mobile INT
);

CREATE TABLE person_phone (
    id INT AUTO_INCREMENT PRIMARY KEY,
    number VARCHAR(20)
);
```
För att läsa ut alla personer:
```sql
select * from person;
```
 För att söka fram person baserat på efternamn. Kommer strikt att följa stora och små bokstäver och stavning.
 ```sql
SELECT * FROM `person` WHERE lastname = "Andersson";
```
Visa alla namn, sorterade på förnamn:
```sql
SELECT * FROM `person` ORDER BY firstname; 
```
### Limit
Limit måste vara sist i anropet.
för att få de första:
```sql
SELECT * FROM `person` order by id limit 1; 
```
de 3 senaste
```sql
SELECT * FROM `person` order by id desc limit 3; 
```
för att hitta personer utan telefonnummer:
```sql
SELECT * FROM `person` WHERE fk_mobile IS null;
-- med eller uttryck
SELECT * FROM `person` WHERE (fk_mobile IS null OR fk_phone IS null);
```
### Wildcard
För att kunna söka med wildcard-tecknet % kan man använda LIKE
```sql
SELECT * FROM `person` WHERE lastname LIKE '%son';
```
För att söka på en specifik bokstav (case insensitive)
```sql
SELECT * FROM `person` WHERE firstname LIKE '%n%';
```
För att söka på nummer som börjar på +45
```sql
SELECT * FROM `person_phone` WHERE number LIKE '+45%'
```
För att hitta alla telefonnummer med bindestreck i (sorterad i ordning)
```sql
SELECT * FROM `person_phone` WHERE number LIKE '%-%' ORDER BY number ASC
```

### Distinct
```sql
-- visa bara unika namn
select DISTINCT(lastname) from person order by lastname;
-- visa hur många som heter andersson
select count(*) from person WHERE lastname ="Andersson";
```

för att ta bort
```sql
DELETE from person where lastname="Andersson"
```

Om man vill skapa text utifrån ingående information
```sql
SELECT lastname, concat ('https://www.linkedin.com/in/', firstname, ".", lastname) as "linkedinurl" from person;
```
## Join
```sql
SELECT * FROM person 
	INNER JOIN person_phone as phone 
	--person_phone sparas som phone
    on person.fk_phone = phone.id ;
    -- där person.fk_phone matchar phone.id :)
```
## Tips
För att se hur en tabell är skapad kan man skriva:
```sql
DESCRIBE --namn på tabell
```
