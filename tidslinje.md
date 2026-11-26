# Tidslinje: rita vad som händer

Välj snutt 5 och snutt 10. Fyll i tabellerna steg för steg. Skriv vad som ligger i varje del **efter** varje steg.

Förklaring:

- **Anropsstacken** är den kod som körs just nu.
- **Microtask-kön** är promise-callbacks som väntar på att få köra.
- **Task-kön** är timer-callbacks och liknande som väntar på att få köra.
- Regel: när stacken är tom körs först ALLA microtasks, sedan EN task, sedan tomma microtasks igen, och så vidare.

## Snutt 5

| Steg | Vad händer | Stacken | Microtask-kön | Task-kön | Utskrift hittills |
|---|---|---|---|---|---|
| 1 | setTimeout anropas | setTimeout (startar timern, sedan klar) | tom | timeout-callbacken (väntar på 0 ms) | inget |
| 2 | Promise.resolve().then(...) anropas | Promise.resolve().then | promise-callbacken | timeout-callbacken | inget |
| 3 | console.log("sync") | console.log("sync") | promise-callbacken | timeout-callbacken | sync |
| 4 | Stacken tom: vad körs först? | promise-callbacken (microtask först) | tom | timeout-callbacken | sync, promise |
| 5 | Nästa | timeout-callbacken (nu får tasken köra) | tom | tom | sync, promise, timeout |

## Snutt 10

| Steg | Vad händer | Stacken | Microtask-kön | Task-kön | Utskrift hittills |
|---|---|---|---|---|---|
| 1 | console.log("1") | console.log("1") | tom | tom | 1 |
| 2 | första setTimeout | setTimeout | tom | task A (skriver 2) | 1 |
| 3 | Promise.resolve().then(3) | Promise.resolve().then | microtask 3 | task A | 1 |
| 4 | andra setTimeout | setTimeout | microtask 3 | task A, task B (skriver 4) | 1 |
| 5 | console.log("6") | console.log("6") | microtask 3 | task A, task B | 1, 6 |
| 6 | stacken tom, microtask-kön först | microtask 3 | tom | task A, task B | 1, 6, 3 |
| 7 | första tasken | task A (skriver 2) | tom | task B | 1, 6, 3, 2 |
| 8 | andra tasken | task B (skriver 4, lägger microtask 5 i kön) | microtask 5 | tom | 1, 6, 3, 2, 4 |
| 9 | microtask 5 | microtask 5 (skriver 5) | tom | tom | 1, 6, 3, 2, 4, 5 |

## Min förklaring i en mening

Varför kommer en promise-callback före en timer med 0 ms? För att när stacken är tom tömmer JavaScript hela microtask-kön först, och timerns callback ligger i task-kön och får vänta tills microtasks är slut.
