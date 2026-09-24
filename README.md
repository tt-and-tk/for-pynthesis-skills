# for-pynthesis-skills

Pynthesisプロジェクト群 (specification/pynesis/pyntaxis/qurge) で共有する，GitHub issueの起票・対応を支援するClaude Codeスキル集(プラグイン)．

## 導入手順

1. 利用したいプロジェクトの`.claude/settings.json`に，マーケットプレイスとプラグインの有効化を記載する．

   ```json
   {
     "extraKnownMarketplaces": {
       "for-pynthesis-skills": {
         "source": {
           "source": "github",
           "repo": "tt-and-tk/for-pynthesis-skills"
         }
       }
     },
     "enabledPlugins": {
       "for-pynthesis-skills@for-pynthesis-skills": true
     }
   }
   ```

2. Claude Codeで`/plugin install for-pynthesis-skills@for-pynthesis-skills`を実行する．

   インストール先は`--scope`で選べる(省略時は`user`)．

   - `user`: ユーザーホーム配下の`~/.claude/settings.json`に登録される．どのプロジェクトのリポジトリにも属さない設定のため，一度実行すれば，以降は別プロジェクト・別セッションでも再インストール不要．**複数プロジェクトで共有利用する本プラグインでは，このスコープでの導入を推奨する**
   - `project`: リポジトリ直下の`.claude/settings.json`に登録される．このファイルはリポジトリにコミットされる想定のため，クローンした他のメンバーにも共有される
   - `local`: リポジトリ直下の`.claude/settings.local.json`に登録される．このファイルは通常`.gitignore`で除外されリポジトリにコミットされないため，自分のローカル環境限定になる

Claude Code on the web(クラウド)のセッションは，リポジトリの`.claude/settings.json`に書いたマーケットプレイスとプラグインの設定を読まない．クラウドで使う場合は，claude.aiのアカウントでプラグインを有効にする．有効にしたプラグインはクラウドのセッションに同期されて読み込まれる．

## 更新手順

このリポジトリの内容(スキル等)を更新しても，インストール済みのプラグインは自動的には最新化されない．更新内容を反映するには以下を実行する．

1. マーケットプレイスのメタデータをGitHub上の最新コミットへ更新する．

   ```
   claude plugin marketplace update for-pynthesis-skills
   ```

2. インストール済みプラグイン本体を最新化する．

   ```
   claude plugin update for-pynthesis-skills@for-pynthesis-skills
   ```

   デフォルトのスコープは`user`(導入手順で推奨したスコープと一致)．`project`/`local`スコープで導入した場合は`--scope`で指定する．反映にはClaude Codeの再起動が必要．

## 含まれる内容

### スキル

- `issue-create`: 課題や要望をGitHub issueとして起票する
- `issue-resolve`: GitHub issueに対応する(調査・ブランチ作成・修正・PR作成)．1issue=1回の実行が単位

### コーディング規約

`coding-conventions.md`に，文章・コメントの書き方や情報の残し方などのコーディング規約を持つ．プラグインのSessionStartフック(`hooks/hooks.json`)がセッションの開始時・クリア時・コンパクト時にその内容を出力し，文脈に読み込ませる．`claude -p`で起動したセッションにも読み込まれるため，`issue-resolve`のコーディング規約レビューのレビュアーもこの規約を参照する．規約を変えるときは`coding-conventions.md`だけを直せばよい．
