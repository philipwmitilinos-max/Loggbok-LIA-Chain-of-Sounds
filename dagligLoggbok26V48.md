# Daglig LIA-loggbok – Måndag 23 november 2026

## 1. Arbetsuppgifter

- Deltog i veckans sprintplanering tillsammans med teamet.
- Diskuterade verksamheten och Chain of Sounds affärsmodell. Nisse berättade att företaget är en startup och att intäkterna kommer från prenumerationer.
- Frågade Johan om möjligheten att köra GitHub Actions-workflows manuellt.
- Fick individuell handledning av Johan. Han föreslog att test-branchen skulle mergas in i feature-branchen och att en ny Pull Request skulle skapas.
- Stängde den tidigare Pull Requesten #261.
- Kontrollerade statusen på test-branchen och feature-branchen innan merge.
- Genomförde merge från `test/mixerlab-database-persistence-java-test` till `feature/mixerlab-database-persistence-java`.
- Mergen genomfördes utan konflikter.
- Skapade merge-commit `bc38eeece`.
- Tog bort `workflow_dispatch` från GitHub Actions-konfigurationen och skapade commit `a41ce4876`.
- Genomförde backend-verifiering med Maven Wrapper:
  `.\mvnw.cmd clean verify`.
- Verifierade att följande tester passerade:
  - MixServiceTest: 15/15.
  - MixIT: 7/7.
  - FlywaySchemaReconciliationIT: 2/2.
- Hela Maven-bygget avslutades med `BUILD SUCCESS`.
- Kontrollerade att arbetskatalogen var ren innan push.
- Pushade feature-branchen till GitHub. Totalt 24 commits publicerades.
- Skapade en ny Pull Request, #265, från `feature/mixerlab-database-persistence-java` till `dev`.
- Kontrollerade GitHub Actions-resultatet. Samtliga sju automatiserade kontroller passerade:
  - CI / .NET Backend.
  - CI / Java Backend (Maven).
  - CI / React Frontend.
  - CodeQL – C#.
  - CodeQL – Java/Kotlin.
  - CodeQL – JavaScript/TypeScript.
  - Playwright Tests.
- Arbetade vidare med Copilots tidigare kodgranskningsanmärkningar från PR #261.
- Undersökte den första anmärkningen med hög allvarlighetsgrad, som handlade om problem med dubblerade constraints i databasmigrationerna.
- Granskade migrationsfilen `V26__create_mixes_tables.sql` och jämförde den nuvarande versionen med en äldre version från Git-historiken.
- Identifierade att den äldre V26-migrationen försökte skapa foreign keys både när tabellerna skapades och genom efterföljande `ALTER TABLE`-instruktioner.
- Identifierade även att den äldre migrationen försökte byta namn på kolumnen `track_ids` till `track_id`, trots att den nya tabellstrukturen redan definierade `track_id`.
- Kontrollerade commit `3b37c29f0` (`Fix V26 mix table migration`) och konstaterade att den överflödiga hanteringen av foreign keys hade tagits bort.
- Granskade den nuvarande V26-migrationen och verifierade att tabellerna `mixes` och `mix_tracks` skapas med sina respektive foreign keys direkt i `CREATE TABLE`.
- Granskade `FlywaySchemaReconciliationIT` för att undersöka vilka migrationsscenarier som testas.
- Granskade `MixIT` och konstaterade att det innehåller sju integrationstester för MixerLab-funktionaliteten.
- Granskade `AbstractIntegrationTest` och `application-test.properties` för att förstå hur testdatabasen konfigureras.
- Identifierade att Hibernate använder `ddl-auto=create` och att Flyway är avstängt i den vanliga testprofilen.
- Konstaterade därför att `MixIT` testar databaspersistens men inte verifierar att Flyway-migrationen V26 fungerar.
- Identifierade att `FlywaySchemaReconciliationIT` testar äldre migrationer upp till V15, men inte uttryckligen verifierar V26.
- Skapade därför ett separat integrationstest, `MixFlywayMigrationIT`, för att testa Flyway-migrationen mot en separat PostgreSQL-databas via Testcontainers.
- Konfigurerade testet så att Flyway utgår från version 25 och kör migrationen V26.
- Lade till kontroller som verifierar att tabellerna `mixes` och `mix_tracks` skapas och att de två förväntade foreign keys finns.
- Rättade variabelnamn i testet efter ett kompileringsfel som berodde på att variabeln `result` deklarerades flera gånger inom samma metod.
- Körningen av `MixFlywayMigrationIT` lyckades:
  - Tests run: 1.
  - Failures: 0.
  - Errors: 0.
  - Skipped: 0.
  - `BUILD SUCCESS`.
- Fortsatte undersöka Code Quality Bots kommentarer på PR #265.
- Lade till en explicit Lombok-annotering `@ToString` på `Mix` och exkluderade `eq` från `toString()` för att undvika problem med objektens representation.
- Skapade commit `b4a25171a` (`fix: add explicit toString annotation to Mix`) och verifierade ändringen med Maven.
- Efter ytterligare en granskningskommentar exkluderades även `user` från `Mix`-klassens `toString()` med `@ToString.Exclude`.
- Skapade commit `16b1cced0` (`fix: exclude user from Mix toString`).
- Körningen av `.\mvnw.cmd clean verify` efter den senaste ändringen lyckades.
- Den senaste commiten `16b1cced0` är ännu inte pushad till GitHub.

## 2. Tekniker och verktyg

- Git och GitHub.
- Git-brancher, merge och commits.
- Pull Requests och kodgranskning.
- GitHub Actions.
- CI/CD och automatiserade tester.
- Java och Spring Boot.
- Maven Wrapper.
- JUnit 5 och Mockito.
- Testcontainers och PostgreSQL.
- Flyway och SQL-migrationer.
- Hibernate och JPA.
- Lombok och annoteringar, bland annat `@ToString` och `@ToString.Exclude`.
- React och Playwright.
- PowerShell och terminalkommandon.
- Git-historik och `git show` för att jämföra tidigare kodversioner.
- SQL-frågor mot `information_schema` för att kontrollera databastabeller och constraints.

## 3. Problem och lösningar

- Den tidigare Pull Requesten #261 behövde ersättas enligt Johans förslag. Den stängdes och en ny PR skapades efter att test-branchen mergats till feature-branchen.
- Feature-branchen innehöll nio lokala commits som inte hade pushats till GitHub. Dessa behölls och kombinerades med ändringarna från test-branchen.
- Mergen genomfördes automatiskt utan konflikter.
- Maven kunde inte köras direkt via `mvn`, men projektet innehöll Maven Wrapper. Därför användes `mvnw.cmd` för att genomföra verifieringen.
- När ändringen i `.github/workflows/ci.yml` skulle genomföras uppstod ett sökvägsfel eftersom jag befann mig i `studio-bass-api`. Problemet löstes genom att använda den relativa sökvägen `../.github/workflows/ci.yml`.
- GitHub PR #265 visar att minst två godkända granskningar från personer med skrivbehörighet krävs innan merge. Detta är ett krav i repositoryts inställningar och inte ett testfel.
- Vid granskningen av Copilots tidigare anmärkning om V26 hittades överflödig SQL-kod som försökte ta bort och återskapa foreign keys som redan definierades när tabellerna skapades. Denna kod hade tagits bort i commit `3b37c29f0`.
- Den äldre migrationen innehöll även en `RENAME COLUMN`-instruktion som förutsatte att kolumnen `track_ids` redan fanns. Den instruktionen finns inte längre i den nuvarande V26.
- Undersökningen visade att befintliga integrationstester inte uttryckligen verifierade Flyway-migrationen V26. `MixIT` använder Hibernate för att skapa databasschemat och har Flyway avstängt i testprofilen.
- Detta löstes genom att skapa ett separat test, `MixFlywayMigrationIT`, som kör Flyway mot en separat PostgreSQL-databas.
- Det nya migrationstestet kördes framgångsrikt och verifierade att V26 kan skapa tabellerna och deras foreign keys i en ny databas.
- Vid testskrivningen uppstod ett kompileringsfel eftersom flera variabler hade samma namn (`result`). Problemet löstes genom att ge SQL-resultaten egna variabelnamn: `tableResult` och `constraintResult`.
- Code Quality Bot uppmärksammade problem med `toString()` i relation till objekt som `MixEq` och `User`. Detta hanterades genom att lägga till `@ToString` på `Mix` och exkludera `eq` och `user` med `@ToString.Exclude`.
- Den senaste ändringen verifierades med `clean verify`, men commit `16b1cced0` har ännu inte pushats.

## 4. Ny kunskap

