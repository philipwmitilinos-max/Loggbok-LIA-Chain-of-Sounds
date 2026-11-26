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

# Daglig LIA-loggbok – Torsdag 26 november 2026

## 1. Arbetsuppgifter

- Deltog i dagens dagliga videomöte tillsammans med Clara Westberg och Ellinor Höijer. Johan deltog inte.
- Berättade att jag ska hålla en muntlig LIA-redovisning på skolan fredagen den 27 november.
- Fortsatte arbetet med att granska backend-implementationen för MixerLab och de tidigare kommentarerna från Copilots kodgranskning.
- Arbetade vidare med integrationstester för att kontrollera att användare endast kan hantera sina egna sparade mixes.
- Lade till tester som verifierar att en användare inte kan uppdatera eller ta bort en annan användares mix.
- Lade till `@Transactional` på berörda integrationstester efter att ett `LazyInitializationException` uppstod vid åtkomst till `trackIds`.
- Rättade ett fel i ett av testerna där felaktig testdata användes.
- Verifierade att testerna för användarägarskap fungerade.
- Skapade commit `e7431caaf` (`test: add mix ownership integration tests`) och pushade ändringen till GitHub.
- Fortsatte granska hanteringen av JWT-subject i `UserService`.
- Lade till validering med `StringUtils.hasText(subject)` för att kontrollera att JWT-subject inte är null, tom eller endast innehåller blanksteg.
- Lade till tre tester för att verifiera valideringen av subject.
- Körningen av `UserServiceTest` lyckades med 16 godkända tester.
- Skapade commit `8f5489a67` (`fix: validate JWT subject before user lookup`) och pushade ändringen till GitHub.
- Granskade Copilots senaste kodgranskning av Pull Request #265.
- Konstaterade att den senaste granskningen inte identifierade något nytt blockerande problem i den senaste ändringen.
- Fortsatte undersöka återstående frågor kring säkerhet, validering och databashantering.
- Påbörjade undersökningen av frontend-integrationen för MixerLab.
- Granskade `mixer.client.ts` och `mixer.types.ts` för att se hur frontend kommunicerar med Mixer-backend.
- Granskade `MixerLab.tsx` och identifierade hur den nuvarande lösningen sparar mixinställningar i `localStorage`.
- Identifierade att den nuvarande sparade mixen innehåller EQ-inställningar, backtrack-ID och track-ID:n, men saknar ett namn och ett databas-ID.
- Jämförde frontendens nuvarande datastruktur med backendens `MixRequestDto` och `MixResponseDto`.
- Granskade `eqTypes.ts` och undersökte hur frontendens EQ-värden kan mappas till backendens `MixEq`.
- Granskade `BacktrackSelector.tsx` och konstanterna för backtracks för att förstå hur val av backtrack hanteras.
- Granskade `Tracks/constants.ts` för att undersöka hur MixerLab identifierar och hämtar sina spår.
- Granskade `frontend/src/api/index.ts` och `useApiClient.ts` för att förstå hur befintliga API-klienter skapas och används.
- Undersökte exempel på hur andra komponenter använder `useApiClient()` och Mixer-klienten.
- Identifierade att frontend redan har en etablerad lösning för autentiserad kommunikation med backend genom `useApiClient()`.
- Diskuterade den önskade funktionaliteten för flera sparade mixes med egna namn, exempelvis "Min rockmix", "Jazzmix" och "Testmix".
- Påbörjade planeringen för hur användaren ska kunna skapa, spara, lista, återställa, uppdatera och ta bort sina mixes.
- Inga ändringar i frontend-koden genomfördes under dagen. Arbetet bestod av undersökning och planering.

## 2. Tekniker och verktyg

- Java och Spring Boot.
- Spring Data JPA och Hibernate.
- PostgreSQL och Testcontainers.
- JUnit 5 och Mockito.
- Maven Wrapper.
- JWT och Spring Security.
- `StringUtils.hasText()` för validering.
- React och TypeScript.
- `useEffect` och React state.
- Auth0 och autentiserad API-kommunikation.
- `localStorage`.
- Git och GitHub.
- Pull Requests och kodgranskning.
- Copilot Code Review.
- IntelliJ IDEA.
- GitHub Actions.

## 3. Problem och lösningar

- Vid integrationstesterna uppstod ett `LazyInitializationException` när testet försökte komma åt `trackIds` efter att Hibernate-sessionen hade stängts. Problemet löstes genom att lägga till `@Transactional` på berörda testmetoder.
- Ett av integrationstesterna innehöll felaktig testdata. Detta rättades och testerna kördes igen.
- Det var viktigt att verifiera att användare inte kan uppdatera eller ta bort andra användares mixes. Detta hanterades genom tester som kontrollerar användarägarskapet.
- JWT-subject behövde valideras innan det används för att hitta eller skapa en användare. Detta löstes genom att kontrollera värdet med `StringUtils.hasText(subject)` och lägga till tillhörande tester.
- Vid granskningen av backend återstod frågor kring säkerheten för HTTP-anrop och hur JWT-autentisering ska verifieras genom tester. De befintliga integrationstesterna testar tjänstelogik direkt och verifierar därför inte hela HTTP- och JWT-flödet.
- Vid undersökningen av frontend-integrationen upptäcktes att MixerLab fortfarande använder `localStorage` för att spara en mix under nyckeln `mixerlab_mix`.
- Frontendens nuvarande datastruktur skiljer sig från backendens DTO:er. Frontend saknar bland annat ett namn för mixen, medan backendens `MixRequestDto` kräver ett namn.
- EQ-strukturen i frontend behöver mappas till backendens `MixEq`, eftersom frontend använder separata grupper för amplitud och frekvens.
- Den befintliga API-klienten saknar metoder för att skapa, hämta, uppdatera och ta bort mixes. Lösningen blir att utöka den befintliga Mixer-klienten i stället för att skapa en separat kommunikationslösning.
- Backtrack-ID:n behöver kontrolleras så att de motsvarar de ID:n som backend förväntar sig. Detta är ännu inte verifierat.

