# Overview
## 목적
- data fetching
- data caching
- 서버상태와의 동기화 및 업데이트

<br>

## Motivation 
- 데이터 `caching`
- 동일 데이터에 대한 중복 요청의 `단일 요청 처리 `
- `백그라운드`영역에서의 데이터 업데이트
- 데이터 업데이트를 최대한 빠르게 반영하기 `(최신데이터 반영하기)`
- 데이터의 `오래된 시점(out of time)` 알기
- lazy 로딩 및 페이지네이션과 같은 `성능최적화`
- 서버상태의 메모리 및 가비지 컬렉션 관리
- 쿼리데이터를 기억하고 구조적으로 공유하기

<br>

# 설치(latest : v4)
- `npm i @tanstack/react-query`
## ESLint 플러그인 쿼리 설치(Recommendations)
- https://tanstack.com/query/latest/docs/react/eslint/eslint-plugin-query

<br>

# Quick Start 
- Queries(데이터요청:get)
- Mutation(서버데이터 변형요청:post,update)
- Query무효화 

```jsx

import {
  useQuery,
  useMutation,
  useQueryClient,
  QueryClient,
  QueryClientProvider,
} from '@tanstack/react-query'
import { getTodos, postTodo } from '../my-api'

// Create a client
const queryClient = new QueryClient()

function App() {
  return (
    // Provide the client to your App
    <QueryClientProvider client={queryClient}>
      <Todos />
    </QueryClientProvider>
  )
}

function Todos() {
  // Access the client
  const queryClient = useQueryClient()

  // Queries
  const query = useQuery({ queryKey: ['todos'], queryFn: getTodos })

  // Mutations
  const mutation = useMutation({
    mutationFn: postTodo,
    onSuccess: () => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['todos'] })
    },
  })

  return (
    <div>
      <ul>
        {query.data?.map((todo) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>

      <button
        onClick={() => {
          mutation.mutate({
            id: Date.now(),
            title: 'Do Laundry',
          })
        }}
      >
        Add Todo
      </button>
    </div>
  )
}

render(<App />, document.getElementById('root'))

```
1. new QueryClient()로 queryClient 객체 생성
2. 적용하려는 앱(Todos)을 `<QueryClientProvider client={queryClient}>`로 감싸주면 끝
3. 이제 Todos앱에서 React-query 사용가능

<br>

# Guides & Concepts

<br>
<br>
<br>

## useQuery Hook
1. `(queryKey:)` 쿼리키 
2. `(queryFn:)` Promise(resolve된 데이터 or  Error)를 리턴하는 fetch함수 
```jsx
import { useQuery } from '@tanstack/react-query'

function App() {
  const info = useQuery({ queryKey: ['todos'], queryFn: fetchTodoList })
}
// useQuery('todos',fetchTodoList) 형태의 축약 호출도 가능
```
- 제공한 고유 `쿼리키`는 app에서 전역적으로 쿼리를 다시 가져오고, 캐싱하고, 공유하기 위해 내부적으로 사용됨.

<br>
<br>
<br>

## React Query훅을 사용하는 두 가지 방법
1. 객체 리터럴 형식: (`하나의 객체에 key-value 형태로 전달`)
```jsx
const result = useQuery({
  queryKey: 'post',
  queryFn: getPostFunction,
  staleTime: 3000, //number type (리페치 여부 판단시간, 지나면 리페치)
  cacheTime: undefined, //디폴트값은 undefined로 전달 (캐시유지시간, 지나면 리로드)
});
```
2. 인라인 속성: (`파라미터 1,2,3 형태로 전달`)
```jsx
const result = useQuery('post', getPostFunction, {
  staleTime: 3000,
  cacheTime: undefined,
});
```

<br>
<br>
<br>

## StaleTime vs CacheTime
- React Query의 staleTime과 cacheTime은 데이터의 갱신 및 캐싱 동작을 제어하는 데 사용되는 옵션입니다.

- `staleTime`(신선도 유지 시간): 이 옵션은 데이터가 "stale" 상태로 간주되는 시간을 지정합니다. "stale" 상태란 이전에 캐시된 데이터가 아직 유효하다고 판단되는 상태를 말합니다. 즉, `staleTime 이후에는 데이터를 갱신해야 함을 나타냅니다. `기본값은 0이며, 이는 항상 데이터를 갱신하도록 설정됩니다. 만약 staleTime을 양수 값으로 설정한다면, 해당 시간 동안은 이전에 `캐시된 데이터를 사용하여 UI를 렌더링하고,` 그 이후에만 새로운 데이터를 가져올 수 있습니다.

- `cacheTime`(캐시 유지 시간): 이 옵션은 데이터가 `캐시에 유지되는 시간` 을 지정합니다. 데이터가 가져와진 후, cacheTime 동안에는 캐시된 데이터를 사용하여 UI를 렌더링하고, 그 이후에만 새로운 데이터를 다시 가져옵니다. 기본값은 5분(300,000 밀리초)이며, 이 기간 동안은 동일한 데이터를 다시 가져오지 않습니다. cacheTime을 Infinity로 설정하면 데이터를 영구적으로 캐시하게 됩니다.

- 즉, staleTime은 데이터가 갱신되어야 하는 시간 간격을 정의하고, cacheTime은 데이터가 캐시에 유지되는 시간 간격을 정의합니다. 이 두 옵션을 조합하여 적절한 데이터 캐싱 및 갱신 전략을 구성할 수 있습니다.

<br>
<br>
<br>

__useQuery훅이 반환한 `result객체는` 여러 중요한 상태값을 포함하는 객체임__
```jsx
const result = useQuery({ queryKey: ['todos'], queryFn: fetchTodoList })
```
- 쿼리는 다음의 상태중 하나만 존재할 수 있음
- `isLoading` or `status === 'loading'`: 쿼리 요청 데이터가 아직 없을 때
- `isError` or `status === 'error'`: 쿼리요청중 에러 발생시
- `isSuccess` or`status === success'`: 쿼리가 성공적으로 데이터를 받아왔을 경우
- 쿼리의 특정 상태에 따라 사용할 수 있는 property들
- `error`: 에러 발생시 에러는 error프로퍼티로 접근가능
- `data`: 데이터를 성공적으로 받아온 경우 data프로퍼티를 사용할 수 있음

<br>

