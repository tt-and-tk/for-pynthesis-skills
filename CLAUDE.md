# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト全体における位置づけ

このリポジトリ(GitHubリポジトリ名: `tt-and-tk/for-pynthesis-skills`)は，自作PCプロジェクト全体で使う，GitHub issueの起票・対応を支援するClaude Codeスキル集(プラグイン)を提供する．

自作PCプロジェクトは，PYNQ-Z2上に実装する自作CPUを含むハードウェア一式と，それを動かすソフトウェア群(コンパイラ・アセンブラ)から成り，複数の独立したGitHubリポジトリで構成される．

```
入力(.pn) → [コンパイラ pynesis] → アセンブリ(.pt) → [アセンブラ pyntaxis] → SystemVerilog ROM(.sv) → [Vivado] → PYNQ-Z2上のハードウェア(qurgeから合成)
```

| リポジトリ(GitHub) | 役割 |
|:-|:-|
| `specification` | CPUアーキテクチャ・ISA・アセンブリ言語・コンパイラ仕様のドキュメント(Claude Codeプロジェクトを持たない) |
| `pynesis` | 独自C系言語(`.pn`) → アセンブリ言語へのコンパイラ |
| `pyntaxis` | アセンブリ言語(`.pt`) → SystemVerilog ROM(`.sv`)へのアセンブラ |
| `qurge` | CPU・メモリ等を含むハードウェア一式のVivadoプロジェクト(ハードウェア実装) |
| `for-pynthesis-skills`(本リポジトリ) | 上記各リポジトリで共有するissue起票・対応支援スキル(`issue-create`/`issue-resolve`)を提供する．自身はハードウェア・コンパイラ・アセンブラのソースを持たない |

## 含まれるスキル

`README.md`を参照．
