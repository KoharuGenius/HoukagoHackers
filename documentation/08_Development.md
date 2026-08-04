# 08_Development.md

# 開発方針

## 基本方針

NEXUS FRONTIER はブラウザ上で動作するカードバトルゲームとして開発する。

ゲーム本体はHTML、CSS、JavaScriptを中心に実装し、
レトロフューチャー・サイバーパンク風のコンソールUIによる演出を重視する。

複雑なゲームエンジンを使用するよりも、
軽量で修正しやすい構成を優先する。

---

# 開発環境

## 推奨環境

- Visual Studio Code
- Git
- GitHub

## 使用技術

- HTML5
- CSS3
- JavaScript

---

# ディレクトリ構成
NEXUSFrontier/

├── index.html

├── css/
│ ├── terminal.css
│ ├── card.css
│ ├── dialog.css
│ └── animation.css

├── js/
│ ├── game.js
│ ├── card.js
│ ├── battle.js
│ ├── field.js
│ ├── ui.js
│ └── dialog.js

├── img/
│ ├── characters/
│ ├── cards/
│ └── logo/

├── fonts/
│ ├── dseg7/
│ └── ui-font/

└── docs/
├── GDD.md
└── specifications/

---

# 実装方針

## 初期段階

ゲームルール検証を優先する。

実装済み：

- カードバトル
- 攻守交代
- LP管理
- マーカー移動
- 勝利判定

---

## 次段階

UIと演出を追加する。

追加予定：

- コンソール風画面
- ネットワークライン表示
- カード表示
- ログ表示
- STATUS表示

---

# UI実装方針

## メイン画面

HTML + CSSによるコンソール表示。

イメージ：

- 1980年代軍用コンピュータ
- CRTディスプレイ
- 緑色発光文字
- 黒背景
- 走査線

---

## カード表示

カードは画像ではなくHTML/CSSで生成する。

理由：

- 状態変化が容易
- LPによる使用可否表示が可能
- 演出追加が容易

---

## カード状態

STATUS表示を実装する。

例：

通常：


STATUS : READY


LP不足：


STATUS : LOW POWER


実行中：


STATUS : EXECUTING...


使用済み：


STATUS : ARCHIVED


---

# フォント

## 漢字

カード名：

- 明朝系フォント

例：


強襲


---

## ルビ

ドイツ語読み：

- ゴシック系フォント

例：


シュトゥルムアングリフ


---

## 数値表示

7セグメント風フォントを使用。

対象：

- 攻撃力表示
- ノード数
- LP

例：


+8


---

# キャラクター表示

キャラクターは常時表示しない。

基本画面：


コンソールUI


イベント時：


キャラクターカットイン
+
会話ウィンドウ


という構成にする。

理由：

- 実装量を抑える
- ゲーム画面の世界観を維持する
- キャラクター演出を特別なものにする

---

# 演出システム

## プログラム実行演出

カード使用時：

1. カード選択
2. STATUS変更


READY


↓


EXECUTING...


3. コンソールログ表示

例：


STURM-08 START

TARGET LOCKED

TRANSMISSION COMPLETE


4. バトル結果表示

---

# Git管理

## 基本方針

変更履歴を残す。

コミット例：


add: terminal UI prototype

update: card status display

fix: LP calculation

change: card name


---

# ドキュメント管理

仕様変更はMarkdownで管理する。

重要な変更はDecisionLog.mdへ記録する。

例：


2026/08/03

牽制を突破へ変更。

理由：
最低限の前進能力を表現するため。


---

# 開発優先順位

## Phase 1

ゲームルール完成

□ バトル処理  
□ 勝敗判定  
□ カード処理  


## Phase 2

UI完成

□ コンソール画面  
□ カード表示  
□ ネットワーク表示  


## Phase 3

演出追加

□ カットイン  
□ 会話イベント  
□ ログ演出  


## Phase 4

完成度向上

□ 効果音  
□ アニメーション  
□ バランス調整  


---

# 開発コンセプト

「簡単なカードゲームを、
本格的な未来戦争システムのように見せる」

ゲームルールそのものはシンプルに保ち、
UI、演出、キャラクターによって世界観を広げる。
