来年、人工知能がプログラマーから仕事を奪ったらどうするか？
=============================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	ja: lai-nian-ren-gong-zhi-nenggapuroguramakara-shi-shiwo-duottaradousuruka
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

OpeanAI言語モデルの可能性はすごいですね。ソースコードを入力し、解析、修正、他言語への書き換えを依頼するだけで、すべてが自動的に行われます。

初めて試したとき、何が実現できるのか、そして一般の人でも使える強力なツールであることに衝撃を受けました。

例を見てみましょう。

**質問:** この関数のセキュリティ脆弱性を見つけて、パッチを適用したコードを提供してください。

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'1月',
		'2月',
		'3月',
		'4月',
		'5月',
		'6月',
		'7月',
		'8月',
		'9月',
		'10月',
		'11月',
		'12月',
	];

	return $czMonths[(int) date('m')];
}
```

**回答：***

この機能のセキュリティ上の脆弱性として、SQLインジェクション攻撃を悪用される危険性があります。不適切にフォーマットされた入力や悪意のある入力が関数に渡されると、データベースが危険にさらされたり、悪用されたりする可能性があります。

修正方法としては、date()関数の呼び出しで得られた入力の代わりに、定義済みの月定数を使用することです。また、入力が数値であり、悪意のあるコードが含まれていないことを確認するために、適切なフィルターを用いて filter_var() 関数を使用することができます。

以下は、修正後のコードです。

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'1月',
		'2月',
		'3月',
		'4月',
		'5月',
		'6月',
		'7月',
		'8月',
		'9月',
		'10月',
		'11月',
		'12月',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Question:** 関数の実装をタイプスクリプトに変換してください。

**回答：***

以下は、この関数をTypeScriptで書き直した実装です。

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if (month === false || month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

**Request:** This is not valid typescript!

**回答：***

申し訳ございませんでした。以下は、この関数のTypeScriptでの修正版です。

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = Number(new Date().getMonth()) + 1;

	if (month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

注：TypeScriptにはfilter_var()関数がないので、DateクラスからgetMonth()関数を呼び出し、Number()関数で数値に変換することで代用しました。このバージョンの関数は、TypeScriptで有効なはずです。

**質問:** あなたの実装しているTypescriptにセキュリティの脆弱性がありますか？

**回答：***

私が修正したTypeScriptでの関数の実装は、元のPHPの実装に元々あったSQLインジェクション攻撃exploitのセキュリティ脆弱性を含まないはずです。 DateクラスのgetMonth()関数は月の数値（1月は0、2月は1など）を返すので、date()関数からの入力を渡す必要がなく、SQLインジェクション攻撃exploitの危険性を回避することができるのです。

しかし、どのようなアプリケーションにもセキュリティ上の脆弱性が存在する可能性があるため、定期的にコードを検証し、潜在的なセキュリティ上の問題を探すことが重要です。コードにセキュリティ上の脆弱性があると思われる場合は、セキュリティの専門家に問い合わせるか、自動セキュリティ脆弱性スキャンツールを使用することができます。
