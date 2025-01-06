# 206.Reverse Linked Listを解きました。レビューの程よろしくお願いします。

## ## 参考にした方々（Pythonで書かれた直近５名）
- https://github.com/olsen-blue/Arai60/pull/7/files
- https://github.com/t0hsumi/leetcode/pull/7/files
- https://github.com/rinost081/LeetCode/pull/8/files
- https://github.com/katataku/leetcode/pull/7/files
- https://github.com/ichika0615/arai60/pull/6/files

## Step 1
### 考えたこと
- Stackの問題らしい。Linked Listを順番に読んでいってstackに格納して、逆順に読み出した値を補完するLinked Listを番兵付きで作れば良さそう
- が、Memory Limit Exceededで動かずギブアップ。下記は動かなかった時のコード。

```Python
# Memory Limit Exceededで動かないコード
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        scan = head
        stack = []

        while scan:
            stack.append(scan)
            scan = scan.next

        sentinel = ListNode(0, scan)
        reversed = sentinel

        while stack:
            reversed.next = stack[-1]
            stack.pop()
            reversed = reversed.next

        return sentinel.next
```
- 他の人の回答をみてると全部のnextの情報を繋ぐのをやめてメモリを節約するとMemoryLimitに収まるらしいので書いてみる 。printしてみるとリストの各要素がnextの果てまで記録していて、空間計算量がO(N^2)になっていた様で、全く気づいてなかった。
- reversed = stack[-1]をする必要はなくて、reversed = stack.pop()とするだけで消したvalueを返してくれる

```Python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        scan = head
        stack = []

        while scan:
            stack.append(scan)
            scan = scan.next

        sentinel = ListNode(0, scan)
        reversed = sentinel

        while stack:
            last_node = stack.pop()
            reversed.next = ListNode(last_node.val)
            reversed = reversed.next

        return sentinel.next
```

## Step 2
### 学んだこと
- List.reverese()で一発で逆順にしてくれる
- Stackを使う方法、再帰を使う方法（https://github.com/goto-untrapped/Arai60/pull/27/files/14646ec0859dd9411e6983bf6c63e6f15a1f9f32#r1638693522）、 切断して繋ぐ方法、後ろから繋いでいく方法があった。後ろから繋いでいく方法が短くて綺麗。こちらの方が一通りやられている： https://github.com/ichika0615/arai60/pull/6/files


### 後ろから繋いでいく方法
```Python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        original_node = head
        reversed_node = None

        while original_node:
            reversed_node = ListNode(original_node.val, reversed_node)
            original_node = original_node.next

        return reversed_node
```

### 切断して繋ぎ変えていく方法（reversedとforwardの２つのLinked Listを用意して、headから前進させつつその前の値をreversedに格納）
- While内の１番上はtempに入れてるだけ、２行目が繋ぎ変えの操作、下２行は２つのノードの更新処理
```Python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        scan = head
        reversed = None

        while scan:
            scan_next = scan.next
            scan.next = reversed
            reversed = scan
            scan = scan_next

        return reversed
```


## Step 3
### コメント
- 切断してから繋ぐやり方の理解が怪しい（過去の問題のPRで指摘され中）のでこれで書いてみる
- 2:30, 1:57, 1:55

```Python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        scan = head
        previous_scan = None

        while scan:
            # scan.nextはtempに同じ
            scan_next = scan.next
            # 実際の仕事（ListNodeをひっくり返す）
            scan.next = previous_scan
            # 更新
            previous_scan = scan
            scan = scan_next

        return previous_scan
```