__대부분의 쿼리 의 경우 일반적으로 isLoading상태를 확인한 다음 isError상태를 확인한 다음 마지막으로 데이터를 사용할 수 있다고 가정하고 성공적인 상태를 렌더링하는 것으로 충분합니다 .__
```jsx
nction Todos() {
  const { isLoading, isError, data, error } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodoList,
  })

  if (isLoading) {
    return <span>Loading...</span>
  }

  if (isError) {
    return <span>Error: {error.message}</span>
  }

  // We can assume by this point that `isSuccess === true`
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```
1. isLoading에 따른 return할 jsx (데이터를 처음 가져오는 중..)
2. isError에 따른 return할 jsx (에러발생)
3. data를 받아온 후 return할 jsx (fetch성공시)
>`TypeScript will also narrow the type of data correctly if you've checked for loading and error before accessing it.(loading과 error를 체크한다면 타입스크립트는 data접근 전 자동으로 타입을 내로잉해줄 것임.)`


<br>

## isLoading(데이터의 상태), isFetching(쿼리 요청 상태) 프로퍼티의 차이 

__FetchStatus__
- result객체는 `status`프로퍼티 외에도 `fetchStatus`의 프로퍼티도 얻을 수 있습니다.
1.`fetchStatus === 'fetching'`- 쿼리가 현재 가져오는 중입니다.
2.`fetchStatus === 'paused'`- 쿼리를 가져오려고 했지만 일시 중지되었습니다. 이에 대한 자세한 내용은 네트워크 모드 가이드를 참조하십시오.
3.`fetchStatus === 'idle'`- 쿼리가 현재 아무 작업도 수행하지 않습니다.

<br>

__Why two different states?__ (status, fetchStatus?)
`idle(fetchStatus, 아무동작x)`상태이더라도 백그라운드에서 데이터를 가져오는 중이라면 `success(status)`일 수 있다. (이 말은 캐시된 데이터:success를 가져온 경우 idle일 수 있음을 의미)

1. isLoading은 `데이터의 상태(status: loading,error,success)`를 나타내는 프로퍼티
- 초기에 서버에서 페칭시 true(status: loading) -> false(status: success)
2. isFetching은 데이터페칭시 `쿼리의 요청상태(fetchStatus: fetching,paused,idle)` true(fetchStatus:fetcing) ->false(fetchStatus: idle)
- 즉 `isLoading값은 데이터의 상태, isFetching은 쿼리의 요청상태`로 이해하자 .
- __백그라운드에 캐싱된 데이터를 가져오는경우 isLoading은 데이터가 이미 존재하므로 false이지만, 쿼리요청 후 백그라운드에서 가져오는 요청중의 상태인 isFetching은 true가 될 수 있다.__

> 쿼리를 맨 처음 요청하는경우 isLoading(true), 이전에 요청한 쿼리가 캐시되어있고 다시 요청하는경우 isLoading(false),isFetching(true)
[isLoading,isFetching]의 차이는 status와 fetchStatus로 분리된 두 개념을 통해 이해해야한다.

<br>
<br>

## QueryKey
1. 리액트는 쿼리키를 기반으로 쿼리를 캐싱
2. 쿼리키는 배열로 오버라이딩
3. 계층적구조로 이루어짐 

<br>

__쿼리키는 객체키의 순서가 아닌 배열 항목의 순서로 구분된다.__
```jsx

//모두 동일한 쿼리의 예시:
useQuery({ queryKey: ['todos', { status, page }], ... })
useQuery({ queryKey: ['todos', { page, status }], ...})
useQuery({ queryKey: ['todos', { page, status, other: undefined }], ... })

//모두 동일하지 않은 쿼리의 예시:
useQuery({ queryKey: ['todos', status, page], ... })
useQuery({ queryKey: ['todos', page, status], ...})
useQuery({ queryKey: ['todos', undefined, page, status], ...})
```

__쿼리키에 의존적인 쿼리함수 만들기__
```jsx
function Todos({ todoId }) {
  const result = useQuery({
    queryKey: ['todos', todoId],
    queryFn: () => fetchTodoById(todoId),
  })
}
```
- 쿼리키는 fetch하는 데이터를 유니크하게 표현하므로 데이터가 변하는 값이라면 쿼리함수에 `변수`를 전달하여야함
- queryKey : data  `(1 : 1 match)`

<br>

## Query Functions
- 쿼리함수는 Promise를 리턴하는 그 어떤 함수여도 된다.
  - (단, Resolve data혹은 error를 throw하는 경우의 함수여야함 )
```jsx
useQuery({ queryKey: ['todos'], queryFn: fetchAllTodos })
useQuery({ queryKey: ['todos', todoId], queryFn: () => fetchTodoById(todoId) })
useQuery({
  queryKey: ['todos', todoId],
  queryFn: async () => {
    const data = await fetchTodoById(todoId)
    return data
  },
})
useQuery({
  queryKey: ['todos', todoId],
  queryFn: ({ queryKey }) => fetchTodoById(queryKey[1]),
})
```

<br>

### Handling and Throwing Errors
- tanstack쿼리가 에러를 확인하는 경우 `쿼리함수`는 반드시 `error를 throw`하거나 `rejected Promise`해야함.
```jsx
const { error } = useQuery({
  queryKey: ['todos', todoId],
  queryFn: async () => {
    if (somethingGoesWrong) {
      throw new Error('Oh no!')
    }
    if (somethingElseGoesWrong) {
      return Promise.reject(new Error('Oh no!'))
    }

    return data
  },
})
```
- 에러를 맞이한 쿼리함수는 반드시 에러를 throw하거나, rejected Promise를 리턴해야함.
- `status가 error 혹은 isError = true 인 경우` error프로퍼티 활용가능함.

<br>

### Query Function Variables
- Query Function은 `QueryKey`로부터 변수를 추출해낼 수 있다.
```jsx
function Todos({ status, page }) {
  const result = useQuery({
    queryKey: ['todos', { status, page }],
    queryFn: fetchTodoList,
  })
}

// Access the key, status and page variables in your query function!
function fetchTodoList({ queryKey }) {
  const [_key, { status, page }] = queryKey
  return new Promise()
}
```
- `queryKey`변수를 쿼리함수에 전달하는경우 쿼리의 Key를 변수로 구조분해할당하여 사용할 수 있음.

<br>

### cf. QueryFunctionContext
1. `queryKey`변수는 쿼리펑션에서 활용시 쿼리키를 담고있다.
2. `pageParam?`변수는 `Infinite Queries`사용시 쿼리펑션에서 사용가능한 변수
3. `signal?`변수는 Query Cancellation에서 사용가능

<br>

## Parallel Queries(병렬쿼리) : 여러 쿼리 동시에 처리하기 
- react suspense에서는 병렬쿼리가 동작하지 않으므로 주의.
- 만약 렌더링마다 실행해야하는 쿼리 수가 바뀐다면 이는 `리액트 훅의 원칙을 위배`합니다.
  - 이 경우 useQueries를 통해 원하는 수의 쿼리를 동적으로 실행할 수 있습니다.
  - `useQueries는 배열 형태의 쿼리 구성 객체를 받아들여, 각 쿼리를 독립적으로 실행합니다.`
  - { queryKey: , queryFn: } 으로 구성된 쿼리구성객체들의 배열을 `{queries: 구성객체}` 형태의 인자로 전달한다.
