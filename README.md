
# ACGO 巅峰赛\#15 \- 题目解析



> 间隔四个月再战 ACGO Rated，鉴于最近学业繁忙，比赛打地都不是很频繁。虽然这次没有 AK 排位赛（我可以说是因为周末太忙，没有充足的时间思考题目…（好吧，其实也许是因为我把 T5 给想复杂了））。
> 
> 
> 本文依旧提供每道题的完整解析（因为我在赛后把题目做出来了）。


### T1 \- 高塔


**题目链接跳转：[点击跳转](https://github.com)**



> 插一句题外话，这道题的题目编号挺有趣的。


没有什么特别难的点，循环读入每一个数字，读入后跟第一个输入的数字比较大小，如果读入的数字比第一个读入的数字要大（即 ai\>a1），直接输出 i 并结束主程序即可。


本题的 C\+\+ 代码如下：



```
#include 
using namespace std;

int n, arr[105];

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    cin >> n;
    for (int i=1; i<=n; i++){
        cin >> arr[i];
        if (arr[i] > arr[1]){
            cout << i << endl;
            return 0;
        }
    }
    cout << -1 << endl;
    return 0;
}

```

本题的 Python 代码如下：



```
n = int(input())
arr = list(map(int, input().split()))

for i in range(1, n + 1):
    if arr[i - 1] > arr[0]:
        print(i)
        break
else:
    print(-1)

```

### T2 \- 营养均衡


**题目链接跳转：[点击跳转](https://github.com)**


也是一道入门题目，没有什么比较难的地方，重点是把题目读清楚了。


我们设置一个数组 arr，其中 arri 表示种营养元素还需要的摄入量。那么，如果 arri≤0 的话，就表示该种营养元素的摄入量已经达到了 “健康饮食” 的所需标准了。按照题意模拟一下即可，最后遍历一整个数组判断是否有无法满足的元素。换句话说，只要有任意的 ∀i，满足 arri\>0 就需要输出 `No`。


本题的 C\+\+ 代码如下：



```
#include 
using namespace std;

int n, m;
long long arr[1005];

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    cin >> n >> m;
    for (int i=1; i<=m; i++) 
        cin >> arr[i];
    for (int i=1; i<=n; i++){
        for (int j=1; j<=m; j++){
            int t; cin >> t;
            arr[j] -= t;
        }
    }
    for (int i=1; i<=m; i++){
        if (arr[i] > 0){
            cout << "No" << endl;
            return 0;
        }
    }
    cout << "Yes" << endl;
    return 0;
}

```

本题的 Python 代码如下：



```
n, m = map(int, input().split())

arr = list(map(int, input().split()))

for _ in range(n):
    t = list(map(int, input().split()))
    for j in range(m):
        arr[j] -= t[j]

if any(x > 0 for x in arr):
    print("No")
else:
    print("Yes")

```

### T3 \- ^\_^ 还是 😦


**题目链接跳转：[点击跳转](https://github.com)**



> 一道简单的思维题目，难度定在【普及\-】还算是合理的。不过 USACO 的 Bronze 组别特别喜欢考这种类似的思维题目。


**普通算法**


考虑采用贪心的思路，先把序列按照从大到小的原则排序。暴力枚举一个节点 i，判断是否有可能满足选择前 i 个数字 −1，剩下的数字都至少 \+1 的情况下所有的数字都大于零。


那么该如何快速的判断是否所有的数字都大于零呢？首先可以肯定的是，后 n−i 个数字一定是大于零的，因为这些数字只会增加不会减少。所以我们把重点放在前 i 个数字上面。由于数组已经是有序的，因此如果第 i 个数字是大于 1 的，那么前 i 个数字在减去 1 之后也一定是正整数。


由于使用了排序算法，本算法的单次查询时间复杂度在 O(Nlog2⁡N) 级别，总时间复杂度为 O(N2log2⁡N)，可以在 1s 内通过所有的测试点。


本题的 C\+\+ 代码如下：



```
#include 
#include 
#include 
#include 
using namespace std;

int n;
int arr[1005];

void solve(){
    cin >> n;
    long long sum = 0;
    for (int i=1, t; i<=n; i++){
        cin >> arr[i];
    }
    sort(arr+1, arr+1+n, greater<int>());
    if (n == 1) {
    	cout << ":-(" << endl;
        return ;
    }
    // 暴力枚举，选择前 i 个数字 - 1，剩下的所有数字都至少 + 1。
    bool flag = 0;
    for (int i=1; i<=n; i++){
        sum += arr[i];
        if (arr[i] == 1) break;
        if (sum - (n - i) >= i) flag = 1;
    }
    cout << (flag ? "^_^" : ":-(") << endl;
    return ;
}

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    int T; cin >> T;
    while(T--) solve();
    return 0;
}

```

本题的 Python 代码如下：



```
def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    # 对数组降序排序
    arr.sort(reverse=True)
    
    if n == 1:
        print(":-(")
        return

    # 暴力枚举前 i 个数字 - 1，剩下的数字 +1
    sum_ = 0
    flag = False
    for i in range(1, n + 1):
        sum_ += arr[i - 1]
        if arr[i - 1] == 1:
            break
        if sum_ - (n - i) >= i:
            flag = True
    
    print("^_^" if flag else ":-(")

def main():
    T = int(input())
    for _ in range(T):
        solve()

if __name__ == "__main__":
    main()

```

**二分答案优化**


注意到答案是单调的，因此可以使用二分答案的算法来将算法的单次查询复杂度降低到 O(log2⁡N) 级别，因此该算法的总时间复杂度为 O(Nlog2⁡N)。


优化后的 C\+\+ 代码如下：



```
#include 
#include 
using namespace std;

int n;
int arr[1005];

void solve() {
    cin >> n;
    for (int i = 1; i <= n; i++) 
        cin >> arr[i];
    
    sort(arr + 1, arr + 1 + n, greater<int>());
    
    if (n == 1) {
        cout << ":-(" << endl;
        return;
    }

    int left = 1, right = n, res = -1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (arr[mid] > 1) {
            res = mid;
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    cout << (res != -1 ? "^_^" : ":-(") << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    int T; cin >> T;
    while (T--) solve();
    return 0;
}

```

优化后的 Python 算法如下：



```
def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    # 对数组降序排序
    arr.sort(reverse=True)

    if n == 1:
        print(":-(")
        return

    left, right, res = 0, n - 1, -1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] > 1:
            res = mid
            right = mid - 1
        else:
            left = mid + 1

    print("^_^" if res != -1 else ":-(")

def main():
    T = int(input())
    for _ in range(T):
        solve()

if __name__ == "__main__":
    main()

```

### T4 \- Azusa的计划


**题目链接跳转：[点击跳转](https://github.com)**


这道题的难度也不是很高，稍微思考一下即可。


任何事件时间 t 对 (a\+b) 取模后，事件可以映射到一个固定的周期内。这样，问题就转化为一个固定长度的区间检查问题。


因此，在读入数字后，将所有的数字对 (a\+b) 取模并排序，如果数字分布（序列的最大值和最小值的差值天数）在 a 范围内即可满足将所有的日程安排在休息日当中。但需要注意的是，两个日期的差值天数不能单纯地使用数字相减的方法求得。以正常 7 天为一周作为范例，周一和周日的日期差值为 1 天，而不是 7−1\=6 天。这也是本题最难的部分。


如果做过 **区间 DP** 的用户应该能非常快速地想到如果数据是一个 “环状” 的情况下该如何解决问题（参考题目：石子合并（标准版））。我们可以使用 “剖环成链” 的方法，将环中的元素复制一遍并将每个数字增加 (a\+b)，拼接在原数组的末尾，这样一个长度为 n 的环就被扩展为一个长度为 2n 的线性数组。


最后只需要遍历这个数组内所有长度为 n 的区间 \[i,n\+i−1]，判断是否有任意一个区间的最大值和最小值的差在 a 以内即可判断是否可以讲所有的日程安排都分不在休息日中。


本题的时间复杂度为 O(Nlog2⁡N)。


本题的 C\+\+ 代码如下：



```
#include 
#include 
using namespace std;

int n, a, b;
int arr[500005];
int maximum, minimum = 0x7f7f7f7f;

int main(){
	ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    cin >> n >> a >> b;
    for (int i=1; i<=n; i++){
    	cin >> arr[i];
        arr[i] %= (a + b);
    }
    sort(arr+1, arr+1+n);
    for (int i=1; i<=n; i++){
    	arr[i+n] = arr[i] + (a + b);
    }
    bool flag = 0;
    for (int i=1; i+n-1<=2*n; i++) {
        if (arr[i+n-1] - arr[i] < a)
            flag = 1;
    }
    cout << (flag ? "Yes" : "No") << endl;
}

```

本题的 Python 代码如下：



```
def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    n, a, b = map(int, data[:3])
    arr = list(map(int, data[3:]))
    
    mod_value = a + b
    arr = [x % mod_value for x in arr]
    
    arr.sort()
    
    arr += [x + mod_value for x in arr]

    flag = False
    for i in range(n):
        if arr[i + n - 1] - arr[i] < a:
            flag = True
            break

    print("Yes" if flag else "No")

if __name__ == "__main__":
    main()

```

### T5 \- 前缀和问题


**题目链接跳转：[点击跳转](https://github.com)**



> 我个人认为这道题比最后一道题要难，也许是因为这类题目做的比较少的原因，看到题目后不知道从哪下手。


使用分类讨论的方法，设置一个阈值 S，考虑暴力枚举所有 b\>S 的情况，并离线优化 b≤S 的情况。将 S 设置为 N，则有：


1. 对于大步长 b\>S，任意一次查询只需要最多遍历 550（即 N）次就可以算出答案，因此暴力枚举这部分。
2. 对于小步长 b≤S，按 b 分组批量离线查询。


对于大步长部分，每一次查询的时间复杂度为 O(N)，在最坏情况下总时间复杂度为 O(N×N)。对于小步长的部分，每一次查询的时间复杂度约为 O(n)，在最坏情况下的时间复杂度为 O(N×N)，因此本题在最坏情况下的渐进时间复杂度为：


O(N×N)最后，本题的 C\+\+ 代码如下：



```
#include 
using namespace std;

typedef long long LL;

struct Query {
    int id;  
    int a, b;   
};

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    
    int n; cin >> n;
    
    vector a_arr(n + 1, 0);
    for(int i =1; i <=n; i++) cin >> a_arr[i];
    
    int q; cin >> q;
    
    vector queries(q);
    for(int i =0; i < q; i++){
        cin >> queries[i].a >> queries[i].b;
        queries[i].id = i;
    }
    
    int S = 550;
    
    // 分组查询：小步长和大步长
    // 对于小步长 b <= S，按 b 分组
    // 对于大步长 b > S，单独存储
    vectorint, int>>> small_b_queries(S +1, vectorint, int>>()); // small_b_queries[b]存储 (a, id)
    vectorint, int>> large_b_queries; // 存储 (a, id) for b > S
    
    for(int i =0; i < q; i++) {
        if(queries[i].b <= S)
            small_b_queries[queries[i].b].emplace_back(queries[i].a, queries[i].id);
        else
            large_b_queries.emplace_back(make_pair(queries[i].a, queries[i].id));
    }
    
    vector res(q, 0);
    
    // 预处理小步长查询
    // 对每个 b =1 to S
    for(int b =1; b <= S; b++){
        if(small_b_queries[b].empty()) continue;
        
        // 创建一个临时数组 s_arr，用于存储当前步长 b 的累加和
        // 从 n downto 1
        // s_arr[a] = a_arr[a] + s_arr[a + b] (如果 a + b <=n)
        // 否则 s_arr[a] = a[a]
        vector s_arr(n + 5, 0);
        for(int a = n; a >=1; a--){
            if(a + b <= n){
                s_arr[a] = a_arr[a] + s_arr[a + b];
            }
            else{
                s_arr[a] = a_arr[a];
            }
        }
        
        // 回答所有步长为 b 的查询
        for(auto &[a, id] : small_b_queries[b]){
            res[id] = s_arr[a];
        }
    }
    
    // 处理大步长查询
    // 由于 b > S，且 S = 550，所以每个查询最多需要 ~550 次操作
    for(auto &[a, id] : large_b_queries){
        LL sum = 0;
        int current = a;
        while(current <= n){
            sum += a_arr[current];
            current += queries[id].b;
        }
        res[id] = sum;
    }
    
    for(int i =0; i < q; i++) 
        cout << res[i] << "\n";
    
    return 0;
}

```

本题的 Python 代码如下（不保证可以通过所有的测试点）：



```
class Query:
    def __init__(self, id, a, b):
        self.id = id
        self.a = a
        self.b = b

def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    n = int(data[0])
    a_arr = [0] * (n + 1)
    for i in range(1, n + 1):
        a_arr[i] = int(data[i])
    
    q = int(data[n + 1])
    queries = []
    idx = n + 2
    for i in range(q):
        a, b = int(data[idx]), int(data[idx + 1])
        queries.append(Query(i, a, b))
        idx += 2
    
    S = 550
    small_b_queries = [[] for _ in range(S + 1)]
    large_b_queries = []
    
    for query in queries:
        if query.b <= S:
            small_b_queries[query.b].append((query.a, query.id))
        else:
            large_b_queries.append((query.a, query.id))
    
    res = [0] * q
    
    for b in range(1, S + 1):
        if not small_b_queries[b]:
            continue
        
        s_arr = [0] * (n + 5)
        for a in range(n, 0, -1):
            if a + b <= n:
                s_arr[a] = a_arr[a] + s_arr[a + b]
            else:
                s_arr[a] = a_arr[a]
        
        for a, id in small_b_queries[b]:
            res[id] = s_arr[a]
    
    for a, id in large_b_queries:
        sum_val = 0
        current = a
        b = queries[id].b
        while current <= n:
            sum_val += a_arr[current]
            current += b
        res[id] = sum_val
    
    sys.stdout.write("\n".join(map(str, res)) + "\n")

if __name__ == "__main__":
    main()

```

### T6 \- 划分区间


**题目链接跳转：[点击跳转](https://github.com)**


一道线段树优化动态规划的题目，难度趋近于 CSP 提高组的题目和 USACO 铂金组的中等题。一眼可以看出题目是一个典型的动态规划问题，但奈何数据量太大了，O(N2) 的复杂度肯定会 TLE。但无论如何都是 “车到山前必有路”，看到数据范围不用怕，先打一个暴力的动态规划再优化。


按照一位 OI 大神的说法：“所有的动态规划优化都是在基础的代码上等量代换”。


与打家劫舍等线性动态规划类似，对于本题而言，设状态的定义为 dpi 表示对 \[1,i] 这个序列划分后可得到的最大贡献。通过暴力遍历 j,(1≤j\<i)，表示将 (j,i] 归位一组。另设 A(j,i) 为区间 (j,i] 的贡献值。根据以上信息可以得到状态转移方程：


dpi\=max0≤j\<i(dpj\+A(j,i))接下来就是关于 A(j,i) 的计算了。设前缀和数组 Si 表示从区间 \[1,i] 的和，那么 (j,i] 区间的和可以被表示为 S\[i]−S\[j]。根据不同的 S\[i]−S\[j]，则有以下三种情况：


1. 当 S\[i]−S\[j]\>0 时，证明该区间的和是正数，贡献为 i−j。
2. 当 S\[i]−S\[j]\=0 时，该区间的和为零，贡献为 0。
3. 当 S\[i]−S\[j]\<0 时，证明该区间的和是负数，贡献为 −(i−j)\=j−i。


综上所述，可以写出一个暴力版本的动态规划代码：



```
#include 
#include 
#include 
#include 
using namespace std;

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    int n;
    cin >> n;
    vector<int> A(n + 1);
    vector<long long> S(n + 1, 0); 

    for (int i = 1; i <= n; i++) {
        cin >> A[i];
        S[i] = S[i - 1] + A[i];
    }

    vector<long long> dp(n + 1, LLONG_MIN);
    dp[0] = 0;

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            if (S[i] - S[j] > 0)
                dp[i] = max(dp[i], dp[j] + (i - j));
            if (S[i] - S[j] < 0)
                dp[i] = max(dp[i], dp[j] - (i - j));
            if (S[i] - S[j] == 0)
                dp[i] = max(dp[i], dp[j]);
        }
    }

    cout << dp[n] << endl;
    return 0;
}

```

接下来考虑优化这个动态规划，注意到每一次寻找 max 都非常耗时，每一次都需要遍历一遍才能求出最大值。有没有一种方法可以快速求出某一个区间的最大值呢？答案就是线段树。线段树是一个非常好的快速求解区间最值问题的数据结构。



> 更多有关区间最值问题的学习请参考：\[\# 浅入线段树与区间最值问题](\# 浅入线段树与区间最值问题)


综上，我们可以通过构建线段树来快速求得答案。简化三种情况可得：



```
if (S[i] - S[j] > 0)
    dp[i] = max(dp[i], dp[j] - j + i);
if (S[i] - S[j] < 0)
    dp[i] = max(dp[i], dp[j] + j - i));
if (S[i] - S[j] == 0)
    dp[i] = max(dp[i], dp[j]);

```

因此我们构造三棵线段树，分别来维护这三个区间：


1. max0≤j\<idpj
2. max0≤j\<i(dpj−j)
3. max0≤j\<i(dpj\+j)


然而我们的线段树不能仅仅维护这个区间，因为这三个的最大值还被 A(j,i) 的三种状态所限制着，因此，我们需要找的是满足 Si−Sj 在特定条件下的最大值。这样就出现了另一个严重的问题，Si 的值可能非常的大，因此我们需要对前缀和数组离散化一下（坐标压缩：类似于权值线段树的写法）才可以防止内存超限。


这样子对于每次寻找最大值，都可以在 O(log2⁡N) 的情况下找到。本算法的总时间复杂度也控制在了 O(N×log2⁡N) 级别。


本题的 C\+\+ 代码如下：



```
#include 
#include 
#include 
#include 
#define int long long
using namespace std;

const int MAX = 500005;

struct SegmentTree {
    int size;
    vector<int> tree;

    SegmentTree(int n_) {
        size = 1;
        while (size < n_) size <<=1;
        tree.assign(2*size, LLONG_MIN);
    }

    void update(int pos, int value){
        pos += size -1;
        tree[pos] = max(tree[pos], value);
        while(pos >1){
            pos >>=1;
            tree[pos] = max(tree[2*pos], tree[2*pos+1]);
        }
    }

    int query(int l, int r){
        l += size -1; r += size -1;
        int res = LLONG_MIN;
        while(l <= r){
            if(l%2 ==1)
                res = max(res, tree[l++]);
            if(r%2 ==0)
                res = max(res, tree[r--]);
            l >>=1; r >>=1;
        }
        return res;
    }
};

int n;
int A[MAX], S[MAX], aintS_arr[MAX];

signed main(){
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    
    cin >> n;
    for(int i=1;i<=n;i++) cin >> A[i];
    
    S[0] = 0;
    for(int i=1;i<=n;i++) S[i] = S[i-1] + A[i];
    
    for(int i=0;i<=n;i++) aintS_arr[i] = S[i];
    sort(aintS_arr, aintS_arr + n +1);
    int m = unique(aintS_arr, aintS_arr + n +1) - aintS_arr;
    
    auto get_idx = [&](int x) -> int {
        return lower_bound(aintS_arr, aintS_arr + m, x) - aintS_arr +1;
    };
    
    SegmentTree BIT1(m); // max(dp[j]-j)
    SegmentTree BIT2(m); // max(dp[j])
    SegmentTree BIT3(m); // max(dp[j]+j)
    
    int idx_S0 = get_idx(S[0]);
    BIT1.update(idx_S0, 0);
    BIT2.update(idx_S0, 0);
    BIT3.update(idx_S0, 0);
    
    int dp_i = LLONG_MIN;
    for(int i=1;i<=n;i++){
        int Si = S[i];
        int idx_Si = get_idx(Si);
        
        int option1 = LLONG_MIN;
        if(idx_Si >1){
            int temp = BIT1.query(1, idx_Si -1);
            if(temp != LLONG_MIN){
                option1 = temp + i;
            }
        }
        
        int option2 = BIT2.query(idx_Si, idx_Si);
        
        int option3 = LLONG_MIN;
        if(idx_Si < m){
            int temp = BIT3.query(idx_Si +1, m);
            if(temp != LLONG_MIN){
                option3 = temp - i;
            }
        }
        
        dp_i = max(option1, max(option2, option3));
        
        BIT1.update(idx_Si, dp_i - i);
        BIT2.update(idx_Si, dp_i);
        BIT3.update(idx_Si, dp_i + i);
    }
    
    cout << dp_i;
}

```

本题的 Python 代码如下（由于 Python 常数过大，因此没有办法通过这道题所有的测试点，但是代码的正确性没有问题）：



```
class SegmentTree:
    def __init__(self, n):
        self.size = 1
        while self.size < n:
            self.size *= 2
        self.tree = [float('-inf')] * (2 * self.size)

    def update(self, pos, value):
        pos += self.size - 1
        self.tree[pos] = max(self.tree[pos], value)
        while pos > 1:
            pos //= 2
            self.tree[pos] = max(self.tree[2 * pos], self.tree[2 * pos + 1])

    def query(self, l, r):
        l += self.size - 1
        r += self.size - 1
        res = float('-inf')
        while l <= r:
            if l % 2 == 1:
                res = max(res, self.tree[l])
                l += 1
            if r % 2 == 0:
                res = max(res, self.tree[r])
                r -= 1
            l //= 2
            r //= 2
        return res


def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    n = int(data[0])
    A = list(map(int, data[1:n + 1]))
    
    S = [0] * (n + 1)
    for i in range(1, n + 1):
        S[i] = S[i - 1] + A[i - 1]
    
    aintS_arr = S[:]
    aintS_arr.sort()
    m = len(set(aintS_arr))
    aintS_arr = sorted(set(aintS_arr))
    
    def get_idx(x):
        # Return the index in the compressed array
        return aintS_arr.index(x) + 1
    
    BIT1 = SegmentTree(m)  # max(dp[j] - j)
    BIT2 = SegmentTree(m)  # max(dp[j])
    BIT3 = SegmentTree(m)  # max(dp[j] + j)
    
    idx_S0 = get_idx(S[0])
    BIT1.update(idx_S0, 0)
    BIT2.update(idx_S0, 0)
    BIT3.update(idx_S0, 0)
    
    dp_i = float('-inf')
    for i in range(1, n + 1):
        Si = S[i]
        idx_Si = get_idx(Si)
        
        option1 = float('-inf')
        if idx_Si > 1:
            temp = BIT1.query(1, idx_Si - 1)
            if temp != float('-inf'):
                option1 = temp + i
        
        option2 = BIT2.query(idx_Si, idx_Si)
        
        option3 = float('-inf')
        if idx_Si < m:
            temp = BIT3.query(idx_Si + 1, m)
            if temp != float('-inf'):
                option3 = temp - i
        
        dp_i = max(option1, option2, option3)
        
        BIT1.update(idx_Si, dp_i - i)
        BIT2.update(idx_Si, dp_i)
        BIT3.update(idx_Si, dp_i + i)
    
    print(dp_i)

if __name__ == "__main__":
    main()

```

# ACGO 巅峰赛\#15 \- 题目解析



> 间隔四个月再战 ACGO Rated，鉴于最近学业繁忙，比赛打地都不是很频繁。虽然这次没有 AK 排位赛（我可以说是因为周末太忙，没有充足的时间思考题目…（好吧，其实也许是因为我把 T5 给想复杂了））。
> 
> 
> 本文依旧提供每道题的完整解析（因为我在赛后把题目做出来了）。


### T1 \- 高塔


**题目链接跳转：[点击跳转](https://github.com):[cmespeed楚门加速器](https://77yingba.com)**



> 插一句题外话，这道题的题目编号挺有趣的。


没有什么特别难的点，循环读入每一个数字，读入后跟第一个输入的数字比较大小，如果读入的数字比第一个读入的数字要大（即 ai\>a1），直接输出 i 并结束主程序即可。


本题的 C\+\+ 代码如下：



```
#include 
using namespace std;

int n, arr[105];

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    cin >> n;
    for (int i=1; i<=n; i++){
        cin >> arr[i];
        if (arr[i] > arr[1]){
            cout << i << endl;
            return 0;
        }
    }
    cout << -1 << endl;
    return 0;
}

```

本题的 Python 代码如下：



```
n = int(input())
arr = list(map(int, input().split()))

for i in range(1, n + 1):
    if arr[i - 1] > arr[0]:
        print(i)
        break
else:
    print(-1)

```

### T2 \- 营养均衡


**题目链接跳转：[点击跳转](https://github.com)**


也是一道入门题目，没有什么比较难的地方，重点是把题目读清楚了。


我们设置一个数组 arr，其中 arri 表示种营养元素还需要的摄入量。那么，如果 arri≤0 的话，就表示该种营养元素的摄入量已经达到了 “健康饮食” 的所需标准了。按照题意模拟一下即可，最后遍历一整个数组判断是否有无法满足的元素。换句话说，只要有任意的 ∀i，满足 arri\>0 就需要输出 `No`。


本题的 C\+\+ 代码如下：



```
#include 
using namespace std;

int n, m;
long long arr[1005];

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    cin >> n >> m;
    for (int i=1; i<=m; i++) 
        cin >> arr[i];
    for (int i=1; i<=n; i++){
        for (int j=1; j<=m; j++){
            int t; cin >> t;
            arr[j] -= t;
        }
    }
    for (int i=1; i<=m; i++){
        if (arr[i] > 0){
            cout << "No" << endl;
            return 0;
        }
    }
    cout << "Yes" << endl;
    return 0;
}

```

本题的 Python 代码如下：



```
n, m = map(int, input().split())

arr = list(map(int, input().split()))

for _ in range(n):
    t = list(map(int, input().split()))
    for j in range(m):
        arr[j] -= t[j]

if any(x > 0 for x in arr):
    print("No")
else:
    print("Yes")

```

### T3 \- ^\_^ 还是 😦


**题目链接跳转：[点击跳转](https://github.com)**



> 一道简单的思维题目，难度定在【普及\-】还算是合理的。不过 USACO 的 Bronze 组别特别喜欢考这种类似的思维题目。


**普通算法**


考虑采用贪心的思路，先把序列按照从大到小的原则排序。暴力枚举一个节点 i，判断是否有可能满足选择前 i 个数字 −1，剩下的数字都至少 \+1 的情况下所有的数字都大于零。


那么该如何快速的判断是否所有的数字都大于零呢？首先可以肯定的是，后 n−i 个数字一定是大于零的，因为这些数字只会增加不会减少。所以我们把重点放在前 i 个数字上面。由于数组已经是有序的，因此如果第 i 个数字是大于 1 的，那么前 i 个数字在减去 1 之后也一定是正整数。


由于使用了排序算法，本算法的单次查询时间复杂度在 O(Nlog2⁡N) 级别，总时间复杂度为 O(N2log2⁡N)，可以在 1s 内通过所有的测试点。


本题的 C\+\+ 代码如下：



```
#include 
#include 
#include 
#include 
using namespace std;

int n;
int arr[1005];

void solve(){
    cin >> n;
    long long sum = 0;
    for (int i=1, t; i<=n; i++){
        cin >> arr[i];
    }
    sort(arr+1, arr+1+n, greater<int>());
    if (n == 1) {
    	cout << ":-(" << endl;
        return ;
    }
    // 暴力枚举，选择前 i 个数字 - 1，剩下的所有数字都至少 + 1。
    bool flag = 0;
    for (int i=1; i<=n; i++){
        sum += arr[i];
        if (arr[i] == 1) break;
        if (sum - (n - i) >= i) flag = 1;
    }
    cout << (flag ? "^_^" : ":-(") << endl;
    return ;
}

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    int T; cin >> T;
    while(T--) solve();
    return 0;
}

```

本题的 Python 代码如下：



```
def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    # 对数组降序排序
    arr.sort(reverse=True)
    
    if n == 1:
        print(":-(")
        return

    # 暴力枚举前 i 个数字 - 1，剩下的数字 +1
    sum_ = 0
    flag = False
    for i in range(1, n + 1):
        sum_ += arr[i - 1]
        if arr[i - 1] == 1:
            break
        if sum_ - (n - i) >= i:
            flag = True
    
    print("^_^" if flag else ":-(")

def main():
    T = int(input())
    for _ in range(T):
        solve()

if __name__ == "__main__":
    main()

```

**二分答案优化**


注意到答案是单调的，因此可以使用二分答案的算法来将算法的单次查询复杂度降低到 O(log2⁡N) 级别，因此该算法的总时间复杂度为 O(Nlog2⁡N)。


优化后的 C\+\+ 代码如下：



```
#include 
#include 
using namespace std;

int n;
int arr[1005];

void solve() {
    cin >> n;
    for (int i = 1; i <= n; i++) 
        cin >> arr[i];
    
    sort(arr + 1, arr + 1 + n, greater<int>());
    
    if (n == 1) {
        cout << ":-(" << endl;
        return;
    }

    int left = 1, right = n, res = -1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (arr[mid] > 1) {
            res = mid;
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    cout << (res != -1 ? "^_^" : ":-(") << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    int T; cin >> T;
    while (T--) solve();
    return 0;
}

```

优化后的 Python 算法如下：



```
def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    # 对数组降序排序
    arr.sort(reverse=True)

    if n == 1:
        print(":-(")
        return

    left, right, res = 0, n - 1, -1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] > 1:
            res = mid
            right = mid - 1
        else:
            left = mid + 1

    print("^_^" if res != -1 else ":-(")

def main():
    T = int(input())
    for _ in range(T):
        solve()

if __name__ == "__main__":
    main()

```

### T4 \- Azusa的计划


**题目链接跳转：[点击跳转](https://github.com)**


这道题的难度也不是很高，稍微思考一下即可。


任何事件时间 t 对 (a\+b) 取模后，事件可以映射到一个固定的周期内。这样，问题就转化为一个固定长度的区间检查问题。


因此，在读入数字后，将所有的数字对 (a\+b) 取模并排序，如果数字分布（序列的最大值和最小值的差值天数）在 a 范围内即可满足将所有的日程安排在休息日当中。但需要注意的是，两个日期的差值天数不能单纯地使用数字相减的方法求得。以正常 7 天为一周作为范例，周一和周日的日期差值为 1 天，而不是 7−1\=6 天。这也是本题最难的部分。


如果做过 **区间 DP** 的用户应该能非常快速地想到如果数据是一个 “环状” 的情况下该如何解决问题（参考题目：石子合并（标准版））。我们可以使用 “剖环成链” 的方法，将环中的元素复制一遍并将每个数字增加 (a\+b)，拼接在原数组的末尾，这样一个长度为 n 的环就被扩展为一个长度为 2n 的线性数组。


最后只需要遍历这个数组内所有长度为 n 的区间 \[i,n\+i−1]，判断是否有任意一个区间的最大值和最小值的差在 a 以内即可判断是否可以讲所有的日程安排都分不在休息日中。


本题的时间复杂度为 O(Nlog2⁡N)。


本题的 C\+\+ 代码如下：



```
#include 
#include 
using namespace std;

int n, a, b;
int arr[500005];
int maximum, minimum = 0x7f7f7f7f;

int main(){
	ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    cin >> n >> a >> b;
    for (int i=1; i<=n; i++){
    	cin >> arr[i];
        arr[i] %= (a + b);
    }
    sort(arr+1, arr+1+n);
    for (int i=1; i<=n; i++){
    	arr[i+n] = arr[i] + (a + b);
    }
    bool flag = 0;
    for (int i=1; i+n-1<=2*n; i++) {
        if (arr[i+n-1] - arr[i] < a)
            flag = 1;
    }
    cout << (flag ? "Yes" : "No") << endl;
}

```

本题的 Python 代码如下：



```
def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    n, a, b = map(int, data[:3])
    arr = list(map(int, data[3:]))
    
    mod_value = a + b
    arr = [x % mod_value for x in arr]
    
    arr.sort()
    
    arr += [x + mod_value for x in arr]

    flag = False
    for i in range(n):
        if arr[i + n - 1] - arr[i] < a:
            flag = True
            break

    print("Yes" if flag else "No")

if __name__ == "__main__":
    main()

```

### T5 \- 前缀和问题


**题目链接跳转：[点击跳转](https://github.com)**



> 我个人认为这道题比最后一道题要难，也许是因为这类题目做的比较少的原因，看到题目后不知道从哪下手。


使用分类讨论的方法，设置一个阈值 S，考虑暴力枚举所有 b\>S 的情况，并离线优化 b≤S 的情况。将 S 设置为 N，则有：


1. 对于大步长 b\>S，任意一次查询只需要最多遍历 550（即 N）次就可以算出答案，因此暴力枚举这部分。
2. 对于小步长 b≤S，按 b 分组批量离线查询。


对于大步长部分，每一次查询的时间复杂度为 O(N)，在最坏情况下总时间复杂度为 O(N×N)。对于小步长的部分，每一次查询的时间复杂度约为 O(n)，在最坏情况下的时间复杂度为 O(N×N)，因此本题在最坏情况下的渐进时间复杂度为：


O(N×N)最后，本题的 C\+\+ 代码如下：



```
#include 
using namespace std;

typedef long long LL;

struct Query {
    int id;  
    int a, b;   
};

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    
    int n; cin >> n;
    
    vector a_arr(n + 1, 0);
    for(int i =1; i <=n; i++) cin >> a_arr[i];
    
    int q; cin >> q;
    
    vector queries(q);
    for(int i =0; i < q; i++){
        cin >> queries[i].a >> queries[i].b;
        queries[i].id = i;
    }
    
    int S = 550;
    
    // 分组查询：小步长和大步长
    // 对于小步长 b <= S，按 b 分组
    // 对于大步长 b > S，单独存储
    vectorint, int>>> small_b_queries(S +1, vectorint, int>>()); // small_b_queries[b]存储 (a, id)
    vectorint, int>> large_b_queries; // 存储 (a, id) for b > S
    
    for(int i =0; i < q; i++) {
        if(queries[i].b <= S)
            small_b_queries[queries[i].b].emplace_back(queries[i].a, queries[i].id);
        else
            large_b_queries.emplace_back(make_pair(queries[i].a, queries[i].id));
    }
    
    vector res(q, 0);
    
    // 预处理小步长查询
    // 对每个 b =1 to S
    for(int b =1; b <= S; b++){
        if(small_b_queries[b].empty()) continue;
        
        // 创建一个临时数组 s_arr，用于存储当前步长 b 的累加和
        // 从 n downto 1
        // s_arr[a] = a_arr[a] + s_arr[a + b] (如果 a + b <=n)
        // 否则 s_arr[a] = a[a]
        vector s_arr(n + 5, 0);
        for(int a = n; a >=1; a--){
            if(a + b <= n){
                s_arr[a] = a_arr[a] + s_arr[a + b];
            }
            else{
                s_arr[a] = a_arr[a];
            }
        }
        
        // 回答所有步长为 b 的查询
        for(auto &[a, id] : small_b_queries[b]){
            res[id] = s_arr[a];
        }
    }
    
    // 处理大步长查询
    // 由于 b > S，且 S = 550，所以每个查询最多需要 ~550 次操作
    for(auto &[a, id] : large_b_queries){
        LL sum = 0;
        int current = a;
        while(current <= n){
            sum += a_arr[current];
            current += queries[id].b;
        }
        res[id] = sum;
    }
    
    for(int i =0; i < q; i++) 
        cout << res[i] << "\n";
    
    return 0;
}

```

本题的 Python 代码如下（不保证可以通过所有的测试点）：



```
class Query:
    def __init__(self, id, a, b):
        self.id = id
        self.a = a
        self.b = b

def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    n = int(data[0])
    a_arr = [0] * (n + 1)
    for i in range(1, n + 1):
        a_arr[i] = int(data[i])
    
    q = int(data[n + 1])
    queries = []
    idx = n + 2
    for i in range(q):
        a, b = int(data[idx]), int(data[idx + 1])
        queries.append(Query(i, a, b))
        idx += 2
    
    S = 550
    small_b_queries = [[] for _ in range(S + 1)]
    large_b_queries = []
    
    for query in queries:
        if query.b <= S:
            small_b_queries[query.b].append((query.a, query.id))
        else:
            large_b_queries.append((query.a, query.id))
    
    res = [0] * q
    
    for b in range(1, S + 1):
        if not small_b_queries[b]:
            continue
        
        s_arr = [0] * (n + 5)
        for a in range(n, 0, -1):
            if a + b <= n:
                s_arr[a] = a_arr[a] + s_arr[a + b]
            else:
                s_arr[a] = a_arr[a]
        
        for a, id in small_b_queries[b]:
            res[id] = s_arr[a]
    
    for a, id in large_b_queries:
        sum_val = 0
        current = a
        b = queries[id].b
        while current <= n:
            sum_val += a_arr[current]
            current += b
        res[id] = sum_val
    
    sys.stdout.write("\n".join(map(str, res)) + "\n")

if __name__ == "__main__":
    main()

```

### T6 \- 划分区间


**题目链接跳转：[点击跳转](https://github.com)**


一道线段树优化动态规划的题目，难度趋近于 CSP 提高组的题目和 USACO 铂金组的中等题。一眼可以看出题目是一个典型的动态规划问题，但奈何数据量太大了，O(N2) 的复杂度肯定会 TLE。但无论如何都是 “车到山前必有路”，看到数据范围不用怕，先打一个暴力的动态规划再优化。


按照一位 OI 大神的说法：“所有的动态规划优化都是在基础的代码上等量代换”。


与打家劫舍等线性动态规划类似，对于本题而言，设状态的定义为 dpi 表示对 \[1,i] 这个序列划分后可得到的最大贡献。通过暴力遍历 j,(1≤j\<i)，表示将 (j,i] 归位一组。另设 A(j,i) 为区间 (j,i] 的贡献值。根据以上信息可以得到状态转移方程：


dpi\=max0≤j\<i(dpj\+A(j,i))接下来就是关于 A(j,i) 的计算了。设前缀和数组 Si 表示从区间 \[1,i] 的和，那么 (j,i] 区间的和可以被表示为 S\[i]−S\[j]。根据不同的 S\[i]−S\[j]，则有以下三种情况：


1. 当 S\[i]−S\[j]\>0 时，证明该区间的和是正数，贡献为 i−j。
2. 当 S\[i]−S\[j]\=0 时，该区间的和为零，贡献为 0。
3. 当 S\[i]−S\[j]\<0 时，证明该区间的和是负数，贡献为 −(i−j)\=j−i。


综上所述，可以写出一个暴力版本的动态规划代码：



```
#include 
#include 
#include 
#include 
using namespace std;

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    int n;
    cin >> n;
    vector<int> A(n + 1);
    vector<long long> S(n + 1, 0); 

    for (int i = 1; i <= n; i++) {
        cin >> A[i];
        S[i] = S[i - 1] + A[i];
    }

    vector<long long> dp(n + 1, LLONG_MIN);
    dp[0] = 0;

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            if (S[i] - S[j] > 0)
                dp[i] = max(dp[i], dp[j] + (i - j));
            if (S[i] - S[j] < 0)
                dp[i] = max(dp[i], dp[j] - (i - j));
            if (S[i] - S[j] == 0)
                dp[i] = max(dp[i], dp[j]);
        }
    }

    cout << dp[n] << endl;
    return 0;
}

```

接下来考虑优化这个动态规划，注意到每一次寻找 max 都非常耗时，每一次都需要遍历一遍才能求出最大值。有没有一种方法可以快速求出某一个区间的最大值呢？答案就是线段树。线段树是一个非常好的快速求解区间最值问题的数据结构。



> 更多有关区间最值问题的学习请参考：\[\# 浅入线段树与区间最值问题](\# 浅入线段树与区间最值问题)


综上，我们可以通过构建线段树来快速求得答案。简化三种情况可得：



```
if (S[i] - S[j] > 0)
    dp[i] = max(dp[i], dp[j] - j + i);
if (S[i] - S[j] < 0)
    dp[i] = max(dp[i], dp[j] + j - i));
if (S[i] - S[j] == 0)
    dp[i] = max(dp[i], dp[j]);

```

因此我们构造三棵线段树，分别来维护这三个区间：


1. max0≤j\<idpj
2. max0≤j\<i(dpj−j)
3. max0≤j\<i(dpj\+j)


然而我们的线段树不能仅仅维护这个区间，因为这三个的最大值还被 A(j,i) 的三种状态所限制着，因此，我们需要找的是满足 Si−Sj 在特定条件下的最大值。这样就出现了另一个严重的问题，Si 的值可能非常的大，因此我们需要对前缀和数组离散化一下（坐标压缩：类似于权值线段树的写法）才可以防止内存超限。


这样子对于每次寻找最大值，都可以在 O(log2⁡N) 的情况下找到。本算法的总时间复杂度也控制在了 O(N×log2⁡N) 级别。


本题的 C\+\+ 代码如下：



```
#include 
#include 
#include 
#include 
#define int long long
using namespace std;

const int MAX = 500005;

struct SegmentTree {
    int size;
    vector<int> tree;

    SegmentTree(int n_) {
        size = 1;
        while (size < n_) size <<=1;
        tree.assign(2*size, LLONG_MIN);
    }

    void update(int pos, int value){
        pos += size -1;
        tree[pos] = max(tree[pos], value);
        while(pos >1){
            pos >>=1;
            tree[pos] = max(tree[2*pos], tree[2*pos+1]);
        }
    }

    int query(int l, int r){
        l += size -1; r += size -1;
        int res = LLONG_MIN;
        while(l <= r){
            if(l%2 ==1)
                res = max(res, tree[l++]);
            if(r%2 ==0)
                res = max(res, tree[r--]);
            l >>=1; r >>=1;
        }
        return res;
    }
};

int n;
int A[MAX], S[MAX], aintS_arr[MAX];

signed main(){
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    
    cin >> n;
    for(int i=1;i<=n;i++) cin >> A[i];
    
    S[0] = 0;
    for(int i=1;i<=n;i++) S[i] = S[i-1] + A[i];
    
    for(int i=0;i<=n;i++) aintS_arr[i] = S[i];
    sort(aintS_arr, aintS_arr + n +1);
    int m = unique(aintS_arr, aintS_arr + n +1) - aintS_arr;
    
    auto get_idx = [&](int x) -> int {
        return lower_bound(aintS_arr, aintS_arr + m, x) - aintS_arr +1;
    };
    
    SegmentTree BIT1(m); // max(dp[j]-j)
    SegmentTree BIT2(m); // max(dp[j])
    SegmentTree BIT3(m); // max(dp[j]+j)
    
    int idx_S0 = get_idx(S[0]);
    BIT1.update(idx_S0, 0);
    BIT2.update(idx_S0, 0);
    BIT3.update(idx_S0, 0);
    
    int dp_i = LLONG_MIN;
    for(int i=1;i<=n;i++){
        int Si = S[i];
        int idx_Si = get_idx(Si);
        
        int option1 = LLONG_MIN;
        if(idx_Si >1){
            int temp = BIT1.query(1, idx_Si -1);
            if(temp != LLONG_MIN){
                option1 = temp + i;
            }
        }
        
        int option2 = BIT2.query(idx_Si, idx_Si);
        
        int option3 = LLONG_MIN;
        if(idx_Si < m){
            int temp = BIT3.query(idx_Si +1, m);
            if(temp != LLONG_MIN){
                option3 = temp - i;
            }
        }
        
        dp_i = max(option1, max(option2, option3));
        
        BIT1.update(idx_Si, dp_i - i);
        BIT2.update(idx_Si, dp_i);
        BIT3.update(idx_Si, dp_i + i);
    }
    
    cout << dp_i;
}

```

本题的 Python 代码如下（由于 Python 常数过大，因此没有办法通过这道题所有的测试点，但是代码的正确性没有问题）：



```
class SegmentTree:
    def __init__(self, n):
        self.size = 1
        while self.size < n:
            self.size *= 2
        self.tree = [float('-inf')] * (2 * self.size)

    def update(self, pos, value):
        pos += self.size - 1
        self.tree[pos] = max(self.tree[pos], value)
        while pos > 1:
            pos //= 2
            self.tree[pos] = max(self.tree[2 * pos], self.tree[2 * pos + 1])

    def query(self, l, r):
        l += self.size - 1
        r += self.size - 1
        res = float('-inf')
        while l <= r:
            if l % 2 == 1:
                res = max(res, self.tree[l])
                l += 1
            if r % 2 == 0:
                res = max(res, self.tree[r])
                r -= 1
            l //= 2
            r //= 2
        return res


def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    n = int(data[0])
    A = list(map(int, data[1:n + 1]))
    
    S = [0] * (n + 1)
    for i in range(1, n + 1):
        S[i] = S[i - 1] + A[i - 1]
    
    aintS_arr = S[:]
    aintS_arr.sort()
    m = len(set(aintS_arr))
    aintS_arr = sorted(set(aintS_arr))
    
    def get_idx(x):
        # Return the index in the compressed array
        return aintS_arr.index(x) + 1
    
    BIT1 = SegmentTree(m)  # max(dp[j] - j)
    BIT2 = SegmentTree(m)  # max(dp[j])
    BIT3 = SegmentTree(m)  # max(dp[j] + j)
    
    idx_S0 = get_idx(S[0])
    BIT1.update(idx_S0, 0)
    BIT2.update(idx_S0, 0)
    BIT3.update(idx_S0, 0)
    
    dp_i = float('-inf')
    for i in range(1, n + 1):
        Si = S[i]
        idx_Si = get_idx(Si)
        
        option1 = float('-inf')
        if idx_Si > 1:
            temp = BIT1.query(1, idx_Si - 1)
            if temp != float('-inf'):
                option1 = temp + i
        
        option2 = BIT2.query(idx_Si, idx_Si)
        
        option3 = float('-inf')
        if idx_Si < m:
            temp = BIT3.query(idx_Si + 1, m)
            if temp != float('-inf'):
                option3 = temp - i
        
        dp_i = max(option1, option2, option3)
        
        BIT1.update(idx_Si, dp_i - i)
        BIT2.update(idx_Si, dp_i)
        BIT3.update(idx_Si, dp_i + i)
    
    print(dp_i)

if __name__ == "__main__":
    main()

```

当然也可以用树状数组来写，速度可能会更快一点：



```
#include 
#include 
#include 
#include 
#define int long long
using namespace std;

const int MAX = 500005;

struct FenwickTree {
    int size;
    vector<int> tree;

    FenwickTree(int n_) {
        size = n_;
        tree.assign(size + 1, LLONG_MIN);
    }

    void update(int pos, int value) {
        while (pos <= size) {
            tree[pos] = max(tree[pos], value);
            pos += pos & -pos;
        }
    }

    int query(int pos) {
        int res = LLONG_MIN;
        while (pos > 0) {
            res = max(res, tree[pos]);
            pos -= pos & -pos;
        }
        return res;
    }

    int query(int l, int r) {
        return max(query(r), (l > 1 ? query(l - 1) : LLONG_MIN));
    }
};

int n;
int A[MAX], S[MAX], aintS_arr[MAX];

signed main() {
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    cin >> n;
    for (int i = 1; i <= n; i++) cin >> A[i];

    S[0] = 0;
    for (int i = 1; i <= n; i++) S[i] = S[i - 1] + A[i];

    for (int i = 0; i <= n; i++) aintS_arr[i] = S[i];
    sort(aintS_arr, aintS_arr + n + 1);
    int m = unique(aintS_arr, aintS_arr + n + 1) - aintS_arr;

    auto get_idx = [&](int x) -> int {
        return lower_bound(aintS_arr, aintS_arr + m, x) - aintS_arr + 1;
    };

    FenwickTree BIT1(m); // max(dp[j] - j)
    FenwickTree BIT2(m); // max(dp[j])
    FenwickTree BIT3(m); // max(dp[j] + j)

    int idx_S0 = get_idx(S[0]);
    BIT1.update(idx_S0, 0);
    BIT2.update(idx_S0, 0);
    BIT3.update(idx_S0, 0);

    int dp_i = LLONG_MIN;
    for (int i = 1; i <= n; i++) {
        int Si = S[i];
        int idx_Si = get_idx(Si);

        int option1 = LLONG_MIN;
        if (idx_Si > 1) {
            option1 = BIT1.query(1, idx_Si - 1) + i;
        }

        int option2 = BIT2.query(idx_Si, idx_Si);

        int option3 = LLONG_MIN;
        if (idx_Si < m) {
            option3 = BIT3.query(idx_Si + 1, m) - i;
        }

        dp_i = max(option1, max(option2, option3));

        BIT1.update(idx_Si, dp_i - i);
        BIT2.update(idx_Si, dp_i);
        BIT3.update(idx_Si, dp_i + i);
    }

    cout << dp_i;
}

```

