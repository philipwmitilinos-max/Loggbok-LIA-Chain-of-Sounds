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
