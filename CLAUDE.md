# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト全体における位置づけ

このリポジトリ(GitHubリポジトリ名: `tt-and-tk/for-pynthesis-skills`)は，自作PCプロジェクト全体で使う，GitHub issueの起票・対応を支援するClaude Codeスキル集(プラグイン)を提供する．

自作PCプロジェクトは，PYNQ-Z2上に実装する自作CPUを含むハードウェア一式と，それを動かすソフトウェア群(コンパイラ・アセンブラ・OS)から成り，複数の独立したGitHubリポジトリで構成される．

```
入力(独自言語Pynesis, .pn) → [コンパイラ] → アセンブリ(自作アセンブリ言語Pyntaxis, .pt) → [アセンブラ] → SystemVerilog ROM(.sv) → [Vivado] → PYNQ-Z2上のハードウェア(qurgeから合成)
```

`pynesis`・`pyntaxis`はそれぞれの言語の名称であり，同時にその言語を処理するツール(コンパイラ・アセンブラ)を格納するGitHubリポジトリ名でもある．

| リポジトリ(GitHub) | ディレクトリ(`pc/`配下) | 役割 |
|:-|:-|:-|
| `specification` | `specification/` | CPUアーキテクチャ・ISA・アセンブリ言語・コンパイラ・Qosmosの仕様のドキュメント(唯一の一次情報源．Claude Codeプロジェクトを持たない) |
| `pyntaxis` | `assembler/` | 自作アセンブリ言語Pyntaxis(`.pt`) → SystemVerilog ROM(`.sv`)へのアセンブラ |
| `pynesis` | `compiler/` | 自作プログラミング言語Pynesis(`.pn`) → アセンブリ言語Pyntaxisへのコンパイラ．`pyntaxis`のソースファイルをincludeして使用し，`.sv`まで一貫変換も可能 |
| `qurge` | `mypc/` | CPU・メモリ・ROM等のハードウェア全体のVivadoプロジェクト(SystemVerilog + PS側C++) |
| (専用のリポジトリなし．`qurge`内) | `mypc/mypc.srcs/pn/` | ROM上で動く自作OS Qosmos(シェルやファイルシステムなど)のPynesisソース．仕様は`specification`の`qosmos.md` |
| `for-pynthesis-skills`(本リポジトリ) | `for-pynthesis-skills/` | 上記各リポジトリで共有するissue起票・対応支援スキルを提供する．特定のリポジトリが主担当と判断できない，全リポジトリに影響するissueの起票先(受け皿)でもある |

## 含まれるスキル

`README.md`を参照．

## スキル間で共有する記述

スキルは個別に読み込まれ共通ファイルを参照できないため，複数のスキルで共有する記述は各スキルの`SKILL.md`に複製することになり，片方だけを直して乖離しやすい．そのためissue対応の手順は，影響リポジトリが1つか複数かによらず`issue-resolve`1つにまとめ，手順の一部を共有する別のスキルを設けない．

複製するのは「コマンド実行の規定」(`issue-create`・`issue-resolve`)だけとし，両スキルの`SKILL.md`で一字一句同一の内容に保ち，片方だけを修正しない(見出し行のレベルは各ファイルの階層に合わせるため一致しなくてよく，同一に保つのは見出し配下の本文である)．スキルを実行している最中に常時従う規律であり，参照にすると読み手が別のスキルを開くまで守るべきことが分からないため，参照ではなく複製する．

## プラグイン内容変更後の更新

このリポジトリの内容(スキル等)を変更するPRがマージされたら，`issue-resolve`の後片付け手順の一環として，以下を実行してインストール済みプラグインを最新化する．

```
claude plugin marketplace update for-pynthesis-skills
claude plugin update for-pynthesis-skills@for-pynthesis-skills
```

反映にはClaude Codeの再起動が必要なため，実行後はユーザーに再起動が必要な旨を伝える．
