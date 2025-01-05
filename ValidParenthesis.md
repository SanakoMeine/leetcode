# 20.valid parentheses

## すでに解いた方々（in:レビュー依頼 titleで検索）
- https://github.com/tarinaihitori/leetcode/pull/7/files
- https://github.com/katataku/leetcode/pull/6/files
- https://github.com/frinfo702/software-engineering-association/pull/9/files
- https://github.com/t0hsumi/leetcode/pull/6/files
- https://github.com/SuperHotDogCat/coding-interview/pull/41/files

## Step 1
### 考えたこと
- Pythonの文字列の操作とstackの使い方が怪しかったので早々に諦めてこれを確認。まずは素朴に場合分けしてみた。
- len(stack)は思いつかずにAraiさんの動画を確認

* 前回「一連の練習を終えるのに１問６時間かかってしまう」と悩んでおりましたが、標準出力を使えることを前回教えてもらったおかげで今回はStep1に20分、トータルでも２時間半くらいで済みました。つまらないことでお騒がせしてすいません…。

```Python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []

        for char in s:
            if char == "(" or char == "{" or char == "[":
                stack.append(char)
            elif stack and char == ")" and stack[-1] == "(":
                stack.pop()
            elif stack and char == "}" and stack[-1] == "{":
                stack.pop()
            elif stack and char == "]" and stack[-1] == "[":
                stack.pop()
            else:
                print(stack)
                return False

        return len(stack) == 0
```

## Step 2
### 学んだこと
- 場合分けが多いので、continueや早期returnを用いていかに読みやすくするかが結構論じられている：https://github.com/tarinaihitori/leetcode/pull/7/files
- 辞書を使うやり方が場合分けが減らせてスマート
- Return len(stack)するよりimplicit falseを使う方流儀もあるが、趣味の範囲っぽいので周囲に合わせれば良いとのこと
- charが([{と一致するかはc in “([{”でも通るのは知らなかった： https://github.com/katataku/leetcode/pull/6/files#r1847770818
- 番兵を入れておくやり方、気になる：https://github.com/tarinaihitori/leetcode/pull/7/files#r1817714323
- 今回はListでpopとappendしたが、dequeを使えばstackの左側からも取り出したり追加したりできる： https://note.nkmk.me/python-collections-deque/
- チョムスキー階層、プッシュダウンオートマトンはよく分からないのでとりあえずスルー。こちらの方が関係ある箇所をまとめてくれていた：https://github.com/BumbuShoji/Leetcode/pull/7/files （後で読む：https://str.i.kyushu-u.ac.jp/~takeda/Lectures/FormalLanguageTheory2019/Resume/FormalLanguageTheory2019-11.pdf）
- 何が問題になっているのか理解が怪しいけれど、「双方向リストの機能を使っていないのにdequeを使うと無駄な処理が増えるので、O(1)同士でも違いがあることに敏感になろう」という話と理解した: https://github.com/BumbuShoji/Leetcode/pull/7/files#r1810557932 
- 一度辞書を使って書いてみよう

```Python
class Solution:
    def isValid(self, s: str) -> bool:
        bracket_pair = {"(": ")", "{": "}", "[": "]"}
        stack = []

        for char in s:
            if char in bracket_pair:
                stack.append(char)
                continue
            if not stack:
                return False
            if char != bracket_pair[stack[-1]]:
                return False
            stack.pop()

        return len(stack) == 0

```

## Step 3
### コメント
- Step2で書いたIf文にcontinueを重ねるのが読みやすいが、頭からこれが書いていける感じがしない。。。一度別のコードを書いてから上の形に書き直す感じになりそうなので、自分にとって頭から自然に書ける書き方でしばらく書いてみたが、結果これまでと違ってStep3の書き方がなかなか１通りに収束しない。Step２の様なコードを頭から書きくだせる様な練習をすべきなのか…？ -> 「if continueになれていない」というのが原因に思えるので、「常識」的な感覚に合わせるためにもstep2が自然に感じる様に練習する方がいい気がする。
- 5:15, 5:30, 4:10

```Python
    def isValid(self, s: str) -> bool:
        bracket_pairs = {"(": ")", "[": "]", "{": "}"}
        stack = []

        for char in s:
            if char in bracket_pairs:
                stack.append(char)
            elif char in bracket_pairs.values():
                if stack and char == bracket_pairs[stack[-1]]:
                    stack.pop()
                else:
                    return False

        return len(stack) == 0
```

## 悩んだけれど最終的にこっちで書いた
```Python
class Solution:
    def isValid(self, s: str) -> bool:
        bracket_pairs = {"(": ")", "[": "]", "{": "}"}
        stack = []

        for char in s:
            if char in bracket_pairs:
                stack.append(char)
                continue
            if not stack:
                return False
            if char != bracket_pairs[stack[-1]]:
                return False
            stack.pop()

        return len(stack) == 0
```


