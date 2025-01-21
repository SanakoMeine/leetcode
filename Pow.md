# 50. Pow(x, n)
ブランクが空いたので前回指定した「次に解く問題」ではなく、別の問題の別解として馴染みのある再帰の問題を選びました。

## 参考にした方々（Pythonで書かれた直近５名）
- https://github.com/nittoco/leetcode/pull/17/files
- https://github.com/Mike0121/LeetCode/pull/17/files
- https://github.com/Ryotaro25/leetcode_first60/pull/48/file
- https://github.com/Yoshiki-Iwasa/Arai60/pull/38/files

## Step 1
### 考えたこと
- まずは再帰で書いてみる。
- n = 0の場合をベースケースとして、+-で掛け算と割り算を繰り返せば良いと思った。他の方のコードで呼び出し上限について言及してるの見たことあるな、こういう風に困るのか…。
- が、呼び出し回数に引っかかって通らない。ここでギブアップ。

下記、通りません：
```Python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        elif n <= -1:
            return self.myPow(x, n + 1) / x
        else:
            return x * self.myPow(x, n - 1)
```

- ここで答えを見ると、x^n = (x^(n/2))^2の操作を繰り返すと計算回数を減らせると知ったので書き直してみる。
- 愚直にPowを２回呼び出すと結局再帰呼び出し回数に引っかかってしまうので、temp的な変数（half…）に入れ直す
- nが小数の時とかどうしてるんだろう→先人のgitにPowのプログラムへのリンクがあり、生まれて初めてCPythonのソースコードを読んでみたい気になる。が、手も脚も出ない：https://github.com/python/cpython/blob/d5ba4fc9bc9b2d9eff2a90893e8d500e0c367237/Objects/longobject.c#L4849

```Python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        elif n < 0:
            n = -n
            x = 1 / x

        if n % 2 == 0:
            half_even = self.myPow(x, n / 2)
            return half_even * half_even
        if n % 2 == 1:
            half_odd = self.myPow(x, (n - 1) / 2)
            return x * half_odd * half_odd
```


## Step 2
### 学んだこと
- モジュラ逆数？(https://github.com/nittoco/leetcode/pull/17/files) 公式へのリンクが貼ってあって、合同式の中で逆数をどう定義するかの話らしい ¶。例がピンとこないのでネットに落ちてた記事に頼る https://qiita.com/Cper0/items/1dc7e7db1eddcbfd259f。
- みんな浮動小数点の話をしてる。 → 2^(-n)の扱いや型を変換した際に丸め誤差などが乗ることを恐れているっぽい。確かにうっすら怖かったが、自分の場合それに対処できるほど意識を回せていない。とりあえずこれに目を通してみたところ、精度を改善するための方法がいろいろあるらしい：https://docs.python.org/ja/3.5/tutorial/floatingpoint.html
- 末尾再帰とその最適化という言葉を初めて知った。その関数自身のみを呼び出す形にすることで計算回数を減らす考え方らしい。：https://zenn.dev/kj455/articles/dfa23c8357b274
- 「再帰は若干認知負荷が高くて読みにくいよね」みたいな話が以前にもあったのでwhile文で書いてみる
- accumulated_prodはわかりやすくて良い。Cpythonは読めなかったけど、accumみたいな名前の付け方をしていた。
- 時間の都合でギブしたが後でもう一度この方のPRを読む：https://github.com/fhiyo/leetcode/pull/46/files。bit演算するアプローチも追えてないが、計算誤差避けの手段っぽい。

```Python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n < 0:
            n = -n
            x = 1 / x
        elif n == 0:
            return 1

        accum_prod = 1

        while n > 0:
            if n % 2 == 1:
                accum_prod = accum_prod * x
            x = x * x
            n = n // 2
            print(f"{accum_prod}")

        return accum_prod
```

## Step 3

### コメント
- 先人のPRを見ているとwhileの方が可読性が高いのだろうと思いつつ、苦手だった再帰を書ける様にしておこう＆めずらしく再帰の方がしっくり来たので、まずは再帰で３回通してみる。エンジニアリングの趣旨から逸脱するのが怖いので、whileでもその後練習してみる。
- 1回目：5:36, 2回目：3:42 3回目：2:15

### 再帰で書いた方
```Python
        if n == 0:
            return 1.0

        if n < 0:
            x = 1 / x
            n = -n

        if n % 2 == 0:
            half_pow = self.myPow(x, n / 2)
            return half_pow * half_pow
        else:
            half_pow = self.myPow(x, (n - 1) / 2)
            return x * half_pow * half_pow
```

### whileで書いた方
```Python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        if n < 0:
            n = -n
            x = 1 / x

        accum_prod = 1

        while n > 0:
            if n % 2 == 1:
                accum_prod *= x
            x *= x
            n //= 2

        return accum_prod
```
