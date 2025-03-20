---
title: "すみません、もうObsidian使ってません…テキストエディタを乗り換えた5つの理由"
source: "https://jmatsuzaki.com/archives/28115"
author:
  - "[[jMatsuzaki]]"
published: 2022-01-13
created: 2025-03-20
description:
tags:
  - "clippings"
image: "https://jmatsuzaki.com/wp-content/uploads/27dd3bbea0f92f4ed59d70dfff1fb5ec.jpg"
PermanentNote:
---
![すみません、もうObsidian使ってません](https://jmatsuzaki.com/wp-content/uploads/27dd3bbea0f92f4ed59d70dfff1fb5ec-800x450.jpg)

私の愛しいアップルパイへ

ナレッジノート [Zettelkasten](https://jmatsuzaki.com/archives/26856) の構築にあたって利用を始めたノートアプリケーション [「Obsidian」](https://jmatsuzaki.com/archives/26813) でしたが、 **半年ほどで使うのをやめて現在は [Neovim](https://neovim.io/) へ乗り換え** ました。

愛すべき読者の方からしばしば「今はObsidianを使っていないのですか？」と聞かれることがあるので、今日はその理由を整理しようと思った次第です。

我が記事を読んでツールを使い始めてくれた方もいるので、Obsidianこそ最強であると謳った責任もあろうと筆を取りました。後日談として使わなくなった理由を明確にしておきます。

Obsidianは優れたツールですし、特別大きな問題があったわけではありません。ただ、 **Zettelkastenにはより適したテキストエディタが存在するため、あえてObsidianを選択する必要がなくなった** という感じです。

率直に述べるなら **Zettelkastenを構築する上でObsidianは最適ではないと判断したため** です。

今回はあなたにとって今後のツール選定の参考になればと、私がObsidianの使用をやめて他のツール（Neovim）への乗り換えることを判断した理由をまとめます。

## Obsidianの使用をやめた5つの要因

### ファイル保存フォーマットの堅牢性が低い

Zettelkastenの構築を始めて以来、どのようなフォーマットでデジタルノートを保存していくべきか試行錯誤を続けてきました。

詳細は [Zettelkastenに適したノートの保存フォーマットについてまとめ](https://jmatsuzaki.com/archives/27754) をご参照ください。

これはノートの取り回しを楽にすると同時に、 **Zettelkastenの堅牢性を高める** 条件として制定しました。ここでの堅牢性とは、 **ソフトウェアの開発が止まったりPCを買い替えたりしても、永続的にZettelkastenを使い続けられるようにすること** です。

Zettelkastenなどのナレッジノートは [第二の脳](https://jmatsuzaki.com/archives/27866) と表現されることもあるだけあって、自らの知識を外部化した結晶です。ある日、ソフトウェアの開発元が破産したとか、PCが起動不可能になったという理由だけでZettelkastenが使えなくなってしまっては困ります。

（Zettelkasten考案者のニクラス・ルーマンがそうしたように） **生涯の相棒として機能するシステム** であることが望ましいものです。

そう考えると、 **Obsidianの仕様は一部堅牢性に欠けています** 。これはObsidianの使用をやめることにした一番の理由でもあります。他の理由も挙げていますが、基本的はこれが最も根本的で決定的な理由です。

中でも特に大きな点をいくつか挙げておく。

まずノートのタイトルがそのままファイル名になるので、 **ノートタイトルを日本語にするとファイル名も日本語になってしまいます** 。これは上述した記事にも書きましたが、非常に脆弱な仕様です。この問題についてはObsidianのフォーラムでも議論されています。 [<sup>1</sup>](https://jmatsuzaki.com/archives/#easy-footnote-bottom-1-28115)

また、Obsidianはデフォルトでノート間のリンクにWikilink形式の記述を採用しています。 **Wikilinkはフォルダ階層の変更に弱いだけでなく、ソフトウェアによって解釈がまちまちであるため堅牢性に欠けるので推奨できません** 。

WikilinkとMarkdown linkは設定で切り替えられるものの、デフォルト設定で使い始めるユーザーも多いでしょうから配慮に欠けていると感じます。かくいう私もデフォルト設定で使い始めてしまったので、後ほど [自分でPythonスクリプトを書いて修正](https://github.com/jmatsuzaki/note-normalization-for-zettelkasten) することになりました。

それから、Markdownフレーバーとして「Multimarkdown」が採用されているのも最適ではありません。汎用性と堅牢性を目指すなら **Pandocの方が望ましい** と考えます。 [<sup>2</sup>](https://jmatsuzaki.com/archives/#easy-footnote-bottom-2-28115) [<sup>3</sup>](https://jmatsuzaki.com/archives/#easy-footnote-bottom-3-28115)

### プラグインの選択肢が少ない

Obsidianは新しいテキストエディタですから、 **プラグインの選択肢がまだまだ少ない** です。Visual Studio CodeやVimなどの大手テキストエディタと比べるとどうしても見劣りしてしまいます。

テキストエディタはアウトプットの効率に直結するため **どこまでカスタマイズできるか** は重要な要素です。日常的に執筆する場合はなお更です。

アウトプットのスタイルというのは人によってまちまちですから、プラグインがどのくらい存在していて開発者がどのくらい存在するかは重要です。 **多ければ多いほどカスタマイズの選択肢が高まるだけでなく、プラグインの開発環境が洗練されていることでもあります** 。必要なプラグインがまだなければ自分で開発するのもやりやすいことになります。

Visual Studio CodeやVimのようにオープンソースを採用しているテキストエディタと比べると、今後もプラグインやその開発スピードが追いつくことはないでしょう。

Obsidianで使えなかった（ないし使い勝手の悪かった）重要なプラグインとしてGitとTextlintがあります。どちらも私にとっては執筆に不可欠のプラグインであったので、Obsidianからの移行を余儀なくされました。

Gitはフルバックアップと差分バックアップを特定の会社の技術へ依存しないために必要です。これはやはりZettelkastenの堅牢性を高めることにつながります。ただし、実際には利便性を考えてDropboxとGitによる二重バックアップを行っています。

Textlintは誤字脱字のチェックを自動化するなどノートの質を高めてくれる校閲作業を自動化・効率化してくれるプラグインです。

### 設定やカスタマイズの柔軟性が低い

プラグインの選択肢が少ないということは柔軟性が低いということでもあります。プラグインだけでなく、 **設定やスクリプト・ショートカットキーの作り込みなど従来のテキストエディタと比べると柔軟性は低いです。**

Obsidianはローカルのプレーンテキスト編集に特化しており、特にナレッジノートの蓄積を主たる目的としたテキストエディタです。用途を限定している分、特徴は分かりやすいのですがカスタマイズにはやはり限界があります。

もちろんこれは用途がシンプルで導入しやすいといったメリットでもあるのですが、Obsidianに興味をもつほどメモやアウトプットにこだわりのあるユーザーにとっては少々物足りないのではと感じました。

特定の用途に特化したノートツールであればクラウドベースのサービスが多数ありますし、プレーンテキストを用いて執筆に集中したいコアなユーザーであればとことんカスタマイズできるテキストエディタの方が向いている気がします。

### 日本語対応に難がある

これについてはObsidianだけ特に問題があるというわけではないのですが、やはり **日本語入力には少々難があります。**

日本ユーザーのほとんどは日本語で文章を執筆することになるでしょうが、プログラミングにおいて日本語というのは非常に独特の配慮が必要で、新しいテキストエディタというのは得てして日本語入力に難があるものです。その後アップデートを重ねる中で少しずつ改善していくことが多いです。

Obsidianもやはり新しいテキストエディタなので日本語入力は難があります。アップデートの中で少しずつ改善はされてきていますが、安定するまで時間はかかるでしょう。

### メインツールにしたくなる特別な機能がない

テキストエディタというライバルが無数にいる界隈でObsidianのような後発のテキストエディタが選ばれるには、相当ユニークな機能が必要です。

Obsidianがもつ機能の中で、他のテキストエディタの機能やプラグインでは代替が難しい機能を強いて挙げるのであれば、 **Markdownファイルのグラフ化機能** ということになるでしょう。Obsidianのグラフ化機能では、 **Markdownファイル間のリンクをネットワーク図のような形で可視化** してくれます。

多くのテキストエディタはMarkdownファイルにだけ特化している訳ではないので、この手の **Markdownファイルに特化したグラフ化機能は珍しいです。** 見た目のインパクトも強く、当初は私自身もこの機能に惹かれました。

では、Zettelkastenのような第二の脳と呼べるノート群を構築するにあたって、このようなMarkdownファイル間のリンク構造をグラフ化する機能がどれほど有効かというと **「ほとんど使わない」** というのが私の結論です。

第一に、Zettelkastenのように第二の脳と呼ばれるようなナレッジノート群において、全体像を把握しようとする努力は無駄です。これはニクラス・ルーマンのZettelkastenを分析した解説本「TAKE NOTES!」で語られていることとも一致します。 [<sup>4</sup>](https://jmatsuzaki.com/archives/#easy-footnote-bottom-4-28115)

**脳というのは取りとめがなく一貫性のないものであり、その概要や全体像を知ることはほとんど不毛な努力で終わる** に違いありません。同じく第二の脳と呼ばれるようなノート群についても、その概要や全体像を知るのは無駄な努力に終わるでしょう。

第二に、ネットワーク図によって提示されるリンク集もやはりさほど有効でないのです。Zettelkastenのようなノートにおいて **何よりも重要なのはノートからノートへ直接貼られる直接リンク** です。そして、その際 **どのような文脈でリンクされているか** が特に重要です。

つまり、 **ノート本文内に自分の言葉で記述されたリンク** です（プログラムによって自動生成されたリンクのリストではなく）。これに集中できていればそれでよいとすら言えます。

自動的に生成されたリンクのリストや関連ノートの暗示などは文脈を持たず、重要性は格段に落ちます。もっといえば **思考の脱線を助長しかねません** 。私自身、Obsidianを使う途中で自動的に生成されたネットワーク図やバックリンクの一覧などが目障りに感じてOFFにした経緯があります。

有名なZettelkasten実践者であるSascha氏は、バックリンクのような自動的なリンク集は無意味どころか有害なことすらあると述べています。今は私もこれに同意します。 [<sup>5</sup>](https://jmatsuzaki.com/archives/#easy-footnote-bottom-5-28115) [<sup>6</sup>](https://jmatsuzaki.com/archives/#easy-footnote-bottom-6-28115)

Zettelkastenは分散した数々のノートを行ったり来たりするには向いていません。むしろ、 **重要なノートへフォーカスすることによって真価を発揮** するものです。ですから、 **ノート内の直接リンクとその文脈だけにフォーカスすると割り切る** くらいがちょうどよいのです。それだけでも驚くほど多くのことを考えられます。それ以外の自動リンクなどはノイズになりかねません。

今後もし考えが変わってネットワーク図などの可視化が必要になったらオープンソースの [Mermaid](https://mermaid-js.github.io/mermaid/#/) などを使って自作すればよいかと考えています。ただし、現時点では必要になることはないであろうと感じています。

もう1つObsidianならではの機能としてMarkdownファイルのリアルタイムプレビュー機能があります。実は、個人的にはこの機能が最もユニークで実用的な機能と感じていたのですが、だからといってこの機能のためにObsidianを採用するほどクリティカルな機能ではありませんでした。

## 結論：あえてObsidianを採用する理由がない

ここで挙げた要因は、ナレッジノートを蓄積する上で特段大きな問題にはならないとも考えられます。しかし一方で、少なくとも私が考える限りにおいては **Zettelkastenを作る上であえてObsidianを選択する理由は見当たりませんでした。** これがObsidianを使わなくなった一番の理由と言えます。

ローカルのテキストファイルを編集するためのテキストエディタには30年近い歴史があり、競合はとてつもなく多いです。歴史が長いほどプラグインの充実度も高いので、あえて後発のテキストエディタを選ぶのは相当特別な理由が必要です。

代表的なものでは [Visual Studio Code](https://code.visualstudio.com/) や [Vim](https://www.vim.org/) 、IDEも含めるなら [Emacs](https://www.gnu.org/software/emacs/) などがあります。日本産であれば [秀丸エディタ](https://hide.maruo.co.jp/software/hidemaru8/index.html) や [サクラエディタ](https://sakura-editor.github.io/) があります。ナレッジノートならニクラス・ルーマンのZettelkastenを研究してソフトウェア化した [Zkn3](http://zettelkasten.danielluedecke.de/) などもあります。

私が愛用しているVimは初回リリースが1991年ですから、ちょうど30年前です。Vimの土台となったviに至っては44年前となる1976年。さらにその前身のedに至っては1973年であり、50年近い歴史があります。

世界的にはこれらの歴史のある成熟したツールを用いてナレッジノートを蓄積したり、書籍を執筆したりする例が数えきれないほどありますし、ノウハウや専用のプラグインもそろっています。

これらのツールはプログラミングを想定しており、Markdownファイルの編集には疎いのではと考えるでしょうか。しかし、プログラムに必ずといってよいほど添付されるReadmeファイルは標準的にMarkdownファイルが使われますので、Markdownに特化した機能やプラグインも十分にそろっています。

そういった中であえてObsidianを選択するのは少々中途半端な選択に思えてきたというのが正直なところです。半年ほど使い込んでいくうちに、 **クラウドベースのノートアプリケーションほどユニークではなく、かといって伝統的なテキストエディタほど高機能でもない** と感じることが増えました。

ObsidianはノートをMarkdownファイルの形式で保存するため、他のテキストエディタへ移行するのは容易です。結局私はほとんど躊躇するなくNeovimへと移行できました。このあたりの移行の容易性はObsidianを選んでよかった点です。

ちなみに [Neovim](https://neovim.io/ "https://neovim.io/") はVim界隈ではここ数年トレンドとなっている新しいフォークです。Neovimの魅力については後ほど語れればと思います。

Vimのような総合的なテキストエディタであれば執筆以外にも使えます。日常的にプログラムを書く私にとってはなおさら都合がよかったです。執筆用とプログラミング用のソフトウェアを統合できるためです。

## 補足：Obsidianを選ぶのが好ましい条件

とはいえ、Obsidianがまったく好ましくない選択肢かというとそういうわけでもありません。

まず、 **Markdownファイルの編集だけに特化したテキストエディタというのは間違いなくニッチでユニーク** ですから、今後の発展に期待したいです。

Obsidianに特別大きな問題はないのですでにObsidianを使っていて、Obsidianを気に入っているのであればそのまま使い続けるのが良いでしょう。

これからテキストエディタを使い始める場合、現時点でObsidianを選ぶのが好ましいユーザーや条件は、 **マイルドなZettelkasten実践者** です。ここにはEvergreen notesやLYT、PARAなどZettelkastenからの派生も含みます。

Zettelkastenほど厳密なシステムを使うことに抵抗のあるマイルドな用途であればObsidianは十分に選択肢になり得ます。特に、 **これからお試しでZettelkastenなどのナレッジノートを書き始めるビギナー** にとってはとっつきやすいと言えるでしょう。

ローカルのMarkdownファイル管理に慣れてきたら、より高機能で専門的なテキストエディタを選べばよいでしょう。

貴下の従順なる下僕　松崎より

## 参考文献

1. [Use H1 or front-matter title instead of or in addition to filename as display name, sam.baron, Obsidian, 2020/05/08](https://forum.obsidian.md/t/use-h1-or-front-matter-title-instead-of-or-in-addition-to-filename-as-display-name/687/103 "https://forum.obsidian.md/t/use-h1-or-front-matter-title-instead-of-or-in-addition-to-filename-as-display-name/687/103")
2. [Using pandoc | Pandoc User’s Guide](https://pandoc.org/MANUAL.html#pandocs-markdown "https://pandoc.org/MANUAL.html#pandocs-markdown")
3. [John MacFarlane, Pandoc vs Multimarkdown, jgm/pandoc Wiki, Sep. 08, 2016.](https://github.com/jgm/pandoc/wiki/Pandoc-vs-Multimarkdown "https://github.com/jgm/pandoc/wiki/Pandoc-vs-Multimarkdown")
4. [TAKE NOTES!――メモで、あなただけのアウトプットが自然にできるようになる p.176, Sönke Ahrens, 日経BP, 2021/10/14](https://amzn.to/3qjlISW "https://amzn.to/3qjlISW")
5. [Backlinking Is Not Very Useful — Often Even Harmful, Sascha, Zettelkasten Method, 2020/11/12](https://zettelkasten.de/posts/backlinks-are-bad-links/ "https://zettelkasten.de/posts/backlinks-are-bad-links/")
6. [RE: Backlinks Should Be Context-Rich, Sascha, Zettelkasten Method, 2021/07/20](https://zettelkasten.de/posts/re-backlinks-should-be-context-rich/)

2022-01-13