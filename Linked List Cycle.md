# 141. Linked-list-cycle
前回はやり方が分かっておらず、実質Step1止まりだったのでStep2~3をやり直しさせてください。

## Step 1 （ここは前回すでにレビューをいただいています）
 手も足も出なかったので新井さんの動画を見てとりあえず真似して書いてみる
```Python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:

        # 1つずつ進むポインタ
        slow_pointer = head
        # 先行するポインタ
        fast_pointer = head

        # 先行するポインタが最後尾にたどり着かない限り繰り返す
        while fast_pointer and fast_pointer.next:
            fast_pointer = fast_pointer.next.next
            slow_pointer = slow_pointer.next

            # while中にfast_pointerとslow_pointerが合致した場合，ループありと判断
            if fast_pointer == slow_pointer:
                return True

        return False

```

## Step 2
### 参考にした方々（in:レビュー依頼 titleで検索）
- https://github.com/nittoco/leetcode/pull/12
- https://github.com/NobukiFukui/Grind75-ProgrammingTraining/pull/27
- https://docs.google.com/document/d/1WPrRCrCJw5XqrvOgzo7Dlq06AniqpZoFq-pzTKkb1aw/edit?tab=t.0
- https://github.com/tk-hirom/Arai60/pull/1
- https://github.com/lilnoahhh/leetcode/pull/

> ヒットした全てのPull Requestを見て
とのコメントをいただいたので取り組んでみたのですが、初心者すぎてか全く読み終わらず永遠に２問目に進めない。
進めてるうちに負荷が下がると期待して、一度諦めて次の問題に。レビュー依頼いただいたものについてはあとで読む方針にする。

> setを使った解き方もある
とコメントをいただいたのでDiscord内を調べるとたくさん例があったので自分でも書いてみる。

- 青いランプの話を見て、if文一つまで考えるものなのかとびっくりしたのでメモ。真似してrerturn Trueする側を前に置いてみる。 https://discord.com/channels/1084280443945353267/1192728121644945439/1194203372115464272
- 「PEP8によるとis noneとかis not noneで書いた方が良い」というコメントを見たので、while node and node.nextとしていたところを修正
- PEP8を事前に読んでおかなければいけないことすら知らなかったのでリソースの範囲で読んでいきたい。
- Step1で見た回答にはWhileの .nextなくても動きそうだなと思って1回外してみるも普通に通る。が、elseの中の.nextが存在しなかったらエラー吐きそうだなと思って付け直す。
- current_nodeと名付けたい気持ちがあったが、currentが意味するものが正鵠を射ていないらしいとのコメントを見つけ、普通にnodeとしてみた。

```Python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        node = head
        visited_node = set()

        while node and node.next is not None:
            if node in visited_node:
                return True
            else:
                visited_node.add(node)
                node = node.next

        return False
```

## Step 3
Fast nodeとslow nodeを使うやり方は自分では絶対に思いつかないので、setを使う方を選択。最後は解くのに2:40くらい。

```Python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        node = head
        visited_node = set()

        while node and node.next is not None:
            if node in visited_node:
                return True
            else:
                visited_node.add(node)
                node = node.next

        return False
```

