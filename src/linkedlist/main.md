# 連結リスト

一連のノードが，任意のデータフィールド群と，1つか2つの参照（リンク）を持つ．参照は，次（および前）のノードを指している．つまり，連結リストは自己参照型のデータ型であり，同じデータ型の別のノードへのリンク（またはポインタ）を含んでいる．

連結リストの利点は，リスト上のノードを様々な順番で検索可能な点である．場所（ポインタ）が分かっていれば，ノードの挿入や削除を\\(O(1) \\)で行うことができる．

一方，添字（インデックス）によるアクセスには，\\(O(N) \\)かかる．

連結リストには，片方向リスト，双方向リスト，線形リスト，循環リストなどがある．

片方向リストの実装は以下．

```py
class Node:
  def __init__(self, value):
    self.value = value
    self.next = None
 
class LinkedList:
  def __init__(self):
    self.head = None
 
  def insert(self, index, value):
    node = Node(value)
    if index == 0:
      node.next = self.head
      self.head = node
      return
    left = self.head
    right = left.next
    for _ in range(index-1):
      left = right
      right = right.next
    
    node.next = right
    left.next = node
 
  def delete(self, index):
    if index == 0:
      self.head = self.head.next
      return
    left = self.head
    right = left.next
    for _ in range(index-1):
      left = right
      right = right.next
    
    left.next = right.next
 
  def __iter__(self):
    self._next = self.head
    return self

  def __next__(self):
    ret = self._next
    if ret is None:
      raise StopIteration()
    self._next = self._next.next
    return ret.value

# example
# ll = LinkedList()
# for i in range(10):
#   ll.insert(i, i+1)
# print(*[*ll])
```