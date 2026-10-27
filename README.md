# ふたりの行きたいお店

行きたいお店をリスト化して、**招待した相手とふたりだけで、リアルタイムに共有**する Web アプリです。
画面は `index.html` 1ファイル（GitHub Pages で公開）、データは Firebase（Authentication + Firestore）に保存します。

公開URL: https://yokonori0125-ops.github.io/futari-shops/

## 機能

- **お店一覧**：店名・エリア・ジャンル・雰囲気・Google マップ／食べログ／Instagram のリンク・メモ・「行った／行きたい」
- **検索・絞り込み**：エリア・ジャンル・雰囲気・状態・キーワード（一覧とマップで共通）
- **マップ**：お店の位置をピン表示（Google Maps API キー設定時は Google マップ、未設定なら OpenStreetMap）
- **ふたりで共有**：Google ログイン → リスト作成 → 招待リンクで相手が参加。追加・編集・削除は相手の画面に即時反映
- **オフライン対応**：電波が悪いときは端末に一時保存し、つながったら自動で送信
- **バックアップ**：JSON の書き出し・読み込み

## 使い方

1. ひとり目が公開URLを開き、Google でログイン →「新しいリストを作る」
2. 「設定・共有」タブの **招待リンク** を相手に送る
3. 相手がリンクを開いて Google でログインすると参加完了。**2人そろった時点で招待リンクは無効**になります

LINE のアプリ内ブラウザでは Google ログインができないため、招待リンクには `?openExternalBrowser=1` を付けて外部ブラウザで開くようにしています。

## セキュリティ

アクセス制御は Firestore のセキュリティルール（[`firestore.rules`](firestore.rules)）で行っています。

- リスト（部屋）の中身を読み書きできるのはメンバーだけ。部屋の一覧取得は禁止
- メンバー以外ができるのは「2人目として自分を追加する」ことだけ。2人そろうと誰も参加できない
- メンバーの削除・入れ替え、部屋の削除は不可

`index.html` 内の `firebaseConfig` はページに公開する前提の値で、秘密情報ではありません。

## Firebase 側の設定（作り直す場合）

1. Firebase プロジェクトを作成し、ウェブアプリを登録して `firebaseConfig` を `index.html` に貼る
2. Authentication → Sign-in method で **Google** を有効化し、承認済みドメインに `yokonori0125-ops.github.io` を追加
3. Firestore Database を作成（本番環境モード）
4. Firestore → ルール に `firestore.rules` の内容を貼り付けて公開

## Google Maps API キー（任意）

Google Cloud Console で Maps JavaScript API と Geocoding API を有効化してキーを作成し、
**アプリケーションの制限 → ウェブサイト** に `https://yokonori0125-ops.github.io/*` を設定してから、
ページの「設定・共有」タブに入力してください。キーは入力した端末にのみ保存されます。
