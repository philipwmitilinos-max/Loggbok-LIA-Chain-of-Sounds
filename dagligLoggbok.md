**Datum:** 9 november 2026  
**Veckodag:** Måndag  
**Arbetsplats:** Chain Of Sounds

### 1. Arbetsuppgifter

Idag började jag undersöka min LIA-uppgift, som handlar om att ändra MixerLab så att användarnas mixes sparas i en databas i stället för i webbläsarens localStorage.

Jag började med att bekanta mig med projektets struktur och undersöka hur frontend och backend kommunicerar med varandra.

Jag undersökte bland annat:

- Hur MixerLab sparar och laddar användarens mixar idag.
- Hur frontendens API-klient är uppbyggd.
- Hur Mixer-backendens databas är strukturerad.
- Hur användare och andra entiteter representeras i databasen.
- Hur befintliga API-endpoints och databasanrop fungerar.

Jag tittade även på befintliga Entity Framework-konfigurationer och databasmigrationer för att förstå hur projektet hanterar datalagring.

### 2. Tekniker och verktyg

- React och TypeScript.
- C# och .NET 10.
- Entity Framework Core.
- PostgreSQL.
- Git och GitHub.
- Visual Studio Code / utvecklingsmiljön.

### 3. Problem och lösningar

En utmaning var att förstå hur projektets olika delar hänger ihop, eftersom det är ett större projekt med flera lager.

Jag började därför undersöka befintlig kod steg för steg, bland annat databaskonfigurationer, migrationer, API-endpoints och Query Handlers.

Jag har ännu inte gjort några kodändringar. Fokus idag har varit att förstå den befintliga arkitekturen innan jag börjar implementera lösningen.

### 4. Ny kunskap

Jag har fått en bättre förståelse för hur Mixer-backenden är uppbyggd med olika lager och hur de kommunicerar med varandra.

Jag har bland annat lärt mig mer om:

- Hur Entity Framework Core används för att koppla applikationen till databasen.
- Hur databasmigrationer används för att förändra databasschemat.
- Hur egna domäntyper, exempelvis UserId och BacktrackId, hanteras.
- Hur relationer mellan entiteter konfigureras.
- Hur API-endpoints använder Commands och Queries för att kommunicera med Application-lagret.
- Hur Query Handlers hämtar information från databasen.

### 5. Reflektion

Dagen har framför allt handlat om att sätta mig in i ett befintligt projekt och förstå hur det är uppbyggt.

Jag har märkt att det är viktigt att förstå projektets arkitektur och befintliga arbetssätt innan man börjar göra förändringar. Det gör det lättare att bygga vidare på projektet utan att riskera att påverka befintlig funktionalitet.

Nästa steg blir att fortsätta undersöka hur den inloggade användaren identifieras i backend och hur användardata kopplas till databasen. Därefter kan jag börja planera hur MixerLab:s mixes ska sparas.

**Arbetstid:** 09:30–17:00

- 09:30–11:23: Videomöte med 10 minuters paus kl. 10:50.
- 11:23–12:55: Eget arbete med LIA-uppgiften.
- 12:55–13:55: Lunch.
- 13:55–17:00: Fortsatt arbete med LIA-uppgiften.

**Effektiv arbetstid:** 6 timmar och 20 minuter.

**Sammanfattning av dagen**

Dagen började med ett videomöte där vi gick igenom arbetet och LIA-uppgiften. Efter mötet ägnade jag resten av dagen åt att bekanta mig med MixerLab-projektets struktur och undersöka hur frontend, backend och databasen samverkar. Jag fokuserade särskilt på hur befintlig data sparas, hur databasen är uppbyggd och hur backend hanterar API-anrop. Målet var att få en bättre förståelse för projektet innan jag påbörjar själva implementationen.

**Datum:** 10 november 2026  
**Veckodag:** Tisdag  
**Arbetsplats:** Chain Of Sounds

### 1. Arbetsuppgifter

Idag fortsatte jag arbetet med min LIA-uppgift, som handlar om att ändra MixerLab så att användarnas mixes sparas i en databas i stället för i webbläsarens localStorage.

Dagen började med ett videomöte med Johan Hellberg där vi gick igenom kraven för den nya sparfunktionen och hur arbetet skulle läggas upp.

Efter mötet fortsatte jag undersöka projektets befintliga struktur och hur MixerLab fungerar.

