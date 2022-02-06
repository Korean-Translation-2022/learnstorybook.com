---
title: 'addons에 대한 소개'
tocTitle: '소개'
description: 'addon의 구조'
---

<div class="aside">본 가이드는 스토리북 addons를 구축하고자 하는 <b>프로페셔널한 개발자</b>들을 위해 작성되었다. 중급 이상의 자바스크립트나 리액트에 대한 경험을 요한다. 스토리북에 대한 기본 역시 알아야 하는데, 예를 들면 스토리를 쓴다거나 config를 쓰는 방법 등을 말한다. (<a href="/intro-to-storybook">Intro to Storybook</a> 에서 기본을 가르치고 있다).
</div>

<br/>

스토리북은 앱 외부에서 분리된 공간에서의 UI 컴포넌트 개발을 위한 도구이다.
Addons는 워크플로우 부분들을 자동화하고 향상시키는 것을 도와준다. 실제로, 스토리북의 중요한 특징들은 addons와 만들어져있다. 예를 들면 :[문서 Documentation](https://storybook.js.org/docs/react/writing-docs/introduction), [접근성 테스트 Accessibility testing](https://storybook.js.org/addons/@storybook/addon-a11y) and [상호작용 통제 Interactive controls](https://storybook.js.org/docs/react/essentials/controls) 등이 있다. 200여 개의 addons가[over 200](https://storybook.js.org/addons) UI 개발자들의 시간 절약을 돕고 있다.

## 무엇을 만들 것인가?

당신의 CSS 레이아웃이 디자인과 맞는 지에 대해 확언하는 것은 어렵다. Eyeballing alignment은 미묘할 수 있다. DOM elements가 멀리 떨어져 있거나 이상한 모양을 가지고 있으면 말이다.

[Outline addon](https://storybook.js.org/addons/storybook-addon-outline) Outline addons는 CSS를 이용해서 UI 엘리먼트의 아웃라인을 잡는 툴바 버튼을 더한다. 이것은 포지셔닝이나 배치에 대한 것들을 한 눈에 쉽게 한다. 아래 예제를 살펴 보자.

![Outline Addon](../../images/outline-addon-hero.gif)

## Addon의 구조

Addons는 스토리북으로 가능한 것들을 더욱 확장하게 도와준다. 인터페이스에 있는 것들부터 API까지 모두 말이다. 이것들은 UI 엘리먼트 개발 워크플로우를 크게 돕는다.

Addons는 두 가지의 넓은 카테고리로 나뉜다:

- **UI-based:** 인터페이스를 커스터마이징하고, 반복적인 일이나 포맷, 추가적인 정보 배치에 단축키를 더한다. 예를 들면 문서화, 접근성 테스트, 상호작용통제, 그리고 디자인 미리보기 등이 있다.

- **Presets:** 스토리북에 자동적으로 적용된 환경설정의 모음이다. 이것들은 주로 스토리북을 다른 특정 기술과 함께 사용하도록 빠르게 짝지어준다. 예를 들면, preset-create-react-app, preset-nuxt 그리고 preset-scss 이 있다.

## UI 기반의(UI-based) addons

Addons는 세가지 타입의 인터페이스 엘리멘트를 만들 수 있다:

1. 툴바에 툴을 더할 수 있다. 예를 들어 [Grid and Background](https://storybook.js.org/docs/react/essentials/backgrounds) 도구가 있다.

![](../../images/toolbar.png)

2. [Actions addon](https://storybook.js.org/docs/react/essentials/actions) 을 닮은 addon 판넬을 만들 수 있다. 현황 일지를 배치한다.

![](../../images/panel.png)

3. 새로운 탭을 만든다. [Storybook Docs](https://storybook.js.org/docs/react/writing-docs/introduction) 컴포넌트 문서화를 표시한다.

![](../../images/tab.png)

Addons가 많은 것을 할 수 있다는 것은 확실하다. 그래서 대체 addons는 뭘 하는가?

The Outline addon은 개발자로 하여금 스토리에 있는 각 엘리먼트의 아웃라인을 그릴 수 있는 툴바의 버튼을 클릭하게 도와준다. 버튼을 한 번 더 누르면, 아웃라인들은 이제 지워진다.

Addon code는 앞으로 우리가 다룰 챕터에서 네 가지 부분으로 나뉘어 설명된다.:

- **Addon UI** 툴바에 “tool” 버튼을 만든다. 이 버튼이 사용자가 클릭하는 것이다.
- **Registration** 스토리북에 addon을 등록한다.
- **State management** 툴의 토글 상태를 확인한다. 이것으로 아웃라인을 시각적으로 보이게 하느냐 마느냐를 통제할 수 있다.
- **Decorator** CSS를 미리보기 iframe에서 주입하여 아웃라인을 그린다.
