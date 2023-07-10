# ASCII码

```
字符  ASCII码   字符  ASCII码   字符   ASCII码   字符   ASCII码
  A       65     a      97      0        48   空格        32
  B       66     b      98      1        49
  …       …
  Z       90     z     122      9        57
```



# 费马定理

```
(a/b)%mod=a*(b)^(mod-2)%mod
```



# 前n项平方和公式

$$
n(n+1)(2n+1)/6
$$

# 快速幂

```
long long int quick_pow(long long x, long long y, long long mod)
{
	long long sum = 1;
	while (y != 0)
	{
		if ((y & 1) == 1)sum = sum * x % mod;
		x = x * x % mod;
		y = y >> 1;
	}
	return sum;
}
```

# 龟速乘

```
long long int quick_mul(long long x, long long y, long long mod)
{
	long long ans = 0;
	while (y != 0)
	{
		if ((y & 1) == 1)ans += x, ans %= mod;
		x += x, x %= mod;
		y = y >> 1;
	}
	return ans;
}
```

# 龟速乘加快速幂

```
long long int quick_pow(long long x, long long y, long long mod)
{
	long long sum = 1;
	while (y != 0)
	{
		if ((y & 1) == 1)sum = quick_mul(sum, x, mod), sum %= mod;
		x = quick_mul(x, x, mod), x %= mod;
		y = y >> 1;
	}
	return sum;
}
```

# 快速乘

```
long long int a,b,mod;
cin>>a>>b>>mod;
cout<<((a*b-(long long )((long double)a*b/mod)*mod+mod)%mod);
```

# 取模公式

```
/加法取模
(a+b)%mod=(a%mod+b%mod)%mod
//减法取模
(a-b)%mod=(a%mod-b%mod+mod)%mod//因为可能小于0
//乘法取模
(a*b)%mod=(a%mod*b%mod)%mod
//除法取模（较为特殊）
（1）当能整除时
(a/b)%mod=(a%(b*mod))/b
（2）当不论是否能整除，但b与mod互质时
(a/b)%mod=a*qp(b,mod-2,mod)%mod
qp(b,mod-2,mod)=b^(mod-2)%mod
```



# 字符串的数字转换成int类型的数字

```
long long int srtToDigint(string num)
{
	long long int ans = 0;
	for (int i = 0; i < num.size(); i++)
	{
		ans = ans * 10 + num[i] - '0';
	}
	return ans;
}
```



# 十进制转任意进制

```
string tenToX(long long num, int radix)
{
	string ans = "";
	do
	{
		int x = num % radix;
		if (x >= 0 && x < 10)
			ans = char('0' + x) + ans;
		else
			ans = char('A' + x - 10) + ans;
		num /= radix;
	} while (num);
	return ans;
}
```



# 任意进制转十进制

```
long long xToTen(string num, int radix)
{
    long long ans=0;
    for(int i=0;i<num.size();i++)
    {
        if(num[i]>='0'&&num[i]<='9')
            ans=ans*radix+num[i]-'0';
        else
            ans=ans*radix+num[i]-'A'+10;
    }
    return ans;
}
```



# 大整数取模

```
typedef long long int ll;
ll BigIntMod(string num, ll mod)
{
	ll ans = 0;
	for (int i = 0; i < num.size(); i++)
	{
		ans = ans * 10 + num[i] - '0';
		ans %= mod;
	}
	return ans;
}
```

# 回文串判断

```
bool isPal(string s)
{
    string ss=s;
    reverse(s.begin(),s.end());
    return ss==s;
}
```



# 快速排序

```
void quick_sort(int a[], int l, int r)
{
    if (l >= r)return;
    int i = l - 1;
    int j = r + 1;
    int x = a[(i + j) / 2];
    while (i < j)
    {
        do i++; while (a[i] < x);
        do j--; while (a[j] > x);
        if (i < j)swap(a[i], a[j]);
    }
    quick_sort(a, l, j);
    quick_sort(a, j + 1, r);
}
```



# 快速查找

```
int quick_search(int a[], int l, int r,int k)
{
    if (l >= r)return a[l];
    int i = l - 1;
    int j = r + 1;
    int x = a[(i + j) / 2];
    while (i < j)
    {
        do i++; while (a[i] < x);
        do j--; while (a[j] > x);
        if (i < j)swap(a[i], a[j]);
    }
    if (k <= j) quick_search(a, l, j, k);
    else quick_search(a, j + 1, r, k);
}
```



# 归并排序

    int temp[N];
    
    void merge_sort(int q[], int l, int r)
    {
    	if (l >= r) return;
    
    	int mid = l + r >> 1;
    
    	merge_sort(q, l, mid), merge_sort(q, mid + 1, r);
    
    	int k = 0, i = l, j = mid + 1;
    	while (i <= mid && j <= r)
    	{
    		if (q[i] <= q[j]) tmp[k++] = q[i++];
    		else tmp[k++] = q[j++];
    	}
    
    	while (i <= mid) tmp[k++] = q[i++];
    	while (j <= r) tmp[k++] = q[j++];
    
    	for (i = l, j = 0; i <= r; i++, j++) q[i] = tmp[j];
    }

# 归并排序计算逆序对