```jsx
function App({ users }) {
  const userQueries = useQueries({
    queries: users.map((user) => {
      return {
        queryKey: ['user', user.id],
        queryFn: () => fetchUserById(user.id),
      }
    }),
  })
}
```
- queries: 구성객체배열
  - 구성객체배열: [{ queryKey, queryFn} , ...] : map으로 생성

<br>

## Dependent Query(종속 쿼리)
- 종속쿼리(직렬쿼리)는 하나의 쿼리가 실행되어야 종속적으로 이어서 실행될 수 있는 쿼리를 말합니다.
  - `enabled`옵션을 통해 종속쿼리의 실행을 컨트롤 할 수 있습니다.
```jsx
// Get the user
const { data: user } = useQuery({
  queryKey: ['user', email],
  queryFn: getUserByEmail,
})

const userId = user?.id

// Then get the user's projects
const {
  status,
  fetchStatus,
  data: projects,
} = useQuery({
  queryKey: ['projects', userId],
  queryFn: getProjectsByUser,
  // The query will not execute until the userId exists
  enabled: !!userId, //세번째 파라미터로 enabled를 부여 
})
```
- user데이터가 success하게 받아와진다면, 종속쿼리의 enabled옵션에 해당 데이터를 설정하여
실행을 컨트롤 해준다.

<br>

## Background Fetching Indicators(중요!!)
- 초기 데이터를 hard-loading하는 경우 `status === loading` 상태값은 효과적입니다, 하지만 
백그라운드에서 data를 refetch하는 경우 `isFetching`인디케이터를 status변수에 상관없이 사용할 수 있습니다.

1. 초기데이터 fetch중인 경우.. : isLoading(true), isFetching(true)
2. 백그라운드 데이터 fetch중인 경우.. : isLoading(false:이미 백그라운드에 해당 쿼리 데이터가 존재함을 의미, 쿼리키와 데이터는 1:1매칭), isFetching(true: 백그라운드에서 가져오는 경우도 true로 표시)
```jsx
function Todos() {
  const {
    status,
    data: todos,
    error,
    isFetching,
  } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
  })

  return status === 'loading' ? (
    <span>Loading...</span>
  ) : status === 'error' ? (
    <span>Error: {error.message}</span>
  ) : (
    <>
      {isFetching ? <div>Refreshing...</div> : null}

      <div>
        {todos.map((todo) => (
          <Todo todo={todo} />
        ))}
      </div>
    </>
  )
}
```
- loading중이 아니지만(이미 백그라운드 데이터가있지만) isFetching은 true(백그라운드 데이터는fetching중인 경우)의 예시

<br>

### Displaying Global Background Fetching Loading State
- 전역적으로 그 어느 쿼리라도 fetching중임을 확인하기 위해서는 `useIsFetching`훅을 사용하세요
```jsx
import { useIsFetching } from '@tanstack/react-query'

function GlobalLoadingIndicator() {
  const isFetching = useIsFetching() //fetching중인 모든 쿼리에 대해 감지 

  return isFetching ? (
    <div>Queries are fetching in the background...</div>
  ) : null
}
```

<br>

## Window Focus Refetching
- 유저가 사용중인 App을 떠났다가 다시 돌아오면 자동으로 background refetch를 진행
- `refetchOnWindowFocus`옵션으로 조절할 수 있음.
__전역적으로 설정하기__
```jsx
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      refetchOnWindowFocus: false, // default: true
    },
  },
})
function App() {
  return <QueryClientProvider client={queryClient}>...</QueryClientProvider>
}
```
- 쿼리 클라이언트를 통해 옵션객체를 설정

<br>

__유닛별로 설정하기__
```jsx
useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
  refetchOnWindowFocus: false,
})
```
- useQuery훅의 세번째 옵션으로 넣어주기

<br>

### Custom Window Focus Event (윈도우 포커스 이벤트 커스텀하기)
- 사용자가 다시 윈도우에 Focus시 사용자 정의 함수를 실행시킬 수도 있습니다.
  - `focusManager.setEventListener`함수를 사용하세요
```jsx

focusManager.setEventListener((handleFocus) => {
  // Listen to visibilitychange and focus
  if (typeof window !== 'undefined' && window.addEventListener) {
    window.addEventListener('visibilitychange', handleFocus, false)
    window.addEventListener('focus', handleFocus, false)
  }

  return () => {
    // Be sure to unsubscribe if a new handler is set
    window.removeEventListener('visibilitychange', handleFocus)
    window.removeEventListener('focus', handleFocus)
  }
})
```
1. setEventListener 함수는 하나의 매개변수인 handleFocus 콜백 함수를 받습니다. 이 콜백 함수는 가시성 및 포커스 이벤트가 발생할 때 호출될 것입니다.

2. 첫 번째 부분은 가시성 및 포커스 이벤트를 감지하기 위해 window 객체의 visibilitychange 및 focus 이벤트에 대한 리스너를 추가합니다. window.addEventListener 메서드를 사용하여 이벤트 리스너를 등록합니다. 이벤트 타입은 'visibilitychange'와 'focus'로 설정되며, handleFocus 콜백 함수가 발생한 이벤트에 대해 호출되도록 합니다.

3. 두 번째 부분은 컴포넌트가 언마운트될 때 등록된 이벤트 리스너를 해제하기 위해 window.removeEventListener 메서드를 사용합니다. 이전에 등록된 'visibilitychange' 및 'focus' 이벤트 리스너를 제거합니다. 이렇게 함으로써 메모리 누수와 관련된 문제를 방지할 수 있습니다.

4. 마지막으로, setEventListener 함수는 해제 함수(unsubscribe)를 반환합니다. 이 해제 함수는 새로운 핸들러가 설정되는 경우에 이전 이벤트 리스너를 해제하는 데 사용될 수 있습니다.

5. 이 코드는 가시성 및 포커스 이벤트를 감지하여 해당 이벤트가 발생할 때마다 handleFocus 콜백 함수를 호출하는 기능을 제공합니다. 이를 통해 애플리케이션에서 가시성 및 포커스 상태 변화에 대응할 수 있습니다. 


<br>
<br>

