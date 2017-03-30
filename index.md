# フロント案件における開発環境

フロント案件においては、以下の様にいくつかのパターンに分けて開発環境を使い分けていきたい。

1. 静的サイトの作成(数ぺージ程度)
2. 静的サイトの大量ぺージ作成(数十~数百ぺージ)
3. 動的サイトの作成(非WP)
4. 動的サイトの作成(wp)

基本的には、1.をベースに、各パターンに沿ったものを追加していく。

## 1.静的サイトの作成(数ぺージ程度)
特にVagrant等開発環境を設ける必要もないと思うので、

* github
* heroku(検証用)
* circleCI(リントチェック用)

にて開発。githubのマスターブランチにプッシュしたらherokuに展開するようにする。
可能ならば、circleCIにて[HTMLproofer](https://github.com/gjtorikian/html-proofer)を用い、
コーディング規約チェックを行った後にherokuにアップする。

### ディレクトリ構造について
herokuではHTMLプロジェクトの選択はできないので、PHPプロジェクトとして登録する。
その際に、下記の様なディレクトリ構造にする

```
├── Procfile
├── README.md
├── composer.json
└── public
    └── index.php
```

README.mdは省略可能。Procfileには`web: vendor/bin/heroku-php-apache2 public/`という記述を、composer.jsonには`{}`を最小構成として記述。

### 課題

* チェックツールの選定(HTML, PHP, JS, CSSの4項目について)
* CSSのチェックも同様にcircleCIにて行える様にする。
* JSのチェックも同様にcircleCIにて行える様にする。[MOCHA](https://mochajs.org/)
* [Code climate](https://codeclimate.com/)ならRuby,PHP,Python,jsのチェックが可能

## 2. 静的サイトの大量ぺージ作成(数十~数百ぺージ)
大量ぺージ作成においては、要素のテンプレート化が必要。
そのため、静的ぺージ作成ツールを用いて行きたい。

* github
* heroku or lollipop(検証用) 
* circleCI(リントチェック用)
* __[middleman](https://middlemanapp.com/jp/) or [gulp-ejs](https://www.npmjs.com/package/gulp-ejs)__(開発環境)

1.に開発環境として、静的ぺージ作成ツールを足したもの。middlemanはrubyベースでerb形式で作成ができる。gulp-ejsはJavaScriptベースでejsという形式にて作成ができる。herokuのslug容量が200MBまでなので、写真なども多い場合は、lollipopのスタンダードプラン(bareリポジトリ作成にて、gitpushにてデプロイを可能にするため)を検証用サーバにする。

### 課題

* middleman 、gulp-ejsどちらが良いか。
* 検証用サーバはheroku、lollipopどちらが良いか。lollipopを選択した場合、circleCIとの繋ぎこみ等で検証要

## 3. 動的サイトの作成(非WP)
おもに、WPへの組み込みは先方が行うから、phpの形式でぺージだけ作ってね、という案件の場合。

* github
* heroku(検証用)
* circleCI(リントチェック用)
* __vagrant__(開発環境)

### 課題

* vagrantを起動するか、mac自体のapacheを起動して開発するのだったらどちらが良いか。

## 4. 動的サイトの作成(wp)
WPの作成を行う必要があるので、WPの開発環境を用いる。

* github
* heroku(検証用)
* circleCI(リントチェック用)
* __[VCCW](http://vccw.cc/) or [MAMP](https://www.mamp.info/en/)__(開発環境)

VCCWはVagrant上でのWordpressの開発環境をサポートしたツール。VCCMを用いる事で、
- プロジェクトごとに開発環境を用意できる(phpのバージョンなどを本番環境と同様に設定可能)
- 多人数開発において共通の環境構築が短時間で可能。(boxイメージのダウンロードを除いて40分程度)
- VCCWに組み込みの[wordmove](https://github.com/welaika/wordmove)という機能を用いる事で、簡単にリモートのWordPress環境をデータベースごとコピー、デプロイが行える。(ローカルでのwp-config.phpの設定は別途必要)

### 課題

* MAMPについてよく調べていないので、どちらが良いか。