```
int a[N];
int temp[N];
ll merge_sort(int num[], int l, int r)
{
    if (l >= r)
    {
        return 0;
    }
    int mid = (l + r) >> 1;
    ll res = merge_sort(num, l, mid) + merge_sort(num, mid + 1, r);
    int k = 0;
    int i = l;
    int j = mid + 1;
    while (i <= mid && j <= r)
    {
        if (num[i] <= num[j])//！！！逆序对，考虑等于号
        {
            temp[k++] = num[i++];
        }
        else
        {
            res += mid - i + 1;
            //如果当前左半段数大于右半段，则代表存在逆序对，又因为两边数组是顺序排列的，所以当前角标大于i的右半段数组也与这个数构成逆序对，同时移动j
            temp[k++] = num[j++];
        }
    }
    while (i <= mid)
    {
        temp[k++] = num[i++];
    }
    while (j <= r)
    {
        temp[k++] = num[j++];
    }
    for (int i = l, j = 0; i <= r; i++, j++)
    {
        num[i] = temp[j];
    }
    return res;
}
```



# 二分查找

```
while (l < r)
{
	int mid = l + r + 1 >> 1;/*最小值最大化，小于等于目标条件的最大值*/
	// int mid = l + r >> 1;/*最大值最小化，大于等于目标条件的最小值*/
	if (check(mid))//条件
	{
		l = mid;
		// r = mid
	}
	else
	{
		r = mid - 1;
		// l = mid + 1;
	}
	//下拉不加，上拉加
}
```



# 高精度加法

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
vector<int>add(vector<int>& A, vector<int>& B)
{
	if (A.size() < B.size())return add(B, A);
	vector<int>C;
	int t = 0;
	for (int i = 0; i < A.size(); i++)
	{
		t += A[i];
		if (i < B.size())t += B[i];
		C.push_back(t % 10);
		t /= 10;
	}
	if (t)C.push_back(t);
	return C;
}
int main()
{
	string a, b;
	cin >> a >> b;
	vector<int>A, B;
	for (int i = a.size() - 1; i >= 0; i--)
	{
		A.push_back(a[i] - '0');
	}
	for (int i = b.size() - 1; i >= 0; i--)
	{
		B.push_back(b[i] - '0');
	}
	vector<int>C = add(A, B);
	for (int i = C.size() - 1; i >= 0; i--)
	{
		printf("%d", C[i]);
	}
}
```



# 高精度减法

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
bool cmp(vector<int> &A, vector<int> &B)
{
    if (A.size() != B.size()) return A.size() > B.size();

    for (int i = A.size() - 1; i >= 0; i -- )
        if (A[i] != B[i])
            return A[i] > B[i];

    return true;

}

vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ )
    {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;

}

int main()
{
    string a, b;
    vector<int> A, B;
    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i -- ) B.push_back(b[i] - '0');

    vector<int> C;

    if (cmp(A, B)) C = sub(A, B);
    else C = sub(B, A), cout << '-';

    for (int i = C.size() - 1; i >= 0; i -- ) cout << C[i];
    cout << endl;

    return 0;

}
```



# 高精度乘法

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
vector<int>mul(vector<int>& A, int B)
{
    vector<int>C;
    int t = 0;
    for (int i = 0; i < A.size()||t; i++)
    {
        if(i<A.size())t += A[i] * B;
        C.push_back(t % 10);
        t /= 10;
    }
    while (C.size() > 1 && C.back() == 0)C.pop_back();
    return C;
}
int main()
{
    string a;
    int b;
    cin >> a >> b;
    vector<int>A;
    for (int i = a.size() - 1; i >= 0; i--)A.push_back(a[i] - '0');
    vector<int>C=mul(A, b);
    for (int i = C.size() - 1; i >= 0; i--)
    {
        printf("%d", C[i]);
    }
}
```



# 高精度除法

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e6 + 5;
vector<int>div(vector<int>& A, int B,int& t)
{
    vector<int>C;
    t = 0;
    for (int i = A.size() - 1; i >= 0; i--)
    {
        t = t * 10 + A[i];
        C.push_back(t / B);
        t %= B;
    }
    reverse(C.begin(), C.end());
    while (C.size() > 1 && C.back() == 0)C.pop_back();
    return C;
}
int main()
{
    string a;
    int b;
    cin >> a >> b;
    vector<int>A;
    for (int i = a.size() - 1; i >= 0; i--)A.push_back(a[i] - '0');
    int t;
    auto C = div(A, b, t);
    for (int i = C.size() - 1; i >= 0; i--)
    {
        printf("%d", C[i]);
    }
    cout << endl << t;
}
```



# 一维前缀和

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e5 + 5;
int a[N] = {0};
int b[N] = {0};
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)
    {
        scanf("%d", &a[i]);
        b[i] = b[i - 1] + a[i];
    }
    while (m--)
    {
        int l, r;//要计算的区间和
        scanf("%d%d", &l, &r);
        cout << b[r] - b[l - 1] << endl;
    }
}
```



# 二维前缀和

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e3 + 5;
int a[N][N] = {0};//前缀和数组
int main()
{
    int n, m, q;
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            scanf("%d", &a[i][j]);
            a[i][j] += (a[i - 1][j] + a[i][j - 1] - a[i - 1][j - 1]);
        }
    }
    while (q--)
    {
        int x1, y1, x2, y2;
        scanf("%d%d%d%d", &x1, &y1, &x2, &y2);//待求的二维数组的范围
        cout << a[x2][y2] - a[x1 - 1][y2] - a[x2][y1 - 1] + a[x1 - 1][y1 - 1] << endl;
    }
}
```



