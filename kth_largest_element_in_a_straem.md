# 703. Kth longest elements
問題文：https://leetcode.com/problems/kth-largest-element-in-a-stream/description/
次に解く：https://leetcode.com/problems/top-k-frequent-elements/description/

## 参考にした方々（Pythonで書かれた直近５名）
- https://github.com/rinost081/LeetCode/pull/9/files
- https://github.com/t0hsumi/leetcode/pull/8/files
- https://github.com/frinfo702/software-engineering-association/pull/11/files
- https://github.com/ichika0615/arai60/pull/8/files
- https://github.com/olsen-blue/Arai60/pull/8/files

## Step 1
### 考えたこと
- まずはリストとsorted関数で実装してみるも、入力のaddが多い時に計算時間に引っかかる。
- Kとnがどっちも10^4くらいあるし、O(nk)=O(n^2)オーダーになってるからはみ出すのかな

※下は動きません
```Python
# @lc code=start
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.nums = nums

    def add(self, val: int) -> int:
        self.nums.append(val)
        sorted_nums = sorted(self.nums)

        kth_largest = sorted_nums[-self.k]

        return kth_largest
```
- 答えを見ると計算量がO(nlogn)になる様にheapを使うのが普通らしいのでこれを真似してみる。
- 問題的にはいいんだけど、numにaddされた後のリストとか保存しておかなくていいのかな…？

```Python
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.nums = nums
        heapq.heapify(self.nums)

        while len(self.nums) > k:
            heapq.heappop(self.nums)

    def add(self, val: int) -> int:
        heapq.heappush(self.nums, val)

        if len(self.nums) > self.k:
            heapq.heappop(self.nums)
        kth_largest = self.nums[0]

        return kth_largest
```


## Step 2
### 学んだこと
- self.nums=numsとしてもミュータブルなリストは参照渡しされるのか…。copy()しておこう：https://github.com/frinfo702/software-engineering-association/pull/11/files
- 読んだことないならheapの公式ドキュメントは読んでおこうとのこと：https://docs.python.org/3/library/heapq.html#heapq.heappushpop
- Heapifyする代わりにaddで回すのは読みやすくて行数も短いので確かに良さそう。
- top_k_heapはわかりやすいので頂戴しよう
- Heapを知らない自分でも解ける方法はあったのか…：https://github.com/rinost081/LeetCode/pull/9/files のstep1
- sort()は破壊的処理をするのでNoneが帰ってくる：¶ - し、Noneが帰ってくるのもユーザーに気づかせるためだったのか、すごく考えられてて少し感動 - 「シーケンスをインプレースに変化させます」等の意味がすぐに取れないから日本語ですら公式ドキュメントを敬遠してしまってるのだとわかる。
- While使うよりこれの方が良さそう（step1では回答を真似したが、最初はこういうセンスで書こうとしたので、受け入れやすくもある）：https://github.com/rinost081/LeetCode/pull/9/files#r1874734978
- 言語ごとの処理速度の差：https://github.com/ichika0615/arai60/pull/8/files#r1898337850

```Python
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.nums = []

        for num in nums:
            self.add(num)

    def add(self, val: int) -> int:
        heapq.heappush(self.nums, val)

        if len(self.nums) > self.k:
            heapq.heappop(self.nums)

        return self.nums[0]
```

## Step 3
### コメント

- この書き方だと’add’という関数名が微妙に実態からずれて気もする（k番目の値より小さい要素を削ったりしているので）けど、問題文でaddと指定されているのでよしとしよう
- 変数名をtop_k_valueにしただけで今何をしているのかわかりやすくなり、コードが格段に頭に入りやすくなる現象を確認しておもしろい
- 1回目：3m27sec, 2回目：3min10sec, 3回目：1min49sec

```Python
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.top_k_value = []

        for num in nums:
            self.add(num)

    def add(self, val: int) -> int:
        heapq.heappush(self.top_k_value, val)

        if len(self.top_k_value) > self.k:
            heapq.heappop(self.top_k_value)

        return self.top_k_value[0]
```

