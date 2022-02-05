---
title: 'Distribute UI across an organization | 조직을 가로질러 UI를 배포'
tocTitle: 'Distribute | 배포'
description: 'Learn to package and import your design system into other apps | 디자인 시스템을 다른 앱에 패키지하고 가져오는 방법을 배운다.'
commit: 'bba7cb0'
---

<!-- From an architectural perspective, design systems are yet another frontend dependency. They are no different from popular dependencies like moment or lodash. UI components are code, so we can rely on established techniques for code reuse.  -->
설계적인 관점에서 디자인 시스템은 또 다른 프론트엔드 dependency(의존성)에 불과하다. 그런 시스템은 유명한 dependencies인 moment나 loadash과 같이 다를 바가 없다. UI 컴포넌트 코드라서, 재사용을 위해 확립된 기술에 의지해 사용할 수 있다.

<!-- This chapter walks through design system distribution from packaging UI components to importing them into other apps. We’ll also uncover time-saving techniques to streamline versioning and release. -->
이번 챕터에서는 디자인 시스템 배포하며 UI 컴포넌트를 패키징하는 것에서부터 그 컴포넌트를 다른 앱에 가져오는 것까지 해볼 것이다. 또한, release(릴리스)와 버전관리의 효율성을 높이기 위한 시간 절약 기술에 대해서도 알아본다.

![Propagate components to sites](/design-systems-for-developers/design-system-propagation.png)

## Package the design system | 디자인 시스템 패키지

<!-- Organizations have thousands of UI components spread across different apps. Previously, we extracted the most common components into our design system, and now we need to reintroduce those components back into the apps.  -->
조직은 다른 앱들을 가로질러 수 천개의 UI 컴포넌트를 가지고 있다. 이전에서는 디자인 시스템에서 가장 흔한 컴포넌트를 뽑았으며, 이제는 앱에다가 이를 재도입할 필요가 있다.

<!-- Our design system uses JavaScript package manager npm to handle distribution, versioning, and dependency management.  -->
디자인 시스템은 JavaScript 패키지 매니저인 npm을 배포, 버전관리, 그리고 dependency 관리에서 사용한다.

<!-- There are many valid methods for packaging design systems. Gander at design systems from Lonely Planet, Auth0, Salesforce, GitHub, and Microsoft to see a diversity in approaches. Some folks deliver each component as a separate package, and others ship all components in one package.  -->
패키징 디자인 시스템에는 다양한 방법들이 있는데 이러한 다양한 접근 방법은 Lonely Planet의 디자인 시스템인 Gander, Auth0, Salesforce, GitHub 그리고 Microsoft에서 확인할 수 있다. 각각의 컴포넌트를 개별적인 패키지로 관리하거나 하나의 패키지에 모든 컴포넌트를 관리하기도 한다.

<!-- For nascent design systems, the most direct way is to publish a single versioned package that encapsulates: -->
초기의 디자인 시스템을 위한 가장 직접적인 방법은 캡슐화한 하나의 버전 패키지를 설립하는 것이다. 

<!-- - 🏗 Common UI components
- 🎨 Design tokens (a.k.a., style variables)
- 📕 Documentation -->
- 🏗 공통적인 UI 컴포넌트
- 🎨 디자인 토큰 (style variables과 같은)
- 📕 문서화

![Package a design system](/design-systems-for-developers/design-system-package.jpg)

<!-- ## Prepare your design system for export -->
## export 하기 위한 디자인 시스템 준비

