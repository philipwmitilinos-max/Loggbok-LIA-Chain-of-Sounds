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