# Förutsägelser: asynkron kod

Regler:

1. Titta på snutten i `snuttar/sNN.js` (eller på sidan `index.html`). **Kör den inte än.**
2. Skriv din gissning i tabellen: utskrifterna i den ordning du tror att de kommer, separerade med ` | `.
3. Kör snutten (knappen *Kör*). Skriv vad som faktiskt skrevs ut.
4. Hade du rätt? Skriv en regel med egna ord för varje snutt du gissade fel på.

Jag skriver pil (→) mellan utskrifterna istället för streck, annars går tabellen sönder i markdown. Jag körde snuttarna med node och fick samma ordning som på sidan. Enda skillnaden är att node skriver arrayen i snutt 9 som `[ 'kaffe', 'bulle' ]` medan sidan visar `["kaffe","bulle"]`.

| Snutt | Min gissning (i ordning) | Faktisk utskrift | Rätt? | Min regel om jag hade fel |
|---|---|---|---|---|
| 1 | 1: Beställer kaffe → 2: Läser tidningen → 3: Kaffet är klart | 1: Beställer kaffe → 2: Läser tidningen → 3: Kaffet är klart | Rätt | Ingen async här, allt körs uppifrån och ner. |
| 2 | A → C → B | A → C → B | Rätt | Även 0 ms läggs i kön och körs först när all vanlig kod är klar. |
| 3 | start → noll → kort → lång | start → noll → kort → lång | Rätt | Timers körs efter tid, inte efter var de står i filen. |
| 4 | inuti promise → efter → then: klart | inuti promise → efter → then: klart | Rätt | Koden i `new Promise(...)` körs direkt. Bara `then` väntar. |
| 5 | sync → promise → timeout | sync → promise → timeout | Rätt | Promise-callbacks (microtasks) körs före timers, även med 0 ms. |
| 6 | först? → 6 → 7 | först? → 6 → 7 | Rätt | Varje `then` är en egen microtask, och alla väntar tills den vanliga koden är slut. |
| 7 | fel: slut på mjölk → städar | fel: slut på mjölk → städar | Rätt | Ett reject hoppar över alla `then` tills det kommer en `catch`. `finally` körs sist oavsett. |
| 8 | beställer → sätter mig → bullen klar → kaffet klart | beställer → sätter mig → bullen klar → kaffet klart | Rätt | Båda timers startar samtidigt, så den kortaste blir klar först, inte den som beställdes först. |
| 9 | väntar på båda → ["kaffe","bulle"] | väntar på båda → ["kaffe","bulle"] | Rätt | `Promise.all` väntar på den långsammaste, och listan har samma ordning som in-listan, inte i den ordning de blev klara. |
| 10 | 1 → 6 → 3 → 2 → 4 → 5 | 1 → 6 → 3 → 2 → 4 → 5 | Rätt | Först sync, sedan microtasks, sedan en timer. En microtask som skapas inuti en timer (5) körs direkt när den timern är slut, före nästa timer. Här är 5 sist bara för att det inte finns fler timers. |
| 11 | c → ab | c → ab | Rätt | När ett `then` returnerar en promise väntar nästa `then` på den. Det blir `ab` först efter 50 ms. |

Jag hade rätt på alla elva. Jag la därför regeln i sista kolumnen som en kom-ihåg även när jag gissade rätt. Det jag var mest osäker på var snutt 10, om 5 skulle komma före 4 eller efter. Det blir efter, för 5 skapas inuti task 4 och körs först när den är klar.

## Tre meningar med egna ord

1. JavaScript väntar inte på `setTimeout`, utan lägger callbacken i en kö med en timer och går vidare med resten av koden. När tiden gått och stacken är tom körs callbacken.
2. En promise har tre tillstånd: pending (inte klar än), fulfilled (klar med ett värde) och rejected (misslyckades med ett fel).
3. `then` körs alltid senare, som en microtask, när den vanliga koden är slut och promisen är klar. Det gäller även om promisen redan var klar när jag skrev `then`.
