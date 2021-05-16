# ABC200 D - Happy Birthday! 2

[https://atcoder.jp/contests/abc200/tasks/abc200_d](https://atcoder.jp/contests/abc200/tasks/abc200_d)

実行時間制限: 2 sec / メモリ制限: 1024 MB

## 問題文

\\(N \\)個の正整数からなる数列\\(A=(A_1,A_2,...,A_N) \\)が与えられる．以下の条件を全て満たす2つの数列\\(B=(B_1,B_2,...,B_x) \\)，\\(C=(C_1,C_2,...,C_y) \\)が存在するか判定し，存在する場合はひとつ出力せよ．

- \\( 1 \leq x,y \leq N \\)
- \\( 1 \leq B_1 < B_2 < ... < B_x \leq N \\)
- \\( 1 \leq C_1 < C_2 < ... < C_y \leq N \\)
- \\(B \\)と\\(C \\)は，異なる数列である．
    - \\(x \neq y \\)のとき，または，ある整数\\(1 \leq i \leq \mathrm{min}(x,y) \\)が存在して\\(B_i \neq C_i \\)であるとき，\\(B \\)と\\(C \\)は異なるものとする．
- \\(A_{B_1}+A_{B_2}+...+A_{B_x} \\)を\\(200 \\)で割った余りと\\(A_{C_1}+A_{C_2}+...+A_{C_y} \\)を\\(200 \\)で割った余りが等しい．

## 制約

- 入力は全て整数
- \\(2 \leq N \leq 200 \\)
- \\(1 \leq A_i \leq {10}^9 \\)

## 解法

後日加筆

## 実装

```py
from itertools import *
n = min(8, int(input()))
a = [*map(int, input().split())]

mod = [[] for _ in range(200)]
for i in range(1, n+1):
  for c in combinations(a[0:n], i):
    mod[sum(c)%200].append([*c])

l = [*filter(lambda l: len(l)>1, mod)]
if not l:
  print("No")
  exit()
ans = [
  sorted([a.index(e)+1 for e in l[0][0]]),
  sorted([len(a)-a[::-1].index(e) for e in l[0][1]])
]
print("Yes")
[print(len(e), ' '.join(map(str, e))) for e in ans]
```