- Fördjupad förståelse för hur man jämför brancher och kontrollerar skillnader mellan commits.
- Hur man genomför en merge utan att omedelbart skapa en merge-commit med `--no-commit --no-ff`.
- Hur Maven Wrapper kan användas för att bygga och testa ett projekt utan en separat Maven-installation.
- Hur man använder relativa sökvägar i Git-kommandon när man befinner sig i en undermapp.
- Hur GitHub Actions automatiskt verifierar flera delar av ett projekt genom CI.
- Förståelse för hur granskningskrav och behörigheter påverkar möjligheten att merga en Pull Request.
- Fördjupad förståelse för foreign keys och constraints i PostgreSQL.
- Lärt mig att `CREATE TABLE IF NOT EXISTS` inte uppdaterar strukturen på en tabell som redan finns.
- Förståelse för varför man bör undvika att skapa samma foreign key både vid tabellskapandet och genom efterföljande `ALTER TABLE`.
- Lärt mig att Flyway och Hibernate kan hantera databasschemat på olika sätt.
- Förståelse för att integrationstester som använder `ddl-auto=create` inte automatiskt verifierar att Flyways SQL-migrationer fungerar.
- Lärt mig att skapa separata migrationstester med Testcontainers och PostgreSQL.
- Förståelse för hur Flyways baseline-version kan användas för att testa en specifik migration.
- Lärt mig att kontrollera tabeller och constraints genom PostgreSQLs `information_schema`.
- Lärt mig att Git-kommandot `git show` kan användas för att undersöka tidigare versioner av en fil och förstå varför en ändring gjordes.
- Fördjupad förståelse för Lomboks `@ToString` och hur `@ToString.Exclude` kan användas för att undvika att relationer inkluderas i objektens strängrepresentation.

## 5. Reflektion

Dagen har varit produktiv och ett viktigt steg framåt för MixerLab-arbetet. Jag har sammanfört ändringarna från test-branchen till feature-branchen och verifierat backend-implementationen lokalt.

Det var särskilt positivt att samtliga relevanta backend-tester passerade och att alla sju kontroller i GitHub Actions också blev godkända.

Den nya Pull Requesten #265 är nu öppen mot `dev`. Den inväntar fortfarande godkända kodgranskningar innan den kan mergas.

Under eftermiddagen fortsatte jag att undersöka Copilots tidigare kodgranskningsanmärkningar. Genom att jämföra äldre och nuvarande versioner av V26 kunde jag förstå varför den tidigare SQL-koden kunde orsaka migrationsproblem.

En viktig upptäckt var att de befintliga integrationstesterna inte uttryckligen verifierade Flyway-migrationen V26. Jag skapade därför ett separat migrationstest som använder Testcontainers och PostgreSQL. Testet genomfördes framgångsrikt och bekräftade att migrationen kan skapa de förväntade tabellerna och foreign keys i en ny databas.

Detta innebär att vi nu har verifierat den nya databasskapningen genom Flyway. Däremot återstår att undersöka hur migrationen hanterar ett äldre databasschema där tabellerna redan finns.

Jag arbetade även vidare med Code Quality Bots kommentarer och gjorde ändringar i `Mix`-entitetens `toString()`-hantering. Den senaste ändringen verifierades med Maven, men behöver fortfarande pushas till GitHub.

Nästa steg blir att fortsätta granska Copilots återstående anmärkningar, undersöka scenariot med äldre databasscheman och därefter fortsätta med frontend-integrationen.

## Arbetstid

- 09:30–11:00 – Sprintplanering.
- 11:00–11:15 – Individuell handledning med Johan.
- 11:15–12:15 – Git-arbete, branchkontroller, merge och CI-konfiguration.
- 12:45–13:45 – Lunch.
- 13:45–17:00 – Backend-verifiering, testkontroller, push till GitHub och skapande av Pull Request #265. Därefter undersökning av Copilots kodgranskningsanmärkningar, SQL-migration V26, befintliga integrationstester, skapande av `MixFlywayMigrationIT` samt åtgärder efter Code Quality Bots kommentarer.

**Sluttid:**

## Sammanfattning av dagen

Under dagen genomfördes en viktig del av arbetet med MixerLab-databaspersistens. Test-branchen mergades till feature-branchen och backend-verifieringen genomfördes med godkänt resultat.

Feature-branchen pushades till GitHub och en ny Pull Request, #265, skapades mot `dev`. Samtliga sju GitHub Actions-kontroller passerade.

Under eftermiddagen undersöktes Copilots tidigare kodgranskningsanmärkningar. Den första anmärkningen gällde dubblerad hantering av foreign keys i V26-migrationen. Den problematiska SQL-koden hade redan tagits bort i en tidigare commit.

Eftersom de befintliga integrationstesterna inte uttryckligen verifierade V26 skapades ett separat migrationstest. Det testet kördes framgångsrikt och bekräftade att migrationen fungerar när tabellerna skapas i en ny databas.

Arbetet med Code Quality Bots kommentarer fortsatte också. Ändringar gjordes i `Mix`-entitetens `toString()`-hantering och den senaste ändringen verifierades med Maven. Den tillhörande commiten har ännu inte pushats.

Nästa arbetsdag fortsätter arbetet med att granska de återstående kodgranskningsanmärkningarna, undersöka migration mot äldre databasscheman och förbereda fortsatt integration med frontend.

--

# Daglig LIA-loggbok – Tisdag 24 november 2026

## 1. Arbetsuppgifter

- Deltog i ett kort Daily Scrum-möte via videolänk tillsammans med Johan Hellberg och Clara Westberg.
- Under mötet diskuterades dagens arbete och planeringen framåt.
- Fortsatte arbetet med MixerLab och databaspersistens i Java-backend.
- Fortsatte granskningen av Copilots tidigare kodgranskningsanmärkningar från PR #261.
- Undersökte databasmigrationen V26 och dess hantering av befintliga databasscheman.
- Granskade historiken för V26 och jämförde den ursprungliga migrationen med den korrigerade versionen.
- Undersökte om det fanns någon äldre databas eller tidigare migration som skapade tabellerna `mixes` och `mix_tracks`.
- Konstaterade att det inte finns någon gammal databas som behöver stödjas i projektets nuvarande situation. Därför behövs inte de tidigare `ALTER TABLE`-instruktionerna för att hantera ett äldre schema.
- Skapade och körde ett separat migrationstest, `MixFlywayMigrationIT`, med Testcontainers och PostgreSQL. Testet verifierade att V26 skapar tabellerna `mixes` och `mix_tracks` samt deras främmande nycklar.
- Skapade `MixControllerTest` för att komplettera testtäckningen av MixController.
- Implementerade fyra tester för controllerns HTTP-endpoints:
  - `createMix()`
  - `getUserMixes()`
  - `updateMix()`
  - `deleteMix()`
- Testade samtliga fyra metoder och verifierade att hela testklassen gick igenom.
- Körde `.\mvnw.cmd clean verify` för att kontrollera hela Java-backendens tester och bygge. Resultatet blev `BUILD SUCCESS`.
- Granskade Git-status och upptäckte att de två nya testfilerna hade olika versioner i staging och arbetskatalogen. Uppdaterade staging så att de senaste versionerna kom med.
- Skapade commit `5bcd18e43` med meddelandet `test: add MixController and Flyway migration tests`.
- Säkerställde att den tidigare commiten `16b1cced0`, som exkluderar `user` från `Mix.toString()`, också publicerades.
- Pushade båda commitarna till feature-branchen `feature/mixerlab-database-persistence-java`.
- Kontrollerade PR #265 efter pushen.
- Påbörjade granskningen av en ny lista med tolv potentiella förbättringspunkter från Copilot. Den första punkten handlar om JWT-subject och säker hantering av användaridentitet i `MixController`.
- Granskade `SecurityConfig.java`, `application.properties` och `application-dev.properties` för att undersöka hur autentisering och säkerhet är konfigurerade.
- Granskade `MixController.java`, `MixService.java`, `UserService.java` och `User.java` för att förstå hur JWT-subject används för att identifiera användaren.
- Undersökte hur `UserService.getOrCreateUserBySubject()` hanterar användarens subject.
- Konstaterade att det inte finns någon separat `UserServiceTest.java`. Hittade däremot `UserServiceSubscriptionDeletedTest.java`, som testar hantering av avslutade Stripe-prenumerationer.
- Beslutade att först stämma av med Johan innan eventuella ändringar eller nya tester för validering av JWT-subject genomförs.
- Granskade de befintliga testerna i `MixControllerTest` och `MixServiceTest` för att se vilken felhantering som redan testas.
- Identifierade att `MixControllerTest` endast testar lyckade HTTP-anrop: 201, 200 och 204.
- Konstaterade att `MixServiceTest` redan innehåller tester för undantag och för situationer där en användare försöker uppdatera eller radera någon annans mix.
- Undersökte skillnaden mellan servicetester och controller-tester. Servicetesterna kontrollerar logiken och undantagen, medan controller-testerna kontrollerar HTTP-svaren.
- Granskade `ApiExceptionHandler.java` för att undersöka hur projektet mappar undantag till HTTP-statuskoder.
- Granskade `ApiExceptionHandlerTest.java`, som innehåller åtta tester för befintlig felhantering.
- Identifierade att `MixNotFoundException` saknar en koppling till `ApiExceptionHandler` och att `MixService` fortfarande använder en generell `RuntimeException` när en mix inte hittas.
- Granskade den befintliga klassen `MixNotFoundException.java`. Ingen integration med felhanteraren eller komplettering av tester genomfördes.
- Avslutade dagens arbete efter att ha gått igenom de viktigaste delarna av felhanteringen och identifierat vad som behöver kompletteras.

