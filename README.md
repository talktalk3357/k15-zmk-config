# K15 RevC ZMK 設定（未ビルド）
作成日: 2026-09-27

これはソース設定です。UF2ではありません。NICENANOへコピーしないでください。
ZMK v0.3 / nice_nano_v2 を初期ターゲットにしています。実物は互換機であり、
INFO_UF2の機種名だけで電源回路・充電電流の適合性は確定しません。

## 構成
- 左central、右peripheral。各5行×7列、実装32キー。
- 正本: ../k15_split_revC/WIRING.md と左右 key_matrix.csv。
- カソードROWのcol2row。直接GPIO指定（基板印字017 = gpio0 17）。
- transform順は左SW1..32、右SW1..32。右にcol-offset 7。
- 右Alt=ROW4/COL4、Ctrl=ROW4/COL5。古い仕様書のCOL3/4を使わない。
- ベースはCSVの英字配列/HID記号。Windows側のUS/JIS設定により記号の表示が変わる。
- Layerは暫定: 押している間、左1..5→F1..5、右6..0→F6..10、
  右上段Backspace→F11、Home→F12。他は元のキー。
  最終レイヤー内容・日本語配列調整はユーザーと確認する。

## ビルド（別途実行が必要）
このフォルダーをリポジトリのルートとすると .github/workflows/build.yml が
公式ZMKの再利用ワークフローで左右をビルドする。zephyr/module.ymlでshieldを登録する。
GitHubへのアップロードや実行はまだ行っていない。
出力予定: k15_revC_left.uf2 / k15_revC_right.uf2。
成功ログと生成物を検査するまで書き込み可能とは扱わない。
ローカル環境はwest、Zephyr SDK等が必要（現在のPCに確認できず）。

## 初回動作検査
電池・電源線は未接続。基板間を絶縁する。
左右用ファイルを区別して書き込む。右はUSB単独でキーボードとしては動かず、
USB給電しながらBLEで左へ送信する。左右の電池なしで両側を試験できる。
充電電流、実物のB+/B-、短絡・極性の確認が済むまで電池を接続しない。

参照:
- https://github.com/zmkfirmware/zmk/tree/v0.3/app/boards/shields/corne
- https://github.com/zmkfirmware/zmk/blob/v0.3/app/boards/arm/nice_nano/nice_nano_v2.dts
- https://zmk.dev/docs/development/local-toolchain/build-flash
