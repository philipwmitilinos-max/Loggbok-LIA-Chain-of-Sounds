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
- React och Playwright.
- PowerShell och terminalkommandon.
- Git-historik och `git show` för att jämföra tidigare kodversioner.

## 3. Problem och lösningar

- Den tidigare Pull Requesten #261 behövde ersättas enligt Johans förslag. Den stängdes och en ny PR skapades efter att test-branchen mergats till feature-branchen.
- Feature-branchen innehöll nio lokala commits som inte hade pushats till GitHub. Dessa behölls och kombinerades med ändringarna från test-branchen.
- Mergen genomfördes automatiskt utan konflikter.
- Maven kunde inte köras direkt via `mvn`, men projektet innehöll Maven Wrapper. Därför användes `mvnw.cmd` för att genomföra verifieringen.
- När jag skulle lägga till ändringen i `.github/workflows/ci.yml` uppstod ett sökvägsfel eftersom jag befann mig i `studio-bass-api`. Problemet löstes genom att använda den relativa sökvägen `../.github/workflows/ci.yml`.
- GitHub PR #265 visar att minst två godkända granskningar från personer med skrivbehörighet krävs innan merge. Detta är ett krav i repositoryts inställningar och inte ett testfel.
- Vid granskningen av Copilots tidigare anmärkning om V26 hittades överflödig SQL-kod som försökte ta bort och återskapa foreign keys som redan definierades när tabellerna skapades. Denna kod hade tagits bort i commit `3b37c29f0`.
- Den äldre migrationen innehöll även en `RENAME COLUMN`-instruktion som förutsatte att kolumnen `track_ids` redan fanns. Den instruktionen finns inte längre i den nuvarande V26.
- Undersökningen visade att befintliga integrationstester inte uttryckligen verifierar Flyway-migrationen V26. `MixIT` använder Hibernate för att skapa databasschemat och har Flyway avstängt i testprofilen.
- En möjlig lösning är att skapa ett separat migrationstest som kör Flyway mot PostgreSQL och verifierar tabellerna och foreign key-kopplingarna. Detta har ännu inte implementerats.

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
- Lärt mig att Git-kommandot `git show` kan användas för att undersöka tidigare versioner av en fil och förstå varför en ändring gjordes.

## 5. Reflektion

Dagen har varit produktiv och ett viktigt steg framåt för MixerLab-arbetet. Jag har sammanfört ändringarna från test-branchen till feature-branchen och verifierat backend-implementationen lokalt.

Det var särskilt positivt att samtliga relevanta backend-tester passerade och att alla sju kontroller i GitHub Actions också blev godkända.

Den nya Pull Requesten #265 är nu öppen mot `dev`. Arbetet är fortfarande pågående eftersom frontend-integrationen och verifieringen av att sparade mixes finns kvar efter refresh och när applikationen öppnas igen återstår.

Under eftermiddagen började jag även undersöka Copilots tidigare kodgranskningsanmärkningar. Genom att jämföra äldre och nuvarande versioner av V26 kunde jag förstå varför den tidigare SQL-koden kunde orsaka migrationsproblem. Jag fick också en bättre förståelse för skillnaden mellan att testa databaspersistens med Hibernate och att faktiskt verifiera Flyway-migrationerna.

Undersökningen visade att det fortfarande saknas ett separat test som uttryckligen verifierar V26. Detta är något att ta vidare innan den första kodgranskningsanmärkningen kan betraktas som fullständigt verifierad.

Nästa steg är att invänta kodgranskning, fortsätta med frontend-integrationen och undersöka de återstående Copilot-anmärkningarna.

## Arbetstid

- 09:30–11:00 – Sprintplanering.
- 11:00–11:15 – Individuell handledning med Johan.
- 11:15–12:15 – Git-arbete, branchkontroller, merge och CI-konfiguration.
- 12:45–13:45 – Lunch.
- 13:45–pågående – Backend-verifiering, testkontroller, push till GitHub och skapande av Pull Request #265. Därefter undersökning av Copilots kodgranskningsanmärkningar, SQL-migration V26 och befintliga integrationstester.

**Arbetstid:** Sluttid återstår att dokumentera.

## Sammanfattning av dagen

Under dagen genomfördes en viktig del av arbetet med MixerLab-databaspersistens. Test-branchen mergades till feature-branchen och backend-verifieringen genomfördes med godkänt resultat.

Feature-branchen pushades till GitHub och en ny Pull Request, #265, skapades mot `dev`. Samtliga sju GitHub Actions-kontroller passerade.

Under eftermiddagen undersöktes även Copilots tidigare kodgranskningsanmärkningar. Den första anmärkningen gällde dubblerad hantering av foreign keys i V26-migrationen. Den problematiska SQL-koden hade redan tagits bort i en tidigare commit. Däremot upptäcktes att de befintliga integrationstesterna inte uttryckligen verifierar V26 genom Flyway.

Den nya Pull Requesten inväntar fortfarande kodgranskning. Nästa steg är att fortsätta undersöka Copilots återstående anmärkningar, komplettera migrationstestningen vid behov och fortsätta med frontend-integrationen.
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
