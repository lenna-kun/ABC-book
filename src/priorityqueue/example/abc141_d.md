#  ABC201 D - Powerful Discount Tickets

[https://atcoder.jp/contests/abc141/tasks/abc141_d](https://atcoder.jp/contests/abc141/tasks/abc141_d)

実行時間制限: 2 sec / メモリ制限: 1024 MB

## 問題文
高橋くんは\\(N\\)個の品物を\\(1\\)個ずつ順番に買う予定です。

\\(i\\)番目に買う品物の値段は \\(A_i\\)円です。高橋くんは \\(M\\) の割引券を持っています。

品物を買う際に割引券を好きな枚数使うことができます。
\\(X\\)円の品物を買う際に\\(Y\\)枚の割引券を使った場合、その品物を\\(\frac{X}{2^Y}\\)
円(小数点以下切り捨て)で買うことができます。

最小で何円あれば全ての品物を買うことができるでしょうか。

## 制約
 - 入力は全て整数である
 - \\(1\leq N, M \leq  10 ^ 5 \\)
 - \\(1\leq A_i \leq  10 ^ 9 \\)

# 解法
割引券を\\(1\\)枚使った場合をまず考える。この時、以下のように高額な品物に割引券を使った方が支払うお金を抑えられることがわかる。

<div align="center"><img src="abc141_d/abc141_d_1.png"></div>

であれば、なるべく高価な品物に割引券を使えば割引額が大きくなり（=支払う金額が減り）、最終的な支払額を抑えられそうである。となると、商品を高額な順に並べて考えると良さそうである。
<div align="center"><img src="abc141_d/abc141_d_2.png"></div>
では品物の高い順に一枚ずつ商品券を使えばいいかというと、<font color="red"><b>そうではない場合もある。</b></font>以下のような極端なケースがそれに当たる。すなわち、もっとも高額な商品に割引券を使ってもまだその商品がもっとも高額だった時である。
<div align="center"><img src="abc141_d/abc141_d_3.png"></div>
では、もっとも高額な商品がその次に高額な商品よりもいくら高い時に何回商品券を使うか、場合分けしていけば良いのか。これは色々なケースがあって骨が折れそうである。しかし、以下のように考えればその商品に何枚の商品券を使うかは<b>そもそも考えなくて良い</b>ことがわかる。

 1. その時にもっとも高額な商品に商品券を使い、価格を半分にする。
 2. その商品の価格を半分にした後に、再び全ての商品の中で一番高いものを探す。
 3. 1,2を商品券がなくなるまで（すなわち\\(M\\)回)繰り返す。
 4. 最後に残った（割引済）商品の価格の和を求める。

次に実際の実装について。。上記を実現するに当たってListを使うと、毎回
 
 - もっとも高い商品を見つける
 - そのアイテムの値の変更（もっとも高い商品を半額にすること）

する必要があり、最大値を見つけるにはソートをしないといけない。
毎回効率の良いソートを行っても\\(O(M \times N logN)\\)の計算量が必要である。
これでは制限時間に間に合わない。

そこで<font color="red"><b>プライオリティーキュー</b></font>を使う。プライオリティーキューは[ヒープ](https://en.wikipedia.org/wiki/Heap_(data_structure))などを活用したデータの格納を行い、要素の追加が\\(O(logN)\\)、木の先頭（ヒープであれば最大値や最小値にすることができる）の参照であれば\\(O(1)\\)で済むので、今回の計算量は\\(O(M \times logN)\\)となって間に合いそうである。

以下、pythonでの実装について説明する。pythonには[heapq](https://docs.python.org/3/library/heapq.html)
というライブラリがあるので、それを使えば特に複雑な実装をすることなく普通の配列をプライオリティーキュー化可能である。また、最小値の取り出しや値の追加も一つの関数で可能である。
```py
# L : List(Lはリスト)
# Convert L from List to PriorityQUeue(Lをプライオリティーキューに変更)
import heapq # Use heapq library(heapqライブラリを使用)
L = [1,2,3,4,5]
heapq.heapify(L)
print(L)

# heappop: pop out the minimum number and return it 
# 最小値を見つけてそれをプライオリティーキューから取り出し、その値を返す
print(heapq.heappop(L)) # 1
print(L) # [2, 3, 4, 5]

# heappush: add item (新たに値を加える。ここでは100を加えてみる) 
heapq.heappush(L, 100)
print(L) # [2, 3, 4, 5, 100]
```
ところが残念ながら最大値を見つけて取り出す関数はない。(heapq.nlargestは最大値（からいくつか）を示すだけで、取り出してくれない)。しかし、今回のように最大値を取り出したい場合は<b>元の配列の要素を全て\\(-1\\)倍した配列をプライオリティーキュー化すれば、その最小値を取り出すことは元の数列の最大値（の\\(-1\\)倍）の値が取り出せる</b>ことを利用する。そして最後に残った配列の要素を再び\\(-1\\)倍して元に戻せば良い。
この手法で、本問題を解いてみる。

## 実装

```py
N, M = map(int,input().split())
A = list(map(int,input().split()))

# multiply -1 to all the elements (全てのAの要素に-1をかける)
A = [-a for a in A]

# p: number of used discount tickets (pは使用した割引券の数)
p = 0

# convert list to priority queue (Aをプライオリティーキュー化)
import heapq
heapq.heapify(A)

# apply discount ticket to the most expensive product 
# until p reaches to M. 
# pがMになるまで、最高価格の商品に商品券を適用する。

while p < M:
    tmp = heapq.heappop(A)

    # pop the most expensive(this case, the minimum number), 
    # divide by 2 (and add 1 if the original number is negative even)
    # then add to the priority queue  
    # もっとも高価な商品を取り出し（ただし負数にしているので最小値）２で割って割引した値段にし
    # (奇数の場合2で割ってから1を加える必要があることに注意)、元のプライオリティーキューに加え直す

    tmp = tmp // 2 + int(tmp % 2 == 1)
    heapq.heappush(A, tmp)

    #consume one discount ticket(商品券を1枚消費する)
    p += 1
    if p == M:
        break

# culculate the sum of the last left priority queue and multiply -1
# 最後に残ったプライオリティーキューの合計値を求めて、-1をかけて元に戻す
print(-sum(A))
```