## Disabling/Pausing Queries
- 자동으로 실행되는 쿼리를 불능처리하고 싶다면 `enabled = false`옵션을 사용하세요
```jsx
function Todos() {
  const { isInitialLoading, isError, data, error, refetch, isFetching } =
    useQuery({
      queryKey: ['todos'],
      queryFn: fetchTodoList,
      enabled: false,
    })

  return (
    <div>
      <button onClick={() => refetch()}>Fetch Todos</button>

      {data ? (
        <>
          <ul>
            {data.map((todo) => (
              <li key={todo.id}>{todo.title}</li>
            ))}
          </ul>
        </>
      ) : isError ? (
        <span>Error: {error.message}</span>
      ) : isInitialLoading ? (
        <span>Loading...</span>
      ) : (
        <span>Not ready ...</span>
      )}

      <div>{isFetching ? 'Fetching...' : null}</div>
    </div>
  )
}
```
- 위 쿼리는 마운트시 자동으로 데이터를 fetch하지 않을 것입니다.
- 위 쿼리는 자동으로 background refetch를 하지 않을 것입니다.
- 위 쿼리는 쿼리 클라이언트의 `invalidateQuries(쿼리무효화)` 및 `refetchQueries` 호출을 무시합니다.
- __useQuery의 result객체 프로퍼티`refetch`함수는 수동적으로 쿼리를 trigger시킵니다.!__

<br>

### Lazy Queries (비활성쿼리를 특정 작업을 통해 쿼리 활성화시키기)
- "enabled" 옵션은 쿼리를 영구적으로 비활성화하는 것뿐만 아니라 나중에 활성화/비활성화하는 데에도 사용할 수 있습니다. 좋은 예로는 `사용자가 필터 값 입력을 완료한 후에만 첫 번째 요청을 보내고자하는 필터 폼이 있을 수 있습니다.`
```jsx
function Todos() {
  const [filter, setFilter] = React.useState('')

  const { data } = useQuery({
      queryKey: ['todos', filter],
      queryFn: () => fetchTodos(filter), //filter값이 입력된 경우만 쿼리요청
      // ⬇️ disabled as long as the filter is empty
      enabled: !!filter
  })

  return (
      <div>
        // 🚀 applying the filter will enable and execute the query
        <FiltersForm onApply={setFilter} />
        {data && <TodosTable data={data}} />
      </div>
  )
}
```

<br>

### isInitialLoading
- "isInitialLoading"은 lazy 쿼리에 대해서 사용되며, 시작부터 lazy 쿼리는 'loading' 상태일 것입니다. `__'loading'쿼리는 있지만, 아직 데이터가 없음을 의미합니다.__ `기술적으로는 맞지만, 현재 데이터를 가져오고 있지 않으므로 (쿼리가 활성화되지 않았기 때문에) `이 플래그를 사용하여 로딩 스피너를 표시하는 데 사용할 수 없을 가능성이 높습니다.` 
- disabled나 lazy 쿼리를 사용하는 경우 대신에 "isInitialLoading" 플래그를 사용할 수 있습니다. 이 플래그는 다음과 같이 계산되는 파생된 플래그입니다: `isLoading && isFetching`
- 따라서 이 플래그는 쿼리가 현재 첫 번째로 가져오는 데이터를 가져올 때만 true가 될 것입니다. 즉, 초기 데이터를 가져오는 중일 때만 true로 설정됩니다.
  - `lazy query(enable:false)는 data가 없으므로 status === loading 이 유지된다, 따라서 무한 스피너 UI동작 가능성이 있으므로 isLoading&&isFetching의 조합 플래그인 isInitialLoading을 사용하자`

<br>

## Query Retries(쿼리 재시도 횟수)
- useQuery 쿼리가 실패하면 tanstack쿼리는 자동으로 3회 fetch를 재시도합니다
- 전역적으로, 혹은 쿼리 개별적으로 retry옵션을 조절할 수 있습니다.
1. `retry=false`: 쿼리 재시도를 하지 않습니다.
2. `retry=true`: 요청 실패시 무한히 재시도를합니다.
3. `retry=6`: 6번 재시도
4. `retry= (failureCount, error)=> ...` : 커스텀 로직 기반의 재시도 


```jsx
import { useQuery } from '@tanstack/react-query'
Make a specific query retry a certain number of times
const result = useQuery({
  queryKey: ['todos', 1],
  queryFn: fetchTodoListPage,
  retry: 10, // Will retry failed requests 10 times before displaying an error
})
```

<br>

### Retry Delay
- 쿼리 요청 실패후 재시도 delay time은 기본적으로 `1000ms` 입니다.
```jsx
// Configure for all queries
import {
  QueryCache,
  QueryClient,
  QueryClientProvider,
} from '@tanstack/react-query'

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000),
    },
  },
})

function App() {
  return <QueryClientProvider client={queryClient}>...</QueryClientProvider>
}
```
- 위 코드는 쿼리 재시도를 시도할 때마다 점진적으로 delay를 2배로 증가(2000ms)하고 최대 30s로 설정함.
  - `( 1 -> 2 -> 4 -> 8 ..) 점점 느리게 재시도`

### retryDelay

```jsx
const result = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
  retryDelay: 1000, // Will always wait 1000ms to retry, regardless of how many retries
})
```
- 추천되는 방법은 아니지만 ,retryDelay옵션을 설정해서 같은 효과를 얻을 수 있습니다.(단 이 방법은, 모든 재시도에 동일한 retryDelay time을 적용한다.)

<br>