### Genomförda och verifierade moment

- V26:s migrationstest: 1 test, 0 fel och 0 misslyckade tester.
- Samtliga fyra tester i `MixControllerTest` genomförda.
- Hela `MixControllerTest` kördes och gick igenom.
- `.\mvnw.cmd clean verify` genomfördes med resultatet `BUILD SUCCESS`.
- Failsafe-resultatet visade 19 tester, 0 failures, 0 errors och 0 skipped.
- Git-commitarna `16b1cced0` och `5bcd18e43` skapades och pushades till GitHub.
- Arbetskatalogen kontrollerades och var ren efter commit.
- Befintlig säkerhetskonfiguration och felhantering granskades.
- Befintliga tester i `MixServiceTest` och `ApiExceptionHandlerTest` undersöktes.

### Påbörjat arbete

- Granskning av Copilots nya kodgranskningspunkter för PR #265.
- Undersökning av hur JWT-subject hanteras och skyddas i samtliga `/mixes`-endpoints.
- Undersökning av hur ogiltiga eller saknade subject-värden kan hanteras.
- Granskning av hur fel från `MixService` ska översättas till korrekta HTTP-statuskoder.
- Planering av kompletterande tester för `MixControllerTest`.

### Återstår att verifiera eller genomföra

- Stämma av med Johan om han vill att valideringen av JWT-subject ska hanteras centralt i `UserService`.
- Fortsätta granskningen av Copilots återstående kodgranskningspunkter.
- Komplettera `ApiExceptionHandler` med hantering för `MixNotFoundException`.
- Lägga till motsvarande test i `ApiExceptionHandlerTest`.
- Undersöka behovet av en generell hantering av oväntade serverfel (500).
- Uppdatera `MixService` så att den använder ett specifikt undantag när en mix inte hittas.
- Komplettera `MixControllerTest` med tester för felaktiga requests och relevanta HTTP-statuskoder.
- Verifiera att felhanteringen fungerar genom hela kedjan från controller till exception handler.
- Köra relevanta tester och Maven-verifiering efter eventuella kodändringar.

## 2. Tekniker och verktyg

- Java och Spring Boot.
- Spring MVC och Spring Security.
- JWT och autentisering.
- Git och GitHub.
- Pull Requests och kodgranskning.
- Maven Wrapper.
- JUnit 5.
- Mockito.
- MockMvc.
- Spring Security Test och JWT-testning.
- Testcontainers och PostgreSQL.
- Flyway och SQL-migrationer.
- Hibernate och JPA.
- IntelliJ IDEA.
- PowerShell och terminalkommandon.
- GitHub Actions.

## 3. Problem och lösningar

### Granskning av V26-migrationen

Copilots tidigare kodgranskning hade uppmärksammat problem med SQL-instruktioner som försökte ändra och ta bort befintliga constraints samt byta namn på en kolumn.

Genom att granska migrationens Git-historik och tidigare versioner kunde jag identifiera vilka instruktioner som hade funnits från början.

Jag undersökte även om någon tidigare migration skapade tabellerna `mixes` och `mix_tracks`. Det visade sig att det inte finns någon sådan äldre migration och att projektet inte har någon gammal databas som behöver stödjas.

**Lösning:**

De överflödiga `ALTER TABLE`-instruktionerna behövs inte för projektets nuvarande förutsättningar. Den korrigerade V26-migrationen behåller därför skapandet av tabellerna och deras constraints utan de tidigare ändringsinstruktionerna.

För att verifiera lösningen skapade jag `MixFlywayMigrationIT`, som testar migrationen mot en separat PostgreSQL-container. Testet genomfördes utan fel.

### Testning av MixController

Det saknades tidigare tester för controllerns HTTP-endpoints. Jag skapade därför `MixControllerTest` med MockMvc och Mockito.

Under arbetet uppstod ett importfel där fel `MediaType` hade importerats från JUnit i stället för Spring.

**Lösning:**

Bytte till `org.springframework.http.MediaType`. Därefter kunde testerna implementeras och köras.

Följande tester genomfördes:

- `createMix()` – kontrollerar att en mix kan skapas och att API:et returnerar HTTP 201 Created.
- `getUserMixes()` – kontrollerar att användarens mixes hämtas och returneras med HTTP 200 OK.
- `updateMix()` – kontrollerar att en mix kan uppdateras och att API:et returnerar HTTP 200 OK.
- `deleteMix()` – kontrollerar att en mix kan tas bort och att API:et returnerar HTTP 204 No Content.

Samtliga fyra tester gick igenom.

Vid en senare granskning identifierades dock att testerna endast täcker lyckade anrop. Felaktiga requests och andra felaktiga HTTP-svar behöver fortfarande testas.

### Granskning av JWT-subject och användaridentitet

Copilots första nya granskningspunkt handlade om hur JWT-subject används för att identifiera användaren.

Jag granskade säkerhetskonfigurationen och följde hur subject skickas från `MixController` vidare till `MixService` och `UserService`.

Jag identifierade att `UserService.getOrCreateUserBySubject()` saknar en uttrycklig kontroll av om subject är null, tomt eller endast innehåller blanksteg.

Det finns ingen separat `UserServiceTest.java` i projektet.

**Bedömning:**

Eftersom `UserService` används av fler delar av applikationen vill jag först stämma av med Johan innan jag gör en central ändring eller lägger till nya tester.

Därför genomfördes inga kodändringar i denna del.

### Granskning av felhantering och HTTP-statuskoder

Jag undersökte `MixControllerTest`, `MixServiceTest`, `ApiExceptionHandler` och `ApiExceptionHandlerTest`.

Granskningen visade att `MixServiceTest` redan testar flera situationer där användaren eller mixen inte kan hittas och där en användare försöker ändra eller radera någon annans mix.

Däremot testar `MixControllerTest` endast lyckade HTTP-anrop.

I `ApiExceptionHandler` finns redan hantering för bland annat 400, 403, 404, 409 och 503. Däremot saknas en särskild hantering för `MixNotFoundException` och en uttrycklig generell hantering för oväntade serverfel.

**Identifierad förbättring:**

En särskild exception-klass, `MixNotFoundException`, finns redan i projektet. Nästa steg är att koppla den till `ApiExceptionHandler` så att en mix som inte hittas kan returnera HTTP 404 Not Found.

Därefter behöver motsvarande test läggas till i `ApiExceptionHandlerTest`.

Inga ändringar i felhanteringen genomfördes under dagens granskning.

### Git och versionshantering

Vid kontroll av Git-status upptäckte jag att testfilerna hade olika innehåll i staging och arbetskatalogen. De versioner som låg staged innehöll endast tomma testklasser.

**Lösning:**

Uppdaterade staging med de senaste versionerna av båda filerna och kontrollerade ändringarnas omfattning innan commit.

Därefter skapades commiten `5bcd18e43` och ändringarna pushades tillsammans med den tidigare commiten `16b1cced0` till GitHub.

## 4. Ny kunskap

- Fördjupad förståelse för hur Flyway hanterar databasmigrationer och varför migrationer behöver anpassas efter projektets faktiska databashistorik.
- Lärt mig att använda Git-historik för att jämföra tidigare och nuvarande versioner av SQL-migrationer.
- Fördjupad förståelse för hur Testcontainers kan användas för att testa databasmigrationer mot en riktig PostgreSQL-instans i en isolerad testmiljö.
- Arbetat vidare med MockMvc för att testa HTTP-anrop till Spring Boot-controllers.
- Fördjupat kunskapen om hur Mockito används för att mocka serviceklasser och verifiera att rätt metod anropas med rätt argument.
- Fått mer erfarenhet av att testa JWT-autentisering i controller-tester med Spring Security Test.
- Fått bättre förståelse för skillnaden mellan controller-tester och servicetester.
- Fördjupat kunskapen om hur exception-klasser kan användas för att skilja mellan olika typer av fel.
- Fått bättre förståelse för hur `ApiExceptionHandler` översätter Java-undantag till HTTP-statuskoder.
- Fått bättre förståelse för Git staging och varför det är viktigt att kontrollera skillnaden mellan staged och ostagade ändringar före en commit.
- Fördjupat kunskapen om hur Maven `clean verify` används för att kontrollera både tester och bygge innan ändringar publiceras.

