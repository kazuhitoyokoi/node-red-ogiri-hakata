# Raspberry PiのSense HATに天気情報を表示

## 背景
イベント開催には、天気の情報が重要。

<img src="https://1.bp.blogspot.com/-9O3txb7OtvQ/UbVu8wEe3DI/AAAAAAAAUkA/DOA2jrDfVH8/s800/bonodori.png" width="50%"><img src="https://2.bp.blogspot.com/-04yGTd8fSnA/U9y_m5vpsrI/AAAAAAAAjfw/nVqHQN_t9g4/s800/tenki_mark03_gouu.png" width="50%">

晴天か雨天かを常時、表示しておけるデバイスがあると便利そう。

## 開発で使用した機材
Node-REDが動く代表的なデバイスとして、Raspberry Piを使用。

<img src="https://3.bp.blogspot.com/-Wl5LVBDviR4/Vz8HzNnjSEI/AAAAAAAA6tY/BU6AXKS3mj4yjF8nhncl5Ai4cdLYHPrZACLcB/s800/computer_single_board.png" width="50%"><img src="https://images.prismic.io/rpf-products/a222a1d657906db95efbca8b8467037fa1a89def_sense-hat-1733x1080-1-1733x1080.jpg" width="50%">

8x8個のカラーLEDが搭載されたSense HATをRaspberry PiのGPIO端子に接続。

これら機材を用いて天気情報を伝える仕組みを開発した。

## 使用した天気情報提供サービス「OpenWeather」
天気情報を取得するため、[OpenWeather](https://openweathermap.org/)というサービスを利用。
本サービスはアカウントを作成すると、無料で1000回/日だけ天気情報を取得可能。

Node-REDからOpenWeatherへのアクセスには、Node-REDプロジェクトが提供している下記OpenWeatherMapノードを使用。

https://flows.nodered.org/node/node-red-node-openweathermap

## 開発したフロー
定期的に天気情報を取得して、最新の天気情報の文字列をLEDに表示し続けるフローを開発した。

![](https://raw.githubusercontent.com/kazuhitoyokoi/node-red-ogiri-hakata/main/flow.png)
[![](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/kazuhitoyokoi/node-red-ogiri-hakata?embed=1&hideExplorer=1&hideNavigation=1&view=preview)

LEDの色は、晴れの時に赤、曇りの時に白、雨の時に青、その他は緑になる様にした。

Sense HATのLEDに情報を表示する度に、OpenWeather APIにアクセスするとAPIの使用回数の上限に達してしまう。
これを避けるため、取得した情報をコンテキストに保存しておく様にし、LEDに表示する際はコンテキストの情報を使用した。

## Sense HATエミュレータでの動作
手軽に開発を始めるため、Sense HATシミュレータの下記ノードを用いてPC上でフローを開発。

https://flows.nodered.org/node/node-red-node-pi-sense-hat-simulator

上手くフローが動作すると下の様に、シミュレータの画面に天気の情報を表示できた。

https://user-images.githubusercontent.com/20310935/235351040-cb920f5d-d25b-4b67-84ec-b7060fdae4c8.mp4

## Sense HAT実機での動作
シミュレータで期待通り動作することを確認した後、Git連携機能を利用してGitHubリポジトリにフローを置いた。
その後、Raspberry Piでフローをダウンロードして、Sense HAT実機でも同じ様に動作するか確認。
Raspberry Pi上では、Sense HATシミュレータのノードをSense HAT実機用のノードに置き換えた。

```mermaid
graph LR
A(ローカルPC) -- フローをアップロード --> B[(本GitHub<br>リポジトリ)]
B -- フローをダウンロード --> C(Raspberry Pi)
```

実機でも上手く天気情報が表示されました！

https://user-images.githubusercontent.com/20310935/235351023-06600481-37dd-40f1-bfaa-80c37bc1b393.mp4
