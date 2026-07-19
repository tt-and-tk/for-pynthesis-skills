# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト全体における位置づけ

このリポジトリ(GitHubリポジトリ名: `tt-and-tk/for-pynthesis-skills`)は，自作PCプロジェクト全体で使う，GitHub issueの起票・対応を支援するClaude Codeスキル集(プラグイン)を提供する．

自作PCプロジェクトは，PYNQ-Z2上に実装する自作CPUを含むハードウェア一式と，それを動かすソフトウェア群(コンパイラ・アセンブラ)から成り，複数の独立したGitHubリポジトリで構成される．

```
入力(独自言語Pynesis, .pn) → [コンパイラ] → アセンブリ(自作アセンブリ言語Pyntaxis, .pt) → [アセンブラ] → SystemVerilog ROM(.sv) → [Vivado] → PYNQ-Z2上のハードウェア(qurgeから合成)
```

`pynesis`・`pyntaxis`はそれぞれの言語の名称であり，同時にその言語を処理するツール(コンパイラ・アセンブラ)を格納するGitHubリポジトリ名でもある．

| リポジトリ(GitHub) | ディレクトリ(`pc/`配下) | 役割 |
|:-|:-|:-|
| `specification` | `specification/` | CPUアーキテクチャ・ISA・アセンブリ言語・コンパイラ仕様のドキュメント(Claude Codeプロジェクトを持たない) |
| `pynesis` | `compiler/` | 独自言語Pynesis(`.pn`)をアセンブリ言語Pyntaxis(`.pt`)に変換するコンパイラ |
| `pyntaxis` | `assembler/` | 自作アセンブリ言語Pyntaxis(`.pt`)をSystemVerilog ROM(`.sv`)に変換するアセンブラ |
| `qurge` | `mypc/` | CPU・メモリ等を含むハードウェア一式のVivadoプロジェクト(ハードウェア実装) |
| `for-pynthesis-skills`(本リポジトリ) | `for-pynthesis-skills/` | 上記各リポジトリで共有するissue起票・対応支援スキル(`issue-create`/`issue-resolve`)を提供する．自身はハードウェア・コンパイラ・アセンブラのソースを持たない |

## 含まれるスキル

`README.md`を参照．

## 本リポジトリ専用のスキル(プラグイン非配布)

`.claude/skills/cross-project-edit`: 複数プロジェクトに横断的に影響する修正を1issue・1セッションでまとめて行うスキル．`for-pynthesis-skills`の`.claude`ディレクトリにのみ配置し，プラグインとして他プロジェクトには配布しない．
