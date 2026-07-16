# for-pynthesis-skills

Pynthesisプロジェクト群 (specification/pyntaxis/pynesis/qurge) で共有する，GitHub issueの起票・対応を支援するClaude Codeスキル集(プラグイン)．

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

2. Claude Codeで`/plugin install for-pynthesis-skills@for-pynthesis-skills`をUser scopeで実行する．

   上記1.の設定だけでは不十分で，このインストールコマンドの実行が別途必要．User scopeで一度実行すればユーザーホーム配下のグローバル設定・キャッシュに登録されるため，以降は別プロジェクト・別セッションでも再インストールは不要．

## 含まれるスキル

- `issue-create`: 課題や要望をGitHub issueとして起票する
- `issue-resolve`: GitHub issueに対応する(調査・ブランチ作成・修正・PR作成)．1issue=1回の実行が単位
