---
   layout: post
   title: "react-redux基础案例"                                                        
   date: 2020-05-01 09:00:00 +0530
   categories: React
---
  React


# react-redux基础案例

## 第一步

* 写一个管理store     store/ndex.js

```
import {createStore} from 'redux'
import reducer from './reducer'

const store = createStore(reducer);

export default store
```

## 第二步

* 写一个第一步引入 的store的业务处理 reducer.js

```
const defaultState = {//store的默认模板
  inputValue : 'hello',
  list:[]
}

//写业务处理   这里必须是纯函数
export default (state = defaultState,action) => {
  
  return state
}

// 第二步 写个处理store的业务
```

## 第三步

* 在需要使用store的组件上 提供生产者

```
import { Provider } from 'react-redux';
import store from './store/'


<Provider store={store}>
  <TodoList/>
</Provider>

```

### 第四步

* 在组件上使用连接器 connect   并使用无状态组件

```
import React from 'react'

import { connect } from 'react-redux'

//写成无状态主键
const TodoList = (props) => {
    return (
      <div>
        <input/>
        <button>添加</button>
        <ul>
        	<li></li>
        </ul>
      </div>
    )
}

export default connect(参数1, 参数2)(TodoList)
```

## 第五步

* 定义两个参数     并使用解构 添加到dom中
* 参数1: 返回state模板
* 参数2 用于定义active   处理派发事件

```
import React from 'react'
import { connect } from 'react-redux'

const TodoList2 = (props) => {
	//使用解构
	let {inputValue,inputChange,clickButton,deleteItem,list} = props
	
    return (
      <div>
        <input
          value={inputValue}
          onChange={inputChange} />
        <button onClick={clickButton}>添加</button>
        <ul>
          {
            list.map((item, index) => {
              return (
                <li key={index} onClick={()=>{
                  deleteItem(index)
                }}>{item}</li>
              )
            })
          }
        </ul>
      </div>
    )
}

//第一个   即返回模板
const stateToProps = (state) => {
  return {
    inputValue: state.inputValue,
    list: state.list
  }
}

//第二个参数
const dispatchToProps = (dispatch) => {
  return {
    inputChange(e) {
 
    },
    clickButton() {
  
    },
    deleteItem(index){
   
    }
  }
}

export default connect(stateToProps, dispatchToProps)(TodoList2)
```

## 第六步

* 写action 并派发

```
const dispatchToProps = (dispatch) => {
  return {
    inputChange(e) {
      // console.log(e.target.value)

      // 第七步派发事件
      let action = {
        type: 'change_input',
        value: e.target.value
      }
      dispatch(action)
    },
    clickButton() {
      let action = {
        type: 'add_item',
      }
      dispatch(action)
    },
    deleteItem(index){
      let action = {
        type: 'del_item',
        index,
      }
      dispatch(action)
    }
  }
}
```

## 第七步

* 到reducer中接收派发 处理业务

```
const defaultState = {
  inputValue : 'asasfasf',
  list:[]
}

export default (state = defaultState,action) => {

  //处理业务
  if(action.type === 'change_input'){
    let newState = JSON.parse(JSON.stringify(state));
    newState.inputValue = action.value;
    return newState;
  }
  
  if(action.type === 'add_item'){
    let newState = JSON.parse(JSON.stringify(state));
    newState.list.push(newState.inputValue)
    newState.inputValue='';
    return newState;
  }
  
  if(action.type === 'del_item'){
    let newState = JSON.parse(JSON.stringify(state));
    newState.list.splice(action.index,1)
    return newState;
  }
  return state
}

```