# 一维差分

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e6 + 5;
int a[N] = { 0 };//原数组（把原数组当作一个前缀和数组）
int b[N] = { 0 };//差分数组
void insert(int l, int r, int c)
{
    b[l] += c;
    b[r + 1] -= c;
}
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)
    {
        scanf("%d", &a[i]);
        insert(i, i, a[i]);
    }
    while (m--)
    {
        int l, r, c;//待差分区间，以及要加的数
        scanf("%d%d%d", &l, &r, &c);
        insert(l, r, c);
    }
    for (int i = 1; i <= n; i++)
    {
        b[i] += b[i - 1];
        printf("%d ", b[i]);
    }
}
```



# 二维差分

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e3 + 5;
int a[N][N] = { 0 };//原数组
int b[N][N] = { 0 };//差分数组
void insert(int x1, int y1, int x2, int y2, int c)
{
    b[x1][y1] += c;
    b[x2 + 1][y1] -= c;
    b[x1][y2 + 1] -= c;
    b[x2 + 1][y2 + 1] += c;
}
int main()
{
    int n, m, q;
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            scanf("%d", &a[i][j]);
            insert(i, j, i, j, a[i][j]);
        }
    }
    while (q--)
    {
        int x1, y1, x2, y2, c;//待差分的区间以及要加的数
        scanf("%d%d%d%d%d", &x1, &y1, &x2, &y2, &c);
        insert(x1, y1, x2, y2, c);
    }
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            b[i][j] += b[i - 1][j] + b[i][j - 1] - b[i - 1][j - 1];
            printf("%d ", b[i][j]);
        }
        printf("\n");
    }
}
```



# 双指针

```
//最长连续不重复子序列

#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e5 + 5;
int a[N] = { 0 };
int cnt[N] = { 0 };
int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
    }
    int ans = 0;
    for (int i = 0, j = 0; i < n; i++)
    {
        cnt[a[i]]++;
        while (j < i && cnt[a[i]]>1)cnt[a[j++]]--;
        ans = max(ans, j - i + 1);
    }
    printf("%d", ans);
}



//数组元素的目标和

#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e5 + 5;
int a[N] = { 0 };
int b[N] = { 0 };
int main()
{
    int n, m, x;
    scanf("%d%d%d", &n, &m, &x);
    map<int, int>cnt;
    for (int i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
        cnt[a[i]]++;
    }
    int ans1;
    int ans2;
    for (int i = 0; i < m; i++)
    {
        scanf("%d", &b[i]);
        if (cnt[x - b[i]]) 
        {
            ans2 = i;
            for (int j = 0; j < n; j++)
            {
                if (a[j] == x - b[i])
                {
                    ans1 = j;
                }
            }
        }
    }
    cout << ans1 << " " << ans2;
}

//判断子序列（判断a是否为b的非连续子序列）
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e5 + 5;
int a[N] = { 0 };
int b[N] = { 0 };
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
    }
    for (int i = 0; i < m; i++)
    {
        scanf("%d", &b[i]);
    }
    int i = 0;
    int j = 0;
    while (i < n && j < m)
    {
        if (a[i] == b[j])i++;
        j++;
    }
    if (i == n)cout << "Yes";
    else cout << "No";
}
```



# 离散化

```
//区间和
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 3e5 + 6;
vector<pair<int, int>>alldata,section;
vector<int>vec;
int a[N];
int s[N];
int binary_search(int x)
{
    int l = 0;
    int r = vec.size() - 1;
    while (l < r)
    {
        int mid = (l + r) >> 1;
        if (vec[mid] >= x)
        {
            r = mid;
        }
        else
        {
            l = mid + 1;
        }
    }
    return l + 1;
}
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    while (n--)
    {
        int x, c;
        scanf("%d%d", &x, &c);
        alldata.push_back({ x,c });
        vec.push_back(x);
    }
    while (m--)
    {
        int l, r;
        scanf("%d%d", &l, &r);
        section.push_back({ l,r });
        vec.push_back(l);
        vec.push_back(r);
    }
    sort(vec.begin(), vec.end());
    vec.erase(unique(vec.begin(),vec.end()), vec.end());
    for (auto it : alldata)
    {
        int x = binary_search(it.first);
        a[x] += it.second;
    }
    for (int i = 1; i <= vec.size(); i++)
    {
        s[i] = s[i - 1] + a[i];
    }
    for (auto it : section)
    {
        int l = binary_search(it.first);
        int r = binary_search(it.second);
        printf("%d\n", s[r] - s[l - 1]);
    }
}
```



