# Veckologgbok – Vecka 46

**Period:** 9–13 november 2026  
**Arbetsplats:** Chain Of Sounds  
**Vecka:** Första LIA-veckan

## 1. Sammanfattning av veckans arbetsuppgifter

Under min första vecka på LIA hos Chain Of Sounds har jag arbetat med en uppgift som handlar om att ändra MixerLab så att användarnas mixes sparas i en databas i stället för i webbläsarens `localStorage`.

Veckan började med att jag bekantade mig med projektets struktur och undersökte hur frontend, backend och databasen kommunicerar med varandra. Jag undersökte framför allt hur MixerLab hanterar backtracks, tracks och användarnas inställningar samt hur befintlig data sparas.

Under veckan hade jag flera möten med Johan Hellberg där vi gick igenom uppgiftens krav och diskuterade hur implementationen skulle genomföras.

Inledningsvis undersökte jag den befintliga lösningen i C# och .NET 10. Jag påbörjade även en domänklass för Mix i Mixer.Domain. Efter ett möte med Johan den 11 november beslutade vi dock att all användardata ska hanteras i Java i stället för C#. Därför flyttades implementationen till `studio-bass-api`.

Jag skapade en ny Git-branch för Java-implementationen och började bygga upp de klasser som behövs för att hantera sparade mixes.

Arbetet under veckan omfattade bland annat:

- Undersöka MixerLabs befintliga funktionalitet och projektets arkitektur.
- Undersöka hur användare, backtracks, tracks och andra entiteter representeras i databasen.
- Skapa Java-entiteten `Mix` och klassen `MixEq`.
- Skapa `MixRepository` för databasoperationer.
- Skapa `MixRequestDto` och `MixResponseDto`.
- Implementera `MixService` för att skapa, hämta, uppdatera och ta bort mixes.
- Implementera `MixController` med tillhörande API-endpoints.
- Konfigurera Lombok och Maven.
- Installera och konfigurera Infisical CLI.
- Starta PostgreSQL och Java-tjänsten med Docker Compose.
- Arbeta med Flyway-migrationen V26.
- Verifiera backend och API-endpoints genom Swagger.
- Påbörja enhetstester för `MixService` med JUnit 5 och Mockito.

Mot slutet av veckan hade jag färdigställt den grundläggande backendfunktionaliteten för mixhanteringen och påbörjat arbetet med enhetstester.

## 2. Möten och avstämningar

Under veckan hade jag flera möten som hjälpte mig att förstå uppgiften och planera arbetet.

- **Måndag:** Inledande videomöte där vi gick igenom LIA-uppgiften och arbetet.
- **Tisdag:** Videomöte med Johan Hellberg där vi gick igenom kraven för den nya sparfunktionen.
- **Onsdag:** Möte med Johan Hellberg där vi beslutade att flytta implementationen från C# till Java och använda `studio-bass-api`.
- **Torsdag:** Uppföljningsmöte med Johan Hellberg om Java-implementationen och databasanslutningen. Jag fick även instruktioner om att installera Infisical CLI.
- **Fredag:** Kort videomöte med Clara Westberg samt veckouppföljning med Nisse Palm där jag redogjorde för vad jag hade arbetat med under veckan.

## 3. Tekniker och verktyg

Under veckan har jag arbetat med och fått erfarenhet av följande tekniker och verktyg:

- Java och Spring Boot.
- C# och .NET 10.
- React och TypeScript.
- Spring Data JPA och Hibernate.
- Entity Framework Core.
- PostgreSQL.
- Flyway.
- Maven och Lombok.
- Docker och Docker Compose.
- Infisical CLI.
- Swagger.
- JUnit 5 och Mockito.
- IntelliJ IDEA och Visual Studio Code.
- Git och GitHub.

## 4. Utmaningar och problemlösning

En av de största utmaningarna under veckan var att sätta mig in i ett befintligt projekt med flera olika tekniker och lager. Jag behövde undersöka hur frontend, backend och databasen fungerar tillsammans innan jag kunde börja implementera den nya funktionen.

