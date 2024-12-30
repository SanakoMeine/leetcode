問題へのリンク
[https://leetcode.com/problems/linked-list-cycle/description/](url)
※ すいません，初挑戦かつGitの使い方もおぼつきません。
※ プログラミングの練習以前にレビュー以来のやり方自体間違ってる等あればお申し付けください。

Step1で手も足も出ず，あらいさんの回答動画を見て，vscodeのleetcode拡張からPythonで回答を書いている人を見つけてきてマネして書いてみた感じです。
Step2でも申し訳程度にコメントを加えてみただけで，正直ほとんど何も修正できていません。。。
下記はStep3で最後に書きあがったものに，時間が余ったのでコメントを追加してみた感じです。

'''
python

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

'''