# 区间合并

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 3e5 + 6;
vector<pair<int, int>>section;
vector<pair<int, int>>ans;
int main()
{
    int n;
    scanf("%d", &n);
    while (n--)
    {
        int l, r;
        scanf("%d%d", &l, &r);
        section.push_back({ l,r });
    }
    sort(section.begin(), section.end());
    int st = section[0].first;
    int end = section[0].second;
    for (int i = 1; i < section.size(); i++)
    {
        if (section[i].first > end)
        {
            ans.push_back({ st,end });
            st = section[i].first;
            end = section[i].second;
        }
        else
        {
            end = max(section[i].second, end);
        }
    }
    ans.push_back({ st,end });
    printf("%d", ans.size());
}
```



# 单链表

```
const int N = 2e5 + 5;
int head, val[N], ne[N], idx;
void init()
{
    head = -1;
    idx = 0;
}
void add_head(int num)
{
    val[idx] = num;
    ne[idx] = head;
    head = idx++;
}
void add(int k, int num)//加入到第k个数的后面
{
    val[idx] = num;
    ne[idx] = ne[k - 1];
    ne[k - 1] = idx++;
}
void remove(int k)//删除第k个插入数后面的那个数
{
    ne[k - 1] = ne[ne[k - 1]];
}
void display()
{
	for (int i = head; i != -1; i = ne[i])
	{
		cout << val[i] << " ";
	}
}
```



# 双链表

```
const int N;
int head, val[N], l[N], r[N], idx;
void init()
{
    r[0] = 1;
    l[1] = 0;
    idx = 2;
}
void add(int k, int num)
{
    val[idx] = num;
    r[idx] = r[k];
    l[idx] = k;
    l[r[k]] = idx;
    r[k] = idx++;
}
void remove(int k)
{
    l[r[k]] = l[k];
    r[l[k]] = r[k];
}
void display()
{
	for (int i = r[0]; i != 1; i = r[i])
	{
		cout << val[i] << " ";
	}
}
```



# 模拟栈

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e5 + 5;
int stk[N];
int idx;
int main()
{
    idx = 0;
    int m;
    cin >> m;
    while (m--)
    {
        string op;
        cin >> op;
        if (op == "push")
        {
            int x;
            cin >> x;
            stk[++idx] = x;
        }
        else if (op == "pop")
        {
            idx--;
        }
        else if (op == "empty")
        {
            if (idx == 0)
            {
                cout << "YES\n";
            }
            else
            {
                cout << "NO\n";
            }
        }
        else if (op == "query")
        {
            cout << stk[idx] << endl;
        }
    }
}
```



# 表达式求值

```
#include<unordered_map>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
stack<int>num;
stack<char>op;
void eval()
{
    int b = num.top(); num.pop();
    int a = num.top(); num.pop();
    char c = op.top(); op.pop();
    if (c == '+')
    {
        num.push(a + b);
    }
    else if (c == '-')
    {
        num.push(a - b);
    }
    else if (c == '*')
    {
        num.push(a * b);
    }
    else if (c == '/')
    {
        num.push(a / b);
    }
}
int main()
{
    unordered_map<char, int>pr{ {'+', 1}, {'-', 1}, {'*', 2}, {'/', 2} };
    string s;
    cin >> s;
    for (int i = 0; i < s.size(); i++)
    {
        if (isdigit(s[i]))
        {
            int x = 0;
            int j = i;
            while (j < s.size() && isdigit(s[j]))
            {
                x = x * 10 + s[j++] - '0';
            }
            i = j - 1;
            num.push(x);
        }
        else if (s[i] == '(')
        {
            op.push(s[i]);
        }
        else if (s[i] == ')')
        {
            while (op.top() != '(')eval();
            op.pop();
        }
        else
        {
            while (op.size() && op.top() != '(' && pr[op.top()] >= pr[s[i]])eval();
            op.push(s[i]);
        }
    }
    while (op.size())eval();
    cout << num.top();
}
```



# 模拟队列

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e5 + 6;
int q[N];
int hh = 0;
int tt = -1;
int main()
{
    int m;
    cin >> m;
    while (m--)
    {
        string op;
        cin >> op;
        if (op == "push")
        {
            int x;
            cin >> x;
            q[++tt] = x;
        }
        else if (op == "empty")
        {
            if (tt < hh)
            {
                cout << "YES\n";
            }
            else
            {
                cout << "NO\n";
            }
        }
        else if (op == "query")
        {
            cout << q[hh] << endl;
        }
        else if (op == "pop")
        {
            hh++;
        }
    }
}
```



# 单调栈

```
//给定一个长度为 N 的整数数列，输出每个数左边第一个比它小的数，如果不存在则输出 −1。
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e5 + 6;
int idx = 0;
int stk[N];
int main()
{
    int n;
    scanf("%d", &n);
    int x;
    while (n--)
    {
        scanf("%d", &x);
        while(idx && stk[idx] >= x)idx--;
        if (idx)cout << stk[idx] << " ";
        else cout << "-1 ";
        stk[++idx] = x;
    }
}
```



# 滑动窗口

```
//输出滑动窗口中元素的最大值和最小值
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 1e6 + 6;
int a[N], q[N];
int main()
{
    int n, k;
    scanf("%d%d", &n, &k);

    for (int i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
    }

    int hh = 0;
    int tt = -1;
    for (int i = 0; i < n; i++)
    {
        if (hh <= tt && q[hh] < i - k + 1)hh++;
        while (hh <= tt && a[q[tt]] >= a[i])tt--;//最小值
        q[++tt] = i;
        if (i >= k - 1)printf("%d ", a[q[hh]]);
    }

    printf("\n");

    hh = 0;
    tt = -1;
    for (int i = 0; i < n; i++)
    {
        if (hh <= tt && q[hh] < i - k + 1)hh++;
        while (hh <= tt && a[q[tt]] <= a[i])tt--;//最大值
        q[++tt] = i;
        if (i >= k - 1)printf("%d ", a[q[hh]]);
    }
}
```



# KMP

```
//首先是构造next数组
//双指针i,j,对于待匹配字串，i先跑寻找后缀，j后跑寻找前缀，如果当前j指向的字符的下一位可以与后缀匹配上，就用next数组记录下当前j的值
如果不行，则回退到上次匹配成功的地方，然后重复改操作步骤
为什么kmp可以极大程度上优化时间，我们举一个例子abcdfabc,此时我们匹配到c发现不行，如果暴力做法我们要直接回退到这个字符串的起始位置，但是我们发现了一个事情，s(0-2)与s(5-7)相等，5,6可以匹配上，则证明，0 1也可以匹配上，那我们只需要从下标2开始重新匹配即可，对于i也是同理，如果j不需要回溯到最开始位置，i也不用回溯，i,j回溯同步
关于j=next[j],如果当前前后缀匹配失败，我们需要做什么？我们需要拿着当前这段前缀里面有没有跟当前后缀匹配上的部分，顺着这一部分继续衍生寻找新的前缀。相当于对当前所出现的前缀进行一个检索，在这之前的部分里面有没有可以衍生的相等前后缀