En annan viktig förändring var att implementationen först påbörjades i C# men senare flyttades till Java efter diskussion med Johan Hellberg. Jag behövde därför anpassa arbetet och sätta mig in i hur projektets befintliga Java-backend var uppbyggd.

Jag stötte även på problem med Maven och Lombok. Vid kompileringen saknades flera getters och setters. Genom att uppdatera Lombok-versionen och konfigurera Maven Compiler Plugin kunde jag lösa problemet.

Databasanslutningen var ytterligare en utmaning. Jag saknade först de miljövariabler och databasuppgifter som behövdes för att starta Java-tjänsten. Efter att ha installerat Infisical CLI kunde jag använda projektets konfiguration och starta PostgreSQL och Java-tjänsten med Docker Compose.

Det största problemet mot slutet av veckan var Flyway-migrationen V26. Tabellen `mixes` existerade redan i databasen eftersom Hibernate tidigare hade skapat den när `ddl-auto=update` var aktiverat.

Jag undersökte de befintliga tabellerna och deras relationer och anpassade därefter migrationen. Genom att använda bland annat `CREATE TABLE IF NOT EXISTS` kunde jag få migrationen att genomföras.

Jag fick även ett problem med det första enhetstestet för `MixService`, där jag hade använt `@Mock` i stället för `@InjectMocks`. Efter att ha ändrat annoteringen kunde jag testa den riktiga servicelogiken med Mockito.

## 5. Nya kunskaper och erfarenheter

Under min första LIA-vecka har jag fått praktisk erfarenhet av att arbeta i ett större befintligt projekt och utveckla en ny funktion som behöver integreras med projektets befintliga arkitektur.

Jag har framför allt lärt mig mer om:

- Hur Spring Boot-applikationer är uppbyggda med entiteter, repositories, services och controllers.
- Hur JPA används för att representera databastabeller och relationer mellan entiteter.
- Hur `@ManyToOne`, `@Embedded` och `@ElementCollection` används i JPA.
- Hur DTO-klasser används för att överföra information mellan frontend och backend.
- Hur Hibernate och Flyway samverkar vid hantering av databasschemat.
- Hur Docker Compose används för att starta flera tjänster tillsammans.
- Hur Infisical CLI används för att hantera miljövariabler och hemligheter.
- Hur Lombok konfigureras tillsammans med Maven.
- Hur Swagger används för att kontrollera API-endpoints.
- Hur JUnit 5 och Mockito används för att skriva isolerade enhetstester.

Jag har även fått bättre förståelse för hur Git-branches kan användas för att separera olika delar av utvecklingsarbetet.

## 6. Reflektion och nästa steg

Min första vecka på LIA har varit lärorik och innehållit många nya utmaningar. Jag har fått sätta mig in i ett större projekt, förstå dess arkitektur och anpassa mig till projektets arbetssätt och tekniker.

En viktig del av veckan var beslutet att flytta implementationen från C# till Java. Det innebar att jag behövde ändra min ursprungliga plan och börja arbeta med `studio-bass-api`. Jag tycker att jag har kommit långt under veckan eftersom jag lyckats bygga upp de grundläggande Java-klasserna och implementera funktionaliteten för att hantera sparade mixes.

Jag har även fått arbeta praktiskt med databaskonfiguration, Docker, Infisical och Flyway. Det tog tid att lösa problemen med utvecklingsmiljön och databasmigrationen, men jag har fått en bättre förståelse för hur dessa delar fungerar tillsammans.

Mot slutet av veckan kunde jag verifiera att backend startade korrekt och att de nya API-endpointsen fanns tillgängliga i Swagger. Jag har även kommit igång med enhetstesterna för `MixService`.

### Nästa veckas mål

Under nästa vecka planerar jag att:

