========================================
Heap （堆）
========================================


.. contents:: 目錄


Heap 基本介紹
========================================

這裡的 Heap 指的是資料結構上的 Heap，
不是在討論記憶體時所提到的 Heap （動態分配的記憶體）。

Heap 是特殊的樹狀資料結構，
它符合 Heap 的性質，
也就是每對父節點和其子節點都符合同樣的關聯（例如：大於、小於）。
當所有父節點都大於其子節點時，就稱其為「Max Heap」。
當所有父節點都小於其子節點時，就稱其為「Min Heap」。
Heap 不是完全排序過的資料結構，但可以視為部份有序。

Heap 常被作為 Priority Queue （一種 Abstract Data Type）的實作方式。
而一個 Heap 常見的實作為 Binary Heap，它的樹為 Complete Binary Tree。
Heap 在一些圖論的演算法中扮演極其重要的地位，例如 Dijkstra 演算法。
其他還有在排序中的使用，例如 Heapsort。


常見操作
========================================

基本：

* find-min/find-max
* insert
* extract-min/extract-max
* delete-min/delete-max
* replace

建立：

* create-heap
* heapify
* merge (union)
* meld

檢視：

* size
* is-empty

內部：

* decrease-key/increase-key
* delete
* shift-up
* shift-down


Binary Heap
========================================

+--------------+------------+
| 操作         | 時間複雜度 |
+==============+============+
| find-min     | Θ(1)       |
+--------------+------------+
| delete-min   | Θ(log n)   |
+--------------+------------+
| insert       | Θ(log n)   |
+--------------+------------+
| decrease-key | Θ(log n)   |
+--------------+------------+
| merge        | Θ(n)       |
+--------------+------------+



Binomial Heap
========================================

Binomial Heap 是和 Binary Heap 相似的結構，
但是支援快速合併兩個 Heap （時間複雜度為 O(log n)）。

一個 order k 的 Binomial Heap 會擁有 2^k 個節點和高度 k。
而 Binomial Heap 的名稱就來自於
「在 order n 時，在深度 d 會擁有 C(n; k) 個節點。」（Binomial coefficient）。

+-------+-----------------------+-----------------+
| Order | 節點量                | 高度            |
+=======+=======================+=================+
| 0     | 1                     | 0 （只有 root） |
+-------+-----------------------+-----------------+
| 1     | (order 0) add 2^0 = 2 | 1               |
+-------+-----------------------+-----------------+
| 2     | (order 1) add 2^1 = 4 | 2               |
+-------+-----------------------+-----------------+
| 3     | (order 2) add 2^2 = 8 | 3               |
+-------+-----------------------+-----------------+


.. image:: /images/algorithm/Binomial_Trees.svg


由途中可以看出這結構特殊的結構在合併兩個 order k-1 的 Heap 時，
只需要把其中一個 Heap 作為子樹接在最左邊就可以形成新的 order k 的 Heap 了。


+--------------+--------------------------------------------------+
| 操作         | 時間複雜度                                       |
+==============+==================================================+
| find-min     | Θ(1)                                             |
+--------------+--------------------------------------------------+
| delete-min   | Θ(log n)                                         |
+--------------+--------------------------------------------------+
| insert       | O(log n) （同 merge 一個點）, Θ(1) （Amortized） |
+--------------+--------------------------------------------------+
| decrease-key | Θ(log n)                                         |
+--------------+--------------------------------------------------+
| merge        | O(log n) （n 是比較大的 Heap 的 size）           |
+--------------+--------------------------------------------------+



Fibonacci Heap
========================================

Fibonacci Heap 是由 Michael L. Fredman 和 Robert E. Tarjan
在 1984 年發明的資料結構，
並在 1987 年發佈在一個科學期刊上。
Fibonacci Heap 的名稱源自該資料結構內部使用的 Fibonacci 數字。
Fibonacci Heap 跟其他 Heap 資料結構（例如 Binary Heap、Binomial Heap）比起來，
有較好的 Amortized 執行時間。


理論的執行時間和在實務上的狀況並不完全一樣，
實務上 Binary Heap 常因為 Array-based 的實作
而比 Fibonacci Heap 有更好的效能，
所以 Fibonacci Heap 其實沒有很常被使用。



實作
========================================

實作概況
------------------------------

Heap 通常會使用 Array （固定大小或動態決定大小），
如此一來每個資料間就不需要指標來連繫。
當有新的資料插入 Heap 或從 Heap 中移除時，
Heap 的性質可能會被違反，
此時就必須利用內部的操作平衡。

想從已經有資料的 Array 建立 Heap 可利用 Floyd 演算法，
該演算法只需要線性（Linear）時間即可建立（O(n)），
比起從空的 Heap 開始建立所需的對數線性（Log-linear）時間（O(n log n)）還要快。


Binary Heap
------------------------------

Rust
++++++++++++++++++++

``std::collections::BinaryHeap``

* https://doc.rust-lang.org/std/collections/binary_heap/


+-----------------------------------------+------------+
| 操作                                    | 複雜度     |
+=========================================+============+
| poping largest                          | O(log n)   |
+-----------------------------------------+------------+
| checking largest                        | O(1)       |
+-----------------------------------------+------------+
| vector to binary heap (in-place)        | O(n)       |
+-----------------------------------------+------------+
| binary heap to sorted vector (in-place) | O(n log n) |
+-----------------------------------------+------------+


Python
++++++++++++++++++++

``heapq``

* https://docs.python.org/3/library/heapq.html


參考
========================================

* `Wikipedia - Heap (data structure) <https://en.wikipedia.org/wiki/Heap_(data_structure)>`_
* `Wikipedia - Fibonacci heap <https://en.wikipedia.org/wiki/Fibonacci_heap>`_
* `Wikipedia - Binomial heap <https://en.wikipedia.org/wiki/Binomial_heap>`_
