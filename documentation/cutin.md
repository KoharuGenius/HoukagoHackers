**プレイ画面を表示した直後に中央からカットインが出て、少し表示して消える**

まずは、外部ライブラリなしのシンプルなサンプルです。

### HTML

```html
<div id="battleCutin" class="battle-cutin">
  <div class="cutin-line"></div>
  <div class="cutin-text">BATTLE START</div>
  <div class="cutin-line"></div>
</div>
```

### CSS

```css
.battle-cutin {
  position: fixed;
  inset: 0;
  z-index: 1000;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  pointer-events: none;

  opacity: 0;
  visibility: hidden;
}

.battle-cutin.active {
  visibility: visible;
  animation: cutinFade 1.8s ease forwards;
}

.cutin-text {
  font-family: monospace;
  font-size: 2.4rem;
  font-weight: bold;
  letter-spacing: 0.18em;

  text-shadow:
    0 0 5px currentColor,
    0 0 15px currentColor;

  transform: scaleX(1.4);
  animation: textExpand 0.35s ease-out;
}

.cutin-line {
  width: 70%;
  max-width: 500px;
  height: 1px;
  margin: 12px 0;

  background: currentColor;
  box-shadow: 0 0 8px currentColor;

  transform: scaleX(0);
}

.active .cutin-line {
  animation: lineExpand 0.3s ease-out forwards;
}

@keyframes lineExpand {
  to {
    transform: scaleX(1);
  }
}

@keyframes textExpand {
  from {
    opacity: 0;
    transform: scaleX(2.5);
  }

  to {
    opacity: 1;
    transform: scaleX(1.4);
  }
}

@keyframes cutinFade {
  0% {
    opacity: 0;
  }

  15% {
    opacity: 1;
  }

  70% {
    opacity: 1;
  }

  100% {
    opacity: 0;
  }
}
```

### JavaScript

```javascript
function showBattleCutin(text = "BATTLE START") {
  const cutin = document.getElementById("battleCutin");
  const textElement = cutin.querySelector(".cutin-text");

  textElement.textContent = text;

  // アニメーションをリセット
  cutin.classList.remove("active");

  // 再描画させてから再開
  void cutin.offsetWidth;

  cutin.classList.add("active");
}
```

ゲーム開始処理から、

```javascript
showBattleCutin("BATTLE START");
```

と呼ぶだけです。

例えば、

```javascript
function startGame() {
  initializeGame();

  showBattleCutin("BATTLE START");

  setTimeout(() => {
    dealCards();
    updateScreen();
  }, 1800);
}
```

とすれば、

**プレイ画面表示 → BATTLE START → カード配布**

という流れになります。



> 「うわ、かっこいい！　エンゲージだって！」

とか言ってそうなのも、かなり「放課後はっかーず」っぽいです（笑）。
