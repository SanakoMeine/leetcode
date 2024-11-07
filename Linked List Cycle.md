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
