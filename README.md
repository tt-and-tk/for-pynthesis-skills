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

## 含まれるスキル

- `issue-create`: 課題や要望をGitHub issueとして起票する
- `issue-resolve`: GitHub issueに対応する(調査・ブランチ作成・修正・PR作成)．1issue=1回の実行が単位
