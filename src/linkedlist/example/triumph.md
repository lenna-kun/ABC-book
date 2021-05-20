# Triumph

連結リストの問題が見つからなかったので作問した．

[https://mojacoder.app/users/Coordinator/problems/triumph](https://mojacoder.app/users/Coordinator/problems/triumph)

実行時間制限: 2 sec / メモリ制限: 1024 MB

## 問題

スペード，クローバー，ダイヤ，ハートのカードがそれぞれ\\(N \\)枚ずつあり，それぞれのマークについて，\\(1 \\)~\\(N \\)の番号のカードが1枚ずつある．例えば，スペードの\\(1 \\)，クローバーの\\(1 \\)，ダイヤの\\(1 \\)，ハートの\\(1 \\)はそれぞれ\\(S1 \\)，\\(C1 \\)，\\(D1 \\)，\\(H1 \\)で表され，最初，カードは，\\(S1,...,SN,C1,...CN,D1,...,DN,H1,...,HN \\)の順番で並んでいる．
これに対して，\\(Q \\)個のクエリに従ってシャッフルを行う．具体的には，\\(i (1 \leq i \leq Q) \\)番目のクエリでは次の操作をしなければならない．

- \\(L_i,R_i \\)が与えられる．カード\\(L_i \\)からカード\\(R_i \\)までの全てのカードを順番を保ったまま先頭に持っていく．つまり，カードの順列\\(\[a_1,a_2,...,a_{4N}\] \\)において\\(L_i=a_j,R_i=a_k (1 \leq j \leq k \leq 4N) \\) であるとき，順列\\(\[a_1,…,a_{j-1},a_j,…,a_k,a_{k+1}...,a_{4N}\] \\)を\\(\[a_j,…,a_k,a_1,…,a_{j−1},a_{k+1},...a_{4N}\] \\)に変更する．

\\(Q \\)個のクエリを順に処理した後のカードの順列において，全てのスペードのカードのみからなる部分列，全てのクローバーのカードのみからなる部分列，全てのダイヤのカードのみからなる部分列，全てのハートのカードのみからなる部分列をそれぞれこの順で出力せよ．

部分列とは，ある順列から任意数の項を抜き出した上で，"順番を保ったまま"それらを並べてできる順列のことである．

## 制約

- \\(1 \leq N \leq {10}^5 \\)
- \\(1 \leq Q \leq {10}^5 \\)

## 入力

\\(N\ Q \\)

\\(L_i\ R_i(1 \leq i \leq Q) \\)はそれぞれ"S"または"C"または"D"または"H"の1文字と，数字\\(j (1 \leq j \leq N) \\)を並べてできる\\(4N \\)種類の文字列

## 出力

スペードのカードのみからなる部分列，クローバーのカードのみからなる部分列，ダイヤのカードのみからなる部分列，ハートのカードのみからなる部分列をそれぞれこの順で4行で出力せよ．

## サンプル

### 入力例1

```
1 2
C1 D1
D1 S1

```

### 出力例1

```
S1
C1
D1
H1

```

初期状態は，\\(\[S1, C1, D1, H1\] \\)．
1つ目のクエリにより，\\(C1 \\)~\\(D1 \\)が先頭に行くので，\\(\[C1, D1, S1, H1\] \\)になる．
2つ目のクエリにより，\\(D1 \\)~\\(S1 \\)が先頭に行くので，\\(\[D1, S1, C1, H1\] \\)になる．
\\(N=1 \\)の場合，どのように並び替えてもそれぞれ1枚ずつしかないので，出力結果は自明です．

### 入力例2

```
3 2
C2 D1
S3 H3

```

### 出力例2

```
S3 S1 S2
C1 C2 C3
D2 D3 D1
H1 H2 H3

```

初期状態は，\\(\[S1, S2, S3, C1, C2, C3, D1, D2, D3, H1, H2, H3\] \\)．
1つ目のクエリにより，\\(C2 \\)~\\(D1 \\)が先頭に行くので，\\(\[C2, C3, D1, S1, S2, S3, C1, D2, D3, H1, H2, H3\] \\)になる．
2つ目のクエリにより，\\(S3 \\)~\\(H3 \\)が先頭に行くので，\\(\[S3, C1, D2, D3, H1, H2, H3, C2, C3, D1, S1, S2\] \\)になる．
それぞれのマークのみからなる部分列は，スペードが\\(\[S3,S1,S2\] \\)，クローバーが\\(\[C1,C2,C3\] \\)，ダイヤが\\(\[D2,D3,D1\] \\)，ハートが\\(\[H1,H2,H3\] \\)となる．

### 入力例3

```
2 1
C2 C2

```

### 出力例3

```
S1 S2
C2 C1
D1 D2
H1 H2

```

\\(L_i \\)と\\(R_i \\)が等しい場合もあります．その場合は，その1枚を先頭に持っていきます．

## 解法


## 実装

```py
class Node:
  def __init__(self, value):
    self.value = value
    self.left = None
    self.right = None

def card2number(n, card):
  if card[0] == 'S':
    return int(card[1:])-1
  elif card[0] == 'C':
    return n + int(card[1:])-1
  elif card[0] == 'D':
    return 2*n + int(card[1:])-1
  elif card[0] == 'H':
    return 3*n + int(card[1:])-1
  else:
    raise Exception

n, q = map(int, input().split())

head = Node(0)
nodes = [head]
for i in range(1, 4*n):
  node = Node(i)
  nodes[-1].right = node
  node.left = nodes[-1]
  nodes.append(node)

for _ in range(q):
  l, r = map(lambda arg: card2number(n, arg), input().split())
  if nodes[l].left is None:
    continue
  nodes[l].left.right = nodes[r].right
  if nodes[r].right is not None:
    nodes[r].right.left = nodes[l].left
  head.left = nodes[r]
  nodes[r].right = head
  head = nodes[l]
  nodes[l].left = None
  tmp = head

cards = [[], [], [], []]
for _ in range(4*n):
  cards[head.value//n].append(head.value%n+1)
  head = head.right
for i in range(4):
  print(*[f'{"SCDH"[i]}{card}' for card in cards[i]])
```