- Fortsätta utveckla och komplettera enhetstesterna för `MixService`.
- Testa funktionerna för att uppdatera och ta bort mixes.
- Kontrollera att användare endast kan hantera sina egna mixes.
- Fortsätta arbetet med controller-tester.
- Undersöka hur frontend ska kommunicera med de nya Java-endpointsen.
- Påbörja anpassningen av MixerLab i frontend så att mixes kan sparas och hämtas från databasen.
- Fortsätta följa projektets kodstandard och arbetssätt.

--

# Veckovis LIA-loggbok

**Vecka:** 47  
**Datum:** 16–20 november 2026  
**Arbetsplats:** Chain of Sounds

### 1. Sammanfattning av veckan

Veckan har främst handlat om att fortsätta utvecklingen av databaspersistensen för MixerLab i Java-projektet `studio-bass-api`.

Veckan började med ett sprintplaneringsmöte tillsammans med Johan Hellberg och Nisse Palm. Vi gick igenom veckans prioriteringar och arbetet med MixerLab. Jag fick även klartecken från Johan att skapa en lokal databas för utvecklingsarbetet.

Under måndagen fortsatte jag utveckla enhetstesterna för `MixService` med JUnit 5 och Mockito. Jag arbetade med CRUD-operationer, felhantering och användarrelationer. Jag förbättrade även testklassens struktur med `@BeforeEach` och `ArgumentCaptor`.

På tisdagen slutförde jag samtliga 15 tester i `MixServiceTest`. Alla tester blev godkända. Därefter började jag arbeta med integrationstester och skapade `MixIT.java`. Mitt första integrationstest, som kontrollerar att en mix sparas i PostgreSQL, gick igenom utan fel.

Under onsdagen fortsatte jag med integrationstesterna. Jag fick fem tester godkända som kontrollerar användarrelationer, EQ-inställningar, track-ID:n och hämtning av användarens mixar. Jag stötte samtidigt på problem med uppdateringsfunktionen och undersökte två misslyckade tester i GitHub Actions kopplade till Flyway-migrationen V26.

Torsdagen var en föranmäld frånvarodag.

På fredagen gick jag igenom Copilots kodgranskning av PR #261. Jag identifierade sex kvarstående förbättringsområden inom bland annat HTTP-tester, autentisering, validering och felhantering. Jag undersökte även databasmigrationerna och Git-branchernas historik.

Jag uppdaterade CI-konfigurationen med `workflow_dispatch` för att möjliggöra manuell körning. Ändringen committades och pushades till test-branchen. Därefter försökte jag starta CI manuellt, men saknade förutsättningarna för att göra detta. Jag försökte även köra Java Backend lokalt, men PowerShell kunde inte hitta Maven.

### 2. Tekniska kunskaper

Under veckan har jag arbetat med och fördjupat mina kunskaper inom:

- Java och Spring Boot.
- JUnit 5 och Mockito för enhetstester.
- Testcontainers och PostgreSQL för integrationstester.
- Spring Data JPA och Hibernate.
- JPA-relationer mellan entiteter.
- `@BeforeEach`, `ArgumentCaptor` och `@Transactional`.
- Flyway och databasmigrationer.
- Git, branches, commits och pull requests.
- GitHub Actions och CI-konfiguration.
- YAML och PowerShell.

Jag har framför allt fått bättre förståelse för skillnaden mellan enhetstester och integrationstester samt hur Hibernate hanterar relationer och databastransaktioner.

### 3. Utmaningar och problemlösning

En utmaning under veckan var att få integrationstesterna att fungera korrekt med Hibernate.

Jag stötte bland annat på `LazyInitializationException` när track-ID:n skulle hämtas. Detta löstes genom att använda `@Transactional` på testmetoden.

Vid testningen av uppdateringsfunktionen uppstod även ett `UnsupportedOperationException` när Hibernate försökte ändra en oföränderlig lista. Detta problem återstår att lösa.

En annan större utmaning var felen i GitHub Actions. Jag undersökte testloggarna och kunde identifiera att två tester i `FlywaySchemaReconciliationIT` misslyckades på grund av V26-migrationen. Problemet åtgärdades därefter i test-branchen och markerades som löst i Copilots kodgranskning.

