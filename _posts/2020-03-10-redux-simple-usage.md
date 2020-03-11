# Redux 사용법 간단 정리

`Redux`를 `React`같은 라이브러리를 사용하지 않고 순수 Javascript에서 사용하는 법에 대해 핵심만 정리 해보았다.

### redux CDN load
별도로 `npm`으로 인스톨 하지 않고 바로 연습해보려면 CDN load로 해보면 된다. 현재는 `redux-4.0.5` 사용
```
<head>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.min.js"></script>
</head>
```

### store 생성
최종 상태값을 가지고 있는 `store`를 생성해줘야 한다. 그런데 store는 `reducer`를 반드시 필요하다. 그래서 reducer()를 만들어 준다.
```
function reducer(state, action) {
    if (state === undefined) {
        return {
            items: [
                {id: 1, title: 'redux1'},
                {id: 2, title: 'redux2'},
            ]
        }
    }

    let newState = {};
    ...
    return newState;
}

const store = Redux.createStore(reducer);
```
reducer function을 만든 후 밑에서 바로 `createStore()`로 store를 생성한다.

### state 확인
`state`내용을 가져오는 것은 store에서 `getState()`로 가능하다.
```
let state = store.getState();
```

### state 수정
state값 수정(mutation)은 이렇게 `action` 값들을 인자로 주고 `dispatch()`를 호출한다. actino이라는 것은 변경될 값들을 담은 것이다.

```
let action =  {type:'CLICKED', id:1};
store.dispatch(action);
```

dispatch()가 호출되고 나면 redux내부적으로 reducer가 또 다시 호출된다. reducer에서는 필요한 값을 받아서 `Object.assign()`으로 state값을 갱신시켜준다.
```
function reducer(state, action) {
...
	if (action.type === 'CLICKED') {
        newState = Object.assign({}, state, {selected: action.id});
    }
...
}
```
Object.assign()의 2번째는 `previous state`, 3번째는 `new state`이다. 첫번째는???

### 배열값 수정
보통은 
```
newState = Object.assign({}, state, {selected: action.id});
```
처럼 해도 되지만 배열(혹은 리스트) 값에서 뒤에 추가할 때는
```
let items = state.contents.concat();
items.push({id: state.items.length + 1, title: action.title});
newState = Object.assign({}, state, {items, selected: state.contents.length + 1
});
```
이런 형태로 추가시켜 준다.


### state 변경시 render()가 다시 되게 하려면 subscribe등록
state가 변경되어서 다시 rendering 되게 하려면 `render`할 함수를 subscribe에 인자로 줘서 아래처럼 등록해준다.
```
function showItems() {
	let state = store.getState();
	...

	 document.querySelector('#items').innerHTML = `
            <itemDetail>
                <h2>${title}</h2>
            </itemDetail>
        `
}

...
const store = Redux.createStore(reducer);
store.subscribe(showItems);
```