## 4. Ny kunskap

- Fördjupad förståelse för hur `@Transactional` kan användas i integrationstester när testet behöver komma åt lazy-loaded relationer.
- Förståelse för varför Hibernate kan kasta `LazyInitializationException` när en relation hämtas efter att sessionen har stängts.
- Lärt mig mer om hur tester kan verifiera att en användare endast får ändra eller ta bort sina egna databaspersistenta objekt.
- Förståelse för varför JWT-subject behöver valideras innan det används för att identifiera en användare.
- Fördjupad förståelse för hur `StringUtils.hasText()` kan användas för att kontrollera null, tomma strängar och strängar med enbart blanksteg.
- Lärt mig mer om hur frontendens API-klienter är organiserade i projektet.
- Förståelse för hur `createApiClient()` samlar projektets olika API-klienter.
- Lärt mig hur `useApiClient()` används för att skapa autentiserad kommunikation med backend.
- Förståelse för hur frontendens EQ-värden behöver mappas till backendens Java-modell.
- Förståelse för skillnaden mellan att spara en enda mix lokalt i webbläsaren och att spara flera namngivna mixes i en databas.
- Lärt mig att stabila track-ID:n kan användas för att återskapa en mix utan att behöva spara ljudfilernas URL:er i själva mixen.

## 5. Reflektion

Dagen har varit produktiv och inneburit framsteg både inom backend och planeringen av frontend-integrationen.

Under förmiddagen fortsatte jag att stärka backend-implementationen genom att lägga till tester för användarägarskap och validering av JWT-subject. Det var viktigt att säkerställa att användare inte kan komma åt eller ändra andra användares mixes.

Jag fick även arbeta vidare med integrationstester och förstå hur Hibernate hanterar relationer och lazy loading. Problemen med `LazyInitializationException` gav mig en bättre förståelse för när transaktioner behövs i tester.

Under eftermiddagen började jag undersöka hur frontendens MixerLab ska kopplas till den befintliga Java-backenden. Jag gick igenom relevanta komponenter, API-klienter och datatyper för att förstå hur den nuvarande lösningen fungerar.

En viktig upptäckt var att frontend redan har en etablerad struktur för autentiserad API-kommunikation. Det innebär att jag kan bygga vidare på den befintliga Mixer-klienten i stället för att skapa en ny lösning.

Jag har också identifierat skillnader mellan frontendens nuvarande sparade mix och backendens DTO:er. Dessa behöver hanteras innan integrationen kan genomföras.

Jag har inte börjat ändra frontend-koden ännu. Dagens arbete har framför allt handlat om att förstå den befintliga strukturen och planera hur flera namngivna mixes ska kunna sparas och hämtas från databasen.

Nästa arbetsdag ska jag hålla min muntliga LIA-redovisning på skolan. Därefter är planen att fortsätta frontend-integrationen genom att börja utöka TypeScript-typerna och Mixer API-klienten.

## Arbetstid

- 09:30–09:35 – Dagligt videomöte med Clara Westberg och Ellinor Höijer.
- 09:35–12:00 – Backendtester, användarägarskap och JWT-subject-validering.
- 12:00–14:00 – Lunch.
- 14:00–17:00 – Granskning av Copilots kodgranskning och undersökning av frontend-integrationen för MixerLab.

**Sluttid:** 17:00

## Sammanfattning av dagen

Under dagen fortsatte arbetet med MixerLab-databaspersistens. Jag lade till integrationstester för att kontrollera att användare endast kan uppdatera och ta bort sina egna mixes. Jag löste även ett problem med `LazyInitializationException` genom att använda `@Transactional` i berörda tester.

Jag lade till validering av JWT-subject i `UserService` och kompletterade med tre tester. `UserServiceTest` genomfördes med 16 godkända tester.

Ändringarna för integrationstesterna och JWT-subject-valideringen committades och pushades framgångsrikt till GitHub.

Under eftermiddagen granskade jag Copilots kodgranskning och började undersöka frontend-integrationen. Jag gick igenom MixerLab-komponenten, API-klienten, TypeScript-typerna och hur autentiserade API-anrop hanteras i projektet.

Undersökningen visade att frontend behöver utökas för att stödja flera namngivna mixes som sparas i Java-backenden i stället för att en enda mix sparas permanent i `localStorage`.

Frontend-koden har ännu inte ändrats. Nästa steg är att börja implementera stödet för mixes i TypeScript-typerna och Mixer API-klienten.

Fredagen den 27 november ska jag först hålla min muntliga LIA-redovisning på skolan och därefter fortsätta arbetet med frontend-integrationen.

--
