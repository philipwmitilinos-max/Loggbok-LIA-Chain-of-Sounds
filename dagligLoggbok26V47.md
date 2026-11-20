# Daglig loggbok – LIA

**Datum:** 16 november 2026  
**Veckodag:** Måndag  
**Arbetsplats:** Chain of Sounds

### 1. Arbetsuppgifter

Dagen började med ett sprintplaneringsmöte tillsammans med Johan Hellberg och Nisse Palm. Under mötet diskuterade vi vad som ska genomföras under den kommande veckan och vilka arbetsuppgifter som behöver prioriteras.

En del av planeringen handlade om att lägga till och göra om Mission-sidan. Vi diskuterade även att förklara tydligare vad Mission innebär och vilken funktion den har. MixerLab är en del av Mission, som i sin tur tillhör Akademin.

Efter mötet fortsatte jag med mitt pågående arbete med databaspersistens för MixerLab i Java-projektet `studio-bass-api`.

Jag fortsatte framför allt med att utveckla och förbättra enhetstesterna för `MixService` med JUnit 5 och Mockito.

Jag började med att färdigställa de grundläggande testerna för tjänstens CRUD-operationer:

- Skapa en mix.
- Hämta användarens mixes.
- Uppdatera en mix.
- Radera en mix.

Därefter implementerade jag negativa tester för att kontrollera att en användare inte kan uppdatera eller radera en mix som tillhör någon annan användare.

Jag förbättrade även strukturen i `MixServiceTest` genom att använda `@BeforeEach`. På så sätt kunde jag flytta gemensam testdata till en setup-metod och minska upprepningar i testmetoderna.

Jag organiserade också testerna med hjälp av kommentarrubriker för att tydligare skilja mellan lyckade operationer, felhantering och tester för datarelationer.

Efter detta fortsatte jag med felhanteringstester. Jag implementerade och godkände följande tester:

- `createMix_shouldThrowExceptionWhenUserCannotBeFound()`
- `getUserMixes_shouldThrowExceptionWhenUserCannotBeFound()`
- `updateMix_shouldThrowExceptionWhenUserCannotBeFound()`
- `deleteMix_shouldThrowExceptionWhenUserCannotBeFound()`

Dessa tester kontrollerar att rätt undantag kastas när användaren inte kan hittas och att repository inte anropas i onödan.

Jag gick därefter vidare till testerna för datarelationer. Jag implementerade testet `createMix_shouldAssociateMixWithCorrectUser()` med hjälp av `ArgumentCaptor`.

Testet kontrollerar att den skapade mixen kopplas till rätt användare och att information som namn, backtrack-ID, EQ-inställningar och låt-ID:n följer med när mixen skickas till repository.

Avslutningsvis genomförde jag en Git-commit med dagens ändringar. Committen lyckades och innehöll de nya testerna för felhantering och datarelationer i `MixServiceTest`.

### 2. Tekniker och verktyg

- Java
- Spring Boot
- Spring Data JPA
- PostgreSQL
- JUnit 5
- Mockito
- IntelliJ IDEA
- Git och GitHub
- `@BeforeEach`
- `assertThrows()`
- `verify()`
- `when().thenReturn()`
- `ArgumentCaptor`
- `MixService`
- `MixRepository`
- `MixRequestDto`
- `MixResponseDto`
- `Mix`
- `MixEq`

### 3. Problem och lösningar

Under arbetet med `getUserMixes_shouldReturnUserMixes()` uppstod ett kompileringsfel eftersom testet förväntade sig en lista med `Mix` medan metoden i `MixService` returnerar `List<MixResponseDto>`.

Jag anpassade testet till rätt returtyp och ändrade hur jag hämtade namnet från resultatet eftersom `MixResponseDto` är en Java-record.

Jag upptäckte också att flera testmetoder innehöll upprepade deklarationer av användare och testdata. För att minska upprepningarna använde jag `@BeforeEach` och skapade en gemensam setup-metod.

Efter ändringarna körde jag samtliga sex befintliga tester och alla blev godkända.

