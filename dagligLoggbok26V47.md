# Daglig loggbok – LIA

**Datum:** 16 november 2026  
**Veckodag:** Måndag  
**Arbetsplats:** Chain of Sounds

### 1. Arbetsuppgifter

Dagen började med ett sprintplaneringsmöte tillsammans med Johan Hellberg och Nisse Palm. Under mötet diskuterade vi vad som ska genomföras under den kommande veckan och vilka arbetsuppgifter som behöver prioriteras.

En del av planeringen handlade om att lägga till och göra om Mission-sidan. Vi diskuterade även att förklara tydligare vad Mission innebär och vilken funktion den har. MixerLab är en del av Mission, som i sin tur tillhör Akademin.

Efter mötet fortsatte jag med mitt pågående arbete med databaspersistens för MixerLab i Java-projektet `studio-bass-api`.

Jag fortsatte framför allt med att skriva enhetstester för `MixService` med JUnit 5 och Mockito.

Följande tester har nu implementerats och godkänts:

- `createMix_shouldSaveAndReturnMix()`
- `getUserMixes_shouldReturnUserMixes()`
- `updateMix_shouldUpdateExistingMix()`
- `deleteMix_shouldDeleteExistingMix()`

Jag körde samtliga tester i IntelliJ och kunde konstatera att alla fyra blev godkända.

### 2. Tekniker och verktyg

- Java
- Spring Boot
- JUnit 5
- Mockito
- IntelliJ IDEA
- MixService
- MixRepository
- MixResponseDto

### 3. Problem och lösningar

Under arbetet med testet `getUserMixes_shouldReturnUserMixes()` uppstod ett kompileringsfel.

Felet berodde på att metoden `getUserMixes()` returnerar en lista med `MixResponseDto` och inte en lista med `Mix`-objekt.

Jag anpassade därför testet så att det använde rätt returtyp. Jag ändrade även hur jag hämtade namnet från resultatet eftersom `MixResponseDto` är en Java-record och använder `name()`.

Efter ändringarna kunde testet köras korrekt.

### 4. Ny kunskap

Jag har fått mer förståelse för hur man testar tjänstelagret med JUnit 5 och Mockito.

Jag har bland annat arbetat vidare med:

- Hur man använder `@Mock` för att skapa mockade beroenden.
- Hur `when().thenReturn()` används för att styra vad en mockad metod ska returnera.
- Hur `verify()` används för att kontrollera att en metod faktiskt har anropats.
- Hur man delar upp tester i Arrange, Act och Assert.
- Skillnaden mellan en entitet (`Mix`) och en DTO (`MixResponseDto`).
- Hur man testar att en tjänst kan skapa, hämta, uppdatera och radera data.

### 5. Reflektion

Dagens arbete har gått bra. Jag har kunnat fortsätta med enhetstesterna för `MixService` och fått samtliga fyra grundläggande tester godkända.

Jag stötte på ett kompileringsfel när jag arbetade med `getUserMixes_shouldReturnUserMixes()`, men genom att kontrollera returtypen och anpassa testet kunde jag lösa problemet.

Nästa steg blir att skriva negativa tester för att kontrollera att en användare inte kan uppdatera eller radera en mix som tillhör någon annan.

### Arbetstid

| Tid         | Arbetsuppgift                                                                                                                                                        |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 09:30–10:48 | Sprintplaneringsmöte via videomöte med Johan Hellberg och Nisse Palm.                                                                                                |
| 10:48–12:00 | Fortsatt arbete med enhetstester för `MixService`.                                                                                                                   |
| 12:00–13:00 | Lunch.                                                                                                                                                               |
| 13:00–13:26 | Fortsatt arbete med `MixServiceTest`. Implementerade och körde testerna för att hämta, uppdatera och radera mixes. Samtliga fyra grundläggande tester blev godkända. |

**Effektiv arbetstid hittills:** 2 timmar och 56 minuter.

### Sammanfattning av dagen

Dagen har hittills bestått av ett sprintplaneringsmöte och fortsatt utvecklingsarbete med MixerLab-projektets databaspersistens i Java.

Jag har arbetat med enhetstester för `MixService` och fått fyra grundläggande tester godkända. Arbetet har gett mig bättre förståelse för hur JUnit 5 och Mockito kan användas för att testa tjänstelagret utan att behöva ansluta till databasen.

Nästa steg är att fortsätta med negativa tester för att säkerställa att användare endast kan hantera sina egna mixes.