## 5. Reflektion

Dagen har framför allt handlat om kvalitetssäkring av MixerLabs Java-backend.

En viktig del av arbetet var att undersöka Copilots tidigare anmärkning kring V26-migrationen. Genom att granska Git-historiken och kontrollera projektets faktiska förutsättningar kunde jag avgöra att det inte finns någon äldre databas som behöver stödjas. Jag kunde därefter verifiera den korrigerade migrationen med ett separat integrationstest mot PostgreSQL.

Jag har även kompletterat testtäckningen för MixController genom att skapa fyra tester med MockMvc och Mockito. Samtliga tester gick igenom, och därefter kördes hela Java-backendens verifiering med Maven. Bygget avslutades med `BUILD SUCCESS`.

Efter verifieringen genomfördes commits och push till GitHub, vilket uppdaterade PR #265.

Under eftermiddagen fortsatte jag med Copilots nya granskningspunkter. Jag undersökte JWT-subject, användaridentifiering och befintlig felhantering. Jag upptäckte även att `MixControllerTest` behöver kompletteras med tester för misslyckade HTTP-anrop, eftersom de nuvarande testerna endast verifierar lyckade operationer.

Jag identifierade flera möjliga förbättringar men valde att inte genomföra kodändringar innan jag hade undersökt projektets befintliga struktur och tester. Jag vill även stämma av med Johan innan jag gör en central förändring i `UserService`.

Arbetet har gett mig bättre förståelse för hur kodgranskning, säkerhet, automatiserade tester, databasmigrationer och versionshantering tillsammans bidrar till en mer tillförlitlig backend.

Mot slutet av arbetsdagen märkte jag att jag började bli mentalt trött och hade svårt att tänka klart. Jag var nära att glömma att jag arbetade i en separat Git-branch. Det blev en påminnelse om hur viktigt det är att hålla koll på vilken branch jag arbetar i innan jag gör ändringar eller genomför Git-kommandon.

Jag tar också med mig att det är viktigt att ta pauser och inte pressa mig för länge när koncentrationen börjar försämras.

Jag avslutade arbetsdagen när jag började tappa orken. Nästa arbetsdag kan jag fortsätta med felhanteringen och de återstående granskningspunkterna.

## Arbetstid

- 09:30–09:45 – Daily Scrum via videomöte med Johan Hellberg och Clara Westberg.
- 09:45–12:00 – Fortsatt arbete med MixerLab, databasmigrationer, kodgranskning och controller-tester.
- 12:00–12:30 – Lunch.
- 12:30–15:30 – Fortsatt testutveckling, Maven-verifiering, Git-commits, push till GitHub och granskning av nya kodgranskningspunkter.

**Sluttid:** 15:30.

## Sammanfattning av dagen

Dagen inleddes med ett Daily Scrum-möte tillsammans med Johan Hellberg och Clara Westberg. Därefter fortsatte arbetet med MixerLab och kvalitetssäkring av Java-backendens databaspersistens.

Jag undersökte Copilots tidigare anmärkning kring V26-migrationen och kunde konstatera att projektet inte har någon äldre databas som behöver stödjas. Jag skapade även ett separat integrationstest med Testcontainers och PostgreSQL för att verifiera att migrationen skapar rätt tabeller och constraints. Testet gick igenom utan fel.

Utöver detta färdigställde jag `MixControllerTest` med fyra tester för att kontrollera skapande, hämtning, uppdatering och borttagning av mixes. Samtliga tester gick igenom. Hela Java-backendens Maven-verifiering avslutades med `BUILD SUCCESS`.

Jag skapade därefter commit `5bcd18e43` för de nya testerna och pushade den tillsammans med den tidigare commiten `16b1cced0` till feature-branchen för PR #265.

Under eftermiddagen granskade jag Copilots nya förbättringspunkter med fokus på JWT-subject och felhantering. Jag undersökte befintliga tester och projektets `ApiExceptionHandler` och identifierade att `MixControllerTest` behöver kompletteras med tester för misslyckade HTTP-anrop. Jag identifierade även behovet av att koppla `MixNotFoundException` till felhanteraren.

Jag valde att avvakta med ändringar kring JWT-subject tills jag har stämt av frågan med Johan.

Dagen avslutades efter en genomgång av de återstående förbättringarna. Nästa steg blir att fortsätta arbetet med felhantering och kompletterande tester för MixerLab.

--

# LIA-loggbok – Onsdag 25 november 2026

## 1. Arbetsuppgifter

- Dagen inleddes med ett daily sprint/scrum-möte via video.
- En genomgång av MixerLab var planerad, men kunde inte genomföras som tänkt eftersom Nisse inte deltog.
- Förberedde mig inför mötets fortsättning.
- Frågade Johan Hellberg om jag skulle skapa tester för `UserService`. Johan bekräftade att jag skulle göra det.
- Nisse deltog fortfarande inte när mötet fortsatte.
- Efter mötet påbörjade jag arbetet med att skapa en generell testklass för `UserService`.
- Granskade den befintliga klassen `UserService.java` för att identifiera vilka metoder och funktioner som behöver testas.
- Granskade `UserServiceSubscriptionDeletedTest.java` för att följa projektets befintliga struktur för enhetstester med JUnit 5, Mockito och AssertJ.
- Skapade `UserServiceTest.java` med mockade beroenden för `UserRepo` och `StripeSubscriptionService`.
- Implementerade och verifierade 13 tester för följande funktioner:
  - Hämta en befintlig användare.
  - Skapa en ny användare om den inte finns.
  - Tilldela standardrollen `USER` när användaren saknar roller.
  - Tilldela standardrollen `USER` när användarens roller är `null`.
  - Aktivera Premium-medlemskap.
  - Aktivera Supporter-medlemskap.
  - Aktivera Founders Circle-medlemskap.
  - Aktivera en kostnadsfri provperiod (Free Trial).
  - Förhindra att en användare aktiverar Free Trial flera gånger.
  - Förhindra att en användare med ett aktivt betalt medlemskap aktiverar Free Trial.
  - Uppdatera en användares roller.
  - Tilldela standardrollen `USER` när en tom rollista skickas in.
  - Tilldela standardrollen `USER` när rollistan är `null`.
- Undersökte varför ett test för redan använd Free Trial misslyckades. Upptäckte att testanvändaren saknade roller, vilket gjorde att `getOrCreateUserBySubject()` sparade användaren innan undantaget kastades. Kompletterade testdata med standardrollen `USER`.
- Undersökte varför testet för att förhindra Free Trial för betalande användare misslyckades. Granskade `hasPaidMembership()` och `hasActiveMembershipIn()` i `User.java`.
- Upptäckte att ett aktivt betalt medlemskap kräver att medlemskapet tillhör `PAID_TIERS`, att `paidAt` inte är `null` och att medlemskapet inte har gått ut.
- Kompletterade testdata med betalningsdatum och ett framtida utgångsdatum. Därefter gick testet igenom.
- Fortsatte med testning av MixerLab-relaterad kod.
- Granskade `MixService.java` och identifierade att metoderna `updateMix()` och `deleteMix()` fortfarande använde ett generellt `RuntimeException` när en mix inte hittades.
- Ändrade dessa metoder så att de i stället kastar `MixNotFoundException`.
- Uppdaterade motsvarande tester i `MixServiceTest.java` för att kontrollera att rätt undantag kastas när en mix saknas eller tillhör en annan användare.
- Verifierade att samtliga 15 tester i `MixServiceTest` passerade.
- Fortsatte med HTTP-tester i `MixControllerTest.java`.
- Lade till tester för att kontrollera att uppdatering och borttagning av en mix som inte finns returnerar HTTP 404 Not Found.
- Lade till ett test för att kontrollera att skapande av en mix med ett tomt namn returnerar HTTP 400 Bad Request.
- Lade till ytterligare ett test för att kontrollera att uppdatering av en mix med ett tomt namn returnerar HTTP 400 Bad Request.
- Kontrollerade att `MixService` inte anropas när valideringen misslyckas.
- Verifierade att samtliga 8 tester i `MixControllerTest` passerade.
- Granskade den nuvarande klassen `MixEq.java` inför fortsatt arbete med validering av MixerLab-data.
- Upptäckte att klassen saknar valideringsregler för sina åtta fält. Arbetet med att bestämma tillåtna värdeintervall pausades i väntan på att kontrollera vilka intervall frontendens reglage använder.
- Avslutade dagens kodarbete med att kontrollera Git-status, granska ändringarna, skapa en commit och pusha den till GitHub.

### Git och versionshantering

- Kontrollerade ändrade filer med `git status`.
- Granskade ändringarnas omfattning med `git diff --stat`.
- Lade till tre ändrade filer för commit:
  - `MixService.java`
  - `MixServiceTest.java`
  - `MixControllerTest.java`