#include<bits/stdc++.h>

using namespace std;

const int N = 1000010;

int n, m;

int ne[N];
int main()
{
    string p, s;
    cin >> m >> p >> n >> s;

    ne[0] = -1;
    for (int i = 1, j = -1; i < p.size(); i++)
    {
        while (j != -1 && p[j + 1] != p[i]) j = ne[j];
        if (p[j + 1] == p[i]) j++;
        ne[i] = j;
    }

    for (int i = 0, j = -1; i < s.size(); i++)
    {
        while (j != -1 && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j++;
        if (j == p.size()-1)
        {
            cout << i - j << ' ';
            j = ne[j];
        }
    }

    return 0;
}
```



# Trie（字典树）

```
//高校的存储和查找字符串集合的数据结构
//.变量id： id代表字典树中每一个节点的编号，id的大小只与插入字典树的先后顺序有关，它的作用在下面会讲到。

//2.trie[N][26]： 每个trie代表一条边，字典树其中1~N为边上方节点的编号，0代表root节点，1~26为连在i节点下方的26个字母。如果trie[i][x]=0,则代表字典树中目前没有这个点，而trie[i][x]的值代表这个点下方连有的点的编号，例如：trie[i][2]=9代表第i号点和的下方连有一个点‘c’，并且那个点的编号是9，为什么是c呢？因为 ‘c’-‘a’=2

//3.cnt[N]: cnt[i]==0代表编号为i的点不是一个单词的结束点，在上面的图中代表这个点不是空点，但是没有标红，cnt[i]！=0代表编号为i的点是一个单词的结束点，即红点。cnt[i]不一定只为0或1，因为有可能多次输入了同一个单词。

#include<bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;
 
int trie[N][26],cnt[N],idx;//字典树数组，记录单词出现次数，结点标号

void insert(string str)
{
    int p = 0;
    for (int i = 0; i < str.size(); i++)
    {
        int u = str[i] - 'a';
        if (!trie[p][u])trie[p][u] = ++idx;
        p = trie[p][u];
    }
    cnt[p]++;
}

int query(string str)
{
    int p = 0;
    for (int i = 0; i < str.size(); i++)
    {
        int u = str[i] - 'a';
        if (!trie[p][u]) return 0;
        p = trie[p][u];
    }
    return cnt[p];
}

int main()
{
    int n;
    scanf("%d", &n);
    while (n--)
    {
        string op, s;
        cin >> op >> s;
        if (op == "I")insert(s);
        else printf("%d\n", query(s));
    }
    return 0;
}
```



# 最大异或对（字典树）

```
#include<bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;
const int M = 3e6 + 10;

int a[N];
int trie[M][2],idx;//字典树数组，记录单词出现次数，结点标号

void insert(int num)
{
    int p = 0;
    for (int i = 30; i >= 0; i--)
    {
        if (!trie[p][num >> i & 1])trie[p][num >> i & 1] = ++idx;
        p = trie[p][num >> i & 1];
    }
}

int query(int num)
{
    int p = 0, res = 0;
    for (int i = 30; i >= 0; i--)
    {
        if (trie[p][!(num >> i & 1)])
        {
            res += 1 << i;
            p = trie[p][!(num >> i & 1)];
        }
        else
        {
            p = trie[p][num >> i & 1];
        }
    }
    return res;
}

int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
        insert(a[i]);
    }
    int ans = 0;
    for (int i = 0; i < n; i++)
    {
        ans = max(ans, query(a[i]));
    }
    printf("%d", ans);
    return 0;
}
```



# 并查集

```
const int N = 100010;

int fa[N];
//cnt[N];计数
void init()
{
    for (int i = 1; i <= n; i++)
    {
        fa[i] = i;
        //cnt[i] = 1;
    }
}

int find(int x)
{
    if (fa[x] != x) fa[x] = find(fa[x]);
    return fa[x];
}