Jag arbetade bland annat med:

- Undersöka hur MixerLab hanterar backtracks och tracks.
- Undersöka befintliga domänklasser i backend, exempelvis User, Backtrack, TrackSet och Asset.
- Undersöka hur identifierare (ID) är implementerade i domänlagret.
- Undersöka hur MixerLab sparar inställningar i localStorage.
- Påbörja implementationen av en ny domänklass, Mix, i Mixer.Domain.Mixes.

Jag skapade även en grundläggande struktur för Mix-klassen och implementerade en metod för att skapa nya Mix-objekt.

### 2. Tekniker och verktyg

- React och TypeScript.
- C# och .NET 10.
- Entity Framework Core.
- PostgreSQL.
- Git och GitHub.
- IntelliJ / utvecklingsmiljön.

### 3. Problem och lösningar

En utmaning under dagen var att förstå hur identifierare och relationer mellan olika domänklasser fungerar i projektet.

Jag undersökte därför befintliga implementationer av exempelvis BacktrackId, TrackSetId och UserId för att följa samma struktur när jag skapade MixId.

Jag behövde även undersöka hur MixerLab hanterar sina backtracks och tracks i frontend för att förstå vilken information som behöver sparas.

Jag påbörjade implementationen av Mix-klassen och lade till grundläggande validering av mixens titel.

### 4. Ny kunskap

Jag har fått en bättre förståelse för hur domänklasserna är uppbyggda i C# och hur projektet använder starkt typade identifierare.

Jag har bland annat arbetat med:

- Hur statiska metoder kan användas för att skapa domänobjekt.
- Hur privata konstruktorer och privata setters används för att kontrollera objektens skapande och förändring.
- Hur validering kan implementeras direkt i en domänklass.
- Hur `DateTimeOffset` används för att hantera tidsstämplar.
- Hur befintliga domänklasser kan användas som förebilder när nya klasser implementeras.

### 5. Reflektion

Dagen har framför allt handlat om att gå från undersökning och planering till att påbörja själva implementationen.

Jag har nu börjat skapa grunden för Mix-domänklassen och fått en bättre förståelse för hur den kan följa samma struktur som projektets befintliga domänklasser.

Jag har också fått en tydligare bild av vilken information som behöver sparas för att användarnas mixes ska kunna återskapas.

Nästa steg blir att fortsätta utveckla Mix-klassen genom att lägga till stöd för tracks och equalizer-inställningar. Därefter ska jag implementera en metod för att uppdatera befintliga mixes och skriva enhetstester för domänklassen.

**Arbetstid:** 09:30–17:00

- 09:30–10:00: Videomöte med Johan Hellberg.
- 10:00–12:45: Eget arbete med LIA-uppgiften.
- 12:45–13:45: Lunch.
- 13:45–17:00: Fortsatt arbete med LIA-uppgiften.

**Effektiv arbetstid:** 6 timmar och 30 minuter.

### Sammanfattning av dagen

Dagen började med ett videomöte med Johan Hellberg där vi gick igenom kraven för att MixerLab ska kunna spara användarnas mixes i en databas. Efter mötet fortsatte jag undersöka projektets befintliga struktur och hur MixerLab hanterar backtracks, tracks och inställningar. Jag gick därefter vidare till implementationen och skapade grunden för en ny domänklass, Mix, i C#. Jag implementerade även en Create-metod och grundläggande validering av mixens titel. Nästa steg blir att utveckla domänklassen vidare och därefter skriva enhetstester.

**Datum:** 11 november 2026  
**Veckodag:** Onsdag  
**Arbetsplats:** Chain Of Sounds

### 1. Arbetsuppgifter

Idag fortsatte jag arbetet med min LIA-uppgift, som handlar om att ändra MixerLab så att användarnas mixes sparas i en databas i stället för i webbläsarens localStorage.

Dagen började med ett möte med Johan Hellberg mellan 09:30 och 10:40. Under mötet gick vi igenom hur arbetet skulle genomföras och diskuterade vilken teknik som skulle användas.

Vi kom fram till att all användardata ska hanteras i Java i stället för C#. Den tidigare planen att skapa en Mix-klass i mixer-api ändrades därför. I stället ska implementationen göras i studio-bass-api.

Efter mötet skapade jag en ny Git-branch från dev:

`feature/mixerlab-database-persistence-java`

