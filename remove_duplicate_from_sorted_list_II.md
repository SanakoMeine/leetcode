# 82. Remove duplicate from sorted list II

## すでに解いた方々（in:レビュー依頼 titleで検索）
- https://github.com/atomina1/Arai60_review/pull/3/files
- https://github.com/t0hsumi/leetcode/pull/4/files
- https://github.com/katataku/leetcode/pull/3/files
- https://github.com/ichika0615/arai60/pull/5/files
- https://github.com/tarinaihitori/leetcode/pull/4/files

***********************************************************************************************************************************************************************************
以前「やってることは間違っていない」とコメントをいただいているので、その点は安心しているのですが、step1~3を終えるのに6時間くらいかかってしまいました。
度々こういうことがありまして、コードの読み書きに不慣れとはいえ時間がかかりすぎで、何か根本的な問題があるような気がしています。
内訳は
- Step1（２時間くらい）：「どの解法がいいかな」とPRを漁るのに時間がかかる。自分の発想に近い解き方をしている人を探していて、工程がStep2と混ざっているかも。
- Step2 （４時間くらい）：レビューをお願いする5人分を確認し、PRを一通り読んで問題に対してどんな解法があるのか一通り理解する。どんな解法があるのか抑える過程にすごく時間がかかるが、これが理解できないとコードがちゃんと読めていない感じがする。
- Step3（20分くらい）：Step2で見た解法のうち、しっくりくるものを思い出しながら再現する。打ってるうちに覚えてくるので、ここはあまり時間はかからない。
という感じなのですが、改善すべき意識や工程などありますか…？それとも初心者はこんなもので、やってるうちに早くなるものなんでしょうか？
***********************************************************************************************************************************************************************************

## Step 1
### 考えたこと
- １問前の.nextを一つ先まで見ればいけそうだなと思い、node_next = node.nextという変数を導入してトライしてみるが、初手で1, 1と続いたケースが通らない。headを動かしたり重複がみられた変数をset()に放り込むなどやってみたが、こんがらがってきたところでギブアップ。
- 皆さんのコードを眺めてみると、ListNode(0)の次にheadを置いている。言われてみれば当たり前だが、これはサッパリ思いつかなかった。
- Optional[int]を使うとダミー用の番号は不要でNoneで動くらしい：https://discord.com/channels/1084280443945353267/1226508154833993788/1246022270984392724 
- 答えを見たが一度読んだだけではしっくりこず、元の発想通り重複がみられた変数をset()に放り込む方法で書いてみた。納得はできたが、「なぜこれだと動かないのか」「なぜここを変えると動くようになるのか」を納得するのに時間がかかりすぎている感がある（今回ならstep1に２時間とかかかってしまった…）。
  -> 一度自分で解いた解法じゃないと他の人のコードを全然読めていなくて、そもそもコードを読んだり頭の中で挙動を再現する力が弱そう。現状Step１の時点では「誰かの回答を写経して、気になるところを変えながら挙動を理解していく」とかなら時間がかからなそうなので、次は試してみる。
- Inplaceとnot-inplaceというらしい: https://github.com/cheeseNA/leetcode/pull/9/files

### 当初の発想で書いたコード
```Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(None, head)
        node = dummy

        value_duplicated = set()

        while node.next:
            if node.val == node.next.val:
                value_duplicated.add(node.val)
            node = node.next

        node = dummy

        while node:
            while node.next and node.next.val in value_duplicated:
                node.next = node.next.next
            node = node.next

        return dummy.next
```

## Step 2
### 学んだこと
- 例によってsortされてるのでsetとして重複変数を保存する必要はなかった。while文１つにまとめれそう https://discord.com/channels/1084280443945353267/1195700948786491403/1196701558382018590 。
- 長すぎる条件が気になっていたが、これをまとめるアイデアがあった： https://github.com/rinost081/LeetCode/pull/6#discussion_r1744911468 
- 大きく分けて解き方は３つっぽい。再帰で解く、繋いでから切る、２つListNodeを用意して重複のないものだけを繋いでいく。
-  https://github.com/tarinaihitori/leetcode/pull/4/files#r1807830600 を見ると、.next.nextを用いたやり方が自分の最初の発想と連続していて、素直で理解しやすいと感じる。重複の判定を別の関数でやるのもスッキリしているのでこれを目指す。
- whileとif breakはwhile notに書き換えれるらしい： https://discord.com/channels/1084280443945353267/1227073733844406343/1228598526712483902 
- If elseを見ると読み慣れている人はこんなふうに考えるのか： https://github.com/atomina1/Arai60_review/pull/3/files#r1893541458 
- 命名としてdummyではなくsentinel, valじゃなくてvalueの方が良さそう：https://github.com/katataku/leetcode/pull/3/files

###　繋いでから切るコード
```Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        sentinel = ListNode(None, head)
        node = sentinel

        while node:
            is_duplicated = False
            while node.next and node.next.next and node.next.val == node.next.next.val:
                is_duplicated = True
                node.next = node.next.next
            if is_duplicated:
                node.next = node.next.next
            else:
                node = node.next

        return sentinel.next
```
### 重複のないものだけを繋いでいくコード
```Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(None, head)
        unique = dummy
        scan = head

        while scan:
            # 値が重複するときはscanを走査してスキップ
            if scan.next and scan.val == scan.next.val:
                while scan.next and scan.val == scan.next.val:
                    scan.next = scan.next.next
                unique.next = scan.next
            # 値が重複しないときはuniqueに追加
            elif scan.next and scan.val != scan.next.val:
                unique.next = scan
                unique = scan
            # scan.nextがNoneの時は終了
            else:
                print(scan.next)
                unique.next = scan
                break

            scan = scan.next

        return dummy.next
```
###　再帰を使ったコード
回答を見ていると再帰を使っている人も多いので、練習のため１度再帰で通してみる。

```Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        if head.val != head.next.val:
            head.next = self.deleteDuplicates(head.next)
            return head

        while head.next is not None and head.val == head.next.val:
            head = head.next
        return self.deleteDuplicates(head.next)
```



## Step 3
- ListNodeの問題、Nodeを１個ずつ処理していくイメージがあったので、while node:~ node = node.nextでかけて欲しい気持ちがあるが、良い書き方が思い浮かばなかった。
- 3:54, 3:21, 3:08
```Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        sentinel = ListNode(None, head)
        node = sentinel

        while node:
            is_duplicate = False
            while node.next and node.next.next and node.next.val == node.next.next.val:
                node.next = node.next.next
                is_duplicate = True
            if is_duplicate:
                node.next = node.next.next
            else:
                node = node.next

        return sentinel.next
```
### Step4？
しっくり来てなかったので日をおいて解き直し
```Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        sentinel = ListNode(0, head)
        ptr = sentinel

        while ptr:
            scan = ptr
            if scan.next and scan.next.next and scan.next.val == scan.next.next.val:
                while (
                    scan.next and scan.next.next and scan.next.val == scan.next.next.val
                ):
                    scan.next = scan.next.next
                scan.next = scan.next.next
            else:
                ptr.next = scan.next
                ptr = ptr.next

        return sentinel.next
```