void unin(int a,int b)
{
    fa[find(a)] = find(b);
    /*if (find(a) != find(b))
    {
        fa[find(a)] = find(b);
        cnt[find(b)] += cnt[find(a)];
    }*/
}
```



# 并查集（应用）

```
#include<bits/stdc++.h>//食物环问题
using namespace std;
typedef unsigned long long ll;
const int N = 5e4 + 6;
int fa[N];
int dis[N];//记录距离通过距离进行判断
int n, k;
void init()
{
    for (int i = 1; i <= n; i++)
    {
        fa[i] = i;
    }
}
int find(int x)
{
    if (fa[x] != x) 
    {
        int t = find(fa[x]);
        dis[x] += dis[fa[x]];
        fa[x] = t;
    }
    return fa[x];
}
void unin(int a,int b)
{
    fa[find(a)] = find(b);
}
int main()
{
    //0同类，1吃，2被吃
    scanf("%d%d", &n, &k);
    init();
    int ans = 0;
    while (k--)
    {
        int d, x, y;
        scanf("%d%d%d", &d, &x, &y);
        if (x > n || y > n)
        {
            ans++;
        }
        else
        {
            int px = find(x), py = find(y);
            if (d == 1)
            {
                if (px == py && (dis[x] - dis[y]) % 3)ans++;
                else if (px != py)
                {
                    unin(x, y);
                    dis[px] = dis[y] - dis[x];
                }
            }
            else
            {
                if (px == py&& (dis[x] - dis[y] - 1) % 3)ans++;
                else if (px != py)
                {
                    unin(x, y);
                    dis[px] = dis[y] - dis[x] + 1;
                }
            }
        }
    }
    printf("%d\n", ans);
    return 0;
}
```



# 堆排序

```
堆：一个完全二叉树，父节点小于等于两个根节点//下标从1开始 
设当前结点的标号为x，则该节点的右儿子为2*x+1，左儿子为2*x
down操作：数据的下沉操作
up操作：数据的上浮操作
关于堆的一些操作，插入，求最小值，删除
插入：insert(),heap[++size]=x; up(size);先将数据插入到最后一个结点，然后进行上浮操作
最小值：输出头结点的值即可
删除最小值，将最后一个结点的值覆盖给头节点，然后删除尾结点，在进行下沉操作，heap[1]=heap[size];size--;down(size)；
删除任意元素：同上，heap[k]=heap[size];size--;down(k);up(k);
修改任意一个元素：
//堆排序代码
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ll;
const int N = 1e5 + 10;
int n, m;
int heap[N], hsize;
void down(int u)
{
    int t = u;
    if (u * 2 <= hsize && heap[u * 2] < heap[t])t = u * 2;
    if (u * 2 + 1 <= hsize && heap[u * 2 + 1] < heap[t])t = u * 2 + 1;
    if (u != t)
    {
        swap(heap[u], heap[t]);
        down(t);
    }
}
void hdelete(int u)
{
    heap[u] = heap[hsize];
    hsize--;
    down(u);
}
int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)scanf("%d", &heap[i]);

    hsize = n;

    for (int i = n / 2; i; i--)
    {
        down(i);
    }

    while (m--)
    {
        printf("%d ", heap[1]);
        hdelete(1);
    }
    return 0;
}

```



# 模拟堆

```
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ll;
const int N = 1e5 + 10;
int n, hsize, cnt;
int heap[N], hp[N], ph[N];//堆，堆中第k个点是第几个插入点，第k个数在堆中的下标
//代码中的第k个数均指插入顺序下的第k个数
void heap_swap(int a, int b)
{
    swap(ph[hp[a]], ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(heap[a], heap[b]);
}
void up(int u)
{
    while (u / 2 && heap[u] < heap[u / 2])
    {
        heap_swap(u, u / 2);
        u >>= 1;
    }
}
void down(int u)
{
    int t = u;
    if (u * 2 <= hsize && heap[u * 2] < heap[t])t = u * 2;
    if (u * 2 + 1 <= hsize && heap[u * 2 + 1] < heap[t])t = u * 2 + 1;
    if (u != t)
    {
        heap_swap(u, t);
        down(t);
    }
}
void hdelete(int u)
{
    heap_swap(u, hsize);
    hsize--;
    up(u);
    down(u);
}
int main()
{
    scanf("%d", &n);
    while (n--)
    {
        string op;
        cin >> op;
        if (op == "I")//插入一个数
        {
            int num;
            cin >> num;
            hsize++;
            cnt++;
            ph[cnt] = hsize;
            hp[hsize] = cnt;
            heap[hsize] = num;
            up(hsize);
        }
        else if (op == "PM")//输出最小值
        {
            cout << heap[1] << endl;
        }
        else if (op == "DM")//删除最小值
        {
            hdelete(1);
        }
        else if (op == "D")//删除第k个数
        {
            int k;
            scanf("%d", &k);
            k = ph[k];
            hdelete(k);
        }
        else//修改第k个数
        {
            int k, x;
            scanf("%d%d", &k, &x);
            k = ph[k];
            heap[k] = x;
            up(k);
            down(k);
        }
    }
    return 0;
}
```



# 模拟Hash表

```
#include<bits/stdc++.h>//基于链表实现的散列表，核心在于每个Hash槽位对应一个链表，代表该对应值下的所有数
using namespace std;
typedef unsigned long long ll;
const int N = 1e5 + 3;
int n;
int value[N], ne[N], qhash[N], idx;

void insert(int x)
{
    int k = (x % N + N) % N;//Hash函数
    value[idx] = x;//链表实现
    ne[idx] = qhash[k];
    qhash[k] = idx++;
}

bool find(int x)
{
    int k = (x % N + N) % N;
    for (int i = qhash[k]; i!=- 1; i = ne[i])
    {
        if (value[i] == x)
        {
            return true;
        }
    }
    return false;
}
int main()
{
    scanf("%d", &n);
    memset(qhash, -1, sizeof(qhash));//链表头赋值为1
    while (n--)
    {
        string op;
        cin >> op;
        int x;
        scanf("%d", &x);
        if (op == "I")
        {
            insert(x);
        }
        else
        {
            if (find(x))
            {
                printf("Yes\n");
            }
            else
            {
                printf("No\n");
            }
        }
    }
    return 0;
}
```



# 字符串hash

```
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ull;

const int N = 1e5 + 10;
const int P = 131;//将字符串的值转化为p进制的数
int n, m;
ull qhash[N], p[N];
void init(string s)
{
    p[0] = 1;
    for (int i = 1; i < s.size(); i++)
    {
        qhash[i] = qhash[i - 1] * P + s[i];//进制转换
        p[i] = p[i - 1] * P;//记录p的多少次方
    }
}

ull get(int l, int r)
{
    return qhash[r] - qhash[l - 1] * p[r - l + 1];//返回hash值，后半段要进行进制对齐
}

int main()
{
    scanf("%d%d", &n, &m);
    string s;
    cin >> s;

    s = " " + s;

    
    init(s);


    while (m--)
    {
        int l1, r1, l2, r2;
        scanf("%d%d%d%d", &l1, &r1, &l2, &r2);
        if (get(l1, r1) == get(l2, r2))printf("Yes\n");
        else printf("No\n");
    }
    return 0;
}
```



# 全排列（STL函数实现）

```
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ull;

const int N = 7;
int num[N];
int n;
int main()
{
	scanf("%d", &n);
	for (int i = 0; i < n; i++)
	{
		num[i] = i + 1;
	}
	do
	{
		for (int i = 0; i < n; i++)printf("%d ", num[i]);
		printf("\n");
	}while(next_permutation(num, num + n));//对目标数组中的前n个数进行全排列，需注意排列前，数组必须进行升序排序
}
```



# 全排列（DFS）

```
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ull;

const int N = 7;
int path[N];//记录当前选择
bool flag[N + 1];//回溯用，标记
int n;
void dfs(int step)
{
	if (step == n)
	{
		for (int i = 0; i < n; i++)printf("%d ", path[i]);
		printf("\n");
	}
	for (int i = 1; i <= n; i++)
	{
		if (!flag[i])
		{
			flag[i] = 1;
			path[step] = i;
			dfs(step + 1);
			flag[i] = false;
		}
	}
}
int main()
{
	scanf("%d", &n);
	dfs(0);
}

```



# DFS(n-皇后问题优化做法)

```
//n个皇后放在n×n的国际象棋棋盘上，使得皇后不能相互攻击到，即任意两个皇后都不能处于同一行、同一列或同一斜线上。
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ull;

const int N = 12;

int n;

bool col[N], dg[N], udg[N];

char qmap[N][N];
void dfs(int row)//搜索行数
{
	if (row == n)
	{
		for (int i = 0; i < n; i++)printf("%s\n", qmap[i]);
		puts("");
		return;
	}
	for (int i = 0; i < n; i++)//遍历列数
	{
		if (!col[i] && !dg[row + i] && !udg[n - row + i])
		{
			qmap[row][i] = 'Q';
			col[i] = dg[row + i] = udg[n - row + i] = true;
			dfs(row + 1);
			col[i] = dg[row + i] = udg[n - row + i] = false;
			qmap[row][i] = '.';
		}
	}
}

int main()
{
	scanf("%d", &n);
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			qmap[i][j] = '.';
		}
	}
	dfs(0);
}
```



# DFS（n-皇后问题暴力搜索做法）

```
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ull;
//x行y列

