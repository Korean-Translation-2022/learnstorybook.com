---
title: '디자인 시스템을 위한 작업 흐름(workflow)'
tocTitle: '작업 흐름(workflow)'
description: '프론트엔드 개발자를 위한 디자인 시스템 작업 흐름(workflow)의 개요'
commit: 
---

<!-- How frontend tools work together has a significant impact on the ultimate value design and development teams can realize. When done well, it should be seamless to develop and reuse UI components. -->
프론트엔드에서 사용하는 툴이 함께 작동하는 방식은 디자인 및 개발 팀이 실현할 수 있는 궁극적인 가치에 커다란 영향을 끼칩니다.

<!-- This chapter showcases the five-step workflow by introducing a new component AvatarList. -->
이번 챕터에서는 새로운 AvatarList 컴포넌트를 소개함으로써 다섯 단계의 작업 흐름(workflow)을 설명합니다.

![Design system workflow](/design-systems-for-developers/design-system-workflow-horizontal.jpg)

## 설계

<!-- `AvatarList` is a component that displays multiple avatars. Like the other design system components, `AvatarList` started off being pasted into many projects, and that’s why it warrants inclusion in the design system. Let’s assume that the component was developed in another project and jump straight to the finished code for this demo. -->
`AvatarList`는 여러 개의 아바타(avatar)를 보여주는 컴포넌트 입니다. 다른 디자인 시스템 컴포넌트처럼 `AvatarList`는 다른 많은 프로젝트에서 붙여 넣어지며 사용되기 시작했습니다. 그래서 디자인 시스템에 포함되어야 하는 이유입니다. 다른 프로젝트에서 개발된 컴포넌트가 있다는 가정 하에 이 데모의 완성된 코드로 바로 이동하겠습니다.

![AvatarList](/design-systems-for-developers/AvatarList.jpg)

<!-- First, create a new branch where we’ll be tracking this work. -->
우선, 작업을 진행할 새로운 브랜치를 생성합니다. 

```shell
git checkout -b create-avatar-list-component
```

<!-- Download the `AvatarList` component and story to your machine and place them in the `/src` directory: -->
`AvatarList` 컴포넌트와 story를 다운로드 받아서 `/src` 디렉토리에 위치시킵니다.