## Paginated/ Lagged Queries
- paginate된 data를 렌더링하는 것은 아주 흔한 UI패턴입니다. 단순히 `page정보를 쿼리 키에 추가하는 것으로 페이지네이티드 쿼리를 요청할 수 있습니다.
- 하지만 paginated된 쿼리를 요청하면 data status가 `success<->loading`으로 왔다갔다 하는 경험을 맞이합니다.( 매 요청마다 새로운 쿼리를 요청하는 것으로 처리되기 때문)
- 따라서 이것을 극복하기 위해 `keepPreviousData`라는 프로퍼티를 제공합니다.(`:true로 설정`)

<br>

### Better Paginated Queries with keepPreviousData
- `keepPreviousData :true`옵션을 세팅해두면 pagenated쿼리는 쿼리가 변경되었더라도 `직전의` 가장 성공적인 fetch data에 접근할 수 있습니다.`(:과거의 paginated쿼리를 저장해두는 용도)`

```jsx
function Todos() {
  const [page, setPage] = React.useState(0)

  const fetchProjects = (page = 0) => fetch('/api/projects?page=' + page).then((res) => res.json())

  const {
    isLoading,
    isError,
    error,
    data,
    isFetching,
    isPreviousData,
  } = useQuery({
    queryKey: ['projects', page],
    queryFn: () => fetchProjects(page),
    keepPreviousData : true //직전의 page쿼리들을 기억한다
  })

  return (
    <div>
      {isLoading ? (
        <div>Loading...</div>
      ) : isError ? (
        <div>Error: {error.message}</div>
      ) : (
        <div>
          {data.projects.map(project => (
            <p key={project.id}>{project.name}</p>
          ))}
        </div>
      )}
      <span>Current Page: {page + 1}</span>
      <button
        onClick={() => setPage(old => Math.max(old - 1, 0))}
        disabled={page === 0}
      >
        Previous Page 
      </button>{' '} 
      <button
        onClick={() => {
          if (!isPreviousData && data.hasMore) {
            setPage(old => old + 1)
          }
        }}
        // Disable the Next Page button until we know a next page is available
        disabled={isPreviousData || !data?.hasMore}
      >
        Next Page
      </button>
      {isFetching ? <span> Loading...</span> : null}{' '}
    </div>
  )
}
```
- `isPreviousData`: 이전에 fetch해온 page의 쿼리이면 true
- data.hasMore 프로퍼티는 api상 구현 필요 (6개씩 불러오는 경우 남은 리스트가 6개가 아니면 data.hasMore =false로 구현하는 방식 등을 활용..)

<br>

## Inifinite Queries(무한 쿼리)
- `load more` 혹은 `무한 스크롤` 형태의 UI에 사용하는 `useInfiniteQuery` 훅
- 이러한 UI를 위한 useQuery 무한쿼리버전인 `useInifiteQuery`를 제공

```jsx
import { useInfiniteQuery } from '@tanstack/react-query'

function Projects() {
  const fetchProjects = async ({ pageParam = 0 }) => {
    const res = await fetch('/api/projects?cursor=' + pageParam)
    return res.json()
  }

  const {
    data,
    error,
    fetchNextPage,
    hasNextPage,
    isFetching,
    isFetchingNextPage,
    status,
  } = useInfiniteQuery({
    queryKey: ['projects'],
    queryFn: fetchProjects,
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  })

  return status === 'loading' ? (
    <p>Loading...</p>
  ) : status === 'error' ? (
    <p>Error: {error.message}</p>
  ) : (
    <>
      {data.pages.map((group, i) => (
        <React.Fragment key={i}>
          {group.data.map((project) => (
            <p key={project.id}>{project.name}</p>
          ))}
        </React.Fragment>
      ))}
      <div>
        <button
          onClick={() => fetchNextPage()}
          disabled={!hasNextPage || isFetchingNextPage}
        >
          {isFetchingNextPage
            ? 'Loading more...'
            : hasNextPage
            ? 'Load More'
            : 'Nothing more to load'}
        </button>
      </div>
      <div>{isFetching && !isFetchingNextPage ? 'Fetching...' : null}</div>
    </>
  )
}
```
__주요 반환값__
- useInfiniteQuery는 기본적으로 useQuery와 사용법은 비슷하지만, 차이점이 있다.
- useInfiniteQuery는 반환값으로isFetchingNextPage, isFetchingPreviousPage, fetchNextPage, fetchPreviousPage, hasNextPage 등이 추가적으로 있다.

1. `fetchNextPage`: 다음 페이지를 fetch 할 수 있다.
2. `fetchPreviousPage`: 이전 페이지를 fetch 할 수 있다.
3. `isFetchingNextPage`: fetchNextPage 메서드가 다음 페이지를 가져오는 동안 true이다.
4. `isFetchingPreviousPage`: fetchPreviousPage 메서드가 이전 페이지를 가져오는 동안 true이다.
5. `hasNextPage`: 가져올 수 있는 다음 페이지가 있을 경우 true이다.
6. `hasPreviousPage`: 가져올 수 있는 이전 페이지가 있을 경우 true이다.

<br>

__옵션__
- `pageparam`이라는 프로퍼티를 queryFn에 파라미타로 할당해주어야하며, 이 pageParam은
`getNextPageParam`옵션에 할당해주는 함수의 리턴값이 된다.
  - getNextPageParam함수의 첫 번쨰 인자 lastPage는 fetch해온 가장 최근 페이지 목록
  - 두 번째 인자 allPages는 현재까지 가져온 모든 페이지 목록의 배열 

<br>

__pageParam 파라미터 활용하기__
```jsx
const { data } = useInfiniteQuery(["colors"], fetchColors, {
  getNextPageParam: (lastPage, allPages) => {
    return (
      allPages.length < 4 && {
        page: allPages.length + 1,
        etc: "hi",
      };
    )
  },
});
```
- 그러면 queryFn에 넣은 pageParam에서 getNextPageParam에서 return한 객체를 받아올 수 있다.

```jsx
const fetchColors = async ({ pageParam }) => {
  const { page = 1, etc } = pageParam;

  return await axios.get(`http://localhost:4000/colors?_limit=2&_page=${page}`);
};
```
- 즉, getNextPageParam의 반환 값이 pageParam로 들어가기 때문에 pageParam를 원하는 형태로 변경하고 싶다면 getNextPageParam의 반환 값을 설정하면 된다.

<br>

__페이지중 일부 다시 불러오가: refetchPage__
- 전체 페이지 중 일부만 직접 refetch하고 싶을 때에는, useInfiniteQuery가 반환하는 refetch 함수에 refetchPage를 넘겨주면 된다.
- `refetchPage는 각 페이지에 대해 실행되며`,` 이 함수가 true를 반환하는 페이지만`refetch가 된다.

```jsx
const { refetch } = useInfiniteQuery({
  queryKey: ['projects'],
  queryFn: fetchProjects,
  getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
})

// only refetch the first page
refetch({ refetchPage: (page, index) => index === 0 }) //호출시 해당 페이지를 refetch
```
>refetch 변수는 useInfiniteQuery 훅의 반환 값 중 하나이며, 쿼리를 수동으로 다시 불러올 때 사용됩니다. `refetch 함수에는 refetchPage라는 객체가 전달되며,` refetchPage 객체는 페이지를 다시 불러오는 데 필요한 로직을 담은 함수를 가지고 있습니다. refetchPage 함수의 page과 index 파라미터는 쿼리를 다시 불러오는 데 필요한 정보를 전달하기 위해 사용됩니다. `일반적으로 page 파라미터는 불러올 페이지의 번호를 나타내며, index 파라미터는 현재 페이지의 인덱스를 나타냅니다 ` refetch({ refetchPage: (page, index) => index === 0 })는 index가 0인 페이지를 다시 불러오는 refetchPage 함수를 실행하라는 의미입니다. 이는 첫 번째 페이지를 다시 불러와서 최신 데이터로 갱신하고, 화면에 업데이트하는 역할을 수행합니다.

<br>

## Initial Query Data(쿼리 초기값 설정, 캐시되는 값)
- 쿼리에 대한 초기 데이터가 필요하기 전에 캐시에 제공하는 방법이 있다.
- initialData 옵션을 통해서 쿼리를 미리 채우는 데 사용할 수 있으며, 초기 로드 상태도 건너뛸 수도 있다
- 하지만 이 방식은 쿼리캐시에 유지되므로 불완전한 값을 유지하는 부작용이 있을 수 있습니다, `따라서 placeHolder data를 이용하는 것이 좋습니다!`
```jsx
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: initialTodos, //초기데이터 (값을 return하는 함수형태도 OK)
})
```

```jsx
const useSuperHeroData = (heroId: string) => {
    const queryClient = useQueryClient();
    return useQuery(['super-hero', heroId], fetchSuperHero, {
      initialData: () => {
        const queryData = queryClient.getQueryData(['super-heroes']) as any;
        const hero = queryData?.data?.find(
          (hero: Hero) => hero.id === parseInt(heroId)
        );

        if (hero) return { data: hero };
        return undefined;
      },
    });
  };