Jag undersökte även varför GitHub Actions inte startade automatiskt för PR #261. Jag lade till `workflow_dispatch`, men kunde inte starta en manuell körning eftersom ytterligare förutsättningar på repositoryts default branch krävs.

Som alternativ försökte jag köra Java Backend lokalt. Det gick inte eftersom Maven inte kunde hittas av PowerShell. Detta behöver undersökas vidare.

### 4. Personlig utveckling

Den här veckan har jag fått arbeta mer självständigt med testning och felsökning i ett befintligt projekt.

Jag har blivit bättre på att läsa testresultat och loggar för att identifiera var problem uppstår. Jag har även fått mer erfarenhet av att arbeta med både enhetstester och integrationstester.

Arbetet med Copilots kodgranskning har hjälpt mig att se implementationen ur ett bredare perspektiv. Jag har fått en tydligare bild av varför det är viktigt att inte bara kontrollera att funktionaliteten fungerar, utan även att tänka på säkerhet, felhantering, prestanda och testbarhet.

Jag har också fått mer erfarenhet av Git-branchhantering och hur ändringar kan publiceras utan att påverka andra delar av projektets arbetsflöde.

### 5. Reflektion och nästa vecka

Jag tycker att veckan har varit lärorik och att jag har kommit vidare med MixerLabs databaspersistens.

En viktig milstolpe var att slutföra de 15 enhetstesterna och därefter komma igång med integrationstesterna mot PostgreSQL. Jag har också fått en bättre förståelse för hur olika delar av backend-systemet samverkar.

Det finns fortfarande några problem att lösa innan arbetet kan betraktas som färdigt. Framför allt behöver jag fortsätta med uppdateringsfunktionen, förbättra testerna enligt Copilots granskning och säkerställa att databasmigrationerna fungerar korrekt.

**Fokus inför nästa vecka:**

- Lösa problemet med uppdatering av track-ID:n.
- Fortsätta utveckla och förbättra integrationstesterna.
- Åtgärda de sex kvarstående förbättringsområdena från Copilots kodgranskning.
- Verifiera V26-migrationen genom en ny CI-körning.
- Få Maven att fungera lokalt så att Java Backend kan testas.
- Fortsätta arbetet enligt den planerade Git-branchstrukturen.

**Sammanfattningsvis** har veckan inneburit stora framsteg inom testning och databaspersistens, men även en del felsökning kring Hibernate, Flyway och CI. Jag har fått en bättre förståelse för hur man utvecklar, testar och kvalitetssäkrar funktionalitet i ett större Java-projekt.

--

# Veckovis LIA-loggbok

**Vecka:** 48  
**Datum:** 23–27 november 2026  
**Arbetsplats:** Chain of Sounds

## 1. Sammanfattning av veckan

Veckan har främst handlat om att kvalitetssäkra backend-implementationen för MixerLabs databaspersistens i Java-projektet `studio-bass-api` och att påbörja undersökningen inför frontend-integrationen.

Veckan inleddes med sprintplanering tillsammans med teamet och individuell handledning med Johan Hellberg. Vi diskuterade bland annat GitHub Actions och hur arbetet med test-branchen och feature-branchen skulle hanteras. Efter Johans förslag mergades test-branchen in i feature-branchen. Den tidigare Pull Requesten #261 stängdes och en ny Pull Request, #265, skapades mot `dev`.

Under måndagen genomförde jag en omfattande backend-verifiering med Maven Wrapper. `MixServiceTest`, `MixIT` och `FlywaySchemaReconciliationIT` passerade. Hela bygget avslutades med `BUILD SUCCESS`, och samtliga sju automatiserade kontroller i GitHub Actions passerade.