- [Component file](https://raw.githubusercontent.com/chromaui/learnstorybook-design-system/716a4c22160eaeaabb8e2c78241f2807844deed0/src/AvatarList.js)
- [Story file](https://raw.githubusercontent.com/chromaui/learnstorybook-design-system/716a4c22160eaeaabb8e2c78241f2807844deed0/src/AvatarList.stories.js)

![Storybook with AvatarList component](/design-systems-for-developers/storybook-with-avatarlist-6-0.png)

<!-- <div class="aside">
💡 Storybook is set up to automatically detect files ending in <code>*.stories.js</code> and show them in the UI.
</div> -->
<div class="aside">
💡 Storybook은 자동적으로 확장자가 <code>*.stories.js</code>로 끝나는 파일을 감지하도록 설정되어 UI에서 보여줍니다.
</div>

<!-- Nice! Now let’s articulate each UI state supported by `AvatarList`. At a glance, it’s clear that `AvatarList` supports some of `Avatar`’s properties like `small` and `loading`. -->
좋습니다! 이제 `AvatarList`에서 지원되는 각각의 UI 상태를 명확하게 설명하겠습니다. 살짝 보면 `AvatarList`는 `small`및 `loading`과 같은 `Avatar`의 속성을 지원하는 걸 알 수 있습니다.

```js:title=src/AvatarList.stories.js
export const SmallSize = Template.bind({});
SmallSize.args = {
  users: Short.args.users,
  size: 'small',
};

export const Loading = Template.bind({});
Loading.args = {
  loading: true,
};
```

![Storybook with more AvatarList stories](/design-systems-for-developers/storybook-with-avatarlist-loading-6-0.png)

<!-- Given that it’s a list, it should show many avatars. Let’s add stories that showcase what happens with numerous list items and with few list items. -->
리스트인 것을 생각한다면, 여러 아바타들이 보여져야 합니다. 리스템 아이템이 많을 때와 적을 때 어떤 일이 발생하는지 볼 수 있는 스토리를 추가해봅니다.

```js:title=src/AvatarList.stories.js
export const Ellipsized = Template.bind({});
Ellipsized.args = {
  users: [
    ...Short.args.users,
    {
      id: '3',
      name: 'Zoltan Olah',
      avatarUrl: 'https://avatars0.githubusercontent.com/u/81672',
    },
    {
      id: '4',
      name: 'Tim Hingston',
      avatarUrl: 'https://avatars3.githubusercontent.com/u/1831709',
    },
  ],
};

export const BigUserCount = Template.bind({});
BigUserCount.args = {
  users: Ellipsized.args.users,
  userCount: 100,
};

export const Empty = Template.bind({});
Empty.args = {
  users: [],
};
```

<video autoPlay muted playsInline loop>
  <source
    src="/design-systems-for-developers/storybook-with-all-avatarlist-stories-6-0.mp4"
    type="video/mp4"
  />
</video>

<!-- Save your progress and commit. -->
과정을 저장하고 커밋을 해보세요.

```shell
git commit -am "Added AvatarList and stories"
```

## 문서

<!-- Thanks to Storybook Docs, we get customizable documentation with minimal effort. This helps others learn how to use AvatarList by referring to the Docs tab in Storybook. -->
Storybook 문서 덕분에 최소한의 노력으로 사용자가 커스텀할 수 있는 문서를 얻을 수 있습니다. Storybook 문서를 참고해서 AvatarList를 사용하려는 다른 사람들에게 도움이 됩니다.

![Storybook docs with minimal AvatarList info](/design-systems-for-developers/storybook-docs-minimal-avatarlist.png)

<!-- Minimum viable docs! Let’s make AvatarList a bit more human by supplying additional context on how to use it. -->
최소한의 기능이 담긴 문서가 왼성되었습니다! AvatarList를 어떻게 사용하는지 추가적인 설명을 붙여주세요.

```js:title=src/AvatarList.js
/**
 * A list of Avatars, ellipsized to at most 3. Supports passing only a subset of the total user count. | 아바타 리스트는 최대 3개까지 말줄임표로 표시됩니다. 총 사용자 수보다 작은 수만 전달할 수 있습니다.
 */
export function AvatarList({ loading, users, userCount, size, ...props }) {
```

<!-- Sprinkle in some additional details about the supported props. -->
지원되는 props에 대한 추가적인 설명을 제공합니다.

```js:title=src/AvatarList.js
AvatarList.propTypes = {
  /**
   * Are we loading avatar data from the network? | 네트워크에서 받은 아바타 데이터가 로딩되고 있나요?
   */
  loading: PropTypes.bool,
  /**
   * A (sub)-list of the users whose avatars we have data for. Note: only 3 will be displayed. | 데이터를 가지고 있는 아바타의 리스트입니다. 노트 : 3개만 보입니다.
   */
  users: PropTypes.arrayOf(
    PropTypes.shape({
      id: PropTypes.string.isRequired,
      name: PropTypes.string,
      avatarUrl: PropTypes.string,
    })
  ),
  /**
   * The total number of users, if a subset is passed to `users`. | props가 `users`에게 전달될 경우의 총 사용자 수 입니다.
   */
  userCount: PropTypes.number,
  /**
   * AvatarList comes in four sizes. In most cases, you’ll be fine with `medium`. | AvatarList는 네 개의 사이즈로 구성됩니다. 대부분의 경우, `medium`을 사용할거에요.
   */
  size: PropTypes.oneOf(Object.keys(sizes)),
};
```

<!-- Easy as pie! This level of detail is sufficient for now-–we can always customize more using MDX later. -->
간단하죠! 지금으로써는 이 정도의 설명이면 충분합니다. 나중에 MDX를 이용하면 언제나 원하는대로 수정할 수 있습니다.

![Storybook docs with full AvatarList info](/design-systems-for-developers/storybook-docs-full-avatarlist.png)

<!-- Documentation doesn’t have to be tiresome. With automated tooling, we removed the tedium to get straight to writing. -->
문서화는 지루한 작업이 아닙니다. 자동화가 가능한 툴을 사용해서 지루함을 제거하고 글을 바로 쓸 수 있습니다.

<!-- Commit the changes and push to GitHub. -->
변경된 부분을 커밋하고 GitHub에 푸시해보세요.

```shell
git commit -am "Improved AvatarList docs"
```

#### 풀 리퀘스트 생성

<!-- Let’s push our `AvatarList` branch to GitHub and create a pull request: -->
GitHub에 있는 `AvatarList` 브랜치에 푸시하고 풀 리퀘스트를 생성해봅니다. - 

```shell
git push -u origin create-avatar-list-component
```

<!-- Then navigate to GitHub and open a pull request. -->
그리고 GitHub로 가서 풀 리퀘스트를 열어보세요.

![PR created in PR for AvatarList](/design-systems-for-developers/github-pr-create-avatarlist.png)

## 리뷰

<!-- At this point, `AvatarList` is a candidate for design system inclusion. Stakeholders must review the component to see if it meets expectations for functionality and appearance. -->
현 시점에서 `AvatarList`는 디자인 시스템에 포함될 후보입니다. 관계자들이 기대한 대로 동작하고 모양이 맞는지 컴포넌트를 확인하고 반드시 리뷰를 해야합니다.

<!-- The design system’s Storybook is automatically published with each pull request to make review dead simple. Scroll down to the PR checks to find a link to the deployed Storybook. -->
Storybook의 디자인 시스템은 각 풀 리퀘스트 때마다 자동으로 배포돼서 리뷰를 쉽게 할 수 있습니다. PR 확인란으로 스크롤을 내려서 배포된 Storybook 링크를 찾아보세요.

![PR check for deployed PR](/design-systems-for-developers/avatarlist-github-pr-checks-chromatic-changes.png)

<!-- Find the `AvatarList` in your published Storybook. It should look identical to your local Storybook. -->
배포된 Storybook에서 `AvatarList`를 찾아보세요. 로컬 Storybook과 동일하게 보여야 합니다.

![AvatarList in Storybook online](/design-systems-for-developers/netlify-deployed-avatarlist-stories.png)

<!-- The published Storybook is a universal reference point for the team. Share the link to `AvatarList` with other stakeholders to get feedback faster. Your team will love you because they don’t have to deal with code or set up a development environment. -->
배포된 Storybook은 팀이 함께 공유하는 공통의 레퍼런스입니다. 피드백을 빨리 받아보기 위해 다른 관계자들에게 `AvatarList` 링크를 공유해보세요.

![Looks good, ship it!](/design-systems-for-developers/visual-review-shipit.png)

<!-- Reaching consensus with numerous teams often feels like an exercise in futility. Folks reference out-of-date code, don’t have a development environment or scatter feedback across multiple tools. Reviewing Storybook online makes it as simple as sharing a URL. -->
수 많은 팀들과 합의를 하는 과정이 무의미하게 느껴질 수도 있습니다. 오래된 코드의 레퍼런스를 참고하거나, 적절한 개발 환경을 갖지 못하거나, 피드백이 여러 툴에 흩어져있을 수도 있습니다. 하지만 온라인 Storybook 리뷰하는 것은 URL을 공유하는 것만큼이나 간단합니다.

## 테스트

<!-- Our test suite runs in the background every commit. `AvatarList` is a simple presentational component, so unit tests aren’t necessary. But if we look at the PR check, our visual testing tool Chromatic has already detected changes that need reviewing. -->
테스트는 매 커밋 때마다 눈에 띄지 않는 곳에서 실행됩니다. `AvatarList`은 간단한 프레젠테이션 컴포넌트라서 유닛 테스트는 필수적인 게 아닙니다. 하지만 PR 확인란을 보면 시각적 테스트 툴인 Chromatic이 리뷰가 필요한 수정사항을 이미 감지하고 있습니다.

![Chromatic changes on the GitHub PR check](/design-systems-for-developers/avatarlist-github-pr-checks-chromatic-changes.png)

<!-- Since AvatarList is new, there aren’t visual tests for it yet. We’ll need to add baselines for each story. Accept the “new stories” in Chromatic to expand visual test coverage. -->
AvatarList는 새로운 컴포넌트이기 때문에, 이를 위한 시각적 테스트가 아직 없습니다. 각 스토리마다 최소한의 기준이 필요할 것입니다. 시각적 테스트로 확장하기 위해서 Chromatic에서 "new stories"에 동의합니다.

![Chromatic changes to the AvatarList stories](/design-systems-for-developers/chromatic-avatarlist-changes.png)

<!-- Once you’re done, the build will pass in Chromatic. -->
여기까지 완료가 되면 Chromatic에서 빌드가 통과할 것입니다.

![Chromatic changes to the AvatarList stories accepted](/design-systems-for-developers/chromatic-avatarlist-changes-accepted.png)

<!-- Which, in turn, updates the PR check in GitHub. -->
그런 후에, GitHub에서 PR 확인란이 업데이트 됩니다.

![Chromatic changes accepted on the GitHub PR check](/design-systems-for-developers/avatarlist-github-pr-checks-chromatic-changes-accepted.png)

<!-- The tests were successfully updated. In the future, regressions will have a tough time sneaking into the design system. -->
테스트는 성공적으로 업데이트되었습니다. 나중에 회귀 테스트를 하면 디자인 시스템을 파고드느라 꽤나 힘든 시간을 보낼 것입니다.

## 배포

<!-- We have an open pull request that adds `AvatarList` to the design system. We've written our stories, the tests pass, and documentation exists. At last, we’re ready to update our design system package with Auto and npm. -->
디자인 시스템에 추가한 `AvatarList`의 풀 리퀘스트가 열려있는 상태입니다. 스토리를 작성했고, 테스트도 통과했으며, 문서도 작성했습니다. 마지막으로 Auto와 npm을 사용해서 디자인 시스템 패키지를 업데이트하면 됩니다.

<!-- Add the `minor` label to the PR. This tells Auto to update the minor version of the package on merge. -->
PR에 `minor` 라벨을 추가해보세요. 이렇게 하면 Auto에게 merge할 때 마이너 버전의 패키지를 업데이트하라고 알려줄 수 있습니다.

![GitHub PR with labels](/design-systems-for-developers/github-pr-labelled.png)

<!-- Now merge your PR, navigate to your package on npm, and hang tight for a few minutes while the package is updated. -->
이제 PR을 merge하고, npm에 있는 패키지로 이동하여, 패키지가 업데이트되는 몇 분 간을 기다립니다.

![Package published on npm](/design-systems-for-developers/npm-published-package.png)

<!-- Success! Your design system package was updated from the comfort of GitHub. No need to touch the command line or fuss with npm. Update the `learnstorybook-design-system` dependency in the example app to start using AvatarList. -->
성공했습니다! 디자인 시스템 패키지가 GitHub에 업데이트되었습니다. 커맨드 라인을 실행하거나 npm을 연결할 필요가 없습니다. AvatarList를 사용하려면 `learnstorybook-design-system` dependency를 예제 앱에 업데이트해보세요.

## 여정의 시작

<!-- _Design Systems for Developers_ highlights the end-to-end workflow used by professional frontend teams to give you a headstart as you develop your own. As your design system grows, add, rearrange, and extend these tools to fit your team’s needs. -->
_개발자를 위한 디자인 시스템_ 은 전문적인 프론트엔드 팀에서 사용하는 end-to-end 작업 흐름(workflow)을 강조하여 자체 개발 과정에서 유리한 출발점을 제공합니다. 디자인 시스템이 커질 때, 팀의 요구에 맞게 추가하고 재배열하고 확장하도록 합니다.

<!-- Chapter 9 concludes with the complete sample code, helpful resources, and frequently asked questions from developers. -->
챕터 9에서는 완성된 샘플 코드, 유용한 자료들 그리고 개발자들이 흔히하는 질문들로 마무리하겠습니다.