En annan del av arbetet var att förbättra översikten i testklassen. Jag valde att använda kommentarrubriker i stället för mer avancerad gruppering med `@Nested`.

Under arbetet med felhanteringstesterna använde jag Mockito för att simulera situationer där användaren inte kan hittas. Jag kontrollerade även att repository inte anropades när användaren saknades.

När jag började testa datarelationerna använde jag `ArgumentCaptor` för att fånga objektet som skickades till repository. På så sätt kunde jag kontrollera att rätt information fanns i den skapade mixen.

Vid förberedelserna inför Git-commit fick jag en varning om att radslut konverteras från CRLF till LF. Det är en varning om radslut och inte ett fel i Java-koden.

### 4. Ny kunskap

Under dagens arbete har jag fått mer förståelse för hur man strukturerar och testar en serviceklass.

Jag har framför allt arbetat vidare med:

- Hur `@BeforeEach` används för att skapa gemensam testdata.
- Hur Mockito används för att simulera olika situationer och fel.
- Hur `assertThrows()` används för att kontrollera att undantag kastas.
- Hur `verify()` och `never()` används för att kontrollera att metoder anropas eller inte anropas.
- Hur `ArgumentCaptor` kan användas för att fånga och undersöka objekt som skickas till ett repository.
- Skillnaden mellan lyckade operationer och negativa tester.
- Skillnaden mellan entiteter och DTO:er.
- Hur `@ManyToOne`, `@Embedded` och `@ElementCollection` används för att beskriva relationer och lagring av data med JPA.

Jag har även fått en bättre förståelse för skillnaden mellan enhetstester med Mockito och integrationstester som faktiskt testar kommunikationen med databasen.

### 5. Reflektion

Dagens arbete har gått bra och jag har kommit vidare med testningen av `MixService`.

Jag har lagt tid på att inte bara få testerna att fungera utan även göra testklassen mer lättläst och enklare att underhålla.

En viktig del har varit att börja tänka mer på felhantering och vad som ska hända när användaren eller en mix inte kan hittas.

Jag har också börjat kontrollera att rätt information och relationer följer med när en mix skapas. Genom att använda `ArgumentCaptor` kunde jag undersöka objektet som skickades till repository.

Jag är ännu inte färdig med `MixServiceTest`, utan kommer att fortsätta med ytterligare tester för datarelationerna. Därefter behöver jag även undersöka hur JPA-relationerna fungerar mot databasen genom integrationstester.

### Arbetstid:\*\* 09:30-17:00

09:30–10:48: Sprintplaneringsmöte via videomöte med Johan Hellberg och Nisse Palm.
10:48–12:00: Fortsatt arbete med enhetstester för `MixService`. Färdigställde grundläggande tester för CRUD-operationerna och arbetade med negativa tester.
12:00–13:00: Lunch.
13:00–15:38: Fortsatt arbete med `MixServiceTest`. Förbättrade testklassens struktur med `@BeforeEach` och kommentarrubriker. Implementerade och godkände ytterligare två felhanteringstester för situationer där användaren inte hittas vid skapande eller hämtning av mixes.
15:38–17:00: Fortsatte med felhanteringstester för uppdatering och radering när användaren inte kan hittas. Båda testerna godkändes. Påbörjade tester för datarelationer och implementerade ett test med ArgumentCaptor för att kontrollera att en mix kopplas till rätt användare och att dess information följer med. Avslutade arbetet med en lyckad Git-commit.

**Effektiv arbetstid hittills:** 6 timmar och 30 minuter.

### Sammanfattning av dagen

Dagen har bestått av ett sprintplaneringsmöte och fortsatt utvecklingsarbete med MixerLab-projektets databaspersistens i Java.

Jag har arbetat vidare med enhetstesterna för `MixService`, förbättrat strukturen i testklassen och implementerat ytterligare tester för felhantering.

Samtliga tidigare tester är fortsatt godkända efter refaktoreringen. Jag har dessutom fått fyra felhanteringstester godkända och påbörjat testerna för datarelationer.