Jag fortsatte även granskningen av Copilots tidigare kodgranskningsanmärkningar. Jag undersökte framför allt Flyway-migrationen V26 och upptäckte att de befintliga integrationstesterna inte uttryckligen verifierade denna migration. Därför skapade jag `MixFlywayMigrationIT`, som använder Testcontainers och PostgreSQL för att kontrollera att migrationen skapar tabellerna `mixes` och `mix_tracks` samt deras foreign keys.

Under måndagen och tisdagen arbetade jag även med Code Quality Bots kommentarer. Jag gjorde ändringar i `Mix`-entitetens `toString()`-hantering genom att använda `@ToString` och `@ToString.Exclude`. Ändringarna verifierades med Maven och publicerades senare tillsammans med övriga teständringar.

På tisdagen fortsatte jag med testningen av `MixController`. Jag skapade `MixControllerTest` och implementerade tester för controllerns fyra HTTP-endpoints:

- Skapa en mix.
- Hämta användarens mixes.
- Uppdatera en mix.
- Ta bort en mix.

Samtliga fyra tester passerade. Jag körde därefter `.\mvnw.cmd clean verify`, vilket avslutades med `BUILD SUCCESS`. Jag fortsatte också granskningen av Copilots nya kodgranskningspunkter, framför allt kring JWT-subject, användaridentifiering och felhantering.

Under onsdagen arbetade jag med att skapa en generell testklass för `UserService`. Jag skapade `UserServiceTest.java` med mockade beroenden och implementerade 13 tester som täcker användarhantering, medlemskap, Free Trial och uppdatering av användarroller.

Jag fortsatte även med MixerLab-relaterad testning. I `MixService` ersatte jag generella `RuntimeException` med `MixNotFoundException` när en mix inte hittas. Jag kompletterade även `MixControllerTest` med tester för HTTP 400 och HTTP 404. Samtliga 15 tester i `MixServiceTest` och samtliga 8 tester i `MixControllerTest` passerade.

På torsdagen fortsatte jag med integrationstester för användarägarskap. Jag lade till tester som verifierar att en användare inte kan uppdatera eller ta bort en annan användares mix. Jag löste även ett `LazyInitializationException` genom att använda `@Transactional` på berörda testmetoder.

Jag arbetade också vidare med valideringen av JWT-subject i `UserService`. Med hjälp av `StringUtils.hasText(subject)` lade jag till kontroll av null, tomma strängar och strängar som endast innehåller blanksteg. Tre nya tester lades till och `UserServiceTest` genomfördes med 16 godkända tester.

Under torsdagens eftermiddag påbörjade jag undersökningen av frontend-integrationen för MixerLab. Jag granskade bland annat `MixerLab.tsx`, `mixer.client.ts`, `mixer.types.ts`, `eqTypes.ts` och `useApiClient.ts`. Jag identifierade hur den nuvarande lösningen sparar en mix i `localStorage` och vilka förändringar som behövs för att i stället kunna spara flera namngivna mixes i Java-backenden.

På fredagen deltog jag i en muntlig LIA-redovisning på IT-Högskolan mellan kl. 09:00 och 11:00. Jag berättade kort om min praktik hos Chain of Sounds och vad jag har arbetat med under LIA-perioden.

Efter redovisningen utfördes inget ytterligare utvecklingsarbete eftersom jag kände mig under vädret. Arbetet med frontend-integrationen pausades därför till nästa arbetstillfälle.

## 2. Tekniska kunskaper

Under veckan har jag arbetat med och fördjupat mina kunskaper inom:

- Java och Spring Boot.
- Spring Data JPA och Hibernate.
- PostgreSQL och Testcontainers.
- Flyway och SQL-migrationer.
- JUnit 5, Mockito och AssertJ.
- MockMvc och HTTP-tester.
- Spring MVC och Bean Validation.
- Spring Security och JWT.
- `StringUtils.hasText()` för validering.
- `@Transactional` och Hibernate lazy loading.
- Lombok, `@ToString` och `@ToString.Exclude`.
- Maven Wrapper och `clean verify`.
- Git, branches, commits och Pull Requests.
- GitHub Actions och CI/CD.
- Copilot Code Review och Code Quality Bot.
- React och TypeScript.
- React Hooks, bland annat `useEffect` och state.
- Auth0 och autentiserad API-kommunikation.
- `localStorage` och frontendens API-klientstruktur.
- IntelliJ IDEA och PowerShell.

