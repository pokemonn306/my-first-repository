# Space Monitor Lab

macOSのSpace切り替え（トラックパッドのスワイプ）を含む、ブラウザのフルスクリーン監視・動画停止・カメラ静止画取得を検証するための非公式の再現サイトです。

**非公式の独立実装です。** 提供された `react-components-bundle-DFEY51S4.js` / HTMLの動作条件を参照していますが、元の配信サービスと接続しません。著作権のある第三者コードや、HTML内の個人識別子・認証情報・署名付きAWS URLは公開していません。実際の顔認証とHLS動画再生は再現対象外です。

## 使い方

1. GitHub Pages（HTTPS）からサイトにアクセスします。
2. 任意で「カメラを許可・起動」を押し、ブラウザのカメラ許可ダイアログを承認します。
3. 「検証開始・全画面化」を押して動画の模擬再生と全画面監視を開始します。
4. macOSのトラックパッドを左右にスワイプして別のSpaceへ移動し、元のSpaceへ戻ります。
5. イベントログで、`VIDEO_DISABLED_FULL_SCREEN` と `DIAG_VISIBILITY_CHANGE` / `DIAG_WINDOW_BLUR` を比較します。
6. 「JSON出力」でログをダウンロードできます。撮影画像は各サムネイルから個別保存できます。

macOSのジェスチャーはシステム設定によって3本指または4本指になります。

## ソース準拠の項目

| 項目 | 設定 / イベント |
| --- | --- |
| フルスクリーン解除 | `fullscreenchange`, `VIDEO_DISABLED_FULL_SCREEN (195)` |
| フルスクリーン解除回数 | 2回 |
| 解除継続時間 | 60秒 |
| 動画停止 | `VIDEO_PAUSED (120)`, `VIDEO_RESUMED (130)` |
| 動画停止回数 / 停止時間 | 2回 / 60秒 |
| カメラ撮影周期 | 60秒、失敗時15秒再試行 |
| 撮影処理 | `getUserMedia`, `canvas`, JPEG変換、黒画面・同一画像の簡易判定 |
| 動画再生位置変更 | `SYSTEM_BLOCK_SEEK (610)` |
| ネットワーク切断 | `VIDEO_NETWORK_DISCONNECTED (180)` |
| 元HTMLのレッスン長 | 770160 ms（12分50.16秒） |

**元サイトと異なる点：** 本サイトの動画表示は時間経過を模擬した独自アニメーションであり、HLS再生、実教材、サーバーの受講状態、本人認証、IndexedDBバックアップ、S3アップロード、実際のスクリーンショット送信や受講中断とは無関係です。`visibilitychange` と `blur/focus` は追加の診断ログであり、元ソースに存在した監視と区別しています。`isPc: true`, `isCommonLesson: true` の添付HTMLに対応して作成しています。カメラの撮影処理は独立実装です。

## プライバシー・技術制限

- `fetch`/XHR/WebSocketなどによる通信・アップロードなし。
- カメラ映像の取得はユーザーが明示的にボタンを押した後だけです。マイクは取得しません。
- カメラ画像とイベントはページのメモリ内だけで保持され、ページを閉じると消えます。
- JSON出力に顔画像は含まれません。カメラ画像は個別に保存できます。
- GitHub PagesのHTTPS経由でブラウザの `getUserMedia` を使います。
- `fullscreenchange` はOSのSpace切り替えそのものを表すものではありません。ChromeとSafari、macOSのバージョンによって挙動が変わります。

## GitHub Pages

リポジトリ `pokemonn306/my-first-repository` の `main` ブランチに `space-monitor-lab/` として配置する想定です。GitHub Pages設定が `Deploy from a branch`・`main`・`/(root)` なら次のURLで公開できます。

`https://pokemonn306.github.io/my-first-repository/space-monitor-lab/`

Pages未設定の場合は、リポジトリ **Settings > Pages > Build and deployment > Deploy from a branch > main / (root) > Save** の順に有効化してください。