Nästa steg blir att fortsätta testa relationerna mellan olika objekt i systemet och därefter gå vidare med integrationstester mot databasen.

--

**Datum:** 17 november 2026

**Veckodag:** Tisdag

**Arbetsplats:** Chain of Sounds

### 1. Arbetsuppgifter

Dagen började med ett videomöte om den dagliga sprinten tillsammans med Johan Hellberg och Clara Westberg.

Under mötet tog jag upp frågan om databasen för MixerLab och om det var okej att skapa en egen databas för utvecklingsarbetet. Johan godkände att jag skapar databasen lokalt på min egen dator.

Johan påminde mig även om Trello-checklistan som beskriver uppgiftens user story och acceptanskriterier. Uppgiften handlar om att spara användarnas mixes i en databas i stället för i webbläsaren.

Efter mötet fortsatte jag arbetet med databaspersistens för MixerLab i Java-projektet `studio-bass-api`.

Jag fortsatte med tester för datarelationer i `MixServiceTest`. Jag implementerade två ytterligare tester och kontrollerade att flera mixes kan tillhöra samma användare samt att användarrelationen bevaras när en mix uppdateras.

När samtliga tester var färdiga körde jag hela testklassen. Alla 15 tester gick igenom utan fel och jag gjorde därefter en commit.

Efter detta började jag förbereda integrationstester. Jag gick igenom projektets befintliga integrationstestklasser för att förstå hur Testcontainers och PostgreSQL används i testmiljön.

Jag granskade även `MixService`, `Mix`, `MixEq` och `UserService` för att förstå hur delarna fungerar tillsammans.

Därefter skapade jag `MixIT.java` och implementerade mitt första integrationstest, `createMix_shouldPersistMixInDatabase()`. Testet kontrollerar att en mix kan sparas och hämtas från PostgreSQL.

Testet gick igenom utan fel. Jag började sedan förbereda nästa test som ska kontrollera användarrelationen.

### 2. Tekniker och verktyg

- Java
- Spring Boot
- Spring Data JPA
- Hibernate
- PostgreSQL
- Testcontainers
- Docker
- JUnit 5
- Mockito
- AssertJ
- IntelliJ IDEA
- Maven
- Git
- Trello
- `ArgumentCaptor`
- JPA-annoteringar: `@Embedded`, `@Embeddable`, `@ElementCollection`, `@ManyToOne`
- `MixServiceTest`
- `MixIT`

### 3. Problem och lösningar

Under sprintmötet behövde jag stämma av med Johan om databasen skulle skapas lokalt eller på Chain of Sounds servrar. Jag fick klartecken att skapa den lokalt på min egen dator.

När jag började med integrationstesterna behövde jag förstå hur projektets befintliga testmiljö var uppbyggd. Jag upptäckte att projektet redan använder Testcontainers för att starta en PostgreSQL-databas vid testning. Därför kunde jag återanvända den befintliga lösningen.

Jag behövde även förstå skillnaden mellan Mockito-testerna och integrationstesterna. Mockito används för att testa service-logiken med mockade beroenden, medan integrationstesterna kontrollerar att flera delar fungerar tillsammans med en riktig databas.

### 4. Ny kunskap

Under dagen har jag fått mer förståelse för:

- Hur `ArgumentCaptor` används för att kontrollera objekt som skickas till ett repository.
- Hur JPA hanterar relationer mellan entiteter.
- Hur `@Embeddable` och `@ElementCollection` används för att lagra data.
- Hur Testcontainers används tillsammans med Spring Boot och PostgreSQL.
- Skillnaden mellan enhetstester och integrationstester.

Jag har även fått praktisk erfarenhet av att skapa och köra ett integrationstest mot PostgreSQL.

### 5. Reflektion

Dagens arbete har gått bra och jag har kommit vidare med testningen av MixerLab.

Jag har slutfört samtliga 15 Mockito-tester för `MixServiceTest` och fått dem godkända. Därefter kunde jag gå vidare till integrationstesterna.