- Skapade commit med följande information:

  **Commit:** `eef2d98e4`  
  **Commit-meddelande:** `test: add mix validation and not found tests`

- Pushade committen till GitHub utan problem.
- Bekräftade att ändringarna skickades till branchen `feature/mixerlab-database-persistence-java`.

## 2. Tekniker och verktyg

- Java.
- Spring Boot.
- JUnit 5.
- Mockito.
- AssertJ.
- IntelliJ IDEA.
- Maven.
- Git och GitHub.
- Enhetstestning och mockning.
- HTTP-tester med MockMvc.
- Spring MVC och Bean Validation.
- `UserService`.
- `UserRepo`.
- `StripeSubscriptionService`.
- `MixService`.
- `MixServiceTest`.
- `MixControllerTest`.
- `MixNotFoundException`.
- `MixEq`.

## 3. Problem och lösningar

### Testning av aktivering av Premium-medlemskap

Vid testning av `activatePremiumMembership()` fick jag ett Mockito-fel eftersom `userRepo.save()` anropades två gånger i stället för en gång.

**Lösning:**

Undersökte hur metoden fungerar och upptäckte att det första anropet sker när användaren skapas och det andra när medlemskapet aktiveras. Ändrade därför verifieringen till `times(2)`.

### Testning av redan använd Free Trial

Ett test för att förhindra att en användare aktiverar Free Trial flera gånger misslyckades eftersom `userRepo.save()` anropades trots att testet förväntade sig att det inte skulle ske.

**Lösning:**

Upptäckte att testanvändaren saknade roller. `getOrCreateUserBySubject()` tilldelade därför standardrollen `USER` och sparade användaren innan kontrollen av provperioden genomfördes.

Kompletterade testanvändaren med en befintlig roll. Därefter fungerade testet som förväntat.

### Testning av betalt medlemskap

Ett test för att förhindra Free Trial för användare med ett aktivt betalt medlemskap misslyckades eftersom inget undantag kastades.

**Lösning:**

Granskade `hasPaidMembership()` och upptäckte att metoden även kontrollerar `paidAt` och medlemskapets utgångsdatum.

Kompletterade testdata med ett betalningsdatum och ett framtida utgångsdatum. Därefter fungerade testet.

### Hantering av saknade mixes

Vid granskning av `MixService` upptäckte jag att `updateMix()` och `deleteMix()` använde generella `RuntimeException` när en mix inte hittades.

**Lösning:**

Ändrade metoderna så att de använder `MixNotFoundException`. Uppdaterade även testerna i `MixServiceTest` för att kontrollera det nya undantaget.

Detta gör felhanteringen tydligare och gör det möjligt för API:ets undantagshanterare att returnera HTTP 404 Not Found.

### HTTP-validering i MixController

Behövde kontrollera att API:et returnerar rätt HTTP-status när användaren försöker skapa eller uppdatera en mix med ett tomt namn.

**Lösning:**

Kompletterade `MixControllerTest` med tester för både POST och PUT. Kontrollerade även att `MixService` inte anropas när valideringen misslyckas.

Båda testerna passerade med HTTP 400 Bad Request som förväntat.

### Granskning av MixEq

Vid granskning av `MixEq.java` upptäckte jag att de åtta fälten saknar uttryckliga valideringsregler.

Innan några ändringar görs behöver jag undersöka vilka värdeintervall MixerLab använder i frontend. Det är viktigt att valideringen i backend överensstämmer med frontendens reglage.

### Mockito-varningar

Under testkörningarna visades varningar om att Mockito laddar sin Java-agent dynamiskt. Varningarna handlar om framtida kompatibilitet med JDK och är inte orsaken till att testerna misslyckades.

Detta har noterats för eventuell undersökning senare.

## 4. Ny kunskap

- Fördjupat kunskapen om hur man bygger en generell testklass för en serviceklass.
- Arbetat vidare med JUnit 5, Mockito och AssertJ.
- Fått bättre förståelse för hur mockade repositories används för att testa serviceklasser utan en riktig databas.
- Lärt mig hur `verify()` och `times()` används för att kontrollera antalet metodanrop.
- Fördjupat förståelsen för hur `getOrCreateUserBySubject()` fungerar och hur användarens standardroller hanteras.
- Lärt mig att testdata behöver uppfylla samtliga villkor i den faktiska affärslogiken.
- Fördjupat förståelsen för hur `hasPaidMembership()` kontrollerar medlemskap, betalningsdatum och utgångsdatum.
- Fått mer erfarenhet av att felsöka misslyckade enhetstester genom att följa anropskedjan i serviceklasserna.
- Fördjupat förståelsen för hur specifika undantag kan användas i stället för generella `RuntimeException`.
- Fått mer erfarenhet av att testa HTTP-statuskoder med MockMvc.
- Fördjupat förståelsen för hur `@Valid` och Bean Validation används för att kontrollera inkommande API-data.
- Fått mer erfarenhet av Git-arbetsflödet med `git status`, `git diff`, `git add`, `git commit` och `git push`.

## 5. Reflektion

Dagen har innehållit mycket arbete med testning och kvalitetssäkring av backend-koden.

Jag började med att bygga upp testtäckningen för `UserService`. Arbetet var stegvis där jag granskade den befintliga koden och implementerade ett test i taget. Jag stötte på några problem där testerna inte fungerade som förväntat, men genom att undersöka den bakomliggande affärslogiken kunde jag förstå orsakerna och korrigera testdatan.

Därefter fortsatte jag med MixerLab-relaterad kod. Jag förbättrade felhanteringen i `MixService` och kompletterade testerna för att kontrollera att saknade mixes hanteras korrekt.

Jag arbetade även vidare med HTTP-testerna i `MixControllerTest`. Det var värdefullt att kunna kontrollera både lyckade anrop och situationer där användaren skickar ogiltiga uppgifter eller försöker komma åt en mix som inte finns.

En annan viktig del av dagen var att kontrollera och dokumentera ändringarna i Git. Jag skapade en commit och pushade den framgångsrikt till rätt feature-branch på GitHub.

Innan jag avslutade dagen granskade jag även `MixEq.java`. Jag upptäckte att validering saknas, men behöver först kontrollera frontendens värdeintervall innan jag bestämmer vilka regler som ska implementeras.

Sammantaget har jag fått mer erfarenhet av enhetstestning, HTTP-testning, felhantering och versionshantering. Jag har också fått en bättre förståelse för hur viktigt det är att undersöka befintlig kod och affärslogik innan man gör förändringar.

## Arbetstid

- **09:30–09:50:** Daily sprint/scrum via videomöte med Johan Hellberg, Clara Westberg, Ellinor Höijer, Sakarias Eriksson och Nikola Mirosavljevic. En genomgång av MixerLab var planerad, men Nisse deltog inte. Mötet skulle fortsätta kl. 10:00.
- **09:50–10:00:** Förberedde mig inför mötets fortsättning.
- **10:00–10:20:** Mötet fortsatte. Frågade Johan om `UserServiceTest`. Johan sa att jag skulle skapa tester för det. Nisse deltog fortfarande inte.
- **10:20–12:05:** Arbetade med att skapa `UserServiceTest`, granska befintlig servicekod och implementera samt felsöka enhetstester. Hittills hade 13 tester implementerats och verifierats.
- **12:05–12:45:** Lunch.
- **12:45–16:30:** Fortsatte arbetet med `UserServiceTest` och gick därefter vidare med MixerLab-relaterad testning. Arbetade med felhantering i `MixService`, uppdaterade `MixServiceTest` och kompletterade `MixControllerTest` med HTTP 400- och 404-tester. Granskade även `MixEq.java`. Avslutade med att skapa och pusha commit `eef2d98e4` till GitHub.

## Sammanfattning av dagen

Arbetsdagen inleddes med ett daily sprint/scrum-möte tillsammans med Johan Hellberg, Clara Westberg, Ellinor Höijer, Sakarias Eriksson och Nikola Mirosavljevic. En genomgång av MixerLab var planerad, men eftersom Nisse inte deltog kunde den inte genomföras.

Under mötets fortsättning frågade jag Johan om tester för `UserService`, och han bekräftade att jag skulle skapa dessa.

Efter mötet påbörjade jag arbetet med `UserServiceTest.java`. Jag granskade befintlig kod och teststruktur och implementerade 13 tester som täcker användarhantering, medlemskap, Free Trial och uppdatering av användarroller.

Under arbetet uppstod några testfel som krävde att jag undersökte den bakomliggande logiken i `UserService` och `User`. Jag kunde identifiera orsakerna, komplettera testdatan och få testerna att fungera.

Senare fortsatte jag med MixerLab-relaterad testning. Jag förbättrade felhanteringen i `MixService` genom att införa `MixNotFoundException` när en mix inte hittas. Jag uppdaterade även tillhörande tester.

