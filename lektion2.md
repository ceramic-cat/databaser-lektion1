# Repetition från lektion 1
- Databas - en samling med data (strukturerad eller ostrukturerad)
	- Består av minst en tabell (om det är en relationell)
		- Har ett namn
		- Tillhör ett schema
		- En eller flera kolumner/fält (fields)/attribut
			- Specialtyper av kolumner
				- Primär nyckel (unik identifierare av varje rader)
				- Främmande nyckel (eller sekundär nyckel) (sammankopplar rader från en annan tabell, används för att skapa relationer mellan tabeller)
		- Rader, även kallade poster eller records
- Databasmotor - verktyg för att hantera databaser
- Databasmodellering - Att göra en plan eller modell för att skapa en databas
	- Konceptuell
	- Logisk
	- Fysisk
- Relationer
	- 1 till 1
	- 1 till många
	- många till många
		- Behöver en extra tabell, en relationstabell, för att visa hur de relaterar till varandra

## Relationer forts.
När man gör relationsobjekt kan man döpa dem som man vill. Det är inte ovanligt att man döper dem till en sammanslagning på flera ingående tabeller/kolumner.

# Logisk modell
Steget som följer konceptuell modell i utv processen. 
### Objektifiering vid n:m relation
Rita ett objekt och tilldela objektet en egen primärnyckel. Relationer vänds också (om man skapar ett ägarobjekt mellan bil och person ex i powerpoint). 
När man objektifierar en n:m relation så kommer man inte att skapa primärnyckel med många kolumner. Vanligare är att man skapar en relationstabell.

(bra info i slides med bilder)

### Egenrelationer
Relationer mellan olika rader av objekt. Ex om man har en tabell med relationer mellan personal, där det finns en tabell där personens chefs id är med som främmande nyckel. 
De kan även vara många till många, ex omvandlingsfaktorer mellan enheter.

### Funktionellt beroende
Om du vet ett produktID kan du hitta produktnamnet. Det betyder att produktnamnet är funktionellt beroende av produktID.
Nettoinkomsten är funktionellt beroende av bruttoinkomsten och inkomstskatten. 
## Normalisering/Normala Formen
I praktiken är man glad om man når 3e eller 4e normala formen.
Hittills har vi tagit oss från  konceptuell modell till logisk modell med objektifiering.
Man kan även använda normalisering.

Normalisering innebär att testa befintliga tabeller med beroenden, avlägsna redundans och oönskade bieffekter vid radering, insättning och uppdatering av poster i databasen. Man ska säkerställa att problem inte uppstår vid modifiering av poster i databaser.

Man går från första normala formen, sedan vidare i stigande ordning. Man måste fortsätta uppfylla samtliga former.
### Första normala formen
(1 NF)
- **Unik nyckel** - En tabell måste ha en unik nyckel för varje post
- **Atomära fält** - Fält får inte växa på bredden  (flera datapunkter i samma kolumn ex)
	- Leder till att man får dela upp tabellen med den extra informationen
Ex Adresser bryter inte nödvändigtvis mot 1NF, förutom om man kanske vill sortera efter postort eller liknande. Kan finnas en fördel att vara proaktiv. Samma med namn. Bryter inte nödvändigtvis men kan finnas en fördel med att dela upp förnamn och efternamn för att underlätta sökning. 
### Andra normala formen
Alla fält som inte är nycklar (både primära och sekundära) ska vara beroende av alla nycklarna (primära och sekundära). (bra exempel i slides)
### Tredje normala formen
*Det får inte finnas några funktionella beroende mellan icke nyckelfält.*
Det finns fler normala former, men resten är inte krav för kursen. 
### Fördelar
- lättare att skala och uppdatera eftersom det inte finns några beroenden som bråkar med varandra
- Tar mindre plats eftersom man undviker dubbletter
### Nackdelar
- tar längre tid, eftersom det blir många joins när det är dags att presentera datan (eftersom flera tabeller)
	- kan underlättas av en view som innehåller vanliga joins
	- Paginering (select top 10, sen nästa 10, nästa 10...)
- besvärligare programmering
	- Fler tabeller att hålla reda på, men det finns hjälpmedel för att hjälpa till med det
