CRT Glow Canvas アニメーションの技術解説このドキュメントでは、HTML5 Canvas（2D Context）と JavaScript を利用して、レトロなCRT（ブラウン管）モニター風の発光エフェクトやアニメーションを実装する手法を解説します。

## 1. 蛍光・発光表現（Glow Effect）の原理

Canvas でネオンや蛍光管のような「鈍く光る」表現を実現するため、ドロップシャドウ機能（Shadow API） を応用しています。
```
ctx.save();    
ctx.shadowColor = 'rgba(0, 255, 102, 0.8)'; // 光の色    
ctx.shadowBlur = 18;                        // ぼかしの強さ（発光範囲）    
ctx.strokeStyle = '#00ff66';    
ctx.stroke();    
ctx.restore();
```
shadowBlur: 影の拡散半径（ピクセル）を指定します。値を大きくするほど、光線が周囲へ広がる「ぼんやりとした発光効果」が得られます。

shadowColor: 光線の色を指定します。アルファ値（透明度）を含めることで、空気中の光の拡散具合を微調整できます。

save() / restore(): シャドウ効果が他の非発光パーツに影響しないよう、描画コンテキストの状態を一時的に保存・復元しています。

## 2. 3つのセクションの実装とアニメーション手法

① 旧式端末風 緑色フレーム（> HelloWorld_）特徴: 単色のグリーン発光と、古くなった端末特有の微小な「フリッカー（ちらつき）」現象。

アニメーションロジック:Math.sin()（サイン波）を利用して、時間の経過に伴いアルファ値（不透明度）をわずかにゆらめかせています。
```
// time（経過ミリ秒）を用いて 0.70 ～ 1.0 の範囲で高速に揺れる係数を算出    
const flicker = Math.sin(time * 0.015) * 0.15 + 0.85;    
ctx.globalAlpha = flicker;
```
② 赤色パルス発光フレーム（⚠ DANGER ⚠）特徴: 呼吸をするように（ゆっくり点滅するように）明暗が変化する赤い危険信号。

アニメーションロジック:サイン波を 0.0 ～ 1.0 の範囲に正規化し、ぼかし幅（shadowBlur）と透明度（alpha）に連動させています。
```
// 0.0 ～ 1.0 を周期的に往復するパルス値    
const pulse = (Math.sin(time * 0.005) + 1) / 2;   
const glowIntensity = 10 + pulse * 25; // 発光範囲が 10px 〜 35px に伸縮    
const alpha = 0.6 + pulse * 0.4;        // 不透明度が 0.6 〜 1.0 に変化    
```

③ ゲーミングPC風 虹色グラデーション（🎮 GAMING PC 🎮）

特徴: 時間経過で色相が滑らかに回転するマルチカラー（RGB）グラデーション発光。

アニメーションロジック:HSLカラーモデル（Hue, Saturation, Lightness） を活用しています。

色相（Hue: 0〜360度）を時間の経過とともに加算回転させ、createLinearGradient に渡しています。
```
const hueStart = (time * 0.12) % 360; // 時間で変化する基準色相
const gradient = ctx.createLinearGradient(x, y, x + width, y + height);    
gradient.addColorStop(0.0,  `hsl(${hueStart}, 100%, 60%)`);    
gradient.addColorStop(0.33, `hsl(${(hueStart + 120) % 360}, 100%, 60%)`);    
gradient.addColorStop(0.66, `hsl(${(hueStart + 240) % 360}, 100%, 60%)`);    
gradient.addColorStop(1.0,  `hsl(${(hueStart + 360) % 360}, 100%, 60%)`);        
ctx.strokeStyle = gradient;    
```

3. レトロCRT（ブラウン管）走査線の再現CRTモニターの独特な質感を出すため、描画要素の上にオーバーレイ（重ね描き）処理を行っています。
水平走査線（Scanline）:ループ処理により、一定間隔（例: 6pxおき）で薄い黒の半透明横線（rgba(0, 0, 0, 0.35)）を敷き詰めます。

上下移動する電子ビーム（Scanline sweep）:時間経過で Y 座標が上から下へループ移動するグラデーション帯を描画し、走査線の動きを表現します。scanlineY = (time * 0.15) % (height + 100) - 50;    
周辺減光（Vignette / 画面の丸み効果）:中心から外側に向かう放射状グラデーション（createRadialGradient）を生成し、画面の四隅を暗くすることで、昔の曲面CRT管のような奥行き感を出します。

## 4. レンダリングとレスポンシブ最適化

requestAnimationFrame による描画ループ:ディスプレイのリフレッシュレート（60Hz/144Hz等）に同期して毎フレーム再描画を行います。

高解像度（Retina/DPI）対応:window.devicePixelRatio を読み取り、Canvasの内部解像度（width/height）とスタイル上の表示サイズ（style.width/style.height）を分離補正することで、高密度ディスプレイでも文字や線がぼやけず鮮明に表示されるよう調整しています。
