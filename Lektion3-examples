# Exempel med Adventure Works

## Tips
Om man vill testa att modifiera kan det vara bra att göra ett duplikat av tabellen först för att vara säker på att man inte pajar något.
```sql
-- Skapa ny tabell som kopia från Person.Person
select * into demo from Person.Person 

-- För att visa allt
select * from demo

-- För att visa en rad
select * from demo 
where BusinessEntityID = 1

```
Ovan kommer att spara in allt i Person.Person till dbo.demo för att vi inte valt ett annat schema.
## Select
Skapa skript: Högerklick på databasen: New Query
**Kontrollera att du kör mot rätt databas.**
DBO: Default schema. Används mycket för 
allt som är kopplat till en person finns i personschemat, ex Person.Address, Person.Contact. Alla personer sitter i samma schema.

Om vi vill ha info om Sales.SalesOrderDetail så kan man högerklicka på den och sen View Data. Det ger ett kaos med data. Istället vill vi se något mer specifikt.
```sql
select SalesOrderID, LineTotal -- VIlka info jag vill ha
from Sales.SalesOrderDetail -- Från vilken tabell
```

Man måste inte spara sina query, men man kan spara dem som .sql filer. Man kör dem som vi gjort tidigare med kör-knappen.
När man kör scriptet ovan så kommer man få resultat i resultatrutan. 

```sql
select SalesOrderID, SUM(LineTotal) as Total
from Sales.SalesOrderDetail
group by SalesOrderID
```
as Total sparar resultatet från SUM(LineTotal) som ett alias.
group by anger att det ska grupperas via SalesOrderID.

Man kan markera vissa rader i ett skript för att bara exekvera dem.

Om man vill ytterligare filtrera kan man göra följande:
```sql
select SalesOrderID, SUM(LineTotal) as Total
from Sales.SalesOrderDetail
group by SalesOrderID
having sum(LineTotal) > 100000.00
```
## Update
```sql
update demo
set FirstName = 'Nina', LastName = 'Hilton' where BusinessEntityID = 1
```
**VARNING** Om man inte kör where någonting så kommer man skriva över alla rader med uppdateringen. 
## Insert
```sql
insert into dbo.demo
values ('999999', 'EM', '0', 'Mrs', 
'Steve', 'Middlename', 'LastName', '', '0', null, 
'<IndividualSurvey xmlns="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/IndividualSurvey"><TotalPurchaseYTD>0</TotalPurchaseYTD></IndividualSurvey>', 
newid(), GETDATE())

select * from demo
where BusinessEntityID = '999999'
```
innanför parentesen för values ska man skriva alla värden som behövs per rad. 

## Delete
Använd helst bara primär nyckel för delete, så man inte råkar ta bort massor med rader.
```sql
select * into demo2 from Person.Person
select * from demo2

-- för att hitta ett id
select * from demo2
where FirstName = 'Ken' and LastName = 'Kwok'

-- This kills the ken
delete demo2 
where BusinessEntityID ='2300'

-- Om man vill ta bort businessID mindre än 100
delete demo2
where BusinessEntityID < '100'

```
### Truncate
För att ta bort info från en tabell kan man använda Truncate. 
```sql
truncate table demo2
```
Demo2 finns fortfarande kvar med kolumner etc, men det finns inga rader kvar.
## Drop
För att ta bort en tabell
```sql
drop table demo
```
