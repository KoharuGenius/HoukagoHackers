```
BOOT
 ↓
DEAL
 ↓
PLAYER_SELECT
 ↓
CPU_SELECT
 ↓
REVEAL
 ↓
JUDGEMENT
 ↓
LP_UPDATE
 ↓
NODE_UPDATE
 ↓
UI_UPDATE
 ↓
WIN_CHECK
```

状態定数

```
const GAME_PHASE = {
    BOOT: "BOOT",
    DEAL: "DEAL",
    PLAYER_SELECT: "PLAYER_SELECT",
    CPU_SELECT: "CPU_SELECT",
    REVEAL: "REVEAL",
    JUDGEMENT: "JUDGEMENT",
    LP_UPDATE: "LP_UPDATE",
    NODE_UPDATE: "NODE_UPDATE",
    UI_UPDATE: "UI_UPDATE",
    WIN_CHECK: "WIN_CHECK"
};
```

こんな感じで切り替える

```
function setPhase(phase) {
    switch (phase) {

        case GAME_PHASE.BOOT:
            showBootScreen();
            break;

        case GAME_PHASE.DEAL:
            hideBootScreen();
            dealCards();
            break;

        case GAME_PHASE.PLAYER_SELECT:
            showPlayerSelect();
            break;

        // ...
    }
```
}
