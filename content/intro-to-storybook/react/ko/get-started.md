
   
---
title: 'React를 위한 Storybook 튜토리얼'
tocTitle: '시작하기'
description: 'Storybook을 개발 환경에 설치해보세요'
---

Storybook은 개발 모드에서 앱과 함께 실행됩니다. Storybook은 비즈니스 로직과 맥락(context)으로부터 분리된 UI 컴포넌트를 만들 수 있도록 도와줍니다. 현재 문서는 React를 위해 작성되었습니다. 그 밖의 [React Native](/intro-to-storybook/react-native/en/get-started), [Vue](/intro-to-storybook/vue/en/get-started), [Angular](/intro-to-storybook/angular/en/get-started) [Svelte](/intro-to-storybook/svelte/en/get-started) 및 [Ember](/intro-to-storybook/ember/en/get-started)에 대한 다른 문서도 존재합니다.

![Storybook과 여러분의 앱](https://storybook.js.org/tutorials/intro-to-storybook/storybook-relationship.jpg)

## React Storybook 설정

우리의 개발 환경에 빌드 프로세스를 설정하려면 몇 가지 단계를 따라야 합니다. 우선 우리는 [degit](https://github.com/Rich-Harris/degit)을 사용하여 빌드 시스템을 설정하려 합니다. 이 패키지를 사용해 "템플릿"(일부 기본 구성으로 부분적으로 구축된 애플리케이션)을 다운로드해 개발 흐름을 빠르게 파악할 수 있습니다.

다음 명령을 실행해봅시다.

```bash
# 템플릿 복제
npx degit chromaui/intro-storybook-react-template taskbox

cd taskbox

# 의존성(종속성) 설치
yarn
```

```
💡 템플릿에는 이 튜토리얼 버전에 필요한 스타일, assets 및 기본 구성이 포함되어 있습니다.
```

이제 다양한 환경에서 애플리케이션이 올바르게 작동하는지 아래 명령어를 통해 빠르게 확인할 수 있습니다:

```bash
# 터미널에서 테스트 러너 (Jest)를 실행합니다:
yarn test --watchAll

# 6066 포트에서 구성 요소 탐색기를 시작합니다:
yarn storybook

# 3000 포트에서 프론트엔드 앱을 실행합니다:
yarn start
```

<div class="aside"> 
💡 <code>--watchAll</code> 명령어를 포함하여 모든 테스트가 실행되도록 합니다. 이 튜토리얼을 진행하는 동안 다양한 테스트 시나리오가 소개될 것 입니다. 이에 따라 <code>package.json</code>의 scripts를 수정하는 것을 고려해 보는 것이 좋을 것 입니다.
</div>

프론트엔드 앱의 세 가지 양식: 자동화된 테스트(Jest), 컴포넌트 개발(Storybook), 앱 자체 개발이 있습니다.

![3가지 양식](https://storybook.js.org/tutorials/intro-to-storybook/app-three-modalities.png)

당신이 앱(응용 프로그램)의 어떤 부분에 따라 작업하고 있는지에 따라, 이것들 중 하나 이상을 동시에 실행하고 싶을 수 있습니다. 하지만 지금 우리의 초점은 단일 UI 구성 요소를 만드는 것이므로 Storybook을 실행하는데 집중할 것 입니다.


## 변경 사항 커밋
이 단계에서는 파일을 로컬 저장소(repository)에 추가하는 것이 안전합니다. 다음 명령을 실행하여 로컬 저장소를 초기화하고 지금까지 수행하 변경 사항(내용)을 추가, 커밋합니다.

```shell
$ git init
```

뒤이어:
```shell
$ git add .
```

그 다음에:
```shell
$ git commit -m "first commit"
```

그리고 마지막으로:
```shell
$ git branch -M main
```

그럼 이제 우리의 첫 번째 컴포넌트 만들어봅시다!