I `MixControllerTest` lade jag till tester för HTTP 400 och HTTP 404. Testerna kontrollerar att API:et returnerar rätt statuskoder när en mix saknas eller när användaren försöker skapa eller uppdatera en mix med ett tomt namn.

Dagens verifierade testresultat omfattar:

- `UserServiceTest`: 13 tester verifierade.
- `MixServiceTest`: 15 tester godkända.
- `MixControllerTest`: 8 tester godkända.

Jag avslutade arbetet med att granska ändringarna, skapa commit `eef2d98e4` och pusha den till `feature/mixerlab-database-persistence-java` på GitHub. Pushen genomfördes utan problem.

Slutligen granskade jag `MixEq.java` och identifierade att valideringsregler saknas för dess åtta fält. Nästa steg blir att undersöka vilka värdeintervall frontendens reglage använder innan valideringen implementeras.

Dagens arbete har framför allt gett mig mer erfarenhet av testning, felsökning, felhantering och Git.

--

# LIA-loggbok – Torsdag 26 november 2026

## 1. Arbetsuppgifter

### Daily sprint/scrum

- Dagen inleddes med ett kort daily sprint/scrum-möte via video med Clara Westberg och Ellinor Höijer.
- Johan Hellberg deltog inte i mötet, vilket gjorde att mötet blev kortare än vanligt.
- Informerade om att jag på fredagen den 27 november kommer att vara i skolan för en muntlig redovisning av min LIA, där jag ska berätta hur arbetet har gått under LIA-perioden.

### Fortsatt arbete med MixerLab

- Fortsatte med arbetet från föregående arbetsdag.
- Arbetet omfattade fortsatt kvalitetssäkring av Java-backendens hantering av MixerLab-mixes.
- Fortsatte att använda GitHub Copilots tidigare kodgranskningar som underlag för att identifiera återstående problem och förbättringar.

### Fortsatt arbete med integrationstester i MixIT

Arbetet fortsatte med att komplettera integrationstesterna i `MixIT.java` för att verifiera att användare inte kan ändra andra användares mixes.

Följande tester lades till:

1. `updateMix_shouldThrowExceptionWhenMixBelongsToAnotherUser()`
   - Testar att användare B inte kan uppdatera en mix som tillhör användare A.
   - Kontrollerar att `MixNotFoundException` kastas.
   - Kontrollerar att mixens ursprungliga information fortfarande är oförändrad.

2. `deleteMix_shouldThrowExceptionWhenMixBelongsToAnotherUser()`
   - Testar att användare B inte kan ta bort en mix som tillhör användare A.
   - Kontrollerar att `MixNotFoundException` kastas.
   - Kontrollerar att mixen fortfarande finns kvar i databasen och att informationen är oförändrad.

Under arbetet uppstod ett `LazyInitializationException` när testet för uppdatering försökte läsa `trackIds` efter att Hibernate-sessionen hade stängts.

Problemet löstes genom att lägga till `@Transactional` på testmetoden. Därefter kunde samlingen läsas under testets körning.

Båda de nya integrationstesterna kördes och godkändes:

- Uppdatering av en annan användares mix: 1 test, 0 failures och 0 errors.
- Borttagning av en annan användares mix: 1 test, 0 failures och 0 errors.

Det befintliga testet `deleteMix_shouldRemoveMixFromDatabase()` kontrollerades också och fungerade som det skulle.

Ett tidigare stavfel i testsubjectet `mix-delete-test-uset` rättades till `mix-delete-test-user`.

### Commit och push av MixIT

Efter att testerna hade verifierats committades ändringarna.

- Commit: `e7431caaf`
- Commit-meddelande: `test: add mix ownership integration tests`
- Ändrad fil: `MixIT.java`
- Ändringar: 66 insertions och 2 deletions.
- Committen pushades framgångsrikt till `feature/mixerlab-database-persistence-java`.

### Validering av JWT-subject i UserService

Efter arbetet med integrationstesterna fortsatte jag med Copilots synpunkt om att JWT-subject inte validerades innan det användes för att hämta eller skapa användare.

Granskade `MixController.java` och `UserService.java`.

I `MixController` hämtas subject direkt från JWT med `jwt.getSubject()` och skickas vidare till `MixService`.

I `UserService` identifierades att metoden `getOrCreateUserBySubject()` saknade en kontroll av subject innan databassökningen genomfördes.

En validering lades därför till med `StringUtils.hasText(subject)`.

Om subject är null, tomt eller endast innehåller blanksteg kastas nu `IllegalArgumentException` innan något databasanrop görs.

### Utökning av UserServiceTest

För att verifiera den nya valideringen lades tre nya enhetstester till i `UserServiceTest.java`:

1. `getOrCreateUserBySubject_shouldThrowExceptionWhenSubjectIsNull()`
2. `getOrCreateUserBySubject_shouldThrowExceptionWhenSubjectIsEmpty()`
3. `getOrCreateUserBySubject_shouldThrowExceptionWhenSubjectContainsOnlyWhitespace()`

Testerna kontrollerar både att rätt undantag kastas och att `UserRepo` inte kontaktas när subject är ogiltigt.

Vid den första testkörningen förväntade sig testerna `IllegalStateException`, medan produktionskoden korrekt kastade `IllegalArgumentException`.

Detta rättades genom att uppdatera förväntningarna i de tre testerna. Produktionskoden behövde inte ändras.

Därefter kördes hela `UserServiceTest` med följande resultat:

- 16 tester körda.
- 0 failures.
- 0 errors.
- 0 skipped.
- BUILD SUCCESS.

### Commit och push av JWT-validering

Efter att samtliga tester hade godkänts committades ändringarna.

- Commit: `8f5489a67`
- Commit-meddelande: `fix: validate JWT subject before user lookup`
- Ändrade filer:
  - `UserService.java`
  - `UserServiceTest.java`
- Ändringar: 32 insertions.
- Committen pushades framgångsrikt till `feature/mixerlab-database-persistence-java`.

### Ny Code Review från GitHub Copilot

Efter commit `8f5489a67` bad jag GitHub Copilot att genomföra ytterligare en Code Review av PR #265.

Copilot bedömde den senaste committen som en fokuserad förbättring med låg risk.

Följande bekräftades:

- `UserService` avvisar null, tomma och blankstegsbaserade subjects.
- Ogiltiga subjects stoppas innan databasanrop.
- Tester täcker samtliga tre situationer.
- Inga nya uppenbara blockerande problem identifierades.

Copilot uppmärksammade även att `IllegalArgumentException` redan hanteras av `ApiExceptionHandler` och returnerar HTTP 400.

En möjlig förbättring är att använda ett särskilt autentiseringsundantag om ogiltiga JWT-subjects ska behandlas som autentiseringsproblem i stället för vanliga klientfel.

Copilot påpekade också att följande områden fortfarande behöver undersökas:

- Konsekvent autentiseringsskydd för alla endpoints i `MixController`.
- HTTP-tester som verifierar att oautentiserade anrop till create, update och delete avvisas.
- Testning av cross-user authorization genom HTTP och JWT.
- Validering av EQ-värden.
- Bevarande av track-ordning i databasen.

Copilot kunde se att sju CI-kontroller var slutförda, men kunde inte bekräfta om samtliga hade godkänts.

Copilots bedömning var att PR #265 fortfarande inte är redo att mergas innan säkerhetskonfigurationen och HTTP-auktoriseringen har verifierats och övriga merge-krav är uppfyllda.

### Undersökning av frontendintegrationen för MixerLab

Under eftermiddagen började jag undersöka hur den befintliga React-frontendens API-struktur fungerar för att kunna koppla MixerLab till Java-backendens databaslösning.

Målet var att förstå hur frontend kommunicerar med backend innan några kodändringar genomförs.

Följande filer och delar av projektet granskades:

- `frontend/src/api/mixer/mixer.client.ts`
- `frontend/src/api/mixer/mixer.types.ts`
- `frontend/src/api/mixer/index.ts`
- `frontend/src/api/index.ts`
- `frontend/src/hooks/useApiClient.ts`
- `MixerLab.tsx`
- `eqTypes.ts`
- `BacktrackSelector.tsx`
- `Backtracks/constants.ts`
- `Tracks/constants.ts`

Jag undersökte även andra React-komponenter som redan använder `useApiClient()`, bland annat:

- `TracksSelector.tsx`
- `BacktracksPage.tsx`
- `AudioRecorder.tsx`
- `StudioBass.tsx`
- `MyRecordings.tsx`
- `MyProfile.tsx`

Syftet var att hitta ett befintligt arbetssätt för autentiserade API-anrop som kan återanvändas i MixerLab.

### Genomgång av API-klientens struktur

Jag hittade att projektet redan har en gemensam funktion, `createApiClient()`, som skapar och samlar projektets olika API-klienter.

