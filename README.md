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

   - `user`: ユーザーホーム配下の`~/.claude/settings.json`に登録される．どのプロジェクトのリポジトリにも属さない設定のため，一度実行すれば，以降は別プロジェクト・別セッションでも再インストール不要
   - `project`: リポジトリ直下の`.claude/settings.json`に登録される．このファイルはリポジトリにコミットされる想定のため，クローンした他のメンバーにも共有される
   - `local`: リポジトリ直下の`.claude/settings.local.json`に登録される．このファイルは通常`.gitignore`で除外されリポジトリにコミットされないため，自分のローカル環境限定になる

## 含まれるスキル

- `issue-create`: 課題や要望をGitHub issueとして起票する
- `issue-resolve`: GitHub issueに対応する(調査・ブランチ作成・修正・PR作成)．1issue=1回の実行が単位
