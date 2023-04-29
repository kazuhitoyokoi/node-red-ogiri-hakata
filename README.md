# Raspberry PiのSense HATに天気情報を表示

## イベント開催には、天気の情報が重要
<img src="https://1.bp.blogspot.com/-9O3txb7OtvQ/UbVu8wEe3DI/AAAAAAAAUkA/DOA2jrDfVH8/s800/bonodori.png" width="50%"><img src="https://2.bp.blogspot.com/-04yGTd8fSnA/U9y_m5vpsrI/AAAAAAAAjfw/nVqHQN_t9g4/s800/tenki_mark03_gouu.png" width="50%">

## ラズパイとSense HATのLEDを用いて天気情報を伝える仕組みを開発
<img src="https://3.bp.blogspot.com/-Wl5LVBDviR4/Vz8HzNnjSEI/AAAAAAAA6tY/BU6AXKS3mj4yjF8nhncl5Ai4cdLYHPrZACLcB/s800/computer_single_board.png" width="50%"><img src="https://images.prismic.io/rpf-products/a222a1d657906db95efbca8b8467037fa1a89def_sense-hat-1733x1080-1-1733x1080.jpg" width="50%">

## 天気情報提供サービス「OpenWeather」
天気情報を取得するため、[OpenWeather](https://openweathermap.org/)というサービスを利用。
本サービスはアカウントを作成すると、無料で1000回/日だけ天気情報を取得可能。

Node-REDからOpenWeatherへのアクセスには、Node-REDプロジェクト公式の下記ノードを使用。

https://flows.nodered.org/node/node-red-node-openweathermap

## 開発したフロー
![](https://raw.githubusercontent.com/kazuhitoyokoi/node-red-ogiri-hakata/main/flow.png)
[![](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/kazuhitoyokoi/node-red-ogiri-hakata?embed=1&hideExplorer=1&hideNavigation=1&view=preview)

Sense HATのLEDに情報を表示する度に、OpenWeather APIにアクセスするとAPIの使用回数の上限に達してしまう。
これを避けるため、取得した情報をコンテキストに保存しておく様にし、LEDに表示する際はコンテキストの情報を使用する。

## Sense HATエミュレータでの動作
Sense HATシミュレータのノードを用いてPC上でフローを開発。上手く動作すると下の様に、天気の情報が表示される。

https://flows.nodered.org/node/node-red-node-pi-sense-hat-simulator

https://user-images.githubusercontent.com/20310935/233776400-85e7d697-5995-462f-9796-5186ccbf0c97.mp4

## Sense HAT実機での動作
シミュレータで期待通り動作することを確認した後、Git連携機能を利用してGitHubリポジトリにフローを置いた。その後、Raspberry Piでフローをダウンロードして、Sense HAT実機でも同じ様に動作するか確認。

```mermaid
graph LR
A(ローカルPC) -- フローをアップロード --> B[(本GitHub<br>リポジトリ)]
B -- フローをダウンロード --> C(Raspberry Pi)
```

Raspberry Pi上では、Sense HATシミュレータのノードをSense HAT実機用のノードに置き換えた。

https://user-images.githubusercontent.com/20310935/235315974-e0c3bb9c-2624-4ae7-b74c-a0d8ef865093.mp4



