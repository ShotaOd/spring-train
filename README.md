# spring-train
## 第二回 Spring DI
- アノテーション ベース  
- Bean定義ファイル  
- Javaプログラム(JavaConfig)  

#### DI by アノテーション
アノテーションベースでだが、設定ファイル必要  
登場人物
```Java
@Autowired // 呼び出し側
@Component // DI対象ファイルに付与する
@
@
```

## 第xxx回 備考  

### 金額には、BigDecimal or Ratio型を使う。  
> 参考
- [もう一つの金額計算のやり方]("http://qiita.com/kawasima/items/4be8501f2fc32004572e")  
- [金勘定のためのBigDecimalそしてMoney and Currency API](http://www.slideshare.net/miyakawataku/bigdecimal-for-money-counting)  

> まとめ
- 丸め誤差の問題
 - 2進数と10進数での数値の表現方法の違いに由来
-

### BigDecimal
> - 10進数での数値管理
 - BigInteger
- スケールによる小数点管理
 - 小数点を右端から、何桁左に動かすか
- イミュータブル
 - 演算結果等は、新しい値がnewされる。  

> ``` java
new BigDecimal("12.345")
```

> | 実際の数値 | 10進数 | スケール |
|:------:|:-----:|:--:|
| 123    | 123   | 0  |
| 123.45 | 12345 | 2  |
| 12300  | 123   | -2 |

> #### 嵌りそうなポイント
- 等価評価では、10進数とスケールを併せて比較  

> ``` java:sample.java
// ダメな例
BigDecimal total = new BigDecimal("100");
BigDecimal partialA = new BigDecimal("66.6");
BigDecimal partialB = new BigDecimal("33.4");
total.equals(partialA.add(partialB)) // false  
// スケールを統一
BigDecimal total = new BigDecimal("100.0"); // スケールを統一
BigDecimal partialA = new BigDecimal("66.6");
BigDecimal partialB = new BigDecimal("33.4");
total.equals(partialA.add(partialB)) // true  
// もしくは、compareを使う
BigDecimal total = new BigDecimal("100");
BigDecimal partialA = new BigDecimal("66.6");
BigDecimal partialB = new BigDecimal("33.4");
total.compare(partialA.add(partialB)) // true  
```

> - 初期化時注意

> ```java
// Bad
new BigDecimal(123.45) //123.45がDouble型になるので 丸め込み発生
// 文字列にする
new BigDecimal("123.45")
```