Mixer-klienten skapas genom `createMixerClient(getAccessToken)` och är redan kopplad till den gemensamma API-strukturen.

Jag granskade även `useApiClient.ts` och konstaterade att hooken kopplar API-klienten till den aktuella Auth0-sessionen och tillhandahåller en access token för autentiserade backend-anrop.

Därmed behöver vi inte skapa ett nytt system för API-kommunikation. Vi kan i stället utöka den befintliga mixer-klienten med funktioner för att hantera mixes.

### Undersökning av befintlig MixerLab-lagring

I `MixerLab.tsx` identifierades den nuvarande lösningen där mixinställningar sparas i webbläsarens `localStorage` med nyckeln `mixerlab_mix`.

Den nuvarande lösningen sparar bland annat:

- EQ-inställningar.
- Vald backtrack.
- Valda track-ID:n.

Jag konstaterade att frontendens nuvarande `SavedMix`-struktur skiljer sig från Java-backendens DTO-struktur.

Frontend saknar bland annat ett namn för mixen, medan Java-backendens `MixRequestDto` kräver ett namn.

### Genomgång av Java-backendens DTO:er

Jag gick igenom `MixRequestDto.java` och `MixResponseDto.java` för att jämföra frontendens nuvarande datamodell med backendens API-kontrakt.

`MixRequestDto` innehåller:

- `name`
- `backtrackId`
- `eq`
- `trackIds`

Namnet är obligatoriskt genom `@NotBlank` och listan med track-ID:n är obligatorisk genom `@NotNull`.

`MixResponseDto` innehåller samma information samt ett `id` som identifierar den sparade mixen i databasen.

Jag identifierade även att frontendens EQ-struktur behöver översättas till backendens `MixEq`-struktur innan data skickas till Java-backend.

### Genomgång av track- och backtrack-ID:n

Jag granskade `Tracks/constants.ts` och `Backtracks/constants.ts`.

MixerLab använder stabila strängbaserade ID:n för sina tracks. Det innebär att frontend kan skicka dessa ID:n till Java-backendens `List<String> trackIds`.

Jag konstaterade även att backtrack-ID:n behöver hanteras separat och att vi behöver säkerställa att de ID:n som frontend använder överensstämmer med vad backend förväntar sig.

### Beslut om hur användaren ska hantera sparade mixes

Vi diskuterade hur den färdiga funktionen ska fungera för användaren.

Beslutet blev att användaren ska kunna spara flera olika mixes med egna namn, exempelvis:

- Min rockmix
- Jazzmix
- Testmix

Det innebär att MixerLab behöver stöd för att skapa, hämta, uppdatera och ta bort flera sparade mixes per användare.

Vi kom också fram till att den befintliga `localStorage`-lösningen ska ersättas med databaslagring via Java-backend, så att sparade mixes finns kvar efter att sidan uppdateras eller webbläsaren stängs.

Ingen frontendkod ändrades under denna undersökning. Arbetet avslutades med att identifiera hur integrationen bör byggas vidare med projektets befintliga API-struktur.

---

## 2. Tekniker och verktyg

- Java.
- Spring Boot.
- Spring Security.
- JWT-autentisering.
- Spring Data JPA.
- Hibernate.
- PostgreSQL.
- Flyway.
- JUnit 5.
- Mockito.
- AssertJ.
- MockMvc.
- Testcontainers.
- React.
- TypeScript.
- Auth0.
- IntelliJ IDEA.
- Maven.
- Git och GitHub.
- GitHub Copilot Code Review.
- `MixController`.
- `MixService`.
- `MixIT`.
- `MixControllerTest`.
- `MixServiceTest`.
- `MixEq`.
- `UserService`.
- `UserServiceTest`.
- `MixNotFoundException`.
- `ApiExceptionHandler`.
- `StringUtils.hasText()`.
- `@Transactional`.
- `IllegalArgumentException`.
- `MixRequestDto`.
- `MixResponseDto`.
- `MixerLab.tsx`.
- `mixer.client.ts`.
- `mixer.types.ts`.
- `createApiClient()`.
- `createMixerClient()`.
- `useApiClient()`.
- `localStorage`.

---

## 3. Problem och lösningar

### Cross-user authorization

GitHub Copilot identifierade att det saknades integrationstester som bevisade att en användare inte kan uppdatera eller ta bort en annan användares mix.

`MixService` använder `findByIdAndUserId`, vilket innebär att sökningen tar hänsyn till både mixens ID och användarens ID.

**Genomfört:**

- Lade till integrationstest för att förhindra att en annan användare uppdaterar en mix.
- Lade till integrationstest för att förhindra att en annan användare tar bort en mix.
- Verifierade att mixens information är oförändrad efter otillåtna försök.
- Båda testerna godkändes mot PostgreSQL via Testcontainers.

**Återstår:**

HTTP- och JWT-auktorisering behöver fortfarande verifieras med tester som använder två olika JWT-principaler. De nya integrationstesterna verifierar service- och databaslogiken, inte hela HTTP-säkerhetskedjan.

### JWT subject och användarskapande

Copilot identifierade att `jwt.getSubject()` används direkt i `MixController`.

**Genomfört:**

- Granskade `MixController.java` och `UserService.java`.
- Lade till validering i `getOrCreateUserBySubject()` med `StringUtils.hasText(subject)`.
- Null, tomma strängar och blanksteg avvisas nu.
- Ogiltiga subjects stoppas innan databassökningen.
- Lade till tre nya tester i `UserServiceTest`.
- Samtliga 16 tester i `UserServiceTest` godkändes.

**Återstår:**

Undersöka om JWT-subject även behöver särskild hantering på autentiseringsnivå och om ett särskilt autentiseringsundantag är lämpligare än `IllegalArgumentException`.

### Säkerhet i MixController

Copilot har uppmärksammat att endast `GET /mixes` uttryckligen använder `@PreAuthorize("isAuthenticated()")`.

POST, PUT och DELETE förlitar sig på den globala säkerhetskonfigurationen.

**Återstår:**

Granska `SecurityConfig.java` och verifiera att samtliga endpoints skyddas korrekt. Därefter behöver HTTP-tester kompletteras för att kontrollera autentiserade och oautentiserade anrop.

### EQ-validering

`MixEq.java` innehåller åtta fält utan uttryckliga valideringsintervall.

**Återstår:**

Undersöka vilka värden MixerLabs frontend använder innan valideringsregler implementeras. Detta är viktigt för att undvika att backend avvisar giltiga värden från frontend.

### Track-ordning

Copilot uppmärksammade att ordningen på tracks inte uttryckligen bevaras i databasen.

**Återstår:**

Undersöka om track-ordningen är viktig för uppspelning och återställning av en mix. Om den är viktig behöver en lösning med exempelvis `@OrderColumn` utredas.

### Transaktionshantering

`MixService` saknar uttryckliga `@Transactional`-annoteringar.

**Återstår:**

Granska vilka service-metoder som behöver transaktionshantering, framför allt uppdatering, borttagning och hämtning av collections.

### Övriga identifierade problem

Copilot identifierade även följande områden som behöver undersökas:

- Konsekvent JSON-format för valideringsfel.
- Mer informativa felmeddelanden.
- Eventuella begränsningar mot dubbla track IDs.
- Migrationstest mot verklig migrationshistorik.
- Överensstämmelse mellan Hibernate-entiteter och databasschema.
- Mindre förbättringar i befintliga tester.
- Kontroll av EQ efter ett otillåtet uppdateringsförsök.

Dessa punkter är ännu inte åtgärdade eller verifierade.

### Frontendintegration för MixerLab

Vid undersökningen av frontendens nuvarande lagringslösning identifierades flera skillnader mellan frontendens `SavedMix` och Java-backendens DTO:er.

**Identifierat:**

- Frontend använder för närvarande `localStorage` för att spara en mix.
- Java-backendens `MixRequestDto` kräver ett mixnamn.
- Frontendens EQ-struktur skiljer sig från backendens `MixEq`.
- `MixResponseDto` returnerar ett databas-ID som frontend behöver hantera.
- Den befintliga API-klienten saknar ännu metoder för att skapa, hämta, uppdatera och ta bort mixes.

**Planerat:**

- Utöka `mixer.types.ts` med typer för mixarnas request- och response-data.
- Utöka `mixer.client.ts` med API-metoder för mixhantering.
- Använda den befintliga `useApiClient()`-hooken för autentiserade anrop.
- Ersätta den nuvarande permanenta `localStorage`-lagringen med databaslagring.
- Implementera stöd för flera sparade mixes med egna namn.

Ingen kodändring för frontendintegrationen har ännu genomförts.

---

## 4. Ny kunskap

