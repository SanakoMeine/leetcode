# 83.remove duplicates from sorted list

## すでに解いた方々（in:レビュー依頼 titleで検索）
- https://github.com/atomina1/Arai60_review/pull/2
- https://github.com/pineappleYogurt/leetCode/pull/4
- https://github.com/t0hsumi/leetcode/pull/3
- https://github.com/katataku/leetcode/pull/2
- https://github.com/ichika0615/arai60/pull/3

## Step 1
### 考えたこと
- Valueのsetを作って値の重複を逐一確認し、重複がある場合はそのノードを.next.nextでスキップするよう書き換えていけば良いと考えた。 linkedListを進めるwhileループと値が重複した時にスキップするwhileループを用意してトライ。が、何度かエラーをもらったところで時間をかけすぎと思いギブアップ。
- https://discord.com/channels/1084280443945353267/1195700948786491403/1196388760275910747 を見て下記に気づく
    - Valueを保存するsetを作る必要はなく、前後で値を比較するだけで良い
    - While node.nextを外側のwhileに入れてしまったためにNoneまでたどり着いてしまい、node.next.valが存在しなくなってエラーを吐いていたことを知る。素朴にif文でもよかった。
- ↓あたりが原因で迷走した感がある。
    - 元のリストがsortされていることが頭から抜け落ちており、また、先の問題も引きずって「valueのsetを作る」という頭から切り替えられなかったこと
    - コードを頭の中でシミュレーションする力が不足していて、少しテストのパターンが変わると対応できなかったこと
- コードに落とし込む段階でもう少しお絵描きするなど、事前に頭の中を整理しておくといいのかも。
- 供養も兼ねて元の発想のまま修正して通してみる

### Step１のコード
‘’’Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node = head
        appeared = set()

        while node:
            appeared.add(node.val)
            while node.next and node.next.val in appeared:
                node.next = node.next.next
            node = node.next

        return head
‘’’

## Step 2
### 学んだこと
- 再帰で書けることに気づかなかった。呼び出し回数も制限があるらしい。再帰の気持ちを書いてくれているので腹落ちするまで見返せるようにメモ：https://discord.com/channels/1084280443945353267/1231966485610758196/1239417493211320382 
### Step２のコード
- 練習のため再帰で書いてみた。若干写経っぽくなってしまったので、別の問題でリベンジしたい。
- まんま写経だと身に付かなそうだったので今回再帰を書く時は伝言ゲームをイメージしてみたら少し納得感を持って書けた。 「責任者（番号が変わる前の最後の人）は君？違うなら責任者に行き着くまで伝言ゲームをして、責任者には僕に折り返す様に連絡しといて。あと責任者のアドレスリスト作りたいから、見つかった責任者にも同じ指示をするよう伝えといて」のようなイメージで書いてみた。
‘’’Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None:
            return None

        node = head
        while node.next is not None and node.val == node.next.val:
            node = node.next
        node.next = self.deleteDuplicates(node.next)

        return node
‘’’

- Continue使ったことなかったけど結構みんな使ってる。入れ子のif elseと等価に使ったりできるらしい。確かにスッキリしてるし使ってみても良さそう。： https://github.com/tarinaihitori/leetcode/pull/3/files#r1808004503
### Step２のコード（練習のためcontinue使って書いてみた）
‘’’ Python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node = head

        while node is not None:
            if node.next is not None and node.val == node.next.val:
                node.next = node.next.next
                continue
            node = node.next

        return head
‘’’

- Current.next is not noneについて同じ不安を持つことが多かったので勉強になった： https://github.com/ichika0615/arai60/pull/3/files#r1812079706 
- x and yがif x is false, then x, else yなのも初めて知った…。先人がメモってくれてなかったら気づけなかったのでありがたい。https://github.com/ichika0615/arai60/pull/3/files#r1812247443 。
- 空間/時間計算量についてみんな言及しているので書く癖をつけた方が良いかも。Step１のコードだとどっちもO(n)かな（無駄なことしなければ本来空間はO(1)）。


## Step 3
### 
https://discord.com/channels/1084280443945353267/1195700948786491403/1196399353116499970 が短い上に読みやすく、valueのsetも不要で不慣れな再帰も使っていないので、これを目標に書いてみる。

### 時間測定
3:31, 1:20, 1:50 
今回はいろんなパターンで書いてみたのもあってか、２回目以降はなんとなく覚えてしまっていて、全然頭を使えていない感じがするのが少し不安…

### Step３のコード

‘’’Python
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node = head

        while node:
            while node.next and node.val == node.next.val:
                node.next = node.next.next
            node = node.next
        return head
‘’’
