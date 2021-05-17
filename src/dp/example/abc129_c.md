# ABC129 C - Typical Stairs

[https://atcoder.jp/contests/abc129/tasks/abc129_c](https://atcoder.jp/contests/abc129/tasks/abc129_c)

実行時間制限: 2 sec / メモリ制限: 1024 MB

## 問題

\\(N \\)段の階段がある．高橋君は現在，上り口（\\(0 \\)段目）にいる．高橋君は一歩で\\(1 \\)段か\\( 2\\)段上ることができる．

ただし，\\(a_1,a_2,a_3,...a_m \\)段目の床は壊れており，その段に足を踏み入れることは危険．

壊れている床を踏まないようにしながら，最上段（\\(N \\)段目）にたどり着くまでの移動方法は何通りあるか．総数を\\(1000000007 \\)で割った余りを求めよ．

## 制約

- \\(1 \leq N \leq {10}^{5} \\)
- \\(0 \leq M \leq N-1 \\)
- \\(1 \leq a_1 < a_2 < ... < a_m \leq N-1 \\)

## 解法

細かいことを抜きにして考えると，\\(N \\)段目にたどり着く総数は\\(N-1 \\)段目にたどり着く総数と\\(N-2 \\)段目にたどり着く総数の和である．また，これは，\\(N-1 \\)段目（\\(N-2 \\)段目）についても同様のことが成立し，\\(N-1 \\)段目（\\(N-2 \\)段目）にたどり着く総数は\\(N-2 \\)段目（\\(N-3 \\)段目）にたどり着く総数と\\(N-3 \\)段目（\\(N-4 \\)段目）にたどり着く総数の和である．

したがって，この問題において，\\(i （2 \leq i \leq N）\\)段目にたどり着く総数\\(S_i \\)は，以下の漸化式で表される．

\\[
    \begin{align}
    S_0 &= S_1 = 1 \\\\
    S_i &= S_{i-1} + S_{i-2}
    \end{align}
\\]

ここから，あとは壊れた床に注意して，以下のようなコードが思いつく人も少なくないだろう．

```py
import sys
sys.setrecursionlimit(10**6)
mod = 1000000007

n, m = map(int, input().split())
a = set([int(input()) for _ in range(m)])

def rec(i):
  if i == 0:
    return 1
  elif i < 0:
    return 0
  return ((i in a)^1)*(rec(i-1)+rec(i-2))%mod

print(rec(n))
```

単純な再帰関数による実装である．しかし，このプログラムの計算量は\\(O(2^N) \\)であり（2分木を思い浮かべるとわかる），この解法の実行時間は制限を大きく上回ってしまう．

後日加筆

## 実装

メモ化再帰

```py
import sys
sys.setrecursionlimit(10**6)
mod = 1000000007

n, m = map(int, input().split())
a = set([int(input()) for _ in range(m)])

memo = [1]+[None]*(n-1)+[0]
def rec(i):
  if i != n and memo[i] is not None:
    return memo[i]
  memo[i] = ((i in a)^1)*(rec(i-1)+rec(i-2))%mod
  return memo[i]

print(rec(n))
```

DP

```py
mod = 1000000007
n, m = map(int, input().split())
a = set([int(input()) for _ in range(m)])

dp = [1]+[0]*n
for i in range(1, n+1):
  dp[i] = ((i in a)^1)*(dp[i] + dp[i-1] + dp[i-2])%mod

print(dp[n])
```