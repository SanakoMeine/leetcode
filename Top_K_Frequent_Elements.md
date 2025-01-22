# 347. Top K Frequent Elements

## 参考にした方々（Pythonで書かれた直近５名）
- https://github.com/rinost081/LeetCode/pull/10/files
- https://github.com/BumbuShoji/Leetcode/pull/10/files
- https://github.com/t0hsumi/leetcode/pull/9/files
- https://github.com/ichika0615/arai60/pull/7/files
- https://github.com/olsen-blue/Arai60/pull/9/files

## Step 1
### 考えたこと
- 辞書を使いそう…だけど具体的にどうするかまで全くつながらなずギブアップ(getすら知らなかった…)、LeetCodeから答えをみて通るまで試す。
- “for key, val”とかから使い慣れていないし、keyとvalをタプルにして一緒にheappushできるのも知らなかったしで、この辺の処理は今の自分からは絶対出てこないな…。

```Python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        heap = []
        counter = {}

        for num in nums:
            counter[num] = 1 + counter.get(num, 0)

        for key, val in counter.items():
            heapq.heappush(heap, (-val, key))
        
        res = []
        while len(res) < k:
            res.append(heapq.heappop(heap)[1])

        return res
```

## Step 2
### 学んだこと・思ったこと
- 全く知らなかった操作もあったとはいえ、過去の問題に連想が行ったらrinostさんのstep1みたいなアプローチが出来たのか…。あの時辞書の操作がしっくりきておらず、結果として記憶に残らなかった感じがある。
- 全部理解できなくてもいいや精神で一度公式ドキュメント¶に目を通しとこう。
- Step1の回答にあった-1をかけてheappopした値をappendする方法より、heappopで頻度の小さい値を除外する方が手間が少なそう
- Defaultdictを使うとキーの初期化なしで頻度を数えれるのか¶
- 文字の種類がk以下な入力は確かにありそう。みんなValueErrorをいれてる。
- FYさんのステップ5が自然に頭に入ってきたのでこれをお手本にしたい
- Step1で参照した回答ではheapという名前が使われてたけど、実際にはheapにはtop_k_hogeみたいな名前が似合うことが多い印象（これまで見てきたものがpopすることを見越してたからな気がする）
- ソート/セレクト周りのコードを読む元気がない（そもそもよく知らないのですごく工数がかかりそう…）ので余力がある時にまた…。
- 余談：PRを読んでいると、この辺の問題から公式ドキュメントを読みたい気持ちになっている方が他にもいらっしゃって面白い。更にできる人たちになると自力で色んな公式ドキュメントを連想できてい流のに加えて、関連するCPythonのコードを読み初めてる感じがある。この辺真似できるといいんだろうけれど、今のところ何が書いてあるか理解してコードを改良する＆先人のPRやdiscordを引っ張ってくるのがやっとで、負荷的に真似できない。が、公式のドキュメントをチビチビ読むようになったのは進歩な気がする。

```Python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        frequencies_of_appearance = defaultdict(int)

        for num in nums:
            frequencies_of_appearance[num] += 1

        if k > len(frequencies_of_appearance):
            raise ValueError("Fewer than k types of letters appear in nums")

        # 名前が長い...
        sorted_frequencies = sorted(
            frequencies_of_appearance, key=frequencies_of_appearance.get, reverse=True
        )

        top_k_elements = sorted_frequencies[:k]

        return top_k_elements
```

## Step 3
### コメント
- 1回目： 5m 4s, 2回目：4m47s, 3回目：4m8s

```Python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        numbers_of_appearance = defaultdict(int)

        for num in nums:
            numbers_of_appearance[num] += 1

        if numbers_of_appearance < k:
            raise ValueError('Fewer types of letters appear in input nums')

        sorted_numbers_of_appearance = sorted(numbers_of_appearance, key=numbers_of_appearance.get, reverse=True)

        top_k_elements = sorted_numbers_of_appearance[:k]

        return top_k_elements```

