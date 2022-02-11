---
title: '컴포넌트 탐색기'
tocTitle: '컴포넌트 탐색기'
description: 'UI 개발과 시각적 테스팅을 위한 도구'
commit: ''
---

현대의 UI는 셀 수 없이 많은 상태, 언어, 디바이스, 브라우저 그리고 사용자 데이터를 지원합니다. 과거에, UI를 개발하는 일은 다소 번거로운 일이었습니다. 적합한 설정값과 디바이스를 이용해서 주어진 페이지를 다뤄야만 했습니다. 그런 다음 페이지에 도달하기 위해 주변을 클릭하고 정확한 상태값을 가져야만 코딩을 시작할 수 있었습니다.

You build UI components in isolation to focus on each component's supported variations. That allows you to gauge how inputs (props, state) affect the rendered UI and forms the basis of your visual test suite.

**컴포넌트 탐색기는 비즈니스 로직과 애플리케이션 context로부터 UI문제를 분리합니다.** 각 컴포넌트를 기반으로 하는 변화에 초점을 맞추기 위해 UI 컴포넌트를 별도로 빌드합니다. 이렇게 함으로써 입력값(props, state 등)이 렌더링된 UI에 어떻게 영향을 미치고 시각적 test suit의 기초를 형성하는지 판단할 수 있습니다.

Storybook is the industry-standard component explorer that we'll use to demonstrate visual testing. It's adopted by Twitter, Slack, Airbnb, Shopify, Stripe, and thousands of other companies so you can apply the learnings in this guide wherever you end up working.

[Storybook](https://storybook.js.org/)은 시각적 테스팅을 시연하는데 사용할 수 있는 업계 표준 컴포넌트 탐색기입니다. Slack, Airbnb, Shopify, Stripe와 수 천개의 회사에서 채택하고 있어서 앞으로 어디에서 일하든 이 안내서의 학습 내용을 적용할 수 있습니다.

<video autoPlay muted playsInline loop>
  <source
    src="/visual-testing-handbook/storybook-component-explorer-visual-testing.mp4"
    type="video/mp4"/>
</video>

## UI를 분리해서 빌드하는 이유는 무엇일까요?

### 버그(bug) 줄이기

컴포넌트와 상태가 많아질수록, 사용자의 디바이스와 브라우저에서 올바르게 렌더링되는지 확인하는 건 더욱 어려워집니다.

컴포넌트 탐색기는 컴포넌트에 기반하는 변경사항을 화면에 표시해 일관성을 해치는 일을 방지합니다. 이를 통해 개발자는 각각의 state와 관계없이 집중할 수 있습니다. 개별적으로 테스트를 진행할 수 있으며, 모의(mocking)를 통해 복잡한 edge case를 복제할 수 있습니다.

![컴포넌트 테스트 케이스](/visual-testing-handbook/component-test-cases.png)

### 빨라진 개발

애플리케이션 개발은 끝나지 않습니다. 계속해서 반복해야 합니다. 그렇기 때문에 UI 아키텍처는 새로운 기능을 수용할 수 있어야 합니다. 컴포넌트 모델은 UI를 애플리케이션 비즈니스 로직과 백앤드에서 분리해 호환성이 좋습니다.

컴포넌트 탐색기는 sandbox를 제공하고 애플리케이션과 분리된 상황에서 UI를 개발함으로써 구분을 명확하게 합니다. 즉 애플리케이션의 다른 부분에서 방해를 받거나 state가 오염되는 일 없이 각자 다른 UI를 동시에 작업할 수 있습니다. 

### 더 쉬워진 협업

UI의 본질은 시각적이라는 것 입니다. Code로만 이루어진 pull request는 불완전한 작업의 대표주자 입니다. 진정한 협업을 하고싶다면 이해관계자가 UI를 살펴보아야 합니다.

컴포넌트 탐색기는 UI 컴포넌트와 컴포넌트의 모든 변경사항을 시각화합니다. 이는 "이게 맞나요?"라는 질문에 대해 개발자, 디자이너, PM, QA로부터 피드백을 쉽게 얻을 수 있습니다.

<video autoPlay muted playsInline loop>
  <source
    src="/visual-testing-handbook/storybook-workflow-publish.mp4"
    type="video/mp4"/>
</video>

## 내가 쓰고있는 기술 스택 어디에 적합하나요?

컴포넌트 탐색기는 애플리케이션과 함께 작은 독립된 sandbox로서 묶여있습니다. 이는 컴포넌트 변경사항을 개별적으로 시각화하며, 아래의 기능을 포함하고 있습니다.

- 🧱 컴포넌트 격리를 위한 sandbox입니다
- 🔭 컴포넌트 사양 및 속성에 대한 변경사항 시각화 도구입니다
- 🧩 테스트 중에 다시 논의할 수 있도록 "story"로 변경사항을 저장합니다
- 📑 컴포넌트 검색 및 사용 지침에 관한 문서입니다.

![컴포넌트와 컴포넌트 탐색기간의 관계](/visual-testing-handbook/storybook-relationship.png)

## 작업흐름(workflow) 알아보기

컴포넌트 탐색기로 UI를 분리하면 시각적 테스트를 진행할 수 있습니다. 다음 장에서는 테스트 주도 개발(Test-driven development)을 조합하는 방법을 보여줄 것입니다.