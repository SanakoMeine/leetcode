Linked list cycle II



# 142. Linked list cycle II

## すでに解いた方々（in:レビュー依頼 titleで検索）
- https://github.com/atomina1/Arai60_review/pull/1/files
- https://github.com/pineappleYogurt/leetCode/pull/3/files
- https://github.com/t0hsumi/leetcode/pull/2/files
- https://github.com/katataku/leetcode/pull/5/files
- https://github.com/ichika0615/arai60/pull/2/files

## Step 1
- ループが閉じる番号を探すなら順序を考慮するリストの方が良いのかなと思い、最初はリストで解いてみようとするも、計算時間オーバーでギブアップ
- https://github.com/pineappleYogurt/leetCode/pull/3/files を拝見して、先の問題と同じようにsetで解いてnodeをreturnすれば良さそうと理解して書き直す
- すでに解いた方々の回答と付き合わせてもそんなに悪くなさそう…？今の実力だとどんなコメントがつくか予想がつかない。

```Python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        visited_node = set()
        node = head

        while node and node.next is not None:
            if node in visited_node:
                return node
            else:
                visited_node.add(node)
                node = node.next

        return None
```

## Step 2
### Step2.1 
- なぜ機能するのかさっぱりわからなかったフロイドのアルゴリズムについて小田さんが解説されていた：https://discord.com/channels/1084280443945353267/1246383603122966570/1252209488815984710
- 自分では絶対に思いつかないが、お絵描きしてみると言っていることは理解できた。時間が余った時にパズル的に聞かれることはあるとのことなので、おまけ気分で試しに書いてみる
- 先人のコードとレビューを見ていると、フロイドのアルゴリズムを使った場合はどうも前の問題でも言及されていたwhileの見通しが問題が生じやすそう：https://discord.com/channels/1084280443945353267/1221030192609493053/1225674901445283860 Python独自の書き方というのがよく分かっていないので、1度試しに書いてみる（Whileとelseを並列させている部分がPython独自なのかな？）
- が、たいしてif文の中が長くないので他の書き方でもよかった気がする。whileを２つ並べているのがまとめられそうでなんとなく気に食わない。
- 先人の主語云々というのが取り入れられなかった気がするが、fast slowの順番の話なので、今回自分が書いたものとは関係ない…？ https://github.com/katataku/leetcode/pull/5/files#r1845483049

```Python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast_node = head
        slow_node = head

        # detect a collision point
        while fast_node and fast_node.next is not None:
            fast_node = fast_node.next.next
            slow_node = slow_node.next
            if fast_node is slow_node:
                break
        else:
            return None

        node_from_start = head
        node_from_collision = fast_node

        # detect a intersection
        while node_from_start is not node_from_collision:
            node_from_start = node_from_start.next
            node_from_collision = node_from_collision.next
        else:
            return node_from_start
```

## Step 3
とりあえずフロイドの方法のやり方自体は理解したが、実際に書くときはSetで書きたいなと思ったのでそちらで書いてみる。
時間を測りながら書いてみたらStep1のようにwhile->if->elseを使うのではなく、while->ifみたいな構造になっていた。
こっちの方がシンプルそうだなと感じてこちらを採用
1回目：4:59, 2回目：2:20, ３回目：2:20
```Python
	class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node = head
        visited_node = set()

        while node and node.next is not None:
            visited_node.add(node)
            node = node.next
            if node in visited_node:
                return node

        return None

```