Jag har framför allt fördjupat mina kunskaper inom testning av backend-funktionalitet, databasmigrationer, användarägarskap och säker hantering av användaridentitet.

Jag har även fått en bättre förståelse för hur frontendens befintliga API-klient kan byggas ut för att kommunicera med Java-backendens nya mix-endpoints.

## 3. Utmaningar och problemlösning

En viktig del av veckan var att hantera tidigare kodgranskningsanmärkningar och kvalitetssäkra backend-implementationen.

### Flyway-migration och databastester

Jag upptäckte att de befintliga integrationstesterna inte uttryckligen verifierade Flyway-migrationen V26. `MixIT` använde Hibernate för att skapa databasschemat och hade Flyway avstängt i testprofilen.

**Lösning:**

Skapade `MixFlywayMigrationIT` med Testcontainers och PostgreSQL. Testet verifierar att V26 skapar tabellerna `mixes` och `mix_tracks` samt deras foreign keys.

Testet genomfördes framgångsrikt med ett test, noll fel och noll misslyckade tester.

### Problem med Hibernate och LazyInitializationException

Vid integrationstesterna för användarägarskap uppstod ett `LazyInitializationException` när testet försökte komma åt `trackIds` efter att Hibernate-sessionen hade stängts.

**Lösning:**

Lade till `@Transactional` på berörda testmetoder. Därefter kunde testerna genomföras.

### Testning av användarägarskap

Det behövde verifieras att användare inte kan uppdatera eller ta bort andra användares mixes.

**Lösning:**

Lade till integrationstester som kontrollerar att användarägarskapet respekteras vid uppdatering och borttagning av mixes.

### Validering av JWT-subject

Vid granskningen av `UserService` upptäckte jag att metoden `getOrCreateUserBySubject()` saknade en uttrycklig kontroll av om subject är null, tomt eller endast innehåller blanksteg.

**Lösning:**

Införde validering med `StringUtils.hasText(subject)` och lade till tre tester för att verifiera hanteringen av ogiltiga subject-värden.

### Testning av UserService

Vid arbetet med `UserServiceTest` uppstod problem med testdata för Free Trial och betalda medlemskap.

**Lösning:**

Undersökte den bakomliggande affärslogiken i `UserService` och `User`. Jag upptäckte att testdatan behövde innehålla rätt roller, betalningsdatum och ett framtida utgångsdatum för att motsvara villkoren i applikationen.

Efter att testdatan kompletterats fungerade testerna som förväntat.

### Felhantering i MixService

Vid granskningen upptäckte jag att `updateMix()` och `deleteMix()` använde generella `RuntimeException` när en mix inte hittades.

**Lösning:**

Ändrade metoderna så att de använder `MixNotFoundException`. Uppdaterade även tillhörande tester för att kontrollera att rätt undantag kastas.

### HTTP-validering i MixController

De tidigare controller-testerna saknade tester för vissa felaktiga requests.

**Lösning:**

Kompletterade `MixControllerTest` med tester för:

- HTTP 404 när en mix inte hittas vid uppdatering eller borttagning.
- HTTP 400 när en mix skapas med ett tomt namn.
- HTTP 400 när en mix uppdateras med ett tomt namn.

Kontrollerade även att `MixService` inte anropas när valideringen misslyckas.

### Undersökning av frontend-integrationen

Vid granskningen av frontend upptäckte jag att MixerLab fortfarande använder `localStorage` för att spara en enda mix. Den nuvarande datastrukturen saknar även ett namn och ett databas-ID.

Backendens `MixRequestDto` kräver ett namn, och frontendens EQ-struktur skiljer sig från backendens `MixEq`.

