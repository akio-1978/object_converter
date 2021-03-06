# object_converter
convert class instance and built-in containers to each other.

ヘタ英語はここまで。
このリポジトリは[qiita](https://qiita.com/Akio-1978/items/29c480244d2961961834)に記事投稿した際に作成したものです。

## install
自分で使うために書いておきます。
```sh
pip install git+https://github.com/akio-1978/object_converter#egg=objectconverter
```

## 説明
記事の方でもう少し詳しく説明していますが…
dictやlistで作られた汎用コンテナと任意のクラスを相互変換します。

個人的には便利たけど、使う人はいないよね、きっと世の中にはもっと良いものがあるはずだから。

### 汎用コンテナの例
```python
src_dict = {
    'value' : 'AAA',
    'nested' : {
        'nested_value' : 'BBB'
    }
}
```

### クラスの例
```python
# クラスその1
class TestClass():
    def test_method(self):
        return 'assigned value: ' + self.value
# クラスその2 - その1にぶら下がるイメージ
class NestedTestClass:
    def test_method(self):
        return 'nested assigned value: ' + self.nested_value
```

### 変換のルールを定義するdict
```python
mapping = {
    '<MAPPING_ROOT>' : TestClass, # 最上位は名前がないので'<MAPPING_ROOT>'とする
    'nested' : NestedClass
}
```

`<ROOT_CLASS>`:データ構造の最上位の要素を示す仮の名前です。
`nested`:dictの中のキー"nested"に対して、このキーの値の関数を実行します。関数は何らかのオブジェクトを返さなければなりません。返されたオブジェクトに値が設定されていきます。

### 呼び出し方
```python:usage
# コンストラクタでマッピングを取る
converter = ObjectConverter(mapping=mapping)
# 変換メソッドに変換元データを渡す
converted_class = converter.convert(src_dict)
# 変換されたクラスのメソッドを呼ぶ
converted_class.test_method()
```
こんな感じです。
