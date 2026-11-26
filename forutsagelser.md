# Förutsägelser: asynkron kod

Regler:

1. Titta på snutten i `snuttar/sNN.js` (eller på sidan `index.html`). **Kör den inte än.**
2. Skriv din gissning i tabellen: utskrifterna i den ordning du tror att de kommer, separerade med ` | `.
3. Kör snutten (knappen *Kör*). Skriv vad som faktiskt skrevs ut.
4. Hade du rätt? Skriv en regel med egna ord för varje snutt du gissade fel på.

Jag skriver pil (→) mellan utskrifterna istället för streck, annars går tabellen sönder i markdown.

| Snutt | Min gissning (i ordning) | Faktisk utskrift | Rätt? | Min regel om jag hade fel |
|---|---|---|---|---|
| 1 | 1: Beställer kaffe → 2: Läser tidningen → 3: Kaffet är klart | | | |
| 2 | A → C → B | | | |
| 3 | start → noll → kort → lång | | | |
| 4 | inuti promise → efter → then: klart | | | |
| 5 | sync → promise → timeout | | | |
| 6 | först? → 6 → 7 | | | |
| 7 | fel: slut på mjölk → städar | | | |
| 8 | beställer → sätter mig → bullen klar → kaffet klart | | | |
| 9 | väntar på båda → ["kaffe","bulle"] | | | |
| 10 | 1 → 6 → 3 → 2 → 4 → 5 | | | |
| 11 | c → ab | | | |

## Tre meningar med egna ord

1. JavaScript väntar inte på `setTimeout`, utan ...
2. En promise har tre tillstånd: ... 
3. `then` körs alltid ...
