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

    - 等待 Actions 完成（大概几分钟（？））
    - 进入 Settings → Pages
    - Source 选择 "Deploy from a branch"（我这边是默认就是 "Deploy from a branch"）
    - Branch 选择 `gh-pages`
    
    设置完后访问 `https://[github_user_name].github.io/[your wiki name]/` 即可。

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

### 编号

markdown 编号（如 `1.`，`2.`）下面有代码块的时候其下面的标号会被重置为 `1.`，以及代码没有高亮等问题，可通过在根目录的 `mkdocs.yml` 添加如下代码解决：
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

### 渲染

渲染删除线：`pymdownx.tilde`

## 进阶操作

### 嵌入 PDF 预览

1）方法 1：

```md
<iframe src="/CS-learning/assets/pdfs/test.pdf" width="100%" height="600px">
</iframe>
```

<iframe src="/CS-learning/assets/pdfs/test.pdf" width="100%" height="600px">
</iframe>

2）方法 2：

```md
[打开 PDF 文档](/CS-learning/assets/pdfs/test.pdf)
```

[打开 PDF 文档](/CS-learning/assets/pdfs/test.pdf)

## 美化

*待更新