Jag började därefter bygga upp strukturen för den nya funktionen i Java.

Jag arbetade bland annat med:

- Skapa en ny entitetsklass, Mix, för att representera en sparad mix.
- Lägga till en koppling mellan Mix och User.
- Skapa MixEq för att hantera mixens equalizer-inställningar.
- Lägga till stöd för att spara ID:n för valda ljudspår.
- Skapa MixRepository för framtida databasoperationer.
- Påbörja arbetet med MixRequestDTO för att ta emot information från frontend.

Jag undersökte även befintliga entiteter och enum-klasser i projektet för att försöka följa samma struktur och kodstandard.

### 2. Tekniker och verktyg

- Java.
- Spring Boot.
- Spring Data JPA och Hibernate.
- PostgreSQL och Flyway.
- IntelliJ IDEA.
- Git och GitHub.
- Maven.

### 3. Problem och lösningar

En utmaning under dagen var att få igång studio-bass-api lokalt i IntelliJ.

Jag stötte på problem med databasanslutningen eftersom projektet använder miljövariabler för att ansluta till PostgreSQL. Jag saknar för närvarande de lokala databasuppgifterna som krävs för anslutningen.

Jag undersökte projektets konfigurationsfiler och olika profiler för att förstå hur applikationen hanterar databasanslutningen.

Vi kom fram till att jag kan fortsätta utveckla Java-klasserna utan att ansluta till databasen eller använda en testdatabas. Jag kan därmed fortsätta med implementationen och kontrollera kodens struktur innan databasanslutningen är tillgänglig.

Jag undersökte även befintliga enum-klasser i projektet och kunde konstatera att EQ-inställningarna passar bättre som heltalsvärden än som enum, eftersom reglagen ska kunna ha värden mellan 0 och 100.

### 4. Ny kunskap

Jag har fått en bättre förståelse för hur man bygger upp en backendfunktion med Java och Spring Boot.

Jag har bland annat arbetat med:

- Hur `@Entity` används för att representera en databasentitet.
- Hur `@ManyToOne` används för att skapa en relation mellan Mix och User.
- Hur `@Embedded` och `@Embeddable` kan användas för att dela upp information i separata klasser.
- Hur `@ElementCollection` används för att hantera listor med värden som tillhör en entitet.
- Hur `JpaRepository` tillhandahåller färdiga metoder för databasoperationer.
- Hur DTO-klasser används för att överföra information mellan frontend och backend.
- Hur Git-branches kan användas för att separera utvecklingsarbete från dev-branchen.

### 5. Reflektion

Dagen har framför allt handlat om att anpassa implementationen efter den nya tekniska lösningen och påbörja uppbyggnaden av funktionen i Java.

Mötet med Johan gav mig en tydligare bild av hur arbetet ska genomföras och varför användardata ska hanteras i Java i stället för C#.

Jag har kommit igång med de grundläggande klasserna och fått en bättre förståelse för hur entiteter, repositories och DTO-klasser samverkar i en Spring Boot-applikation.

Även om jag inte har kunnat starta applikationen på grund av att jag saknar databasuppgifter har jag kunnat fortsätta med implementationen utan att vara beroende av en databasanslutning.

Nästa steg blir att fortsätta utveckla DTO-klasserna och därefter implementera service- och controller-delarna. Målet är att kunna skapa, hämta, uppdatera och ta bort sparade mixes.

**Arbetstid:** 09:30–15:30

- 09:30–10:40: Möte med Johan Hellberg.
- 10:40–12:30: Eget arbete med LIA-uppgiften.
- 12:30–13:30: Lunch.
- 13:30–15:30: Fortsatt arbete med LIA-uppgiften.

**Effektiv arbetstid:** 5 timmar.

### Sammanfattning av dagen

Dagen började med ett möte med Johan Hellberg där vi beslutade att flytta implementationen av MixerLab-funktionen från C# till Java i studio-bass-api. Efter mötet skapade jag en ny Git-branch och började bygga upp den nya strukturen. Jag skapade entitetsklasserna Mix och MixEq samt MixRepository och påbörjade arbetet med MixRequestDTO. Jag undersökte även projektets befintliga kodstruktur och databasinställningar. Eftersom jag saknar databasuppgifter fortsätter jag tills vidare med implementationen utan databasanslutning. Nästa steg blir att fortsätta med DTO-, service- och controller-delarna.
