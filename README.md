# STUDY HARD

今日の努力が、未来の自分をつくる。
勉強時間で経験値を貯めてレベルアップし、自分だけの勉強部屋を育て、友達とランキングで競う受験生向けの学習管理Webアプリです。

公開URL：https://studyhard-assistant.vercel.app/
（旧URL https://study-hard-inky.vercel.app/ は新しいURLへ転送）

## ファイル構成

| ファイル | 役割 |
|---|---|
| `index.html` | アプリ本体（画面・デザイン・動き） |
| `config.js` | Supabase の接続先（**ここだけ自分で書きかえる**） |
| `vendor/supabase.js` | Supabase の公式ライブラリ（v2.117.2） |
| `supabase/setup.sql` | データベースの初期設定（Supabase の SQL Editor で1回実行） |
| `manifest.webmanifest`, `icon-*.png` | ホーム画面に追加したときのアイコンと名前 |

## しくみ

- ホスティング：Vercel（静的サイト。ビルド不要）
- ログイン・データ保存：Supabase（Auth ＋ Postgres）
- 記録はユーザーごとに保存され、行レベルセキュリティ（RLS）で本人しか読めません。
- ランキング用の公開プロフィール（ニックネーム・レベル・勉強時間・連続日数・部屋）は、ログインした人どうしで見られます。
- 環境音・BGMはブラウザ内で合成しているため、音源ファイルはありません。

## 更新のしかた

`index.html` を新しいものに置きかえて GitHub にアップロードすると、Vercel が自動で公開し直します（1分ほど）。

## URLを変えるとき

アプリの中のリンク（フレンド招待・グループ招待・確認メール）は、開いているページのURLから自動で作るので、`index.html` の書きかえは不要です。データは Supabase にアカウントごとに保存されているので、URLを変えても消えません。

1. **予告**：開発者ページのお知らせで、変える日時と新しいURLを知らせる（「移る前にタイマーを止めてね」も書く）。
2. **Vercel**：Settings → Domains で新しいURLを追加する（この時点では古いURLも使える）。
3. **Supabase**：Authentication → URL Configuration で、Site URL を新しいURLに変え、Redirect URLs に新しいURLを追加する（古いURLもしばらく残す）。
4. **数日待つ**：みんなに新しいURLでログインし直してもらう。
5. **転送**：だれも勉強していない時間に、Vercel で古いURLを「新しいURLへ Redirect」にする。

注意
- 新しいURLでは、全員ログインし直しが必要（データはそのまま）。
- 計測中のタイマーは、止めてから移らないとその途中の時間が消える。
- 転送にした直後に、開発者ページで「最新バージョン」を上げない（全員が一気に再読み込みされる）。
- ホーム画面に追加している人は、新しいURLで追加し直す。
- アカウントを作らず「端末のみ」で使っている人のデータは引き継げない。

## Supabase を新しいプロジェクトに引っ越すとき

データは「コピー」するので、古いプロジェクトはそのまま残ります（うまくいかなくても元に戻せます）。みんなのメールアドレスとパスワードもそのまま使えます（全員ログインし直しは必要）。

1. **予告**：開発者ページのお知らせで、引っ越しの日時を知らせる（その時間は使わないでね、タイマーは止めてね）。
2. **新しいプロジェクトを作る**：Supabase で New project（地域は古いのと同じ Tokyo がおすすめ）。
3. **新しいプロジェクトの SQL Editor で `supabase/setup.sql` を全部実行**。
4. **古いプロジェクトの接続文字列を用意**：古いプロジェクトの「Connect」→「Session pooler」の文字列をコピーし、`[YOUR-PASSWORD]` をデータベースのパスワードに置きかえる。忘れたら Settings → Database → Reset database password（英数字だけのパスワードにする。アプリには影響なし）。
5. **新しいプロジェクトの SQL Editor で `supabase/migrate_copy.sql` を実行**：いちばん下の行に接続文字列を入れて Run。表が10行ならんで全部「OK」なら成功。
6. **新しいプロジェクトの認証設定**：Authentication → URL Configuration（Site URL と Redirect URLs）や、Sign In / Providers の「Confirm email」などを古いプロジェクトと同じにする。
7. **config.js を書きかえる**：新しいプロジェクトの Project URL と Publishable key に変えて GitHub に保存（Vercel が自動で公開し直す）。
8. **古いプロジェクトの SQL Editor で `supabase/old_switch.sql` を実行**：開いたままの画面が自動で読み込み直され、新しいプロジェクトにつながる。
9. **確認**：自分のアカウントでログインし、記録・部屋・ペット・開発者ページが使えるか見る。
10. **片づけ**：新しいプロジェクトの SQL Editor で `supabase/migrate_cleanup.sql` を実行。古いプロジェクトは1週間ほどそのまま残してから一時停止・削除。古いプロジェクトのデータベースのパスワードも変えておく。
