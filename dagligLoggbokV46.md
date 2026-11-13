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

**Datum:** 12 november 2026  
**Veckodag:** Torsdag  
**Arbetsplats:** Chain Of Sounds

### 1. Arbetsuppgifter

Idag fortsatte jag arbetet med min LIA-uppgift, som handlar om att ändra MixerLab så att användarnas mixes sparas i en databas i stället för i webbläsarens localStorage.

Dagen började med ett videomöte med Johan Hellberg mellan 09:30 och 10:50. Under mötet följde vi upp arbetet med implementationen i Java och diskuterade hur jag skulle komma vidare med databasanslutningen och utvecklingsmiljön.

Johan instruerade mig att installera Infisical CLI, som används för att hantera projektets miljövariabler och hemligheter. Detta behövdes för att jag skulle kunna komma åt rätt konfiguration och ansluta till databasen.

Efter mötet fortsatte jag arbetet med `studio-bass-api`.

Jag arbetade bland annat med:

- Uppdatera `Mix`-entiteten så att den innehåller rätt tabellnamn och koppling till EQ-inställningarna.
- Använda `@Embedded` för att koppla samman `Mix` med `MixEq`.
- Justera listan med sparade track-ID:n.
- Lägga till `@Builder.Default` för att hantera standardvärdet för listan med track-ID:n.
- Konfigurera Lombok i Maven för att säkerställa att getters och setters genereras korrekt.
- Installera och konfigurera Infisical CLI.
- Starta PostgreSQL och Java-tjänsten med Docker Compose.
- Arbeta med Flyway-migrationen för att skapa databastabellerna för sparade mixes.

Jag genomförde även en ombyggnad av Java-tjänsten och kontrollerade att projektet kunde kompileras och att testerna fungerade.

### 2. Tekniker och verktyg

- Java.
- Spring Boot.
- Spring Data JPA och Hibernate.
- PostgreSQL.
- Flyway.
- Maven.
- Lombok.
- Docker och Docker Compose.
- Infisical CLI.
- IntelliJ IDEA.
- PowerShell.
- Git och GitHub.

### 3. Problem och lösningar

Ett problem under dagen var att få Java-projektet att kompileras korrekt med Lombok.

Vid kompileringen saknades flera getters och setters i projektets klasser. Jag undersökte Maven-konfigurationen och upptäckte att Lombok behövde konfigureras tydligare.

**Lösning:** Jag uppdaterade Lombok-versionen och konfigurationen för Maven Compiler Plugin. Efter ändringarna kunde projektet kompileras utan problem.

Ett annat problem var att ansluta Java-tjänsten till PostgreSQL när jag försökte starta applikationen lokalt. Databasens värdnamn kunde inte hittas.

**Lösning:** Jag använde i stället Docker Compose för att starta PostgreSQL och Java-tjänsten tillsammans. På så sätt kunde tjänsterna kommunicera inom samma Docker-nätverk.

Jag stötte även på problem med Flyway-migrationen. När applikationen startade försökte Flyway skapa tabellen `mixes`, men PostgreSQL rapporterade att tabellen redan existerade.

Detta beror på att Hibernate tidigare hade skapat tabellen automatiskt när `ddl-auto=update` var aktiverat. Jag har identifierat orsaken, men behöver fortfarande undersöka de befintliga tabellerna och anpassa migrationen.

### 4. Ny kunskap

Jag har fått bättre förståelse för hur olika delar av en Java-backend konfigureras och samverkar.

Jag har bland annat lärt mig mer om:

- Hur Lombok konfigureras tillsammans med Maven Compiler Plugin.
- Hur Infisical CLI används för att hantera miljövariabler och hemligheter.
- Hur Docker Compose kan användas för att starta flera tjänster som behöver kommunicera med varandra.
- Skillnaden mellan Hibernate-inställningarna `ddl-auto=update` och `ddl-auto=validate`.
- Hur Flyway hanterar databasmigrationer och varför det är viktigt att hålla databasschemat och migrationerna synkroniserade.
- Hur Hibernate skapar databastabeller utifrån JPA-entiteter.

Jag fick även praktisk erfarenhet av att felsöka problem som uppstår när flera verktyg och tekniker samverkar i samma projekt.

### 5. Reflektion

Dagen har framför allt handlat om att få utvecklingsmiljön att fungera och komma vidare med databasdelen av MixerLab.

Det tog en del tid att felsöka Lombok, miljövariabler och Docker-konfigurationen, men jag lyckades få Java-projektet att kompileras och starta tillsammans med PostgreSQL.

En viktig framgång under dagen var att samtliga 229 tester passerade vid bygget av projektet.

Jag har däremot inte löst problemet med Flyway-migrationen ännu. Jag har identifierat varför det uppstår och behöver nu undersöka hur de befintliga tabellerna ser ut innan jag gör några ändringar.

Jag känner att jag har fått en bättre förståelse för hur databaskonfiguration, Hibernate och Flyway fungerar tillsammans.

