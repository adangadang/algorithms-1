# 找换硬币
-------------
## 问题

考虑用最少的硬币找n美分零钱的问题。假设每种硬币的面额都是整数。

A.设计贪心算法求解找零问题，假定有25美分、10美分、5美分和1美分4种面额的硬币。证明你的算法能找到最优解。

B.假定硬币的面额是c的幂，即面额c0,c1,...,ck,c和k为整数，c>1,k>=1.证明：贪心算法总能得到最优解。

C.设计一组硬币面额，使得贪心算法不能保证得到最优解。这组硬币面额中应该包含1美分，使得对每个零钱值都存在找零方案。

D.设计一个O(nk)时间的找零算法，适用于任何k种不同面额的硬币，假定问题包含1美分硬币。

-------------

## 证明：
### A:
- cm为<n的最大值,必然存在于最后的一个最优解中
  n > 25: cm = 25 设最优集合A，不包括25，则A包括a1个10，a2个5，a3个1, 25总可以= 2 * 10 + 5 或者 3*10-5，
          总是可构造B包括a0个25，a1-2*a0个10，a2-a0个5，a3个1, a0+a1-2*a0+a2-a0+a3 = a1+a2+a3-2a0 < a1+a2+a3，
          或者a0个25，a1-3*a0个10,a2+a0个5，a0+a1-3a0+a2+a0+a3 = a1+a2+a3-a0<a1+a2+a3
          与A是最优集合矛盾
  其他n的情况同理，所以必然存在最优解包含最大值cm
- 如果选择了最大值cm, 问题归结为在n-cm条件下，求解一个最优解A'，cm'为<n-cm的最大值，必然在A'中
  设n'= n-cm，则问题转化为上一个证明，最终得到A'，而 A = A' + {cm}
  
### B:
- j为使c^j<n的最大值,必然存在于最后的一个最优解中 
  设最优集合为A, 不包括c^j, 则 A包括ai个c^j( 0<=i<=j-1)，总可以找到c^j = sigma(c^(j-i)*c^i)(i = 0...j-1)， 因为c>1，所以c^(j-i) > 1, 所以总
  可以构造一个A', 包括sigma((ai - c^(j-i)))+1个元素，小于sigma(ai), 与A是最优集合矛盾， 所以必然存在最优几个包含c^j
- 如果选择了最大值c^j, 问题归结为在n-c^j条件下，求解一个最优解A'，j'为使c^j'<n-c^j的最大值，必然在A'中
  设n'= n-c^j，则问题转化为上一个证明，最终得到A'，而 A = A' + {c^j}
  
### C:
构造不能被整数个其他元素替换的元素，如25，20，1(题目要求必须包括), 对于n = 60, 最优集合为{20，20，20}， 但贪心结果{25,25 {10{1}}}

### D:
01背包问题

# 最小化平均结束时间的调度
-------------
## 问题
![](https://github.com/shady831213/algorithms/blob/master/greedy/static/greedy16-2.PNG)
![](https://github.com/shady831213/algorithms/blob/master/greedy/static/greedy16-2-1.PNG)

-------------
## 思路

### A:
- p为最小值的a, 设am, 必有一个最优解的第一个元素为am
  设最优解A, 第一个元素为ak, 如果ak = am，得证；如果ak != am，am必为 1...n中的一个，设am为第j个元素，此时平均完成时间为
  1/n * (pk + (2pk + pk+1) + (3pk + 2pk+1 + pk+2) +...+(j * pk + (j-1) * pk+1 + ...+ pj-1 + pm) + ...+(n * pk +...+(n-j) * pm +...+pn))
  = 1/n * ((n * (n-1))/2 * pk + ((n-j) * (n-j-1))/2 * pm + P*) P* 为其余项
  = 1/n * ((pk+pm) * ((n-j) * (n-j-1))/2 + ((n * (n-1))/2 +  ((n-j) * (n-j-1))/2) * pk + P*) P* 为其余项
  构造一个解A'，交换ak和am的顺序得 平均完成时间
  1/n * ((pm+pk) * ((n-j) * (n-j-1))/2 + ((n * (n-1))/2 + ((n-j) * (n-j-1))/2) * pm + P*) P* 为其余项
  因为pm < pk, 所以A'的时间小于A, 与 A为最优解矛盾，得证
- 如果选择了最小p的am, 问题可归结为剩余中寻找最小p的元素
  在原集合去掉am，可转化为上面的问题。
  
### B:
所有任务从t0开始有效，并ri开始递减，每个单位时间，可运行的task组成一个所有task的子集，子集中选择剩余时间最短的。问题可转化为 A
  