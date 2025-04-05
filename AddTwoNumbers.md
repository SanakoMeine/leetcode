# 2. Add two numbers

## すでに解いた方々（in:レビュー依頼 titleで検索）
- https://github.com/olsen-blue/Arai60/pull/5
- https://github.com/atomina1/Arai60_review/pull/4/files
- https://github.com/t0hsumi/leetcode/pull/5/files
- https://github.com/katataku/leetcode/pull/4/files
- https://github.com/ichika0615/arai60/pull/4/files

## Step 1
### 考えたこと
- 繰り上がりがある時をListNode(1, None), ない時をListNode(0, None)とすれば良いと考えた。あとはひたすら場合分け。
```Python
class Solution:
    def addTwoNumbers(
        self, l1: Optional[ListNode], l2: Optional[ListNode]
    ) -> Optional[ListNode]:
        sentinental = ListNode(0, None)
        l_sum = sentinental

        while l1 or l2:
            l_sum.val = l1.val + l2.val + l_sum.val
            # 繰り上がりの処理
            if l_sum.val >= 10:
                l_sum.val = l_sum.val - 10
                l_sum.next = ListNode(1, None)
            else:
                l_sum.next = ListNode(0, None)

            # l1とl2の長さが違う場合の処理
            if l1.next is None and l2.next:
                l1.next = ListNode(0, None)
            elif l1.next and l2.next is None:
                l2.next = ListNode(0, None)
            elif not l1.next and not l2.next and l_sum.next.val == 0:
                l_sum.next = None

            l_sum = l_sum.next
            l1 = l1.next
            l2 = l2.next

        return sentinental
```

## Step 2
### 学んだこと
- 割り算して商と余りを変数に入れる方が素直。//や%しか知らなかったが、divmod()という関数で一括で取れるらしい：https://github.com/t0hsumi/leetcode/pull/5/files#r1857708634 
- If l1 is None else 0という書き方は便利：https://github.com/olsen-blue/Arai60/pull/5/file
- If l1:~ sum+=,  If l2:~ sum+=という書き方が読んでて綺麗だと思ったのでこの方向を目指したい
- Get_valueでNoneを0に置き換えるのはうまいと思った。条件分けが減って読みやすい。：https://github.com/t0hsumi/leetcode/pull/5/files
- l1やl2を直で動かすべきではない
- 実務なら３つの和に拡張したりもある、というのは素晴らしい視点だと思った https://github.com/t0hsumi/leetcode/pull/5/files#r1858112669
- Node(0, None)は渡さなくても良いNode()：https://github.com/katataku/leetcode/pull/4/files#r1840319771 
- 最上位桁を漏らさないwhile l1 is not None or l2 is not None or carry > 0: は思いつかなかった、賢い：https://github.com/katataku/leetcode/pull/4/files

### inner_functionを使う解法
```Python
class Solution:
    def addTwoNumbers(
        self, l1: Optional[ListNode], l2: Optional[ListNode]
    ) -> Optional[ListNode]:
        def get_val(node):
            if node is not None:
                return node.val
            return 0

        node_l1 = l1
        node_l2 = l2
        carry = 0
        dummy_head = ListNode()
        node_sum = dummy_head

        while node_l1 or node_l2 or carry > 0:
            temp_sum = get_val(node_l1) + get_val(node_l2) + carry

            carry, digit = divmod(temp_sum, 10)
            node_sum.next = ListNode(digit)
            node_sum = node_sum.next

            if node_l1:
                node_l1 = node_l1.next
            if node_l2:
                node_l2 = node_l2.next

        return dummy_head.next
```
```Python
class Solution:
    def addTwoNumbers(
        self, l1: Optional[ListNode], l2: Optional[ListNode]
    ) -> Optional[ListNode]:
        node_l1 = l1
        node_l2 = l2
        dummy_head = ListNode()
        node_sum = dummy_head
        carry = 0

        while node_l1 or node_l2 or carry:
            temp_sum = carry
            if node_l1:
                temp_sum += node_l1.val
                node_l1 = node_l1.next
            if node_l2:
                temp_sum += node_l2.val
                node_l2 = node_l2.next

            carry, digit = divmod(temp_sum, 10)
            node_sum.next = ListNode(digit)

            node_sum = node_sum.next

        return dummy_head.next
```
## Step 3
### コメント
- Node = node.nextが気に食わないのでInner functionを使うやり方を採用
- 9:30, 7:36, 4:36

```Python
class Solution:
    def addTwoNumbers(
        self, l1: Optional[ListNode], l2: Optional[ListNode]
    ) -> Optional[ListNode]:
        def get_val(node):
            if node is None:
                return 0
            return node.val

        node_l1 = l1
        node_l2 = l2
        dummy_head = ListNode()
        node_sum = dummy_head
        carry = 0

        while node_l1 or node_l2 or carry:
            temp_sum = get_val(node_l1) + get_val(node_l2) + carry
            carry, digit = divmod(temp_sum, 10)
            node_sum.next = ListNode(digit)

            if node_l1:
                node_l1 = node_l1.next
            if node_l2:
                node_l2 = node_l2.next
            node_sum = node_sum.next

        return dummy_head.next
```

