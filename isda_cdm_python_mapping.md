# ISDA CDM Python向けの基盤技術要素・リポジトリ整理マップ

| 技術要素 / 成果物 | 役割（一言でいうと） | 具体的な機能・CDMにおける位置づけ | FINOSリポジトリでの管理場所 | 開発・基盤主体 |
| :--- | :--- | :--- | :--- | :--- |
| **文法定義ファイル**<br>`(.xtext)` | **文法のルールブック** | Rune DSL（Rosetta）の文法規則そのものをXtextに教えるための定義ファイル。 | **`finos/rune-dsl`**<br>*(言語の基盤コード側)* | **ISDA / FINOS**<br>*(Xtextの仕組みを利用)* |
| **Rune DSL**<br>*(旧 Rosetta DSL)* | **人間用の記述言語** | 金融取引のデータ構造、契約のライフサイクル、計算ロジックなどを記述するための専用テキスト言語。 | **`finos/common-domain-model`**<br>*(この言語を使って大量の `.rosetta` ファイルが記述されている)* | **ISDA / FINOS** |
| **Xtext** | **文法の解釈・翻訳者** | `.xtext` ファイルに基づき、開発者が書いた `.rosetta` テキストを解析（パース）してEcore表現に翻訳するフレームワーク。 | （`rune-dsl` のビルド時やIDEプラグインの内部ライブラリとして組み込まれる） | **Eclipse Foundation**<br>*(主導：itemis社、TypeFox社)* |
| **Ecore表現** | **共通の設計図** | 特定のプログラミング言語に依存しない、純粋なデータ構造や関係性を表現したメタモデル（設計図）。Xtextの翻訳結果は一度ここに格納される。 | （`rune-dsl` 内のJavaメモリ上でモデルオブジェクトとして扱われる） | **Eclipse Foundation** |
| **EMF**<br>*(Eclipse Modeling Framework)* | **モデル駆動開発の土台** | Ecore表現をベースにして、データモデルの管理や、各種ソースコードを自動生成するための基盤フレームワーク（工場）。 | （ジェネレーターのコア基盤として依存関係に含まれる） | **Eclipse Foundation** |
| **Xtend** | **ジェネレーターの記述言語** | **Pythonコードを生成するプログラム自体を記述するための言語。** テンプレート式が強力なため、EcoreからPythonコードを組み立てるロジックの多くはJavaではなくXtend（`.xtend`）で書かれている。 | **`finos/rune-dsl`** や<br>**`rosetta-code-generators`** の内部コード | **Eclipse Foundation**<br>*(主導：itemis社、TypeFox社)* |
| **Rosetta Python Code Generator** | **Pythonコード生成器** | 上記のXtend等で実装されたジェネレーター本体。Ecoreモデルを解析し、Python用のクラス構造（`ObjectGenerator`）や列挙型（`EnumGenerator`）を組み立てて出力する。 | **`finos/rune-python-generator`** など<br>*(各ジェネレーターリポジトリ)* | **ISDA / FINOS**<br>*(共同開発：TradeHeader社等)* |
| **Pydantic**<br>*(ターゲット依存技術)* | **Pythonでのデータバリデーション** | 生成されたPythonコードが依存する強力なライブラリ。CDMの持つ「必須・任意」などの多重度（Cardinality）や型制約をPython上で厳密に担保する。 | （出力されたPythonコードを動かす実行環境/仮想環境にて `pip install pydantic` して使用） | **Pydantic社 (オープンソース)** |
| **生成されたPythonコード**<br>`(.py)` | **Python用CDM成果物** | Python開発者が自身のプロジェクトにインポートして、CDM規格に準拠した取引データの作成、バリデーション、統合を行うために利用するライブラリ。 | パッケージ「**`cdm`**」等として配布、またはローカルビルドして利用 | **FINOS / 各開発者** |
