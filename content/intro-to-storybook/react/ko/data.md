---
title: '데이터 연결하기'
tocTitle: '데이터'
description: 'UI 컴포넌트에 데이터를 연결하는 방법을 배워봅시다'
commit: 'b29407b'
---

지금까지 우리는 독립된 환경에서 상태를 가지지 않는(stateless) 컴포넌트를 만들어보았습니다. 이는 Storybook에는 적합하지만 앱에 데이터를 제공하기 전까지는 유용하지 않습니다.

이번 튜토리얼에서는 앱 제작의 세부 사항에 중점을 두지 않기 때문에 자세히 설명하지 않을 것입니다. 그보다 컨테이너 컴포넌트(container components)에 데이터를 연결하는 일반적인 패턴을 살펴보도록 하겠습니다.

## 컨테이너 컴포넌트

현재 작성된 `TaskList`는 구현됨에 있어서 외부와 어떠한 소통도 하지 않기 때문에 “표상적(presentational)”이라고 할 수 있습니다. ([이 블로그 포스트](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)를 참조해 주세요. 데이터를 얻기 위해서는 “컨테이너(container)”가 필요합니다.)

이 예제는 [Redux](https://redux.js.org/)로 데이터를 저장하기 위해 가장 효과적인 도구 집합(toolset)인 [Redux Toolkit](https://redux-toolkit.js.org/)를 사용하여 앱의 간단한 데이터 모델을 만듭니다. 여기서 사용된 패턴은 [Apollo](https://www.apollographql.com/client/)와 [MobX](https://mobx.js.org/) 같은 다른 데이터 관리 라이브러리에도 적용됩니다.

프로젝트에 필수 dependency를 다음과 같이 설치해 주세요.

```bash
yarn add @reduxjs/toolkit react-redux
```

먼저 `src/lib` 폴더의 `store.js` 파일 (의도적으로 단순하게 작성함)에서 task의 state를 변경하는 동작에 대응하는 간단한 Redux 저장소를 구성해 보겠습니다.

```js:title=src/lib/store.js
/* 간단한 redux store/actions/reducer 구현.
 * 실제 앱은 훨씬 더 복잡하고 파일들이 분리되어 있습니다.
 */
import { configureStore, createSlice } from '@reduxjs/toolkit';

/*
 * 앱이 로드될 때의 저장소 초기 상태
 * 보통은 서버로부터 데이터를 가져옵니다.
 */
const defaultTasks = [
  { id: '1', title: 'Something', state: 'TASK_INBOX' },
  { id: '2', title: 'Something more', state: 'TASK_INBOX' },
  { id: '3', title: 'Something else', state: 'TASK_INBOX' },
  { id: '4', title: 'Something again', state: 'TASK_INBOX' },
];

/*
 * 여기서 저장소는 만들어집니다.
 * 'slice'의 자세한 정보는 아래 문서에서 확인할 수 있습니다.
 * https://redux-toolkit.js.org/api/createSlice
 */
const TasksSlice = createSlice({
  name: 'tasks',
  initialState: defaultTasks,
  reducers: {
    updateTaskState: (state, action) => {
      const { id, newTaskState } = action.payload;
      const task = state.findIndex(task => task.id === id);
      if (task >= 0) {
        state[task].state = newTaskState;
      }
    },
  },
});

// slice 속 포함된 액션 사용을 위해 컴포넌트로부터 내보내집니다. 
export const { updateTaskState } = TasksSlice.actions;

/*
 * 앱의 저장소 환경설정은 다음과 같습니다.
 * 리덕스의 configureStore 의 자세한 정보는 아래 문서에서 확인할 수 있습니다.
 * https://redux-toolkit.js.org/api/configureStore
 */
const store = configureStore({
  reducer: {
    tasks: TasksSlice.reducer,
  },
});

export default store;
```

다음 `TaskList` 컴포넌트를 Redux store와 연결하고, 알고자 하는 task들을 렌더링 하기 위해 업데이트합니다.

```js:title=src/components/TaskList.js
import React from 'react';
import PropTypes from 'prop-types';

import Task from './Task';

import { useDispatch, useSelector } from 'react-redux';
import { updateTaskState } from '../lib/store';

export function PureTaskList({ loading, tasks, onPinTask, onArchiveTask }) {
  /* TaskList의 이전 구현 */
}

PureTaskList.propTypes = {
  /** loading 상태인지 확인하는 데이터 */
  loading: PropTypes.bool,
  /** 작업 목록 데이터 */
  tasks: PropTypes.arrayOf(Task.propTypes.task).isRequired,
  /** 작업을 고정으로 변경하는 이벤트 */
  onPinTask: PropTypes.func.isRequired,
  /** 작업 아카이빙을 위한 이벤트 */
  onArchiveTask: PropTypes.func.isRequired,
};

PureTaskList.defaultProps = {
  loading: false,
};

export function TaskList() {
  // store로부터 상태(state)를 불러옵니다.
  const tasks = useSelector(state => state.tasks);
  // 액션(actions)들을 store로 dispatch 하는 변수를 정의합니다.
  const dispatch = useDispatch();

  const pinTask = value => {
    // 고정된 이벤트들을(Pinned event) store로 dispatch 합니다.
    dispatch(updateTaskState({ id: value, newTaskState: 'TASK_PINNED' }));
  };
  const archiveTask = value => {
    // 아카이브 이벤트들을(Archive event) store로 dispatch 합니다.
    dispatch(updateTaskState({ id: value, newTaskState: 'TASK_ARCHIVED' }));
  };

  const filteredTasks = tasks.filter(t => t.state === 'TASK_INBOX' || t.state === 'TASK_PINNED');
  return (
    <PureTaskList
      tasks={filteredTasks}
      onPinTask={task => pinTask(task)}
      onArchiveTask={task => archiveTask(task)}
    />
  );
}
```

이제 Redux에서 받은 실제 데이터로 생성된 컴포넌트가 있으므로, 이를 `src/app.js`에 연결하여 컴포넌트를 렌더링 할 수 있습니다. 그러나 지금은 먼저 컴포넌트 중심의 여정을 계속해나가도록 하겠습니다.

그에 대한 내용은 다음 챕터에서 다룰 것이므로 걱정하지 않으셔도 됩니다.

이 단계에서 `TaskList`는 컨테이너이며 더 이상 어떠한 props도 받지 않기 때문에 Storybook 테스트는 작동을 멈추었을 것입니다. 대신 `TaskList`는 Redux store에 연결하고 이를 감싸는 `PureTaskList`에서 props를 설정합니다.

하지만 이전 단계에서 진행한 Storybook 스토리의 `export` 구문에 `PureTaskList`(표상적인 컴포넌트)를 간단하게 렌더링함으로써 이러한 문제를 쉽게 해결할 수 있습니다.

```diff:title=src/components/TaskList.stories.js
import React from 'react';

+ import { PureTaskList } from './TaskList';
import * as TaskStories from './Task.stories';

export default {
+ component: PureTaskList,
  title: 'TaskList',
  decorators: [story => <div style={{ padding: '3rem' }}>{story()}</div>],
};

+ const Template = args => <PureTaskList {...args} />;

export const Default = Template.bind({});
Default.args = {
  // args 컴포지션을 통해 스토리를 구성합니다.
  // 이 데이터는 task.stories.js의 Default 스토리를 상속받았습니다.
  tasks: [
    { ...TaskStories.Default.args.task, id: '1', title: 'Task 1' },
    { ...TaskStories.Default.args.task, id: '2', title: 'Task 2' },
    { ...TaskStories.Default.args.task, id: '3', title: 'Task 3' },
    { ...TaskStories.Default.args.task, id: '4', title: 'Task 4' },
    { ...TaskStories.Default.args.task, id: '5', title: 'Task 5' },
    { ...TaskStories.Default.args.task, id: '6', title: 'Task 6' },
  ],
};

export const WithPinnedTasks = Template.bind({});
WithPinnedTasks.args = {
  // args 컴포지션을 통해 스토리를 구성합니다.
  // 위의 Default 스토리에서 상속받은 데이터입니다.
  tasks: [
    ...Default.args.tasks.slice(0, 5),
    { id: '6', title: 'Task 6 (pinned)', state: 'TASK_PINNED' },
  ],
};

export const Loading = Template.bind({});
Loading.args = {
  tasks: [],
  loading: true,
};

export const Empty = Template.bind({});
Empty.args = {
  // args 컴포지션을 통해 스토리를 구성합니다.
  // 위의 Loading 스토리에서 상속받은 데이터입니다.
  ...Loading.args,
  loading: false,
};
```

<video autoPlay muted playsInline loop>
  <source
    src="/intro-to-storybook/finished-tasklist-states.mp4"
    type="video/mp4"
  />
</video>

<div class="aside">
💡 변경과 함께 모든 테스트들은 업데이트를 필요로 합니다. <code>-u</code> 플래그와 함께 import 문을 업데이트하고 테스트 커맨드를 재실행하세요. 깃에 변경한 내역들을 커밋 하는 것도 잊지 마세요!
</div>