Det känns bra att mitt första integrationstest också gick igenom. Nästa steg blir att fortsätta testa användarrelationer, EQ-inställningar och track-ID:n samt hämtning, uppdatering och borttagning av mixes.

### Arbetstid: 09:30-16:00

09:30–10:27: Videomöte om daglig sprint tillsammans med Johan Hellberg och Clara Westberg. Diskuterade databasen för MixerLab och gick igenom Trello-checklistan.

10:27–12:00: Fortsatt arbete med `MixServiceTest`. Implementerade och godkände tre tester för datarelationer.

12:00–13:00: Lunch.

13:00-16:00: Slutförde ytterligare två tester för datarelationer. Körde samtliga 15 tester med godkänt resultat och gjorde en commit. Därefter gick jag igenom projektets integrationsteststruktur, skapade `MixIT.java` och körde mitt första integrationstest.

**Effektiv arbetstid:** 5 timmar och 30 minuter.

### Sammanfattning av dagen

Dagen började med ett sprintmöte där vi diskuterade det fortsatta arbetet med MixerLab. Jag fick klartecken att skapa en lokal databas och blev påmind om Trello-checklistan för uppgiften.

Jag slutförde arbetet med de 15 Mockito-testerna i `MixServiceTest` och gjorde en commit.

Därefter började jag förbereda integrationstester genom att undersöka projektets befintliga testmiljö och hur Testcontainers används med PostgreSQL.

Jag skapade `MixIT.java` och implementerade mitt första integrationstest, som gick igenom utan fel.

Jag har därmed tagit nästa steg i arbetet med att kontrollera att MixerLabs mixes faktiskt sparas i databasen.

--

**Datum:** 18 november 2026

**Veckodag:** Onsdag

**Arbetsplats:** Chain of Sounds

### 1. Arbetsuppgifter

Dagen började med ett videomöte om den dagliga sprinten tillsammans med Johan Hellberg och Clara Westberg.

Under mötet gick vi igenom vad vi hade arbetat med föregående dag. Jag fick i uppgift att fortsätta arbetet med integrationstesterna för MixerLab i Java-projektet `studio-bass-api`.

Efter mötet fortsatte jag arbetet i `MixIT.java`.

Jag implementerade och testade funktionalitet för att kontrollera att mixes sparas med rätt användarrelation, att EQ-inställningar och track-ID:n sparas korrekt samt att en användare endast får tillbaka sina egna mixes.

Under testningen av track-ID:n uppstod ett problem med Hibernate och `LazyInitializationException`. Jag löste problemet genom att använda `@Transactional` på testmetoden. Jag behövde även korrigera ett anrop till `getFirst()` som inte fungerade i testmiljön.

Fem integrationstester är nu godkända. Därefter fortsatte jag med testningen av uppdateringsfunktionen, men stötte på ett problem där Hibernate försökte hantera en oföränderlig lista med track-ID:n. Problemet är ännu inte löst.

Senare undersökte jag varför GitHub Actions rapporterade fel i kontrollen Java Backend (Maven). Genom att läsa testloggen kunde jag identifiera att två tester i `FlywaySchemaReconciliationIT` misslyckades.

Jag undersökte därefter `V26__create_mixes_tables.sql` och upptäckte att migrationen försöker ta bort en constraint som inte finns i databasschemat. Jag granskade även `FlywaySchemaReconciliationIT.java` för att förstå hur testet skapar databasen och kör Flyway.

Arbetet med att felsöka migrationen är inte färdigt. Jag valde att avsluta arbetsdagen efter att ha ringat in problemet och identifierat vad som behöver undersökas vidare.

### 2. Tekniker och verktyg

- Java
- Spring Boot
- Spring Data JPA
- Hibernate
- PostgreSQL
- Testcontainers
- Docker
- JUnit 5
- AssertJ
- Mockito
- IntelliJ IDEA
- Maven
- GitHub Actions
- Flyway
- Git
- `MixIT`
- `MixService`
- `MixRepository`
- `UserRepo`
- `FlywaySchemaReconciliationIT`
- `@Transactional`
- `application-test.properties`