const int N = 12;

int n;

bool row[N],col[N], dg[N], udg[N];

char qmap[N][N];
void dfs(int x,int y,int step)
{
	if (y == n)y = 0, x++;
	if (x == n)
	{
		if (step == n)
		{
			for (int i = 0; i < n; i++)
			{
				printf("%s\n", qmap[i]);
			}
			puts("");
		}
		return;
	}
	dfs(x, y + 1, step);//不放皇后

	if (!row[x] && !col[y] && !dg[x + y] && !udg[x - y + n])//放皇后
	{
		qmap[x][y] = 'Q';
		row[x] = col[y] = dg[x + y] = udg[x - y + n] = true;
		dfs(x, y + 1, step + 1);
		row[x] = col[y] = dg[x + y] = udg[x - y + n] = false;
		qmap[x][y] = '.';
	}
}

int main()
{
	scanf("%d", &n);
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			qmap[i][j] = '.';
		}
	}
	dfs(0, 0, 0);
}
```



# BFS(模拟队列实现)

```
#include<bits/stdc++.h>//权重为1时，可以求得最短路径

using namespace std;

typedef unsigned long long ull;
typedef pair<int, int>PII;

const int N = 105;
int n, m;

int qmap[N][N];//存图
int d[N][N];//存的是每一个点到起点的距离
int dir[4][2] = { 1,0,-1,0,0,1,0,-1 };//向量函数
PII q[N * N];//模拟队列数组


int BFS()
{
	int hh = 0, tt = -1;//头指针，尾指针
	q[++tt] = { 0,0 };//放入起点
	
	memset(d, -1, sizeof(d));//全部赋值为-1，所有-1的点代表该点没有被搜索到

	d[0][0] = 0;//起点距离更新为0

	while (hh <= tt)//判断队列是否为空
	{
		auto t = q[hh++];//取出队首元素
		for (int i = 0; i < 4; i++)
		{
			int x = t.first + dir[i][0];
			int y = t.second + dir[i][1];
			if (x >= 0 && y >= 0 && x < n && y < m && !qmap[x][y] && d[x][y] == -1)
			{
				d[x][y] = d[t.first][t.second] + 1;//距离+1
				q[++tt] = { x,y };//将新搜索到的点放入队列
			}
		}
	}

	return d[n - 1][m - 1];
}

int main()
{
	scanf("%d%d", &n, &m);
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < m; j++)
		{
			scanf("%d", &qmap[i][j]);
		}
	}
	
	printf("%d\n", BFS());
	
}
```



# BFS(STL队列实现)

```
#include<bits/stdc++.h>

