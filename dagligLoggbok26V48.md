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

### Backend – studio-bass-api

Genomfört

- [x] Skapat Mix-entiteten.
- [x] Skapat MixEq.
- [x] Skapat MixRepository.
- [x] Skapat MixRequestDto och MixResponseDto.
- [x] Implementerat MixService.
- [x] Implementerat endpoints för att hantera mixes.
- [x] Implementerat användarrelationer.
- [x] Implementerat databasmigration V26.
- [x] Skapat 15 enhetstester för MixService.
- [x] Skapat MixIT för integrationstester.
- [x] Åtgärdat problemet med V26 som identifierades i den tidigare CI-körningen.

Kvarstående arbete

- [ ] Lösa problemet med uppdatering av track-ID:n.
- [ ] Komplettera HTTP-/controllerintegrationstester.
- [ ] Förbättra autentiseringshanteringen.
- [ ] Validera trackIds.
- [ ] Undersöka N+1-frågor.
- [ ] Förbättra felhantering för mixar som inte hittas.
- [ ] Förbättra integrationstesterna med EntityManager, flush() och clear().

Detta är huvudsakligen förbättringspunkterna från Copilots granskning av PR #261.

### Vad ska vi göra härnäst?

rekommenderar att vi arbetar i den här ordningen:

Steg 1 – Få igång testmiljön

- Kontrollera Maven-installationen.
- Säkerställa att vi kan köra mvn clean verify.
- Kontrollera att V26 fungerar.

Steg 2 – Åtgärda Copilots kommentarer

- Ta en kommentar i taget.
- Ändra befintlig kod så lite som möjligt.
- Skriva eller uppdatera tester.
- Köra testerna efter varje ändring.

Steg 3 – Verifiera backend

- Köra samtliga enhets- och integrationstester.
- Kontrollera API-endpoints.
- Säkerställa att användarnas mixes hanteras korrekt.

Steg 4 – Frontend

- Ansluta MixerLab till Java-backend.
- Ersätta eventuell lokal lagring av mixes med API-anrop.
- Kontrollera att sparade mixes finns kvar efter refresh och omstart.

--

# Daglig LIA-loggbok – Tisdag 24 november 2026

## 1. Arbetsuppgifter

- Deltog i ett kort daily Scrum-möte via videolänk tillsammans med Johan Hellberg och Clara Westberg.
- Under mötet diskuterades dagens arbete och planeringen framåt.
- Beslutet för dagens fortsatta arbete är att fortsätta med MixerLab och arbetet med databaspersistens i Java-backend.

**Planerade arbetsuppgifter för dagen:**

- Fortsätta undersöka Copilots tidigare kodgranskningsanmärkningar från PR #261.
- Fortsätta granskningen av databasmigrationen V26.
- Undersöka hur V26 hanterar äldre databasscheman där tabellerna redan finns.
- Vid behov komplettera `MixFlywayMigrationIT` med ytterligare migrationsscenarier.
- Fortsätta granska och verifiera återstående förbättringspunkter från Copilot.
- Kontrollera den senaste lokala commiten `16b1cced0` och förbereda eventuell push till GitHub.
- Fortsätta kvalitetssäkra backend-implementationen inför den fortsatta frontend-integrationen.

## 2. Tekniker och verktyg

- Java och Spring Boot.
- Git och GitHub.
- Pull Requests och kodgranskning.
- Maven Wrapper.
- JUnit 5.
- Testcontainers och PostgreSQL.
- Flyway och SQL-migrationer.
- Hibernate och JPA.
- IntelliJ IDEA.
- PowerShell och terminalkommandon.
- GitHub Actions.

## 3. Problem och lösningar

Inga nya problem eller lösningar dokumenterade ännu.

Dagens fortsatta arbete kommer bland annat att undersöka hur Flyway-migrationen V26 fungerar med äldre databasscheman.

## 4. Ny kunskap

Kompletteras under dagen utifrån nya kunskaper och erfarenheter från arbetet med MixerLab, databasmigrationer och kodgranskning.

## 5. Reflektion

Dagen inleddes med ett kort daily Scrum-möte där dagens arbete planerades tillsammans med Johan Hellberg och Clara Westberg.

Fokus för dagen är att fortsätta utvecklingen av MixerLab och kvalitetssäkra Java-backendens databaspersistens. Arbetet fortsätter där föregående arbetsdag avslutades, med särskilt fokus på Copilots tidigare kodgranskningsanmärkningar och verifiering av databasmigrationen V26.

Loggboken kompletteras under dagen med genomförda arbetsuppgifter, eventuella problem, lösningar och nya kunskaper.

## Arbetstid

- 09:30–09:45 – Daily Scrum via videomöte med Johan Hellberg och Clara Westberg.
- 09:45–pågående – Fortsatt arbete med MixerLab och databaspersistens i Java-backend.

**Sluttid:** Återstår att dokumentera.

## Sammanfattning av dagen

Dagen inleddes med ett daily Scrum-möte där dagens arbete planerades. Fokus ligger på att fortsätta arbetet med MixerLab, framför allt kvalitetssäkring av Java-backendens databaspersistens och granskning av tidigare kodgranskningsanmärkningar.

Dagens arbete fortsätter från föregående arbetsdag med undersökning av V26-migrationen och hur den hanterar äldre databasscheman.

--