Nästa steg blir att undersöka de befintliga databastabellerna och få Flyway-migrationen att fungera korrekt. Därefter kan jag fortsätta med service- och controller-delarna för att implementera funktionaliteten för att skapa, hämta, uppdatera och ta bort sparade mixes.

**Arbetstid:** 09:30–17:00

- 09:30–10:50: Videomöte med Johan Hellberg.
- 10:50–13:30: Eget arbete med LIA-uppgiften.
- 13:30–14:00: Lunch.
- 14:00–17:00: Fortsatt arbete med Java-backenden, Infisical, Docker och Flyway.

**Effektiv arbetstid:** 7 timmar.

### Sammanfattning av dagen

Dagen började med ett videomöte med Johan Hellberg där vi följde upp arbetet med MixerLab och diskuterade hur jag skulle komma vidare med databasanslutningen. Efter mötet installerade och konfigurerade jag Infisical CLI för att komma åt projektets miljövariabler.

Jag fortsatte utvecklingen av Java-backenden genom att justera entiteterna och Maven-konfigurationen. Jag lyckades lösa kompileringsproblemen med Lombok och fick projektet att byggas framgångsrikt. Samtliga 229 tester passerade.

Jag startade även PostgreSQL och Java-tjänsten med Docker Compose. Ett problem återstår med Flyway-migrationen eftersom tabellen `mixes` redan finns i databasen. Jag har identifierat orsaken och kommer att fortsätta felsökningen nästa arbetsdag.

**Datum:** 13 november 2026  
**Veckodag:** Fredag  
**Arbetsplats:** Chain Of Sounds

### 1. Arbetsuppgifter

Idag fortsatte jag arbetet med min LIA-uppgift, som handlar om att ändra MixerLab så att användarnas mixes sparas i en databas i stället för i webbläsarens localStorage.

Dagen började med att jag fortsatte arbetet med Java-backenden i `studio-bass-api`. Jag färdigställde de återstående delarna för att hantera sparade mixes.

Jag arbetade bland annat med:

- Färdigställa `MixService` med metoder för att skapa, hämta, uppdatera och ta bort användarnas mixes.
- Färdigställa `MixController` med API-endpoints för mixhanteringen.
- Använda den befintliga `UserService` för att koppla sparade mixes till rätt användare.
- Använda `MixRepository` för att hantera databasoperationerna.
- Kontrollera att DTO-klasserna används korrekt för att ta emot och returnera information om mixes.

När Java-klasserna var färdiga fortsatte jag arbetet med databasmigreringen.

Jag behövde först starta Docker Desktop och autentisera mig på nytt med Infisical CLI för att kunna använda projektets miljövariabler och starta tjänsterna.

Därefter undersökte jag problemet med Flyway-migrationen V26. Jag upptäckte att det redan fanns tabeller i databasen som hade skapats tidigare av Hibernate när `ddl-auto=update` var aktiverat.

Jag undersökte de befintliga tabellerna och deras relationer. Bland annat upptäckte jag att det fanns flera tabeller relaterade till mixhanteringen, såsom `mixes`, `mix`, `mix_tracks` och `track_id`.

Jag arbetade sedan med att anpassa Flyway-migrationen så att den kunde hantera de befintliga tabellerna.

Efter ändringarna kunde migrationen genomföras. Jag byggde om Java-tjänsten med Docker Compose och kontrollerade loggarna för att säkerställa att Flyway, Hibernate och Spring Boot startade korrekt.

Jag öppnade även Swagger och kontrollerade att de fyra nya API-endpointsen för Mix fanns tillgängliga.

Under eftermiddagen började jag arbeta med enhetstester för `MixService` med JUnit 5 och Mockito.

Jag skapade testklassen `MixServiceTest` och implementerade två tester:

- `createMix_shouldSaveAndReturnMix()` – kontrollerar att en mix kan skapas och att rätt information returneras.
- `getUserMixes_shouldReturnUserMixes()` – kontrollerar att en användares sparade mixes hämtas och returneras korrekt.

Jag använde Mockito för att simulera `MixRepository` och `UserService`, vilket gör att testerna kan köras utan att behöva skriva testdata till den gemensamma databasen.

När jag var klar med dagens arbete skapade jag en Git-commit på min testbranch för att spara testklassen.

### 2. Tekniker och verktyg

- Java.
- Spring Boot.
- Spring Data JPA och Hibernate.
- PostgreSQL.
- Flyway.
- Maven.
- Lombok.
- Docker och Docker Compose.
- Infisical CLI.
- Swagger.
- JUnit 5.
- Mockito.
- IntelliJ IDEA.
- PowerShell.
- Git och GitHub.

### 3. Problem och lösningar

Ett problem under förmiddagen var att Flyway-migrationen V26 inte kunde genomföras eftersom tabellen `mixes` redan existerade i databasen.

Jag undersökte databasschemat och upptäckte att det fanns flera äldre och nya tabeller relaterade till mixhanteringen. Det fanns även skillnader mellan den befintliga databasstrukturen och den struktur som Java-entiteterna förväntade sig.