- Fördjupat förståelsen för hur GitHub Copilot kan användas för att genomföra upprepade Code Reviews av en pull request.
- Fått en tydligare bild av skillnaden mellan att testa service-logik med mockade beroenden och att verifiera säkerheten genom integrationstester.
- Fördjupat förståelsen för varför användarens JWT-subject behöver valideras innan det används för att hämta eller skapa användardata.
- Fått bättre förståelse för hur `findByIdAndUserId` kan användas för att begränsa åtkomst till användarägda resurser.
- Uppmärksammat skillnaden mellan HTTP 403 och HTTP 404 och hur valet av statuskod kan påverka informationsläckage.
- Fördjupat förståelsen för hur JPA hanterar `@ElementCollection` och varför ordningen i en lista behöver hanteras uttryckligen om den är viktig.
- Fått mer kunskap om varför transaktionshantering är viktig vid databasoperationer.
- Uppmärksammat vikten av att ha ett konsekvent API-format för valideringsfel.
- Fått en bättre överblick över vilka delar av MixerLab-persistensen som fortfarande behöver kvalitetssäkras.
- Förstått hur `LazyInitializationException` kan uppstå när en lazy-loaded collection hämtas efter att Hibernate-sessionen har stängts.
- Lärt mig hur `@Transactional` på en integrationstestmetod kan hålla databassessionen öppen under testets körning.
- Fördjupat förståelsen för hur `StringUtils.hasText()` kan användas för att kontrollera om en sträng innehåller text och inte bara blanksteg.
- Fått bättre förståelse för skillnaden mellan `IllegalArgumentException` och `IllegalStateException` och när respektive undantag är lämpligt.
- Fått mer erfarenhet av att komplettera integrationstester och verifiera att användarägarskap respekteras vid databasoperationer.
- Fått bättre förståelse för hur frontendens API-klient är uppbyggd och hur `createApiClient()` samlar projektets olika API-klienter.
- Lärt mig hur `useApiClient()` kopplar React-komponenter till autentiserade backend-anrop via Auth0.
- Fått bättre förståelse för hur befintliga React-komponenter använder API-klienten för att hämta, skapa, uppdatera och ta bort data.
- Identifierat hur frontendens nuvarande `localStorage`-struktur behöver anpassas till Java-backendens `MixRequestDto` och `MixResponseDto`.

---

## 5. Reflektion

Dagens arbete har fortsatt med kvalitetssäkring av MixerLab-persistensen i Java-backenden.

Den nya Code Review-granskningen från GitHub Copilot var användbar eftersom den bekräftade att flera av de tidigare problemen har åtgärdats.

Under dagen kunde jag komplettera integrationstesterna i `MixIT.java` och verifiera att en användare inte kan uppdatera eller ta bort en annan användares mix.

Jag stötte även på ett problem med `LazyInitializationException`, vilket gav mig en bättre förståelse för hur Hibernate hanterar lazy-loaded collections och hur `@Transactional` kan användas i integrationstester.

Därefter arbetade jag med JWT-subject-valideringen i `UserService`. Genom att lägga till kontrollen med `StringUtils.hasText()` säkerställs att ogiltiga subjects inte når databasanropen. De tre nya testerna och samtliga befintliga tester i `UserServiceTest` passerade.

Jag har även committat och pushat båda delarna av arbetet till feature-branchen. Det känns bra att kunna avsluta respektive förbättring med verifierade tester och en separat commit.

Den senaste Code Review-granskningen visar att det fortfarande finns viktiga områden att arbeta med, särskilt säkerheten kring HTTP-anrop och JWT-auktorisering.

Under eftermiddagen började jag undersöka frontendens befintliga struktur inför integrationen med Java-backenden. Jag gick igenom hur API-klienten skapas, hur autentisering hanteras och hur andra React-komponenter använder `useApiClient()`.

Jag kunde också identifiera skillnader mellan frontendens nuvarande `SavedMix` och backendens DTO:er. En viktig del blir att anpassa frontendens EQ-inställningar och lägga till stöd för mixnamn och databasens ID.

Vi kom fram till att användaren ska kunna spara flera olika mixes med egna namn. Det innebär att MixerLab behöver kunna skapa, hämta, uppdatera och ta bort sparade mixes.

Jag har ännu inte ändrat någon frontendkod. Det känns viktigt att först förstå projektets befintliga struktur och följa de mönster som redan används i andra komponenter, så att integrationen blir konsekvent med resten av projektet.

### Plan för fredagen den 27 november

På fredagen kommer jag att vara i skolan för min muntliga LIA-redovisning. Där ska jag berätta hur arbetet har gått under LIA-perioden och presentera vad jag har arbetat med.

Efter redovisningen, när jag fortsätter med utvecklingsarbetet, är nästa planerade steg att börja strukturera frontendintegrationen för MixerLab.

Fokus blir att:

- Lägga till TypeScript-typer som motsvarar `MixRequestDto` och `MixResponseDto`.
- Utöka den befintliga mixer-klienten med metoder för att hämta, skapa, uppdatera och ta bort mixes.
- Planera hur användaren ska kunna namnge och hantera flera sparade mixes.
- Anpassa frontendens EQ-struktur till Java-backendens `MixEq`.

Arbetet ska genomföras stegvis och med hänsyn till den befintliga frontendstrukturen.

---

## Arbetstid

- **09:30–09:35:** Daily sprint/scrum via videomöte med Clara Westberg och Ellinor Höijer. Johan Hellberg deltog inte i mötet, vilket gjorde att mötet blev kort. Informerade om att jag på fredagen den 27 november ska vara i skolan för en muntlig redovisning av min LIA.
- **09:35–12:00:** Fortsatte med arbetet från föregående arbetsdag. Arbetet omfattade kvalitetssäkring av MixerLab-persistensen och genomgång av GitHub Copilots Code Review.
- **12:00–14:00:** Lunch.
- **14:00–17:00:** Undersökte frontendens befintliga API-struktur och hur MixerLab ska integreras med Java-backendens databaslösning. Granskade API-klienten, `useApiClient()`, relevanta React-komponenter samt backendens request- och response-DTO:er. Planerade hur användaren ska kunna spara flera mixes med egna namn.

---

## Sammanfattning av dagen

Arbetsdagen inleddes med ett kort daily sprint/scrum-möte via video med Clara Westberg och Ellinor Höijer. Johan Hellberg deltog inte vid mötet. Under mötet informerade jag om att jag på fredagen den 27 november kommer att vara i skolan för en muntlig redovisning av min LIA.

Efter mötet fortsatte jag med arbetet från föregående arbetsdag och gick vidare med den senaste Code Review-granskningen från GitHub Copilot.

Under dagen kompletterade jag integrationstesterna i `MixIT.java` med tester som verifierar att en användare inte kan uppdatera eller ta bort en annan användares mix. Båda testerna godkändes. Det befintliga borttagningstestet kontrollerades också och fungerade som det skulle.

Ett problem med `LazyInitializationException` uppstod när testet försökte läsa `trackIds`. Problemet löstes genom att lägga till `@Transactional` på testmetoden.

Ändringarna i `MixIT.java` committades och pushades till GitHub med commit `e7431caaf`.

Därefter arbetade jag med JWT-subject-validering i `UserService`. Jag lade till en kontroll som avvisar null, tomma och blankstegsbaserade subjects innan databasanrop görs. Tre nya tester lades till för att verifiera valideringen. Samtliga 16 tester i `UserServiceTest` godkändes utan failures eller errors.

Ändringarna i `UserService.java` och `UserServiceTest.java` committades och pushades till GitHub med commit `8f5489a67`.

Jag genomförde även en ny Code Review med GitHub Copilot efter den senaste committen. Copilot bedömde ändringen som en fokuserad förbättring utan nya uppenbara blockerande problem. De viktigaste återstående områdena handlar fortfarande om HTTP/JWT-säkerhet, EQ-validering och bevarande av track-ordning.

Under eftermiddagen började jag undersöka frontendens struktur inför integrationen av MixerLab med Java-backenden. Jag granskade hur API-klienten skapas och används, hur Auth0-token hanteras och hur befintliga React-komponenter kommunicerar med backend.

Jag jämförde även frontendens nuvarande lagringsstruktur med Java-backendens `MixRequestDto` och `MixResponseDto`. Undersökningen visade att frontend behöver anpassas för att kunna skicka mixnamn, hantera databasens mix-ID och översätta EQ-inställningarna till backendens struktur.

Vi bestämde att användaren ska kunna spara flera olika mixes med egna namn och senare kunna hämta, uppdatera och ta bort dem.

Ingen frontendkod ändrades under eftermiddagen. Arbetet bestod av undersökning och planering inför implementationen.

Nästa arbetsdag börjar med den muntliga LIA-redovisningen i skolan. Därefter är planen att fortsätta med frontendintegrationen genom att lägga till nödvändiga TypeScript-typer och API-metoder för mixhanteringen.

--
