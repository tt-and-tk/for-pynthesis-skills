# for-pynthesis-skills

Pynthesisプロジェクト群 (specification/pynesis/pyntaxis/qurge) で共有する，GitHub issueの起票・対応を支援するClaude Codeスキル集と，共通のコーディング規約を提供するプラグイン．

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
   claude plugin update for-pynthesis-skills@for-pynthesis-skills --scope user
   ```

   `--scope`を省略すると，実行したディレクトリに`local`・`project`スコープの登録があればそちらだけが更新され，`user`スコープは古い版のまま残る．`user`スコープで導入した場合(推奨)，セッションに読み込まれるのは`user`スコープの版であるため，`--scope user`を指定する．`local`・`project`スコープで導入した場合は，そのスコープを`--scope`で指定する．反映にはClaude Codeの再起動が必要．

   `user`スコープで導入していても，Claude Codeを起動したディレクトリ(worktreeを含む)ごとに`project`スコープの登録が作られ，`claude plugin list`に版の異なる登録として並ぶ．これらの版は読み込まれないため，更新も削除も不要である(`claude plugin uninstall --scope project`は，コミット対象の`.claude/settings.json`から`enabledPlugins`の記載まで消してしまうため使わない)．

## 含まれる内容

### スキル

- `issue-create`: 課題や要望をGitHub issueとして起票する
- `issue-resolve`: GitHub issueに対応する(調査・ブランチ作成・修正・PR作成)．1issue=1回の実行が単位．複数リポジトリに及ぶissueも1セッションでまとめて対応できる

### コーディング規約

`coding-conventions.md`に，言語によらない項目と，言語ごとの書き方から成るコーディング規約を持つ．プラグインのSessionStartフック(`hooks/hooks.json`)が，再開(resume)を除くセッションの開始時などにその内容を出力し，文脈に読み込ませる(再開したセッションは，以前に読み込んだ規約を会話の履歴として引き継ぐため)．`claude -p`で起動したセッションにも読み込まれるため，`issue-resolve`のコーディング規約レビューのレビュアーもこの規約を参照する．

## 変更するときの決まり

### コーディング規約

コーディング規約の本文は`coding-conventions.md`だけが持つ．規約を変えるときは，このファイルだけを直す．

### スキル間で共有する記述

スキルは個別に読み込まれ，共通のファイルを参照できない．このため，複数のスキルで共有する記述は各スキルの`SKILL.md`に複製することになり，片方だけを直して食い違いやすい．そこでissue対応の手順は，影響リポジトリが1つか複数かによらず`issue-resolve`1つにまとめ，手順の一部を共有する別のスキルを設けない．

複製するのは「コマンド実行の規定」(`issue-create`・`issue-resolve`)だけとする．両スキルの`SKILL.md`で一字一句同一の内容に保ち，片方だけを修正しない．同一に保つのは見出し配下の本文であり，見出し行のレベルは各ファイルの階層に合わせるため一致しなくてよい．この規定はスキルを実行している最中に常に従う規律であり，参照にすると読み手が別のスキルを開くまで守るべきことが分からないため，参照ではなく複製する．