```
- 참고로 위 예제에서 `queryClient.getQueryData `메서드는 기존 쿼리의 캐싱 된 데이터를 가져오는 데 사용할 수 있는 동기 함수이다. 쿼리가 존재하지 않으면 `undefined`(__옵셔널체이닝 필요__)를 반환한다.
- 만약 쿼리의 초기 데이터에 접근하는 과정이 `복잡하거나 매번 렌더링할 때마다 수행하고 싶지 않은 경우,initialData 값으로 함수를 전달할 수 있습니다.` 이 함수는 쿼리가 초기화될 때 한 번만 실행되며, 메모리와 CPU를 절약할 수 있습니다.

<br>

## Placeholder Query Data (로딩중 미리 보여줄 초기데이터, 캐시되지않음)
- Placeholder 데이터는 initialData 옵션과 유사하게 쿼리가 이미 데이터를 가지고 있는 것처럼 동작할 수 있게 해줍니다. 그러나 이 데이터는 캐시에 영구적으로 저장되지는 않습니다. 이는 실제 데이터가 백그라운드에서 가져와지는 동안에도 충분한 부분적인 (또는 가짜) 데이터로 쿼리를 성공적으로 렌더링하는 상황에서 유용합니다.

- 예를 들어, 개별 블로그 게시물 쿼리는 블로그 게시물의 부모 목록에서 "미리보기" 데이터를 가져올 수 있습니다. 이 미리보기 데이터는 제목과 게시물 본문의 작은 일부분만 포함합니다. 이러한 부분적인 데이터를 개별 쿼리의 결과로 영구적으로 저장하고 싶지는 않지만, 실제 쿼리가 전체 객체를 가져오기까지 가능한 빠르게 콘텐츠 레이아웃을 보여주는 데 유용합니다.

```jsx
function Todos() {
  const { isLoading, data } = useQuery({
    queryKey: ['todos'],
    queryFn: () => fetch('/todos'),
    placeholderData: placeholderTodos,
  });

  if (isLoading) {
    return <div>{placeholderTodos}</div>;
  }

  // 로딩이 완료되면 실제 데이터를 렌더링
  return <div>{data}</div>;
}
```

<br>

## Prefetching
- prefetch는 말 그대로 미리 fetch해오겠다는 의미이다.
- 비동기 요청은 데이터 양이 클 수록 받아오는 속도가 느리고, 시간이 오래걸린다. 사용자 경험을 위해 데이터를 미리 받아와서 캐싱해놓으면? 새로운 데이터를 받기전에 사용자가 캐싱된 데이터를 볼 수 있어 UX에 좋은 영향을 줄 수 있다.
예를 들어 페이지네이션을 구현했다고 가정하면, 페이지1에서 페이지2로 이동했을 때 페이지3의 데이터를 미리 받아놓는 것이다!
- react query에서는 `queryClient.prefetchQuery을 통해서 prefetch `기능을 제공한다.
```jsx
const prefetchNextPosts = async (nextPage: number) => {
  const queryClient = useQueryClient();
  // 해당 쿼리의 결과는 일반 쿼리들처럼 캐싱된다.
  await queryClient.prefetchQuery(
    ["posts", nextPage],
    () => fetchPosts(nextPage),
    { ...options }
  );
};

// 단순 예
useEffect(() => {
  const nextPage = currentPage + 1;

  if (nextPage < maxPage) {
    prefetchNextPosts(nextPage);
  }
}, [currentPage]);
```
__prefetch예시2__
```jsx
unction Posts() {
  const queryClient = useQueryClient();

  const fetchPosts = async () => {
    // 데이터를 가져오는 비동기 요청 등을 수행
    // ...
    const response = await fetch('/posts');
    const data = await response.json();
    return data;
  };

  const prefetchPosts = async () => {
    await queryClient.prefetchQuery('posts', fetchPosts);
  };

  useEffect(() => {
    prefetchPosts();
  }, []);

  // 데이터를 사용하여 컴포넌트 렌더링
  // ...
}
```
- 위 코드에서 prefetchNextPosts()함수를 실행하면 useQuery훅을 사용하며 다음 페이지를 미리 캐시해둘 수 있음 (UX향상)

<br>
<br>
<br>

## useMutation (param: 뮤테이션함수,설정객체)
- react-query에서 기본적으로 서버에서 데이터를 Get 할 때는 useQuery를 사용한다.
- 반환값(mutation)은 mutate처리 된 새로운객체 (mutation.data로 데이터접근)
- `status: ( idle,loading,error,success)`
- `res객체 : error , data 접근가능`
- 만약 서버의 data를 post, patch, put, delete와 같이 수정하고자 한다면 이때는 useMutation을 이용한다.
- 요약하자면 `R(read)는 useQuery, CUD(Create, Update, Delete)는 useMutation`을 사용한다.
```jsx
const CreateTodo = () => {
  const mutation = useMutation(createTodo, {
    onMutate() { //mutate함수 실행 전에 호출될 함수!
      /* ... */
    },
    onSuccess(data) {
      console.log(data);
    },
    onError(err) {
      console.log(err);
    },
    onSettled() {
      /* ... */
    },
  });

  const onCreateTodo = (e) => {
    e.preventDefault();
    mutation.mutate({ title }); //mutation함수(post할 객체)
  };
  return <>...</>;
};
```

```jsx
useMutation({
  mutationFn: addTodo,
  onSuccess: async () => {
    console.log("I'm first!")
  },
  onSettled: async () => {
    console.log("I'm second!")
  },
})
```

<br>
<br>

__mutate함수 호출 이후 추가적인 콜백을 실행하는 방법__


1. useMutation의 반환값인 mutation객체의 mutate메서드를 이용해 mutation함수를 호출할 수 있다.
  - useMuation의 파라미터는 1. mutation함수 2. 설정객체(onMutate,onSuccess,onError,onSettled)
2. `onMutate메서드`: 뮤테이션 함수 동작 전에 실행할 함수,` mutation 함수가 받을 동일한 변수가 전달된다.`
3. `onError:` 뮤테이션이 실패했을 때 실행되는 콜백 함수입니다
4. `onSuccess`:` 뮤테이션이 성공적으로 완료된 후에 실행되는 콜백 함수입니다. 
5. `onSettled`: 에러, 성공 여부 관계없이 실행할 함수(try..catch...finally구문의 `finally`와 같다)
- __`각 메서드에는 (data, error, variables ..)등의 변수가 할당되는데 이때, data는 mutation.data값 , variables는 mutation함수에 들어가는 파라미터이다.`__

