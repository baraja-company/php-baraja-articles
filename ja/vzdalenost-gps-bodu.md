GPS2点間の距離
=========

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	ja: gps2dian-jianno-ju-li
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

このアルゴリズムにより、2つのGPS地点のおおよその距離を簡単に計算することができます。

PHPの実装
------------------

```php
function getCoordsDistance(
	float $lat1,
	float $lng1,
	float $lat2,
	float $lng2
): float {
	$r = 6371;
	$lat1 = ($lat1 / 180) * M_PI;
	$lat2 = ($lat2 / 180) * M_PI;
	$lng1 = ($lng1 / 180) * M_PI;
	$lng2 = ($lng2 / 180) * M_PI;

	$x = ($lng2 - $lng1) * cos(($lat1 + $lat2) / 2);
	$y = $lat2 - $lat1;
	$d = sqrt($x * $x + $y * $y) * $r;

	return round($d * 1000);
}
```

この関数は、地球が完全な球体であると仮定しているので、出力はあくまで推定値として使用してください。現実的な距離計算をするためには、高度、地形の形状、局所的な平坦化などを考慮する必要があります。

この機能は、eショップの店舗間距離を推定するのに使っています。チェコでは、1kmごとに平均5m程度の偏差があり、実際の運用では十分である。

TypeScriptでの実装
--------------------------

クライアント側で計算する必要があるかもしれませんが、その場合はjavascriptが便利です。

```js
export const getCoordsDistance = (
    lat1: number,
    lng1: number,
    lat2: number,
    lng2: number
): number => {
  const r = 6371;
  const subLat1 = (lat1 / 180) * Math.PI;
  const subLat2 = (lat2 / 180) * Math.PI;
  const subLng1 = (lng1 / 180) * Math.PI;
  const subLng2 = (lng2 / 180) * Math.PI;

  const x = (subLng2 - subLng1) * Math.cos((subLat1 + subLat2) / 2);
  const y = subLat2 - subLat1;
  const d = Math.sqrt(x * x + y * y) * r;

  return Math.round(d * 1000 * 100000000) / 100000000;
};
```

データベースから近くの場所を検索する方法
------------------------------------

特定の点を中心としたデータベース内の周辺位置を求めるには、上に挙げた関数の高価な計算をSQLで直接行うか、スマートに行うかのどちらかです。

個人的には、2つのアルゴリズムを組み合わせて、近くの場所（例えば、Eストアーからお客さんにチェックアウトする支店）を探すのに使っています。

最初のステップでは、選択した点の周囲に四角い領域を読み込むだけです（目的の点から一定値を引き、その間隔を検索するだけで得られます）。これによって、結果の候補のリストが得られるが、それらがどの程度離れているのか、またその順番についてはまだ何もわかっていない。

第二段階では、特定の円の中にある点を探すのか、それとも正方形上のもっと離れた点を探すのかを考える必要がある。個人的には、このカディデイトのリストを循環させて、それぞれの結果の出発点からの距離を計算し、計算した距離で結果を並べることをお勧めします。

3番目のステップでは、簡単なフィルターを使って、定義された円から必要な距離以上離れている場所を削除することができます。