**Planerad lösning:**

Bygga vidare på den befintliga Mixer API-klienten och `useApiClient()` för att skapa autentiserad kommunikation med Java-backenden. Frontendens TypeScript-typer behöver också anpassas till backendens DTO:er.

Inga frontend-ändringar genomfördes under veckan. Arbetet bestod av undersökning och planering.

### Sjukdomskänsla under fredagen

Efter den muntliga LIA-redovisningen på fredagen kände jag mig under vädret och kunde därför inte fortsätta med det planerade utvecklingsarbetet.

**Hantering:**

Valde att avstå från fortsatt programmering under resten av dagen. Frontend-integrationen skjuts upp till nästa arbetstillfälle.

## 4. Personlig utveckling

Den här veckan har jag fått arbeta mer självständigt med kvalitetssäkring och förbättringar av backend-implementationen.

Jag har fått mer erfarenhet av att analysera kodgranskningsanmärkningar och undersöka den bakomliggande orsaken till problem innan jag genomför förändringar.

Arbetet med `UserServiceTest` har gett mig bättre förståelse för hur man testar affärslogik och hur viktigt det är att testdata motsvarar de faktiska villkoren i applikationen.

Jag har även utvecklat min förståelse för integrationstester och hur Hibernate hanterar relationer och transaktioner.

En annan viktig erfarenhet har varit att arbeta med säkerhet och användarägarskap. Jag har fått en tydligare bild av varför det är viktigt att kontrollera användarens identitet innan en operation genomförs.

Under veckan har jag också fått bättre förståelse för hur olika delar av frontendprojektet är organiserade och hur befintliga API-klienter används för autentiserad kommunikation.

Fredagens muntliga LIA-redovisning gav mig dessutom erfarenhet av att sammanfatta och presentera mitt arbete inför klassen.

## 5. Reflektion och nästa vecka

Jag tycker att veckan har varit produktiv och att jag har gjort flera viktiga framsteg med MixerLabs databaspersistens.

En viktig milstolpe var att få backendens tester och verifiering att fungera samtidigt som jag fortsatte förbättra felhantering, användarägarskap och validering.

Jag har även arbetat vidare med Copilots och Code Quality Bots kodgranskningsanmärkningar. Genom att undersöka problemen och komplettera testerna har jag fått en bättre förståelse för hur viktigt det är att kvalitetssäkra både funktionalitet och säkerhet.

Under torsdagen började jag undersöka frontend-integrationen. Jag har nu en bättre bild av hur MixerLab fungerar och vilka delar som behöver anpassas för att stödja flera sparade mixes med egna namn.

Fredagens redovisning genomfördes som planerat, men på grund av sjukdomskänsla kunde jag inte fortsätta med utvecklingsarbetet efteråt.

### Fokus inför nästa vecka

Under nästa vecka planerar jag att:

- Fortsätta frontend-integrationen för MixerLab.
- Utöka TypeScript-typerna i `mixer.types.ts` så att de motsvarar backendens DTO:er.
- Lägga till API-metoder för att skapa, hämta, uppdatera och ta bort mixes i `mixer.client.ts`.
- Använda den befintliga autentiserade API-lösningen genom `useApiClient()`.
- Anpassa EQ-värdena i frontend så att de kan skickas till och hämtas från backendens `MixEq`.
- Implementera stöd för flera sparade mixes med egna namn.
- Undersöka hur användaren ska kunna välja och återställa en tidigare sparad mix.
- Fortsätta kontrollera att frontendens backtrack-ID:n motsvarar de ID:n som backend förväntar sig.
- Testa integrationen mellan frontend och Java-backenden.
- Fortsätta följa upp eventuella kvarstående kodgranskningsanmärkningar.

**Sammanfattningsvis** har veckan inneburit stora framsteg inom backendens testning, säkerhet och felhantering. Jag har även påbörjat förberedelserna för nästa större steg i arbetet: att koppla MixerLab i frontend till Java-backendens databaslösning.

--
