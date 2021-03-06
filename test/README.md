# テストについて
この`test`ディレクトリには、**自動自動正男テスト**のためのファイルが入っています。

テストの手法は、正男のソースコードを変更したときに既存の正男の挙動が変わってしまわないことを確かめるために、自動正男を動作させてその挙動をチェックするというものです。**自動自動正男テスト**は、この一連の処理を自動で行うものです。

## テストの方法
1. `npm install`を実行します。（初回のみ。必要なモジュールがローカルインストールされます。）
2. `npm test`を実行します。
3. SUCCESSと表示されればテスト成功です。ERRORと表示された場合は失敗です。

### スナップショットテスト
自動自動正男テストではスナップショットテストを行っています。これは、ゲームの途中の内部状態（スナップショット）をあらかじめ保存しておき、テストの際に正男を動かして同じ内部状態が再現することを確かめるものです。

ただし、1フレーム分のスナップショットが約300KBあるため全フレームのスナップショットを全て取っておくことは不可能です。そのため、自動正男1つにつき3つ程度のフレームを選出してスナップショットを取っています。テスト対象のフレームとしては、ゲーム動作中の適当なタイミングに加えて、クリア画面が表示されているフレームを1つ選ぶことを推奨します。これにより、ゲームクリアまでのフレーム数が一致することをスナップショットテストにより確かめることができます。

テスト失敗時にスナップショットの相違点を表示するには、`DIFF=1 npm test`を実行します。

スナップショットを変更したい場合、すなわちソースコードの変更の結果として正男の挙動が変化したもののそちらが正しいという場合には、`UPDATE=1 npm test`を実行します。

## ファイル構成
- `karma.conf.js`: テストランナーであるkarmaの設定ファイルです。
- `__tests__/index.js`: テストの本体が記述されたファイルです。
- `__tests__/stages`: テスト対象の自動正男が入っているディレクトリです。個々の正男は`*.masao.json`という名前のファイルとなっています。
- `__tests__/stages/__snapshots__`: スナップショットが入っているディレクトリです。

## 自動正男の追加方法

1. 自動正男を作成し、[masao-json-format](https://spec.masao.space/masao-json-format/)に準拠するJSON形式で出力します。ただし、`params`フィールドと`advanced-map`フィールド（存在する場合）以外は不要なので除去します。
2. JSONファイルを`__tests__/stages/`以下に`*.masao.json`というファイル名で配置します。
3. クレジットを表すフィールドをJSONファイル（できれば先頭）に追加します。決まった方法はありませんが、`_url`でオリジナルのURLを示したり、`_author`で作者名を示すのが推奨されます。また、このファイル下部の謝辞に必要に応じて追加します。
4. `npm run inspect-test ファイル名`を実行すると、ブラウザでその正男が開いて開始されます。正男の動作中現在のフレーム数が表示されているので、スナップショットを取るフレームをいくつか（3つ程度）選択します。1つはゲームクリア画面が表示されているフレームを選ぶが推奨されます。
5. JSONファイルにフレームの情報を追加します。例えば100, 200, 300フレーム目のスナップショットを取る場合は`"_snapshot-frames": [100, 200, 300]`とします。
6. JSONファイルに乱数のシード値の情報を追加します。シード値は適当な整数（`12345678`など）を選び、`"_ran_seed": 12345678`のようにします。
7. `npm test`を実行します。`__tests__/stages/__snapshots__/`以下に新しいスナップショットファイルが作成されます。
8. スナップショットファイルをテキストエディタで開き、内容が正しそうか確認します。ゲームクリア画面のスナップショットの`ml_mode`が400番台（エンディング画面）になっているか確認するのが簡単です。
9. 以上で終了です。追加したJSONファイルとスナップショットファイルは両方コミットします。


## 謝辞
テストに使用されている自動正男は下記のものです。

- `hokanozidou54.masao.json`: [ドッスンスンだらけ2 - !のウェブサイト](http://bi.81.la/masao/hokanozidou/hokanozidou54.html)
- `hokanozidou59.masao.json`: [曲線の床を走り抜けて - !のウェブサイト](http://bi.81.la/masao/hokanozidou/hokanozidou59.html)
- `hokanozidou74.masao.json`: [3ステージボス戦全自動 - !のウェブサイト](http://bi.81.la/masao/hokanozidou/hokanozidou74.html)
- `hokanozidou75.masao.json`: [ドッスンスンを動かして2 - !のウェブサイト](http://bi.81.la/masao/hokanozidou/hokanozidou75.html)
- `hokanozidou87.masao.json`: [人間大砲＜バネ - !のウェブサイト](http://bi.81.la/masao/hokanozidou/hokanozidou87.html)
- `hokanozidou92.masao.json`: [坂道で急上昇 - !のウェブサイト](http://bi.81.la/masao/hokanozidou/hokanozidou92.html)

- `uzushio.masao.json`: 作者: !
- `boss2-babururensya.masao.json`: 作者: !
- `boss2-hariken.masao.json`: 作者: !
- `boss2-baburukaiten.masao.json`: 作者: !
- `boss2-hadoutyokusin.masao.json`: 作者: !
- `boss3-ryuuseigun.masao.json`: 作者: !
- `boss3-kaitentaiatari.masao.json`: 作者: !
- `boss3-kousokukaitentaiatari.masao.json`: 作者: !
- `boss3-kaitenjump.masao.json`: 作者: !
