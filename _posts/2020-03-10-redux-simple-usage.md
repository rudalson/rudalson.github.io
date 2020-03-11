# Redux 사용법 간단 정리

`Redux` 사용에 대한 간단한 정리

### redux CDN load
```
<head>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.min.js"></script>
</head>
```

### store 생성
`store`를 생성해주는데 reducer는 반드시 필요하다. 그래서 reducer()를 만들어 준다.
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

### state 확인
state내용을 가져오는 것은 store에서 getState()로 가능하다.
```
let state = store.getState();
```

### state 수정
state값 수정은 이렇게 dispatch()에 필요한 값들을 인자로 주고 호출한다.


```
let action =  {type:'CLICKED', id:1};
store.dispatch(action);
```

dispatch()가 호출되고 나면 redux내부적으로 reducer가 또 다시 호출된다. 그러면 reducer에서 필요한 값을 받아서 Object.assign()으로 state값을 갱신시켜준다.
```
function reducer(state, action) {
...
	if (action.type === 'CLICKED') {
        newState = Object.assign({}, state, {selected: action.id});
    }
...
}
```
Object.assign()의 2번째는 `previous state`, 3번째는 `new state`이다.

### Object.assign()
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


### state 변경시 render()가 다시 되게 하려면

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


