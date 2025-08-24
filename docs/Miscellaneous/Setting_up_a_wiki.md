# 搭建 wiki

## 作用

构建一个自己的小知识库，就比如 [我的 CS learning](https://tenshi0x0.github.io/CS-learning/)。

## 搭建流程

我是参考 [mkdocs-material 的教程](https://squidfunk.github.io/mkdocs-material/getting-started/)，在我个人 github 博客上搭建的，下文将以我自己搭建流程讲解。

1. 首先在 github 上新建一个 repo，然后 `clone` 到本地，目录假设为 `D:\MyBlog\CS-learning`。

2. `cd` 进 `D:\MyBlog\CS-learning`，执行 `mkdocs new .`。

3. `pip install mkdocs-material`。

4. 修改 `mkdocs.yml` 为

    ```yaml
    site_name: CS Learning
    site_url: https://[github_user_name].github.io/[your wiki name]/
    theme:
        name: material
    ```

5. 创建 GitHub Actions 文件，也就是 `CS-learning` 目录下创建 `.github/workflows/ci.yml`，内容为：

    ```yaml
    name: ci 
    on:
    push:
        branches:
        - master 
        - main
    permissions:
    contents: write
    jobs:
    deploy:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v4
        - name: Configure Git Credentials
            run: |
            git config user.name github-actions[bot]
            git config user.email 41898282+github-actions[bot]@users.noreply.github.com
        - uses: actions/setup-python@v5
            with:
            python-version: 3.x
        - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV
        - uses: actions/cache@v4
            with:
            key: mkdocs-material-${{ env.cache_id }}
            path: .cache
            restore-keys: |
                mkdocs-material-
        - run: pip install mkdocs-material 
        - run: mkdocs gh-deploy --force
    ```

6. 然后推送（执行的时候如果你没登录 github 会终端提醒你登录）：

    ```sh
    git add .
    git commit -m "Initial commit"
    git push -u origin main
    ```

7. 最后设置 GitHub Pages：

    - 等待 Actions 完成（可能要一分钟）
    - 进入 Settings → Pages
    - Source 选择 "Deploy from a branch"（我这边是默认就是 "Deploy from a branch"）
    - Branch 选择 `gh-pages`
    
    设置完后访问 `https://[github_user_name].github.io/[your wiki name]/` 即可。

```cpp
// Problem: D. Chicken Jockey
// Contest: Codeforces - Codeforces Round 1044 (Div. 2)
// URL: https://codeforces.com/contest/2133/problem/D
// Memory Limit: 256 MB
// Time Limit: 2000 ms
// 
// Powered by CP Editor (https://cpeditor.org)

#include<bits/stdc++.h>
using namespace std;
 
#define debug(x) cerr << #x << ": " << (x) << endl
#define rep(i,a,b) for(int i=(a);i<=(b);i++)
#define dwn(i,a,b) for(int i=(a);i>=(b);i--)
#define SZ(a) ((int) (a).size())
#define pb push_back
#define all(x) (x).begin(), (x).end()
 
#define x first
#define y second
#define int long long
using pii = pair<int, int>;
using ll = long long;
 
inline void read(int &x){
    int s=0; x=1;
    char ch=getchar();
    while(ch<'0' || ch>'9') {if(ch=='-')x=-1;ch=getchar();}
    while(ch>='0' && ch<='9') s=(s<<3)+(s<<1)+ch-'0',ch=getchar();
    x*=s;
}

const int N=2e5+5;

int w[N];

void solve(){
	int n; cin>>n;
	int sum=0;
	rep(i, 1, n) read(w[i]), sum+=w[i];
	vector<int> f(n+1);
	f[n]=n;
	int last=0;
	dwn(i, n-1, 1){
		int val=f[i+1]-2+min(i, w[i+1]);
		f[i]=max(f[i+1], val);
		last=val;
	}
	
	int dt=(last);
	// debug(sum);
	cout<<sum-dt<<"\n";
}

signed main(){
	int cs; cin>>cs;
	while(cs--) solve();
	return 0;
}
```

## 后续维护

```sh
# 本地预览
mkdocs serve

# 提交更改
git add .
git commit -m "xxx"
git push

# 当然用一行提交更方便：
git add .; git commit -m "xxx"; git push
```

## 可能遇到的问题

- markdown 标号（如 `1.`，`2.`）下面有代码块的时候其下面的标号会被重置为 `1.`，以及代码没有高亮等问题，可通过在根目录的 `mkdocs.yml` 添加如下代码解决：
    ```yaml
    markdown_extensions:
        - pymdownx.highlight:
            anchor_linenums: true
            line_spans: __span
            pygments_lang_class: true
        - pymdownx.inlinehilite
        - pymdownx.snippets
        - pymdownx.superfences
    ```


## 美化

*待更新