using namespace std;

typedef unsigned long long ull;
typedef pair<int, int>PII;

const int N = 105;
int n, m;

int qmap[N][N];//存图
int d[N][N];//存的是每一个点到起点的距离
int dir[4][2] = { 1,0,-1,0,0,1,0,-1 };
queue<PII>q;


int BFS()
{
	q.push({ 0,0 });
	
	memset(d, -1, sizeof(d));

	d[0][0] = 0;

	while (q.size())
	{
		auto t = q.front();
		q.pop();
		for (int i = 0; i < 4; i++)
		{
			int x = t.first + dir[i][0];
			int y = t.second + dir[i][1];
			if (x >= 0 && y >= 0 && x < n && y < m && !qmap[x][y] && d[x][y] == -1)
			{
				d[x][y] = d[t.first][t.second] + 1;
				q.push({ x,y });
			}
		}
	}

	return d[n - 1][m - 1];
}

int main()
{
	scanf("%d%d", &n, &m);
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < m; j++)
		{
			scanf("%d", &qmap[i][j]);
		}
	}
	
	printf("%d\n", BFS());
	
}
```



# BFS(八数图问题)

```
#include<bits/stdc++.h>
#include<unordered_map>
//核心思路是搜索每一种图的状态
using namespace std;

int dir[4][2] = { 1,0,-1,0,0,1,0,-1 };//方向数组



int BFS(string s)
{
	queue<string>q;
	unordered_map<string, int>dis;//记录距离

	q.push(s);
	dis[s] = 0;


	while (q.size())
	{
		auto t = q.front();
		q.pop();

		string end = "12345678x";

		if (t == end)return dis[t];
		int distance = dis[t];

		int k = t.find('x');

		int x = k / 3;//提取坐标值
		int y = k % 3;//提取坐标值

		for (int i = 0; i < 4; i++)
		{
			int tx = x + dir[i][0];
			int ty = y + dir[i][1];
			if (tx >= 0 && tx < 3 && ty >= 0 && ty < 3)
			{
				swap(t[k], t[tx * 3 + ty]);//坐标值转换回去
				if (!dis.count(t))
				{
					dis[t] = distance + 1;//距离刷新
					q.push(t);//记录状态
				}
				swap(t[k], t[tx * 3 + ty]);//状态恢复
			}
		}
	}
	return -1;
}

int main()
{
	string s;
	char c;
	for (int i = 0; i < 9; i++)
	{
		cin >> c;
		s = s + c;
	}
	cout << BFS(s) << endl;
}
```



# 树与图

```
树是无环连通图
图分有向图和无向图
有向图的邻接矩阵 qmap[a][b]存储a到b的边的权重
有向图的邻接表（链表）也可以用vector实现
重心定义：重心是指树中的一个结点，如果将这个点删除后，剩余各个连通块中点数的最大值最小，那么这个节点被称为树的重心。
```



# 树与图的DFS(树的中心寻找)

```
#include<bits/stdc++.h>
//找到树的重心，并输出删除重心后，剩下的连通区块中最大的结点数
using namespace std;
const int N = 1e5 + 10, M = N * 2;

int h[N], val[M], ne[M], idx;
bool vis[N];
int n;
int ans = N;

void add(int a, int b)
{
	val[idx] = b;
	ne[idx] = h[a];
	h[a] = idx++;
}

int DFS(int start)
{
	vis[start] = true;
	int sum = 1;
	int res = 0;
	for (int i = h[start]; i != -1; i = ne[i])
	{
		if (!vis[val[i]])
		{
			int s = DFS(val[i]);
			res = max(s,res);//记录当前结点向下的连通区块中的最大连通区块
			sum += s;//记录包括当前节点在内所有向下的连通区块的结点总数
		}
	}
	res = max(res, n - sum);//判断当前结点向上的连通区块的节点数是否为最大值
	ans = min(ans, res);//所有最大值里挑一个最小值
	return sum;//返回包括当前结点下面的所有连通区块的结点总数
}
int main()
{
	memset(h, -1, sizeof(h));
	scanf("%d", &n);
	for (int i = 1; i < n; i++)
	{
		int a, b;
		scanf("%d%d", &a, &b);
		add(a, b), add(b, a);
	}
	DFS(1);
	cout << ans << endl;
}
```



# 树与图的BFS（点的层次问题）

```
#include<bits/stdc++.h>
//求从1结点到n结点的最短距离

using namespace std;
const int N = 1e5 + 10;

int h[N], val[N], ne[N], idx;

int d[N], q[N];//记录距离的数组，模拟队列数组

int n, m;

void add(int a, int b)
{
	val[idx] = b;
	ne[idx] = h[a];
	h[a] = idx++;
}

int BFS()
{
	int hh = 0, tt = -1;
	q[++tt] = 1;
	memset(d, -1, sizeof(d));
    d[1] = 0;
	while (hh <= tt)
	{
		int t = q[hh++];
		for (int i = h[t]; i != -1; i = ne[i])
		{
			if (d[val[i]] == -1)
			{
				d[val[i]] = d[t] + 1;
				q[++tt] = val[i];
			}
		}
	}

	return d[n];
}
int main()
{
	scanf("%d%d", &n, &m);
	memset(h, -1, sizeof(h));
	for (int i = 0; i < m; i++)
	{
		int a, b;
		scanf("%d%d", &a, &b);
		add(a, b);
	}
	
	cout << BFS() << endl;
}
```

