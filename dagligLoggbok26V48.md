**Datum:** 23 november 2026

**Veckodag:** Måndag

**Arbetsplats:** Chain of Sounds

### 1. Arbetsuppgifter

**09:30–11:00 – Sprintplaneringsmöte**

Dagen började med ett sprintplaneringsmöte inför den kommande veckan.

Under mötet tog Nisse upp att Chain of Sounds är ett nystartat företag och att företagets intäkter kommer från prenumerationer.

Jag tog även upp frågan med Johan om möjligheten att köra GitHub Actions-workflows manuellt för att kunna testa projektets CI utan att behöva ändra PR:ens målbranch.

Flera andra ämnen diskuterades under mötet, men jag har inte dokumenterat samtliga punkter.

Vi planerade även en genomgång av MixerLab på onsdag den 25 november kl. 10:00.

**11:00–11:15 – Handledning med Johan**

Under handledningen diskuterade vi den nuvarande Git-branchstrukturen för MixerLab-arbetet.

Johan föreslog att jag skulle merga test-branchen till feature-branchen och därefter skapa en ny pull request.

Detta är ett förslag för den fortsatta hanteringen av arbetet och behöver genomföras enligt den överenskomna arbetsgången.

**11:15– – Fortsatt arbete**

Dokumenteras senare.

### 2. Möten och planering

- Sprintplaneringsmöte kl. 09:30–11:00.
- Handledning med Johan kl. 11:00–11:15.
- Genomgång av MixerLab planerad till onsdag 25 november kl. 10:00.

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
