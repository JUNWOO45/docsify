# 페이지 추가

페이지를 추가하고 싶다면, docsify 디렉터리 아래에 마크다운 파일만 생성하면 됩니다. 파일 이름을 `guide.md` 로 생성한다면, `/#/guide` 로 접근할 수 있습니다.

만약 디렉터리 구조가 다음과 같다면:

```text
.
└── docs
    ├── README.md
    ├── guide.md
    └── zh-cn
        ├── README.md
        └── guide.md
```

다음과같이 연결됩니다

```text
docs/README.md        => http://domain.com
docs/guide.md         => http://domain.com/guide
docs/zh-cn/README.md  => http://domain.com/zh-cn/
docs/zh-cn/guide.md   => http://domain.com/zh-cn/guide
```

## 사이드바

사이드바를 만들기위해서는 `_sidebar.md` 를 만들어야합니다. ([사이드바 문서](https://github.com/docsifyjs/docsify/blob/master/docs/_sidebar.md)를 확인하세요):

우선, `loadSidebar`를 **true**로 설정합니다. 자세한사항은  [configuration paragraph](configuration.md#loadsidebar) 를 확인하세요.

```html
<!-- index.html -->

<script>
  window.$docsify = {
    loadSidebar: true
  }
</script>
<script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
```

 `_sidebar.md` 를 만드세요:

```markdown
<!-- docs/_sidebar.md -->

* [Home](/)
* [Guide](guide.md)
```

GitHub Pages가 밑줄로 시작하는 파일을 무시하지 않도록 `.nojekyll` 을 `./docs` 안에 생성하세요.

## Nested 사이드바

사이드바를 현재 디렉터리가 어디에 위치해있는지 보여주는 역할을 하길 원할지도 모릅니다.  각각의 파일에`_sidebar.md`.를 넣어주면 됩니다.

`_sidebar.md` is loaded from each level directory. If the current directory doesn't have `_sidebar.md`, it will fall back to the parent directory. If, for example, the current path is `/guide/quick-start`, the `_sidebar.md` will be loaded from `/guide/_sidebar.md`.

You can specify `alias` to avoid unnecessary fallback.

```html
<script>
  window.$docsify = {
    loadSidebar: true,
    alias: {
      '/.*/_sidebar.md': '/_sidebar.md'
    }
  }
</script>
```

!> You can create a `README.md` file in a subdirectory to use it as the landing page for the route.

## Set Page Titles from Sidebar Selection

A page's `title` tag is generated from the _selected_ sidebar item name. For better SEO, you can customize the title by specifying a string after the filename.

```markdown
<!-- docs/_sidebar.md -->
* [Home](/)
* [Guide](guide.md "The greatest guide in the world")
```

## Table of Contents

Once you've created `_sidebar.md`, the sidebar content is automatically generated based on the headers in the markdown files.

A custom sidebar can also automatically generate a table of contents by setting a `subMaxLevel`, compare [subMaxLevel configuration](configuration.md#submaxlevel).

```html
<!-- index.html -->

<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 2
  }
</script>
<script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
```

## Ignoring Subheaders

When `subMaxLevel` is set, each header is automatically added to the table of contents by default. If you want to ignore a specific header, add `{docsify-ignore}` to it.

```markdown
# Getting Started

## Header {docsify-ignore}

This header won't appear in the sidebar table of contents.
```

To ignore all headers on a specific page, you can use `{docsify-ignore-all}` on the first header of the page.

```markdown
# Getting Started {docsify-ignore-all}

## Header

This header won't appear in the sidebar table of contents.
```

Both `{docsify-ignore}` and `{docsify-ignore-all}` will not be rendered on the page when used.
