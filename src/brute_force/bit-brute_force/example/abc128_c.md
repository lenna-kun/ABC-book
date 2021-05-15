# ABC128 C - Switches

[https://atcoder.jp/contests/abc128/tasks/abc128_c](https://atcoder.jp/contests/abc128/tasks/abc128_c)

実行時間制限: 2 sec / メモリ制限: 1024 MB

## 問題文

onとoffの状態を持つ\\(N \\)個のスイッチと，\\(M \\)個の電球がある．スイッチには\\(1 \\)から\\( N\\)の，電球には\\(1 \\)から\\(M \\)の番号がついている．

電球\\( i\\)は\\(k_i \\)個のスイッチに繋がっていて，スイッチ\\(s_{i1},s_{i2},...,s_{ik_i} \\)のうちonになっているスイッチの個数を\\(2 \\)で割った余りが\\(p_i \\)に等しい時に点灯する．

全ての電球が点灯するようなスイッチのon/offの状態の組み合わせは何通りあるか答えよ．

## 制約

- \\(1 \leq N, M \leq 10 \\)
- \\(1 \leq k_i \leq N \\)
- \\(1 \leq s_{ij} \leq N \\)
- \\(s_{ia} \neq s_{ib} (a \neq b) \\)
- \\(p_i \\)は\\(0 \\)または\\(1 \\)
- 入力は全て整数である

## 解法

難しい数え上げ問題に見えるので，全探索を検討してみる．

スイッチの個数は最大でも\\(10 \\)個なので，スイッチのon/offの組み合わせは最大で\\(2^{10}=1024 \\)通りである．この組み合わせの列挙は，bit全探索により実装することができる．

あるスイッチの組み合わせが，\\(M \\)個全ての電球それぞれの点灯する条件を満たしているかにかかる計算量は，\\(O(MN) \\)．よって，全体の計算量は\\(O(2^NMN) \\)となる．

これは十分に高速である．

## 実装

```py
n, m = map(int, input().split())
com = [[*map(int, input().split())][1:] for _ in range(m)]
p = [*map(int, input().split())]

ans = 0
for bits in range(1<<n):
  ison = lambda sw: (bits>>(sw-1))&1
  ans += all(
    (sum(map(ison, com[i]))%2 == p[i]) for i in range(m)
  )
print(ans)
```