### 3. Problem och lösningar

Under testningen av track-ID:n uppstod ett `LazyInitializationException`. Problemet berodde på att Hibernate försökte läsa en samling efter att sessionen hade stängts. Jag löste detta genom att lägga till `@Transactional` på testmetoden.

Vid testningen av uppdateringsfunktionen uppstod ett `UnsupportedOperationException` när Hibernate försökte ändra en oföränderlig lista. Detta problem är ännu inte löst.

Jag upptäckte även att GitHub Actions rapporterade två misslyckade tester i `FlywaySchemaReconciliationIT`.

Felet visade att migrationen `V26__create_mixes_tables.sql` försöker ta bort en constraint som inte existerar i testdatabasen.

Jag undersökte migrationen och testklassen och kunde identifiera var problemet uppstår. Nästa steg blir att anpassa migrationen så att den fungerar med både befintliga och nya databastabeller.

### 4. Ny kunskap

Under dagens arbete har jag fått mer förståelse för:

- Hur integrationstester kontrollerar relationer mellan användare och mixes.
- Hur `@Transactional` kan användas för att hantera databastransaktioner i tester.
- Varför Hibernate behöver kunna hantera ändringsbara listor vid uppdatering av JPA-entiteter.
- Hur Flyway hanterar databasmigrationer.
- Hur Testcontainers kan användas för att testa migrationer mot en separat PostgreSQL-databas.
- Hur man läser GitHub Actions-loggar för att hitta orsaken till misslyckade tester.

### 5. Reflektion

Dagens arbete har varit ganska krävande. Jag har kommit vidare med integrationstesterna och lyckats få fem tester godkända, men stötte även på problem med uppdateringsfunktionen och Flyway-migrationen.

Det har varit värdefullt att undersöka GitHub Actions-loggarna och förstå skillnaden mellan de vanliga integrationstesterna och testerna som kontrollerar Flyway-migrationerna.

Jag har ringat in orsaken till felen i GitHub Actions, men behöver fortsätta felsökningen nästa arbetsdag. Jag känner att det är bäst att avsluta för idag och ta mig an problemen med nya ögon nästa gång.

### Arbetstid: 09:30-17:00

09:30–10:25: Videomöte om daglig sprint tillsammans med Johan Hellberg och Clara Westberg.

10:25–12:00: Fortsatte arbetet med integrationstesterna i `MixIT.java`. Testade användarrelationer, EQ-inställningar och track-ID:n.

12:00–13:00: Lunch.

13:00-17:00: Fortsatte med integrationstesterna och felsökte uppdateringsfunktionen. Undersökte därefter GitHub Actions-felen, Flyway-migrationen `V26__create_mixes_tables.sql` och testklassen `FlywaySchemaReconciliationIT.java`.

**Effektiv arbetstid:** 6 timmar och 30 minuter.

### Sammanfattning av dagen

Dagen började med ett daily sprint-möte där vi gick igenom föregående dags arbete och planerade det fortsatta arbetet.

Efter mötet fortsatte jag med integrationstesterna för MixerLab. Fem tester är godkända, men arbetet med uppdateringstestet återstår.

Jag undersökte även varför GitHub Actions rapporterade fel och kunde identifiera ett problem i Flyway-migrationen `V26__create_mixes_tables.sql`. Migrationen försöker ta bort en constraint som inte finns i testdatabasen.

Jag har därmed ringat in problemet och vet vad jag behöver undersöka vidare nästa arbetsdag. Jag avslutade dagen efter felsökningen.

--

**Datum:** 19 november 2026

**Veckodag:** Torsdag

**Arbetsplats:** Chain of Sounds

### Arbetstid

**Föranmäld frånvaro – hela arbetsdagen.**

Jag hade tidigare under veckan informerat Johan och ansvarig på Chain of Sounds om att jag skulle vara frånvarande under torsdagen på grund av min pappas begravning och urnsättning.

--