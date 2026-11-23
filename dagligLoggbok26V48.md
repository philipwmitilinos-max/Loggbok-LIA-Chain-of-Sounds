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
- Pushade feature-branchen till GitHub.
- Skapade en ny Pull Request, #265, från feature-branchen till `dev`.
- Kontrollerade GitHub Actions-resultatet. Samtliga sju automatiserade kontroller passerade.

## 2. Tekniker och verktyg

- Git och GitHub.
- Git-brancher, merge och commits.
- Pull Requests och kodgranskning.
- GitHub Actions.
- Java och Spring Boot.
- Maven Wrapper.
- JUnit 5 och Mockito.
- Testcontainers och PostgreSQL.
- Flyway.
- CI/CD och automatiserade tester.

## 3. Problem och lösningar

- Den tidigare Pull Requesten #261 behövde ersättas enligt Johans förslag. Den stängdes och en ny PR skapades efter att test-branchen mergats till feature-branchen.
- Feature-branchen innehöll nio lokala commits som inte hade pushats till GitHub. Dessa behölls och kombinerades med ändringarna från test-branchen.
- Mergen genomfördes automatiskt utan konflikter.
- Maven kunde inte köras direkt via `mvn`, men projektet innehöll Maven Wrapper. Därför användes `mvnw.cmd` för att genomföra verifieringen.
- GitHub PR #265 visar att minst två godkända granskningar från personer med skrivbehörighet krävs innan merge. Detta är ett krav i repositoryts inställningar, inte ett testfel.

## 4. Ny kunskap

- Fördjupad förståelse för hur man jämför brancher och kontrollerar skillnader mellan commits.
- Hur man genomför en merge utan att omedelbart skapa en merge-commit med `--no-commit --no-ff`.
- Hur Maven Wrapper kan användas för att bygga och testa ett projekt utan en separat Maven-installation.
- Hur GitHub Actions automatiskt verifierar flera delar av ett projekt genom CI.
- Förståelse för hur granskningskrav och behörigheter påverkar möjligheten att merga en Pull Request.

## 5. Reflektion

Dagen har varit produktiv och ett viktigt steg framåt för MixerLab-arbetet. Jag har sammanfört ändringarna från test-branchen till feature-branchen och verifierat backend-implementationen lokalt.

Det var särskilt positivt att samtliga relevanta backend-tester passerade och att alla sju kontroller i GitHub Actions också blev godkända.

Den nya Pull Requesten #265 är nu öppen mot `dev`. Arbetet är fortfarande pågående eftersom frontend-integrationen och verifieringen av att sparade mixes finns kvar efter refresh och när applikationen öppnas igen återstår.

## Arbetstid

- 09:30–11:00 – Sprintplanering.
- 11:00–11:15 – Individuell handledning med Johan.
- 11:15–12:45 – Git-arbete, merge, backend-verifiering och skapande av ny Pull Request.

**Arbetstid:** Sluttid och lunchrast återstår att dokumentera.

## Sammanfattning av dagen

Under dagen genomfördes en viktig del av arbetet med MixerLab-databaspersistens. Test-branchen mergades till feature-branchen och backend-verifieringen genomfördes med godkänt resultat.

Feature-branchen pushades till GitHub och en ny Pull Request, #265, skapades mot `dev`. Samtliga sju GitHub Actions-kontroller passerade.

Nästa steg är att invänta kodgranskning och fortsätta med frontend-integrationen.

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
