---
name: cross-project-edit
description: 複数プロジェクトに横断的に影響するissue対応を1セッションでまとめて行う。「issue #Nを全プロジェクトに対応して」等で起動。
---

## 手順

1. `gh issue view <番号> --repo tt-and-tk/for-pynthesis-skills` でissue内容を確認する
2. 原因調査を行い，影響リポジトリ(`specification`/`pynesis`/`pyntaxis`/`qurge`/`for-pynthesis-skills`自身のうち複数)と対象ファイルを特定する
3. 調査結果と修正方針をユーザーに提示し，承認を得る．**これが実装前の最後の確認ポイント**であり，承認後は手順4を人間の追加確認なしに連続して実行する
4. 影響リポジトリ全てについて，このセッション内で連続して以下を行う
   1. 作業場所の準備
      - `for-pynthesis-skills`自身の場合，`EnterWorktree`(name: `fix/issue-<番号>-<内容を表す短い語句>`)で専用の作業ディレクトリとブランチを作成する
      - それ以外のリポジトリの場合，リモートから直接cloneして隔離を作る
        ```
        gh repo clone tt-and-tk/<リポジトリ名> "<現在のプロジェクトルート>/.worktrees/<リポジトリ名>-fix-issue-<番号>-<内容を表す短い語句>"
        git -C "<現在のプロジェクトルート>/.worktrees/<リポジトリ名>-fix-issue-<番号>-<内容を表す短い語句>" checkout -b fix/issue-<番号>-<内容を表す短い語句>
        ```
        `<現在のプロジェクトルート>`は，このセッションが起動した元のプロジェクトディレクトリを指す(`EnterWorktree`のworktree配下には作らない)
   2. 手順3で承認された方針に基づいて修正する
   3. コミット・push・PR作成
      - `for-pynthesis-skills`自身の場合
        ```
        git add <変更したファイル>
        git commit -m "<コミットメッセージ>"
        git push -u origin fix/issue-<番号>-<内容を表す短い語句>
        gh pr create --repo tt-and-tk/for-pynthesis-skills --title "<タイトル>" --body "<本文>" [--draft]
        ```
      - それ以外のリポジトリの場合
        ```
        git -C <cloneしたディレクトリの絶対パス> add <変更したファイル>
        git -C <cloneしたディレクトリの絶対パス> commit -m "<コミットメッセージ>"
        git -C <cloneしたディレクトリの絶対パス> push -u origin fix/issue-<番号>-<内容を表す短い語句>
        gh pr create --repo tt-and-tk/<リポジトリ名> --head fix/issue-<番号>-<内容を表す短い語句> --title "<タイトル>" --body "<本文>" [--draft]
        ```
      - PRの`--body`にcloseキーワード(`Closes tt-and-tk/for-pynthesis-skills#番号`．1issueにつき1箇所のみ，通常はissueが存在する`for-pynthesis-skills`のPR)またはリンクのみ(`Related to tt-and-tk/for-pynthesis-skills#番号`)を含める．`--draft`はcloseキーワードを持つPRにのみ付ける
   4. `claude -p [--add-dir <対象リポジトリのパス>...] -- "<issue番号・対象リポジトリでのgit diffとソース全体のレビューを依頼する文言．レビューのみを行うこと，無関係な既存不具合は修正せず報告のみすることを明記>" > review.md` で自動レビューし，指摘があれば修正・コミット(指摘一つずつ)・再レビューを人間の確認なしに繰り返す
   5. 人間レビューへの対応: Draft解除はユーザーが行う．コメントを取得し，指摘があれば修正してコミット・pushし，該当コメントへ返信する(必須)
      ```
      gh api repos/tt-and-tk/<リポジトリ名>/pulls/<PR番号>/comments
      gh api repos/tt-and-tk/<リポジトリ名>/issues/<PR番号>/comments
      ```
      ```
      gh api repos/tt-and-tk/<リポジトリ名>/pulls/<PR番号>/comments/<コメントID>/replies -f body="<対応内容>"
      gh pr comment <PR番号> --repo tt-and-tk/<リポジトリ名> --body "<対応内容>"
      ```
      対応後は4-4の自動レビューを再実行する
5. マージはユーザー自身が行う
6. マージ完了の報告を受けたら，issueがクローズされたことを確認する．後始末は，`for-pynthesis-skills`自身は`ExitWorktree`(`remove`)，それ以外は`rm -rf <cloneしたディレクトリの絶対パス>`

## 注意

- 人間の確認が必須なのは手順3と破壊的操作(force push，reset --hard等)のみ．それ以外は連続して自動実行する
- 1issueに対して複数のPRがある場合，closeキーワードを持つPRは1つだけにする
- 影響リポジトリが不明な場合は，ユーザーに確認する
- マージは決して行わない
- レビュー指摘(AI・人間とも)は鵜呑みにせず対応要否を判断する．対応しない場合は理由をPRコメントまたはコミットメッセージに残す
- `cd <path> && <command>`のような複合コマンドは使わず，`cd <path>`単独のBash呼び出しと，その後の素のコマンド呼び出しに分けて実行する