<br>

```jsx
useMutation({
  mutationFn: addTodo,
  onSuccess: (data, variables, context) => {
    // I will fire first
  },
  onError: (error, variables, context) => {
    // I will fire first
  },
  onSettled: (data, error, variables, context) => {
    // I will fire first
  },
})

mutate(todo, {
  onSuccess: (data, variables, context) => {
    // I will fire second!
  },
  onError: (error, variables, context) => {
    // I will fire second!
  },
  onSettled: (data, error, variables, context) => {
    // I will fire second!
  },
})
```
- mutate함수 호출이후 성공적이라면? useMuation함수의 onSuccess(1) -> mutate함수의 onSuccess(2) 순서로 호출된다. 

<br>
<br>

### Consecutive Mutation (연속적인 뮤테이션)
```jsx
useMutation({
  mutationFn: addTodo,
  onSuccess: (data, error, variables, context) => {
    // Will be called 3 times (세번의 mutate함수로 인해 세번 동작)
  },
})

[('Todo 1', 'Todo 2', 'Todo 3')].forEach((todo) => {
  mutate(todo, {
    onSuccess: (data, error, variables, context) => {
      // Will execute only once, for the last mutation (Todo 3),(가장 마지막 mutate 성공 이후 한번만 실행된다.)
      // regardless which mutation resolves first(가장 먼저 resolve되는 뮤테이션에 관계없이 한번만)
    },
  })
})
```

<br>
<br>

