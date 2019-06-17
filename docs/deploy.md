# Deploy

 [GitBook](https://www.gitbook.com) 과 비슷하게, GitHub Pages, GitLab Pages 또는 VPS 에 배포할 수 있습니다..

## GitHub Pages

Github 레포지토리에 문서를 넣는 방법은 세가지가 있습니다:

- `docs/` folder
- master branch
- gh-pages branch

저장소의  `master` 브랜치의 `./docs` 하위폴더에 파일을 저장하는 것을 추천합니다. 그 다음 레포지토리 설정 페이지에서 `master branch /docs folder` 를 Github Pages 소스로 선택하세요.

![github pages](_images/deploy-github-pages.png)

!> 루트 데릭터리에 파일을 저장하고 `master branch` 를 선택할 수 있습니다.
 `.nojekyll` 파일을 배포위치에 위치시켜야합니다. (예를 들어 `/docs` 또는 the gh-pages branch)

## GitLab Pages

If you are deploying your master branch, include `.gitlab-ci.yml` with the following script:

?> The `.public` workaround is so `cp` doesn't also copy `public/` to itself in an infinite loop.

```YAML
pages:
  stage: deploy
  script:
  - mkdir .public
  - cp -r * .public
  - mv .public public
  artifacts:
    paths:
    - public
  only:
  - master
```

!> You can replace script with `- cp -r docs/. public`, if `./docs` is your Docsify subfolder.

## Firebase Hosting

!> You'll need to install the Firebase CLI using `npm i -g firebase-tools` after signing into the [Firebase Console](https://console.firebase.google.com) using a Google Account.

Using Terminal determine and navigate to the directory for your Firebase Project - this could be `~/Projects/Docs` etc. From there, run `firebase init`, choosing `Hosting` from the menu (use **space** to select, **arrow keys** to change options and **enter** to confirm). Follow the setup instructions.

You should have your `firebase.json` file looking similar to this (I changed the deployment directory from `public` to `site`):

```json
{
  "hosting": {
    "public": "site",
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**"]
  }
}
```

Once finished, build the starting template by running `docsify init ./site` (replacing site with the deployment directory you determined when running `firebase init` - public by default). Add/edit the documentation, then run `firebase deploy` from the base project directory.

## VPS

Try following nginx config.

```nginx
server {
  listen 80;
  server_name  your.domain.com;

  location / {
    alias /path/to/dir/of/docs/;
    index index.html;
  }
}
```

## Netlify

1.  Login to your [Netlify](https://www.netlify.com/) account.
2.  In the [dashboard](https://app.netlify.com/) page, click **New site from Git**.
3.  Choose a repository where you store your docs, leave the **Build Command** area blank, fill in the Publish directory area with the directory of your `index.html`, for example it should be docs if you populated it at `docs/index.html`.

### HTML5 router

When using the HTML5 router, you need to set up redirect rules that redirect all requests to your `index.html`, it's pretty simple when you're using Netlify, populate a `\redirects` file in the docs directory and you're all set:

```sh
/*    /index.html   200
```

## AWS Amplify

1. Set the routerMode in the Docsify project `index.html` to *history* mode.

```html
<script>
    window.$docsify = {
      loadSidebar: true,
      routerMode: 'history'
    }
</script>
```

2. Login to your [AWS Console](https://aws.amazon.com).
3. Go to the [AWS Amplify Dashboard](https://aws.amazon.com/amplify).
4. Choose the **Deploy** route to setup your project.
5. When prompted, keep the build settings empty if you're serving your docs within the root directory. If you're serving your docs from a different directory, customise your amplify.yml

```yml
version: 0.1
frontend:
  phases:
    build:
      commands: 
        - echo "Nothing to build"
  artifacts:
    baseDirectory: /docs
    files:
      - '**/*'
  cache:
    paths: []

```

6. Add the following Redirect rules in their displayed order.

| Source address | Target address | Type          |
|----------------|----------------|---------------|
| /<*>.md        | /<*>.md        | 200 (Rewrite) |
| /<*>           | /index.html    | 200 (Rewrite) |

