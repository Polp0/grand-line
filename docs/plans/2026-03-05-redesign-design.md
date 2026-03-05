# grand-line Redesign — Saga Map + Log Pose

**Goal**: Trasformare la filler guide da griglia Wordle generica a mappa tematica One Piece organizzata per saghe.

## Search Panel — "Log Pose"

- Input numerico dentro un **cerchio** con bordo dorato (#f4d35e) e glow — evoca il Log Pose
- Ago/freccia CSS punta verso il basso (pseudo-elemento triangolare)
- Micro-animazione rotazione ago al cambio episodio (CSS only)
- Sotto: result card (episodio corrente, next, skip-to-non-filler)
- Legenda colori compatta in basso
- CSS puro: `border-radius: 50%`, `box-shadow`, pseudo-elemento per ago

## Saga Map (scrollabile)

Ogni saga è una **card piena larghezza** con identità visiva tematica.

### Card structure

- `border-radius: 16px`, padding generoso
- Sfondo: gradiente tematico (opacity 0.15-0.25) sopra sfondo scuro
- **Header**: nome saga grassetto 18px + range episodi + count
- **Archi**: sotto-label per ogni arco con episodi raggruppati
- **Griglia episodi**: celle 28px colorate per tipo, raggruppate per arco con gap
- Filler arcs etichettati e opacizzati

### Tra le card

20px gap scuro + linea tratteggiata orizzontale sottile (la rotta).

## 11 Saghe

| # | Saga | Episodi | Gradiente | Archi principali |
|---|------|---------|-----------|------------------|
| 1 | East Blue | 1-61 | Azzurro mare → verde costa | Romance Dawn, Orange Town, Syrup Village, Baratie, Arlong Park, Loguetown, *Warship Island* |
| 2 | Alabasta | 62-135 | Oro sabbia → marrone caldo | Reverse Mountain, Whiskey Peak, Little Garden, Drum Island, Alabasta, *Post-Alabasta* |
| 3 | Sky Island | 136-206 | Bianco nuvola → celeste → oro | *Goat Island*, *Ruluka Island*, Jaya, Skypiea, *G-8* |
| 4 | Water 7 | 207-325 | Teal acqua → blu notte | Long Ring Long Land, *Ocean's Dream*, Water 7, Enies Lobby, Post-Enies Lobby |
| 5 | Thriller Bark | 326-384 | Viola scuro → nero nebbia | *Ice Hunter*, Thriller Bark, *Spa Island* |
| 6 | Summit War | 385-516 | Rosso fuoco → arancio → nero | Sabaody Archipelago, Amazon Lily, Impel Down, Marineford, Post-War |
| 7 | Fish-Man Island | 517-574 | Blu abisso → turchese bioluminescente | Return to Sabaody, Fish-Man Island |
| 8 | Dressrosa | 575-746 | Rosa → magenta → oro colosseo | *Z's Ambition*, Punk Hazard, *Caesar Retrieval*, Dressrosa |
| 9 | Whole Cake Island | 747-877 | Rosa pastello → crema → candy pink | *Silver Mine*, Zou, *Marine Rookie*, Whole Cake Island |
| 10 | Wano Country | 878-1085 | Rosso scuro → nero inchiostro → sakura | Levely, Wano Act 1-3 |
| 11 | Final Saga | 1086-1155 | Bianco → argento → neon azzurro | Egghead |

*Corsivo* = filler arc

## Colori episodi (invariati)

| Tipo | Hex |
|------|-----|
| Manga Canon | #2d6a4f |
| Filler | #c1121f |
| Mixed Canon/Filler | #e9c46a |
| Anime Canon | #219ebc |

## Interazione

- Input → mappa scrolla alla saga giusta, evidenzia cella
- Tap cella → aggiorna search panel
- Tap "next"/"skip" → naviga avanti
- Ago Log Pose ruota al cambio episodio

## Tech

- HTML/CSS/JS statico, zero dipendenze
- CSS puro per tutti gli effetti visivi (gradienti, glow, ago)
- Dati saghe/archi hardcodati in JS
- Nessuna immagine esterna