## Query Invalidation(쿼리무효화) (중요한 기능!)
- __`invalidateQueries은 화면을 최신 상태로 유지하는 가장 간단한 방법이다.`__
- __예를 들면, 게시판 목록에서 어떤 게시글을 작성(Post)하거나 게시글을 제거(Delete)했을 때 화면에 보여주는 게시판 목록을 실시간으로 최신화 해야할 때가 있다.__
- 하지만 이때, query Key가 변하지 않으므로 강제로 쿼리를 무효화하고 최신화를 진행해야 하는데, 이런 경우에 invalidateQueries() 메소드를 이용할 수 있다.
- 즉, query가 오래되었다는 것을 판단하고 다시 refetch를 할 때 사용한다!
```jsx
import { useMutation, useQuery, useQueryClient } from "react-query";

const useAddSuperHeroData = () => {
  const queryClient = useQueryClient();
  return useMutation(addSuperHero, {
    onSuccess(data) {
      queryClient.invalidateQueries(["super-heroes"]); // 이 key에 해당하는 쿼리가 무효화!
      // mutation성공시 해당 키의 쿼리를 무효화하여 다시 refetch시키는 기능!(=>UI상 data최신화)
      console.log(data);
    },
    onError(err) {
      console.log(err);
    },
  });
};
```
- 만약 무효화 하려는 키가 여러 개라면 아래 예제와 같이 다음과 같이 배열로 보내주면 된다.
```jsx
queryClient.invalidateQueries(["super-heroes", "posts", "comment"]);
```
-` useMutation onSuccess메서드에 쿼리무효화 적용하여 data갱신하기!(post후 다시 갱신데이터 get)`
- 위에 enabled/refetch에서도 언급했지만 `enabled: false 옵션을 주면` queryClient가 쿼리를 다시 가져오는 방법 중 `invalidateQueries와 refetchQueries를 무시한다.` [Disabling/Pausing Queries 참고](https://tanstack.com/query/v4/docs/react/guides/disabling-queries?from=reactQueryV3&original=https%3A%2F%2Freact-query-v3.tanstack.com%2Fguides%2Fdisabling-queries)

<br>
<br>

## Updates from Mutation Response
-  뮤테이션을통해 업데이트하여 응답받은 객체를 쿼리set하는방법으로 , 이미 가지고 있는 데이터에 대해 쿼리를 다시 가져와 네트워크 호출을 낭비하는 대신, 뮤테이션 함수가 반환한 객체를 활용하여 기존의 쿼리 데이터를 즉시 업데이트할 수 있습니다.
```jsx
const queryClient = useQueryClient()

const mutation = useMutation({
  mutationFn: editTodo,
  onSuccess: data => {
    queryClient.setQueryData(['todo', { id: 5 }], data)
  }
})

mutation.mutate({
  id: 5,
  name: 'Do the laundry',
})

// The query below will be updated with the response from the
// successful mutation
const { status, data, error } = useQuery({
  queryKey: ['todo', { id: 5 }],
  queryFn: fetchTodoById,
})
```
- 업데이트한 서버객체를 다시 useQuery로 가져오지 않고, 해당 쿼리를 `queryClient.setQueryData( set객체)`로 갱신히여 불필요한 네트워크요청을 방지 

<br>
<br>

  - __아래의 코드에서 onSuccess메서드의 `variables`는 mutation함수에 전달하는 값과 동일한 값이 전달된다.__
```jsx
const useMutateTodo = () => {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: editTodo,
    // Notice the second argument is the variables object that the `mutate` function receives
    onSuccess: (data, variables) => {
      queryClient.setQueryData(['todo', { id: variables.id }], data)
    },
  })
}
```

<br>

__`queryClient.setQueryData(key,data|Fn:()=>data)` 주의점 :불변성__
- 위 방식으로 쿼리 캐시 데이터를 수정할 때 반드시 불변성(immutability)를 유지하는 방식으로 작성해야합니다.
  -  `불변성: `원본을 직접 수정하는 방식이 아닌, `원본은 두고 새로운 객체`를 만들어 수정하는방식
- setQueryData함수는 쿼리를 즉시 수정하는 `동기함수`이다. (바로 반영)
```jsx
queryClient.setQueryData(
  ['posts', { id }],
  (oldData) => {
    if (oldData) {
      // ❌ do not try this
      oldData.title = 'my new post title'
    }
    return oldData
  })

queryClient.setQueryData(
  ['posts', { id }],
  // ✅ this is the way
  (oldData) => oldData ? {
    ...oldData,
    title: 'my new post title'
  } : oldData
)
```

<br>
<br>
<br>

## Query Cancellation(쿼리 취소 , 무효화가아님)
- 쿼리를 수동으로 취소하고 싶을 수도 있다.
   - 예를 들어 요청을 완료하는 데 시간이 오래 걸리는 경우 사용자가 취소 버튼을 클릭하여 요청을 중지하도록 허용할 수 있다.
   - 또는, 아직 HTTP 요청이 끝나지 않았을 때, 페이지를 벗어날 경우에도 중간에 취소해서 불 필요한 네트워크 리소스를 개선할 수 있다.
- 이렇게 하려면 쿼리를 취소하고 이전 상태로 되돌리기 위해 queryClient.cancelQueries(queryKey)를 사용할 수 있다. 또한 react-query는 쿼리 취소뿐만아니라 queryFn의 Promise도 취소한다.

<br>

__매뉴얼하게 쿼리 취소시키기__(queryClient.cancellQueries(key))
```jsx
const query = useQuery(["super-heroes"], {
  /* ...options */
});

const queryClient = useQueryClient();

const onCancelQuery = (e) => {
  e.preventDefault();

  queryClient.cancelQueries(["super-heroes"]);
};

return <button onClick={onCancelQuery}>Cancel</button>;
```
- 수동으로(버튼클릭) 쿼리를 취소하는 사용법

<br

__쿼리 요청중 컴포넌트 언마운트시 자동 취소__(AbortController api 활용한 자동취소)
- 리액트쿼리는 `new AbortCotroller()` 인스턴스의 `instance.signal`를 각각의 쿼리펑션에 제공함
- 즉, 쿼리펑션에 `{signal}`프로퍼티만 넘기면 자동으로 언마운트시 네트워크요청을 중지시켜줌.

```jsx
import axios from 'axios'

const query = useQuery({
  queryKey: ['todos'],
  queryFn: ({ signal }) => //프로피스 리턴 함수(axios요청함수)
    axios.get('/todos', {
      // Pass the signal to `axios`
      signal, //axios 설정객체
    }),
})
```
- 쿼리펑션에 {signal}객체 넘기고, axios 요청의 파라미터로 같이 넣어주면 됨

<br>
<br>

## Optimistic Update(낙관적업데이트)
- 서버 값 업데이트시 UI에서도 어차피 업데이트 할 것이라거 (낙관적)가정해서 `미리 UI를 업데이트`시키고 
서버를 통해 검증을 받고 업데이트 또는 실패시 롤백하는 방식이다.
- 예를 들어 facebook에 좋아요 버튼이 있는데 이것을 유저가 누른다면, 일단 client 쪽 state를 먼저 업데이트한다. 그리고 만약에 실패한다면, 예전 state로 돌아가고 성공하면 필요한 데이터를 다시 fetch해서 서버 데이터와 확실히 연동을 진행한다.
- Optimistic Update가 정말 유용할 때는 인터넷 속도가 느리거나 서버가 느릴 때이다. 유저가 행한 액션을 기다릴 필요 없이 바로 업데이트되는 것처럼 보이기 때문에 사용자 경험(UX) 측면에서 좋다.

```jsx
useMutation({
  mutationFn: updateTodo,
  // When mutate is called:
  onMutate: async (newTodo) => { // 낙관적 업데이트 및 롤백용 쿼리 스냅샷 저장

    // 업데이트 완료 이전 mutate가 다시 호출되는 경우 이전 요청을 취소하는 코드(해당 키를 가진 모든 쿼리를 취소합니다.)
    // 이렇게 함으로써 이전 요청이 완료되기 전에 새로운 요청이 전송되지 않도록 보장할 수 있습니다. 따라서 UI가 업데이트되기 전에 이전 요청에 대한 응답이 도착하면 낙관적인 업데이트가 덮어쓰이지 않고 유지됩니다.
    await queryClient.cancelQueries({ queryKey: ['todos'] })

    // Snapshot the previous value
    // 낙관적 업데이트 전의 todos 캐시데이터를 저장해둠( 에러발생시 다시 롤백하는용도)
    const previousTodos = queryClient.getQueryData(['todos'])

    // Optimistically update to the new value
    // 낙관적으로 캐시데이터 업데이트하기.(Mutation함수에 전달한 newTodo객체로)
    queryClient.setQueryData(['todos'], (old) => [...old, newTodo])

    // Return a context object with the snapshotted value
    return { previousTodos } //리턴값은 onError메서드에 context객체로 할당
  },
  // If the mutation fails,
  // use the context returned from onMutate to roll back
  // context는 onMutate의 반환객체 return {previousTodos}이다.
  onError: (err, newTodo, context) => {
    queryClient.setQueryData(['todos'], context.previousTodos)
  },
  // Always refetch after error or success:
  // 에러가 발생하든 성공하던 결국 todos데이터를 다시 가져와서 갱신한다.
  onSettled: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] })
  },
})
```
- 서버상 객체를 mutation으로 수정 -> 응답객체로 수정객체가 반환됨.
- 즉 mutate호출 후 서버 객체를 응답받아 UI를 업데이트가 완료하기 전, 다시 mutate가 호출되면 mutate실행 이전에 onMutate메서드가 실행되어 (낙관적 업데이트로 덮어쓰기 및 스냅샷저장)이 되기 때문에, 두번 호출시 직전의 요청을 취소하는 cancellQuries(key)이 필요함. (서버데이터와의 불일치 발생하기때문)
  - 즉 1번 요청값으로 업데이트요청 후 2번요청시 1번으로 업데이트값을 받아오는 문제를 취소하기위해 
1. onMutate: 직전 쿼리의 취소(1) 및 스냅샷 저장(2) (return content), 낙관적 업데이트(3)
  - 리턴값으로 롤백용 스냅샷 객체 previousTodo 반환
2. onError: 에러 발생시 스냅샷 개체로 롤백 
3. onSettled: 서버의 todos값 다시 불러오기(최신 data)로 유지해두기

<br>
<br>

## 레퍼런스
- https://tanstack.com/query
- https://velog.io/@gyutato/React-Query-%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%99%80-%EC%BF%BC%EB%A6%AC-%EC%83%81%ED%83%9C%EB%A5%BC-%EB%B6%84%EB%A6%AC%ED%95%98%EA%B8%B0-isLoading-vs-isFetching
- https://github.com/ssi02014/react-query-tutorial#infinite-queries