<!-- As we used [Create React App](https://github.com/facebook/create-react-app) (CRA) as a starting point for our design system, there are still vestiges of the initial app. Let’s clean them up now.  -->
디자인 시스템의 시작점으로 [Create React App](https://github.com/facebook/create-react-app) (CRA)을 사용했으며, 그것은 여전히 처음 앱의 모습을 가지고 있다. 그것을 정리해보자.

<!-- First, update the README.md to something more descriptive:  -->
첫번째로, REAM.md를 조금 더 구체적으로 업데이트한다. 

```markdown:title=README.md
<!-- # Storybook design system tutorial -->
# Storybook 디자인 시스템 튜토리얼

<!-- The Storybook design system tutorial is a subset of the full [Storybook design system](https://github.com/storybookjs/design-system/), created as a learning resource for those interested in learning how to write and publish a design system using best in practice techniques.  -->
Storybook 디자인 시스템 튜토리얼은 [Storybook design system](https://github.com/storybookjs/design-system/)의 일부분이며, 이 시스템을 가장 실용적인 방법으로 어떻게 쓰고, 어떻게 설립할 것인지 관심이 있어 배우고 싶은 이들을 위해 교육용 자료로 만들어졌다.

<!-- Learn more in [Storybook tutorials](https://storybook.js.org/tutorials/).  -->
더 많은 내용을 배우고 싶으면 [Storybook tutorials](https://storybook.js.org/tutorials/)를 참고하자.
```

<!-- Then, let’s create a `src/index.js` file to create a common entry point for our design system. From this file, we’ll export all our design tokens and the components: -->
이제, 디자인 시스템의 시작점을 만둘기 위해 `src/index.js` 파일을 만든다. 이 파일에서부터 모든 디자인 토큰과 컴포넌트를 export 할 것이다. 

```js:title=src/index.js
import * as styles from './shared/styles';
import * as global from './shared/global';
import * as animation from './shared/animation';
import * as icons from './shared/icons';

export { styles, global, animation, icons };

export * from './Avatar';
export * from './Badge';
export * from './Button';
export * from './Icon';
export * from './Link';
```

<!-- We'll need some additional development packages, we're going to use [`@babel/cli`](https://www.npmjs.com/package/@babel/cli) and [`cross-env`](https://www.npmjs.com/package/cross-env) to help us with the build process.  -->
추가적인 개발 패키지가 필요한데, build 과정에서 도와줄 [`@babel/cli`](https://www.npmjs.com/package/@babel/cli)와 [`cross-env`](https://www.npmjs.com/package/cross-env)을 사용할 것이다.

<!-- In your command line, issue the following command:  -->
커멘드 라인에서 다음과 같이 작성한다. 

```shell
yarn add --dev @babel/cli cross-env
```

<!-- With the packages installed, we'll need to implement the build process.  -->
패키지의 설치가 왼료되었으면, build 과정에서 구현해야한다.

<!-- Thankfully for us, Create React App (CRA) has already taken care of this for us. We'll use the existing `build` script and change it to build our design system to the `dist` directory:  -->
다행스럽게도, Create React App (CRA)은 이 과정이 이미 고려되어있다. 디자인 시스템을 `dist` 디렉토리에 build 하기 위해 `build` 스크립트를 사용하고 이를 수정할 것이다. 

```json:title=package.json
{
  "scripts": {
    "build": "cross-env BABEL_ENV=production babel src -d dist"
  }
}
```

<!-- With our build process implemented. We'll need to fine-tune it. Locate the `babel` key in your `package.json` and update it to the following:  -->
build 과정이 구현되었고, 이를 살짝 수정할 것이다. `package.json`에서 `babel`의 키를 다음과 같이 내용을 업데이트한다.

```json:title=package.json
{
  "babel": {
    "presets": [
      [
        "react-app",
        {
          "absoluteRuntime": false
        }
      ]
    ]
  }
}
```

<!-- Now we can run `yarn build` to build our code into the `dist` directory -- we should add that directory to `.gitignore` too, so we don't accidentally commit it: -->
 이제 `dist` 디렉토리 안에서 코드를 build 하기 위해 `yarn build`를 실행할 수 있다. -- 원치 않는 commit을 피하기 위해 이 디렉토리를 `gitignore`에 추가해야한다.

```
// ..
dist
```

<!-- #### Adding package metadata for publication  -->
#### 퍼블리싱을 위한 패키지 메타데이타 추가

<!-- We'll need to make changes to our `package.json` to ensure our package consumers get all the necessary information. The easiest way to do it is simply running `yarn init`--a command that initializes the package for publication: -->
패키지의 사용자가 필요한 모든 정보를 얻을 수 있도록 `package.json`을 수정해야한다. 가장 쉬운 방법은 `yarn init`을 실행하는 것이다. -- 이 명령어는 배포를 위한 패키지의 초기 상태를 설정한다.

```shell
# Initializes a scoped package | 스코프된 패키지 초기화
yarn init --scope=@your-npm-username

yarn init v1.22.5
question name (learnstorybook-design-system): @your-npm-username/learnstorybook-design-system
question version (0.1.0):
question description (Learn Storybook design system):Storybook design systems tutorial
question entry point (dist/index.js):
question repository url (https://github.com/your-username/learnstorybook-design-system.git):
question author (your-npm-username <your-email-address@email-provider.com>):
question license (MIT):
question private: no
```

<!-- The command will ask us a set of questions, some of which will be prefilled with answers, others that we’ll have to think about. You’ll need to pick a unique name for the package on npm (you won’t be able to use `learnstorybook-design-system` -- a good choice is `@your-npm-username/learnstorybook-design-system`).  -->
명령어는 한 세트의 질문을 할텐데, 이 중에서 일부는 답을 채워야할 것이고, 나머지는 생각해봐야한다. npm에 올릴 패키지를 위해 유니크한 이름을 골라야한다. (`learnstorybook-design-system`를 사용할 수 없을 것이다. -- 그렇기 때문에 앞에 npm의 사용자 이름을 적는 다음과 같은 방식이 좋은 선택일 것이다. `@your-npm-username/learnstorybook-design-system`)

<!-- All in all, it will update `package.json` with new values as a result of those questions:  -->
모두 완료했으면, `package.json`에 그 질문들에 대한 결과로써 새로운 값들과 함께 업데이트가 될 것이다. 

```json:title=package.json
{
  "name": "@your-npm-username/learnstorybook-design-system",
  "description": "Storybook design systems tutorial",
  "version": "0.1.0",
  "license": "MIT",
  "main": "dist/index.js",
  "repository": "https://github.com/your-username/learnstorybook-design-system.git"
  // ...
}
```

<!-- <div class="aside">
💡 For brevity purposes <a href="https://docs.npmjs.com/creating-and-publishing-scoped-public-packages">package scopes</a> weren't mentioned. Using scopes allows you to create a package with the same name as a package created by another user or organization without conflict.  -->

<div class="aside">
💡 간결하게 하기 위해 <a href="https://docs.npmjs.com/creating-and-publishing-scoped-public-packages">package scopes</a>는 언급하지 않았는데, 스코프의 이용은 다른 유저가 똑같은 이름의 패키지를 만들고, 또 조직과의 충돌없이 만들 수 있도록 허용한다.
</div>


<!-- Now that we’ve prepared our package, we can publish it to npm for the first time!  -->
패키지의 준비가 되었으면, npm에 처음으로 퍼블리싱할 수 있다!

<!-- ## Release management with Auto  -->
## Auto로 배포 관리

<!-- To publish releases to npm, we’ll use a process that also updates a changelog describing changes, sets a sensible version number, and creates git tag linking that version number to a commit in our repository. To help with all those things, we’ll use an open-source tool called [Auto](https://github.com/intuit/auto), designed for this very purpose.  -->
변경된 부분을 기록하는 changelog를 업데이트하고, 유효한 버전의 숫자를 입력하며, 그리고 레포지토리의 커밋에 있는 버전 숫자와 git 태그가 연결되도록 만드는 과정을 거쳐서 npm에 배포를 한다. 이 모든 과정을 위해 만들어진 [Auto](https://github.com/intuit/auto)라고 불리는 오픈소스 툴을 사용할 것이다. 

<!-- Let’s install Auto:  -->
 Auto를 설치하자.

```shell
yarn add --dev auto
```

<!-- Auto is a command line tool we can use for various common tasks around release management. You can learn more about Auto on [Auto 문서 사이트](https://intuit.github.io/auto/).  -->
Auto는 배포 관리시 일어날 수 있는 다양하고 흔한 과제를 수행할 때 사용할 수 있는 명령어라인 툴이다. [Auto 문서 사이트](https://intuit.github.io/auto/)에서 Auto에 대해 더 알 수 있다.

<!-- #### Getting a GitHub and npm token  -->
 #### GitHub 토큰과 npm 토큰 생성

<!-- For the next few steps, Auto is going to talk to GitHub and npm. For that to work correctly, we’ll need a personal access token. You can get one of those on [this page](https://github.com/settings/tokens) for GitHub. The token will need the `repo` scope.  -->
다음 단계로 더 나아가기 위해서 Auto에서 GitHub와 npm과 통신을 해야할 것이다. 명확한 일처리를 위해 개인적으로 접근이 가능한 토큰이 필요하다. [깃헙 토큰 페이지](https://github.com/settings/tokens)에서 토큰을 얻을 수 있으며, 이 토큰은 `repo` 스코프를 필요로 한다.

<!-- For npm, you can create a token at the URL: https://www.npmjs.com/settings/&lt;your-username&gt;/tokens.  -->
npm에서는, 다음 URL에서 토큰을 생성할 수 있다. : https://www.npmjs.com/settings/&lt;your-username&gt;/tokens

<!-- You’ll need a token with “Read and Publish” permissions.  -->
"읽기와 배포"의 허가에 대한 토큰이 필요할 것이다.

<!-- Let’s add that token to a file called `.env` in our project:  -->
 `env`라고 불리는 파일을 프로젝트에 추가하고 받은 토큰을 추가해보자.

```
GH_TOKEN=<value you just got from GitHub>
NPM_TOKEN=<value you just got from npm>
```

<!-- By adding the file to `.gitignore`, we ensure that we don’t accidentally push this value to an open-source repository that all our users can see! This is crucial. If other maintainers need to publish the package locally (later we’ll set things up to auto-publish when a pull request is merged into the default branch), they should set up their own `.env` file following this process:  -->
`.env` 를 `.gitignore` 에 추가함으로써, 이 토큰값을 모든 사용자가 볼 수 있는 오픈소스 레포지토리에 원치않게 올리는 일이 없도록 주의한다. 이 부분은 중요하다. 만일 다른 관리자들이 로컬에서 이 패키지를 배포해야한다면(이 부분은 기본 브랜치에 PR을 해서 merge할 때 자동 배포가 되도록 나중에 설정을 할 것이다.), 관리자들이 그들만의 `.env` 파일을 이 과정에 따라 설정해야한다. 

```
dist
.env
```

<!-- #### Create labels on GitHub  -->
#### GitHub에 라벨 생성

<!-- The first thing we need to do with Auto is to create a set of labels in GitHub. We’ll use these labels in the future when making changes to the package (see the next chapter), and that’ll allow `auto` to update the package version sensibly and create a changelog and release notes.  -->
Auto로 가장 우선적으로 해야할 것은 GitHub에 라벨 종류를 생성하는 것이다. 이 라벨은 나중에 패키지를 수정하거나(다음 챕터에서 볼 수 있다.) 때 사용할 것인데, 이는 `auto`로 패키지 버전을 업데이트하고, changelog와 릴리즈 노트를 생성할 수 있도록 해준다.

```bash
yarn auto create-labels
```

<!-- If you check on GitHub, you’ll now see a set of labels that `auto` would like us to use:  -->
GitHub를 확인해본다면, 이제 `auto`가 추천하는 라벨 종류가 생성된 걸 볼 수 있을 것이다. 

![Set of labels created on GitHub by auto](/design-systems-for-developers/github-auto-labels.png)

<!-- We should tag all future PRs with one of the labels: `major`, `minor`, `patch`, `skip-release`, `prerelease`, `internal`, `documentation` before merging them.  -->
Pmerge(병합)하기 전에 이 모든 라벨 중에서 하나를 골라 PR시 태그해야한다. 

<!-- #### Publish our first release with Auto manually  -->
#### 매뉴얼적으로 Auto를 이용해 처음으로 배포
<!-- 
In the future, we’ll calculate new version numbers with `auto` via scripts, but for the first release, let’s run the commands manually to understand what they do. Let’s generate our first changelog entry:  -->
 나중에는 새로운 버전 숫자를 스크립트를 이용해서 `auto` (자동)로 계산하겠지만, 처음 배포할 때는 명령어를 실행해서 무엇을 하는 지 원리를 이해해보자. changlog를 처음으로 발생시켜보자. 

```shell
yarn auto changelog
```

<!-- It will generate a long changelog entry with every commit we’ve created so far (and a warning we’ve been pushing to the default branch, which we should stop doing soon).  -->
생성한 모든 commit마다 장문의 changlog가 발생될 것인데 (기본 브랜치에 push하면 경고가 뜨는데, 이는 곧 멈추게 할 것이다.)

<!-- Although it is helpful to have an auto-generated changelog, so you don’t miss things, it’s also a good idea to manually edit it and craft the message in the most useful way for users. In this case, the users don’t need to know about all the commits along the way. Let’s make a nice simple message for our first v0.1.0 version. First undo the commit that Auto just created (but keep the changes:  -->
자동적으로 발생한 changlog는 유용하지만, 사용자를 위한 changelog를 수정하고, 도움이 되는 메세지를 남기는 것도 좋은 방법이다. 이 상황에서는 사용자가 모든 커밋을 알아야할 필요가 없어 첫번째 v0.1.0 버전의 간단한 메세지를 만들어보자. Auto가 방금 생성한 커밋을 되돌리지만, 변경사항은 유지한다.

```shell
git reset HEAD^
```

<!-- Then we’ll update the changelog and commit it:  -->
그리고 changelog를 업데이트하고 이를 커밋한다.

```
# v0.1.0 (Tue Mar 09 2021)

- Created first version of the design system, with `Avatar`, `Badge`, `Button`, `Icon` and `Link` components. | `Avatar`, `Badge`, `Button`, `Icon` 그리고 `Link` 컴포넌트의 첫 디자인 시스템 버전을 만들었다.

#### Authors: 1

- [your-username](https://github.com/your-username)
```

<!-- Let’s add that changelog to git. Note that we use `[skip ci]` to tell CI platforms to ignore these commits, else we end up in their build and publish loop.  -->
git에 changelog를 추가해보자. 알아둘 것은 CI 플랫폼에 이 커밋들을 무시하도록 `[skip ci]`를 하고, 그렇지 않으면 빌드와 배포의 순환이 계속 된다.

```shell
git add CHANGELOG.md
git commit -m "Changelog for v0.1.0 [skip ci]"
```

<!-- Now we can publish:  -->
이제 배포할 수 있다.

```shell
npm --allow-same-version version 0.1.0 -m "Bump version to: %s [skip ci]"
npm publish --access=public
```

<!-- <div class="aside">
💡 Don't forget to adjust the commands accordingly if you're using <a href="https://classic.yarnpkg.com/en/docs/cli/">yarn</a> to publish your package.
</div> -->

<div class="aside">
💡 <a href="https://classic.yarnpkg.com/en/docs/cli/">yarn</a>을 사용한다면 그에 알맞게 명령어 사용하는 걸 잊지말자. 
</div>

<!-- And use Auto to create a release on GitHub:  -->
그리고 Auto를 사용해서 GitHub에 릴리즈를 생성한다.

```shell
git push --follow-tags origin main
yarn auto release
```

<!-- Yay! We’ve successfully published our package to npm and created a release on GitHub (with luck!).  -->
오예! 성공적으로 npm에 배포하고 GitHub에 릴리즈를 만들었다.

![Package published on npm](/design-systems-for-developers/npm-published-package.png)

![Release published to GitHub](/design-systems-for-developers/github-published-release.png)

<!-- (Note that although `auto` auto-generated the release notes for the first release, we've also modified them to make sense for a first version).  -->
 (`auto`가 자동적으로 첫번째 릴리즈를 릴리즈 노트에 기록해주지만, 첫번째 버전에 맞게 수정했다는 걸 알아두자. )

<!-- #### Set up scripts to use Auto  -->
#### Auto를 사용해서 스크립트 설정

<!-- Let’s set up Auto to follow the same process when we want to publish the package in the future. We’ll add the following scripts to our `package.json`:  -->
후에 패키지를 배포하고 싶을 때와 같은 과정을 따라서 Auto를 설정해보자. 다음과 같은 스크립트를 `package.json`에 추가한다.

```json:title=package.json
{
  "scripts": {
    "release": "auto shipit --base-branch=main"
  }
}
```

<!-- Now, when we run `yarn release`, we'll go through all the steps we ran above (except using the auto-generated changelog) in an automated fashion. All commits to the default branch will be published.  -->
 이제, `yarn release`를 실행할 때 (자동 생성 changelog를 사용할 때 제외하고) 자동적으로 위와 같은 모든 과정을 밟을 것이다. 기본 브랜치에 푸시한 모든 커밋들은 배포된다.

<!-- Congratulations! You set up the infrastructure to manually publish your design system releases. Now learn how to automate releases with continuous integration.  -->
축하한다! 매뉴얼적으로 디자인 시스템을 배포하기위한 기본적인 인프라 구축을 설정했다. 이제 지속적인 통합(CI)으로 어떻게 자동으로 배포할 것인지 알아보자.

<!-- ## Publish releases automatically  -->
## 자동으로 릴리즈 배포

<!-- We use GitHub Actions for continuous integration. But before proceeding, we need to securely store the GitHub and NPM tokens from earlier so that Actions can access them.  -->
지속적인 통합을 위해서 GitHub Actions를 사용한다. 하지만 이 절차를 하기 전에, 앞에서 언급했던 GitHub 토큰과 NPM 토큰을 보관한 안전하게 저장해야한다. 그래야 Actions가 GitHub와 NPM에 접근할 수 있다.  

<!-- #### Add your tokens to GitHub Secrets  -->
#### GitHub Secrets에 토큰 저장

<!-- GitHub Secrets allow us to store sensitive information in our repository. In a browser window, open your GitHub repository.  -->
 GitHub Secrets은 레포지토리에 민감한 정보를 저장할 수 있게 한다. 브라우저에서 GitHub 레포지토리를 열어본다. 

<!-- Click the ⚙️ Settings tab then the Secrets link in the sidebar. You'll see the following screen:  -->
⚙️ 설정 탭을 클릭하고, 사이드바에 있는 Secrets 링크를 클릭한다. 그러면 다음과 같은 스크린이 보인다.

![Empty GitHub secrets page](/design-systems-for-developers/github-empty-secrets-page.png)

<!-- Click the **New secret** button. Use `NPM_TOKEN` for the name and paste the token you got from npm earlier in this chapter.  -->
 **New secret** 버튼을 클릭한다. 이름으로 `NPM_TOKEN`을 사용하고, 이 챕터를 진행하며 npm에서 얻은 토큰을 붙여넣는다.

![Filled GitHub secrets form](/design-systems-for-developers/github-secrets-form-filled.png)

<!-- When you add the npm secret to your repository, you'll be able to access it as `secrets.NPM_TOKEN`. You don't need to set up another secret for your GitHub token. All GitHub users automatically get a `secrets.GITHUB_TOKEN` associated with their account.  -->
레포지토리에 npm 토큰을 추가하며, `secrets.NPM_TOKEN`으로써 이를 접근할 수 있다. 그러면 GitHub 토큰을 위해 또 다른 secret 설정을 할 필요가 없다.

<!-- #### Automate releases with GitHub Actions  -->
#### GitHub Actions으로 릴리즈 자동화

<!-- Every time we merge a pull request, we want to publish the design system automatically. Create a new file called `push.yml` in the same folder we used earlier to <a href="https://storybook.js.org/tutorials/design-systems-for-developers/react/en/review/#publish-storybook">publish Storybook</a> and add the following:  -->
PR을 매번 병합할 때마다, 자동적으로 디자인 시스템이 배포되길 원할 것이다. `push.yml`이라는 새로운 이름의 파일을 <a href="https://storybook.js.org/tutorials/design-systems-for-developers/react/en/review/#publish-storybook">publish Storybook</a>에서 썼던 폴더와 같은 위치에 생성하고 다음과 같이 추가한다.

```yml:title=.github/workflows/push.yml
# Name of our action 
# action의 이름
name: Release

# The event that will trigger the action 
# action을 발생시킬 이벤트
on:
  push:
    branches: [main]

# what the action will do 
# action이 무엇을 할 것인지
jobs:
  release:
    # The operating system it will run on 
    # 실행될 운영 체제
    runs-on: ubuntu-latest
    # This check needs to be in place to prevent a publish loop with auto and github actions 
    # 자동으로 배포될 루프와 github actions를 방지하려면 이 확인이 필요
    if: "!contains(github.event.head_commit.message, 'ci skip') && !contains(github.event.head_commit.message, 'skip ci')"
    # The list of steps that the action will go through 
    # action이 진행될 순서 목록
    steps:
      - uses: actions/checkout@v2
      - name: Prepare repository
        run: git fetch --unshallow --tags
      - name: Use Node.js 12.x
        uses: actions/setup-node@v1
        with:
          node-version: 12.x
      - name: Cache node modules
        uses: actions/cache@v1
        with:
          path: node_modules
          key: yarn-deps-${{ hashFiles('yarn.lock') }}
          restore-keys: |
            yarn-deps-${{ hashFiles('yarn.lock') }}
      - name: Create Release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          #👇 npm token, see https://storybook.js.org/tutorials/design-systems-for-developers/react/en/distribute/ to obtain it 
          #👇 npm 토큰, https://storybook.js.org/tutorials/design-systems-for-developers/react/en/distribute/ 다음 문서를 참조한다.
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: |
          yarn install --frozen-lockfile
          yarn build
          yarn release
```

<!-- Save and commit your changes to the remote repository.  -->
저장하고 변화된 부분을 remote 레포지토리에 커밋한다.

<!-- Success! Now every time you merge a PR to the default branch, it will automatically publish a new version, incrementing the version number as appropriate due to the labels you’ve added.  -->
성공! 이제 기본 브랜치에 PR을 병합할 때마다, 자동적으로 새로운 버전이 배포가 되며, 추가했던 라벨에 맞춰 버전 숫자로 업데이트 된다. 

<!-- <div class="aside">💡 We didn’t cover all of Auto’s many features and integrations that might be useful for growing design systems. Read the docs <a href="https://github.com/intuit/auto">here</a>
</div> -->
<div class="aside">
💡 디자인 시스템 확장에 도움을 줄 수 있는 Auto의 모든 기능과 통합을 알아보지는 못했다. 더 많은 내용을 알고 싶으면 <a href="https://github.com/intuit/auto"> 이 문서를 참고하자.
</div>

![Import the design system](/design-systems-for-developers/design-system-import.png)

<!-- ## Import the design system in an app  -->
## 앱에 디자인 시스템 적용

<!-- Now that our design system lives online installing the dependency and using the UI components is trivial.  -->
이제 디자인 시스템이 온라인에 존재하고 있으니 dependency로 설치하고, UI 컴포넌트로 사용하는 것은 간단하다.

<!-- #### Get the example app  -->
#### 예제 앱 준비

<!-- Earlier in this tutorial, we standardized on a popular frontend stack that includes React and Styled Components. That means our example app must also use React and Styled Components to take full advantage of the design system.  -->
이 튜토리얼의 앞 부분에서, 많이 쓰이는 프론트엔드 스택을 표준화를 했다. 이 스택에는 React와 Styled Components가 포함되는데, 즉, 디자인 시스템을 최대한 활용하려면 예제 앱에서도 React 및 Styled Components를 사용해야 한다.


<!-- <div class="aside">💡 Other promising methods like Svelte or web components may allow you to ship framework-agnostic UI components. However, they are relatively new, under-documented, or lack widespread adoption, so they’re not included in this guide yet.</div> -->

<div class="aside">
💡 다른 Svelte나 web components와 같은 다른 유망한 방법을 사용하면 프레임워크에 구애받지 않고 UI 컴포넌트를 구성할 수 있다. 하지만, Svelte나 web components는 상대적으로 새로운 방식이라 문서화가 덜 되었거나 널리 채택되지 않았기 때문에 이 가이드에는 아직 포함되지 않았다.
</div>

<!-- The example app uses Storybook to facilitate [Component-Driven Development](https://www.componentdriven.org/), an app development methodology for building UIs from the bottom, starting with components ending with pages. We’ll run two Storybooks side-by-side during the demo: one for our example app and one for our design system.  -->
 예제 앱은 Storybook을 사용하여 [컴포넌트 주도 개발 Component-Driven Development](https://www.componentdriven.org/)를 용이하게 한다. 이 개발 방법은 UI를 아래에서부터, 즉 컴포넌트 개발부터 시작해서 페이지로 끝내는 방식이다. 데모하는 동안에 두 개의 Storybook을 번갈아가며 실행할 것이다. 하나는 예제 앱을 위한 것이고 다른 하나는 디자인 시스템을 위한 것이다. 

<!-- Run the following commands in your command line to set up the example app:  -->
에재 앱을 설정하기 위해서 커맨드 라인에 다음과 같이 명령어를 실행한다.

```shell
# Clones the files locally 
# 지역적으로 파일을 클론
npx degit chromaui/learnstorybook-design-system-example-app example-app

cd example-app

# Install the dependencies 
# dependencies를 설치
yarn install

## Start Storybook 
## Storybook 실행
yarn storybook
```

<!-- You should see the Storybook running with the stories for the simple components the app uses:  -->
앱에서 사용하는 간단한 컴포넌트의 stories와 함께 Storybook이 실행되는 것을 볼 수 있어야 한다.

![Initial storybook for example app](/design-systems-for-developers/example-app-starting-storybook-6-0.png)

<!-- <h4>Integrating the design system</h4> -->
<h4>디자인 시스템 통합</h4>

<!-- We have our design system's Storybook published. Let's add it to our example app. We can do that by updating example app’s `.storybook/main.js` to the following:  -->
배포한 Storybook의 디자인 시스템을 가지고 있다. 이제 Storybook을 예제 앱에 추가해보자. 예제 앱의 `.storybook/main.js`를 다음과 같이 업데이트할 수 있다.

```diff:title=.storybook/main.js
// .storybook/main.js

module.exports = {
  stories: ['../src/**/*.stories.@(js|jsx|ts|tsx)'],
+ refs: {
+   'design-system': {
+     title: 'My design system',
+     //👇 The url provided by Chromatic when it was deployed
+     url: 'https://your-published-url.chromatic.com',
+   },
+ },
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/preset-create-react-app',
  ],
  framework: '@storybook/react',
  staticDirs: ['../public'],
};
```

<video autoPlay muted playsInline loop>
  <source
    src="/design-systems-for-developers/storybook-composition-6-0.mp4"
    type="video/mp4"
  />
</video>

<!-- <div class="aside">
💡 Adding the <code>refs</code> key to <code>.storybook/main.js</code>, allows us to <a href="https://storybook.js.org/docs/react/workflows/storybook-composition">compose</a> multiple Storybooks into one. This is helpful when working with big projects that might spread around multiple repositories or use different tech stacks. 
</div> -->
<div class="aside">
💡 <code>refs</code> 키를 <code>.storybook/main.js</code>에 추가하는 것은, 여러 개의 Storybook을 <a href="https://storybook.js.org/docs/react/workflows/storybook-composition">구성</a>에 맞게 하나로 통할 수 있게 한다. 이것은 여러 개의 레포지토리가 분산되어 있거나 다른 기술 스택을 쓰는 규모가 큰 프로젝트를 진행할 때 도움이 된다.
</div>

<!-- You’ll now be able to browse the design system components and docs while developing the example app. Showcasing the design system during feature development increases the likelihood that developers will reuse existing components instead of wasting time inventing their own. -->
이제 예제 앱을 개발하는 동안 디자인 시스템 컴포넌트와 문서를 검색할 수 있다. 기능 개발을 하는 도중에 디자인 시스템을 보여주면 개발자가 본인의 컴포넌트를 구성하는데 시간을 낭비하는 대신 기존에 존재하는 컴포넌트를 재사용할 가능성이 높아진다.

<!-- We have what we need, time to add our design system and start using it. Run the following command in your terminal:  -->
필요한 것을 가지고 있으니 디자인 시스템을 추가하고 사용할 시간이다. 다음과 같이 터미널에 명령어를 실행한다.

```shell
yarn add @your-npm-username/learnstorybook-design-system
```

<!-- We'll need to use the same global styles defined in the design system, so we'll need to update [`.storybook/preview.js`](https://storybook.js.org/docs/react/configure/overview#configure-story-rendering) config file and add a [global decorator](https://storybook.js.org/docs/react/writing-stories/decorators#global-decorators).  -->
디자인 시스템에 정의한 것과 같은 전역 스타일을 사용할 예정이어서 [`.storybook/preview.js`](https://storybook.js.org/docs/react/configure/overview#configure-story-rendering) config 파일을 업데이트하고 [global decorator](https://storybook.js.org/docs/react/writing-stories/decorators#global-decorators)을 추가한다.

```js:title=.storybook/preview.js
import React from 'react';

// The styles imported from the design system. 
// 디자인 시스템에서 import된 스타일
import { global as designSystemGlobal } from '@your-npm-username/learnstorybook-design-system';

const { GlobalStyle } = designSystemGlobal;

/*
 * Adds a global decorator to include the imported styles from the design system. | 디자인 시스템에서 imported된 스타일이 포함된 전역적인 decorator를 추가한다.
 * More on Storybook decorators at: | Storybook decorators에 대한 더 많은 정보는 아래를 참고
 * https://storybook.js.org/docs/react/writing-stories/decorators#global-decorators
 */
export const decorators = [
  Story => (
    <>
      <GlobalStyle />
      <Story />
    </>
  ),
];
/*
 * More on Storybook parameters at: | Storybook의 parameters에 대한 더 많은 정보는 아래를 참고
 * https://storybook.js.org/docs/react/writing-stories/parameters#global-parameters
 */
export const parameters = {
  actions: { argTypesRegex: '^on[A-Z].*' },
};
```

![Example app storybook with design system stories](/design-systems-for-developers/example-app-storybook-with-design-system-stories-6-0.png)

<!-- We’ll use the `Avatar` component from our design system in the example app’s `UserItem` component. `UserItem` should render information about a user, including a name and profile photo.  -->
예제 앱의 `UserItem` 컴포넌트에 있는 디자인 시스템을  `Avatar` 컴포넌트에 사용한다. `UserItem`은 반드시 사용자의 이름과 프로필 사진을 포함한 정보를 렌더해야 한다.

<!-- In your editor, open the `UserItem` component located in `src/components/UserItem.js`. Also, select `UserItem` in your Storybook to see the code changes we're about to make instantly with hot module reload.  -->
에디터에서 `src/components/UserItem.js`에 위치한 `UserItem` 컴포넌트를 연다. 그리고 곧이어 변경할 코드가 hot module 재로딩되는 것을 보기 위해서 Storybook에 있는 `UserItem`을 선택한다.

<!-- Import the Avatar component.  -->
Avatar component를 import한다.

```js:title=src/components/UserItem.js
import { Avatar } from '@your-npm-username/learnstorybook-design-system';
```

<!-- We want to render the Avatar beside the username.  -->
사용자 이름 옆에 Avatar를 보여준다.

```diff:title=src/components/UserItem.js
import React from 'react';

import styled from 'styled-components';

+ import { Avatar } from '@your-npm-username/learnstorybook-design-system';

const Container = styled.div`
  background: #eee;
  margin-bottom: 1em;
  padding: 0.5em;
`;

- const Avatar = styled.img`
-   border: 1px solid black;
-   width: 30px;
-   height: 30px;
-   margin-right: 0.5em;
- `;

const Name = styled.span`
  color: #333;
  font-size: 16px;
`;

export default ({ user: { name, avatarUrl } }) => (
  <Container>
+   <Avatar username={name} src={avatarUrl} />
    <Name>{name}</Name>
  </Container>
);
```

<!-- Upon save, the `UserItem` component will update in Storybook to show the new Avatar component. Since `UserItem` is a part of the `UserList` component, you’ll also see the `Avatar` in `UserList`.  -->
위의 내용을 저장하고, `UserItem` 컴포넌트는 새로운 Avatar 컴포넌트를 보여주기 위해서 Storybook에 업데이트 될 것이다. `UserItem`이 `UsetList`에 포함되어 있기 때문에 `Avatar`가 `UserList`에 있는 걸 볼 수 있다.

![Example app using the Design System](/design-systems-for-developers/example-app-storybook-using-design-system-6-0.png)

<!-- There you have it! You just imported a design system component into the example app. Whenever you publish an update to the Avatar component in the design system, that change will also be reflected in the example app when you update the package.  -->
됐다! 방금 예제 앱에 디자인 시스템 컴포넌트를 import했다. Avatar 컴포넌트를 업데이트해서 배포할 때마다, 패키지를 업데이트 할 시 변경된 부분 또한 예제 앱에 반영된다.

![Distribute design systems](/design-systems-for-developers/design-system-propagation-storybook.png)

<!-- ## Master the design system workflow  -->
## 디자인 시스템 일의 흐름 마스터

<!-- The design system workflow starts with developing UI components in Storybook and ends with distributing them to client apps. That’s not all though. Design systems must continually evolve to serve ever-changing product requirements, and our work has only just begun.  -->
디자인 시스템 일의 흐름은 Storybook에 있는 UI 컴포넌트를 개발하는 것에서부터 시작해서 개발한 컴포넌트를 클라이언트 앱에 배포하는 것으로 끝을 맺는다. 그것이 전부가 아니다. 디자인 시스템은 끊임없이 변화하는 제품의 요구사항을 충족하기 위해 지속적으로 발전해야하며, 이것은 시작일 뿐이다. 

<!-- Chapter 8 illustrates the end-to-end design system workflow we created in this guide. We’ll see how UI changes ripple outward from the design system.  -->
챕터 8에서는 이 가이드에서 만든 end-to-end 디자인 시스템의 흐름을 설명한다. 외부에서 변경한 UI가 어떻게 디자인 시스템을 영향을 끼치는지 알아본다. 