**Lösning:** Jag anpassade Flyway-migrationen genom att bland annat använda `CREATE TABLE IF NOT EXISTS` för att hantera tabeller som redan existerade. Jag arbetade även med kolumnnamnen och relationerna mellan tabellerna. Efter ändringarna kunde Flyway genomföra migrationen och Java-tjänsten startade korrekt.

Ett annat problem var att jag inte ville genomföra tester som skapade eller ändrade poster i den gemensamma databasen utan att först få klartecken från projektansvarig.

**Lösning:** Jag valde att börja med isolerade enhetstester med JUnit 5 och Mockito. På så sätt kan jag testa servicelogiken genom att simulera repository-anropen i stället för att använda den riktiga databasen.

Under arbetet med det första enhetstestet uppstod även ett problem eftersom jag hade använt `@Mock` på `MixService` i stället för `@InjectMocks`.

**Lösning:** Jag ändrade annoteringen till `@InjectMocks`. Skillnaden är att `@Mock` skapar en simulerad klass, medan `@InjectMocks` skapar den klass som ska testas och injicerar dess beroenden. Efter ändringen kunde testet köras mot den riktiga servicelogiken.

### 4. Ny kunskap

Jag har fått bättre förståelse för hur olika delar av en Java-backend samverkar när man implementerar databaspersistens.

Jag har bland annat lärt mig mer om:

- Hur en service och controller samverkar för att hantera API-anrop.
- Hur en användares sparade data kan kopplas till rätt användare genom den befintliga användarservicen.
- Hur Flyway hanterar migrationer när databastabeller redan existerar.
- Hur befintliga databastabeller kan påverka nya migrationer.
- Hur Hibernate och Flyway behöver samverka för att hålla databasschemat korrekt.
- Hur Swagger kan användas för att kontrollera att API-endpoints finns tillgängliga.
- Skillnaden mellan `@Mock` och `@InjectMocks`.
- Hur `when()`, `thenReturn()` och `thenAnswer()` används i Mockito.
- Hur `verify()` används för att kontrollera att en metod har anropats.
- Hur enhetstester kan genomföras utan att ansluta till en riktig databas.

Jag har även fått mer praktisk erfarenhet av att arbeta med Git-branches och separera testarbete från den ursprungliga implementationen.

### 5. Reflektion

Dagen har varit innehållsrik och jag har kommit vidare med flera viktiga delar av min LIA-uppgift.

Under förmiddagen lyckades jag färdigställa Java-klasserna för mixhanteringen och lösa problemet med Flyway-migrationen. Det krävde en del undersökning av den befintliga databasen eftersom tabellerna inte helt överensstämde med den nya implementationen.

En viktig framgång var att Flyway kunde genomföra V26 och att `studio-bass-api` startade korrekt. Jag kunde även kontrollera att de nya API-endpointsen fanns tillgängliga i Swagger.

Under eftermiddagen kom jag igång med enhetstesterna för `MixService`. Jag fick en bättre förståelse för hur Mockito fungerar och hur man kan testa servicelogik utan att behöva använda en riktig databas.

Jag har nu två fungerande testfall för att skapa och hämta mixes. Nästa steg blir att fortsätta med tester för uppdatering och borttagning av mixes samt kontrollera att användare inte kan ändra eller ta bort andra användares mixes.

### Arbetstid:

**Arbetstid:** 09:30–16:30

- 09:30–09:40: Kort videomöte med Clara Westberg.
- 09:40–12:00: Färdigställande av Mix-klasserna samt arbete med Docker, Infisical och Flyway-migrationen.
- 12:00–12:30: Lunch.
- 12:30–13:00: Verifiering av backend och påbörjat arbete med enhetstester för `MixService`.
- 13:00–13:30: Videomöte med Nisse Palm. Jag redogjorde för vad jag hade arbetat med under veckan.
- 13:30–16:30: Fortsatt arbete med backend och utveckling av enhetstester för `MixService`.

**Effektiv arbetstid:** 6 timmar och 30 minuter.

### Sammanfattning av dagen

Dagen började med att jag färdigställde Java-klasserna för att hantera sparade mixes i MixerLab. Därefter fortsatte jag arbetet med databasmigreringen och undersökte varför Flyway inte kunde skapa tabellen `mixes`.

Jag undersökte den befintliga databasschemat och anpassade Flyway-migrationen V26 för att hantera de tabeller som redan existerade. Efter ändringarna kunde migrationen genomföras och Java-tjänsten startade korrekt via Docker Compose.

Jag kontrollerade även de nya API-endpointsen i Swagger.

Under eftermiddagen började jag skriva enhetstester för `MixService` med JUnit 5 och Mockito. Jag skapade två testfall för att kontrollera att mixes kan skapas och hämtas korrekt. Jag valde att använda mockade beroenden för att undvika att skriva testdata till den gemensamma databasen.

Avslutningsvis committade jag testklassen till min testbranch med commit-meddelandet `test: add MixService unit tests`.
