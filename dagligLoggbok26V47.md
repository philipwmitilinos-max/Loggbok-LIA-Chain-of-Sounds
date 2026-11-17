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
13:00–17:00: Fortsatt arbete med `MixServiceTest`. Förbättrade testklassens struktur med `@BeforeEach` och kommentarrubriker. Implementerade och godkände ytterligare två felhanteringstester för situationer där användaren inte hittas vid skapande eller hämtning av mixes.
15:38–16:41: Fortsatte med felhanteringstester för uppdatering och radering när användaren inte kan hittas. Båda testerna godkändes. Påbörjade tester för datarelationer och implementerade ett test med ArgumentCaptor för att kontrollera att en mix kopplas till rätt användare och att dess information följer med. Avslutade arbetet med en lyckad Git-commit.

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

Under mötet tog jag upp frågan om databasen och om det var okej att skapa en egen databas för utvecklingsarbetet med MixerLab. Clara lämnade mötet tidigare och jag fortsatte diskussionen med Johan. Han godkände att jag skapar en databas lokalt på min egen dator.

Johan påminde mig även om att det finns en checklista på Trello som beskriver vad vi ska uppnå med MixerLab-uppgiften.

Checklistan handlar om att ändra MixerLab så att användarnas mixes sparas i en databas i stället för i webbläsaren.

**User story:**

Som användare vill jag att mina mixes i MixerLab sparas ordentligt i en databas så att de inte försvinner när jag refreshar eller stänger ner webbsidan.

**Metod för att spara MixerLab:**

- Det ska gå att spara MixerLab-inställningar (backtracks, tracks och equalizer-volymer).
- Det ska gå att spara flera varianter med namn.
- Det ska gå att uppdatera en befintlig sparning.

**Acceptanskriterier:**

1. Användare ska kunna spara flera mixes.
2. Användare kan spara sina mixes i MixerLab.
3. Användarens mixes sparas i en databas.
4. Sparade mixes försvinner inte när sidan stängs ner eller uppdateras.
5. Alla funktioner i MixerLab är anpassade till förändringen.

### 2. Tekniker och verktyg

- Trello
- MixerLab
- Java
- Spring Boot
- PostgreSQL
- Databaspersistens

### 3. Problem och lösningar

Jag behövde stämma av med Johan om det var tillåtet att skapa en egen databas för utvecklingen av MixerLabs databaspersistens.

Efter diskussionen fick jag klartecken att skapa databasen lokalt på min dator.

Johan påminde mig också om Trello-checklistan, vilket gav mig en tydligare överblick över vilka krav och acceptanskriterier som ska uppfyllas.

### 4. Ny kunskap

Jag har fått en tydligare bild av hur projektets user story och acceptanskriterier ska användas för att styra utvecklingsarbetet.

Jag har även fått klartecken att arbeta med en lokal databas under utvecklingen av MixerLabs databaspersistens.

### 5. Reflektion

Dagens möte gav mig en bättre överblick över vad som förväntas av MixerLab-uppgiften.

Det känns bra att ha fått klartecken att skapa en lokal databas eftersom det ger mig möjlighet att fortsätta utveckla och testa databaspersistensen i min egen utvecklingsmiljö.

Jag kommer att använda Trello-checklistan som stöd för att kontrollera att implementationen uppfyller samtliga acceptanskriterier.

### Arbetstid

09:30–10:27 Videomöte om daglig sprint tillsammans med Johan Hellberg och Clara Westberg. Diskuterade bland annat databasen för MixerLab, fick klartecken att skapa en lokal databas och gick igenom Trello-checklistan för uppgiften.

**Effektiv arbetstid hittills:** 57 minuter.

### Sammanfattning av dagen

Dagen började med ett sprintmöte där vi diskuterade det fortsatta arbetet. Jag fick klartecken att skapa en lokal databas för MixerLab och blev påmind om Trello-checklistan med uppgiftens user story och acceptanskriterier.

Nästa steg blir att fortsätta utvecklingsarbetet med databaspersistensen och använda checklistan som stöd.
