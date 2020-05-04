---
   layout: post
   title: "react-router和Hock摘要"                                                        
   date: 2020-05-01 10:00:00 +0530
   categories: React
---
  React


# react-router和Hock摘要

## 路由动态传值和参数获取

```
<Link to="/list/:id"></Link>

this.props.match.params.id
```

## 重定向

```

this.props.history.push('/hone')

//需要引入
<Redirect to="/home"/>
```

## Hock

```
import {useState,useEffect} from 'react'

const [count,setCount] = useState(0);

useEffect(()=>{
    
},[])

//第二个参数数数组 表示组件销毁

[]如果填值 表示当这个值变化的时候解绑定(执行销毁函数)

useEffect(()=>{
    
    return () => {
        //组件销毁时执行
    }
},[])

```

## useContext   父子之间传值

```
import React, { useState, createContext, useContext } from 'react'


function AppRouter(){

  const CountContext = createContext();//创建 默认值
  

  const [count,setCount] = useState(0)

  function btnclick(){
    setCount(count+1)
  }


  function Counter(){//子组件
    const count = useContext(CountCContext)   使用
    return (<h2>{count}</h2>)
  }

  return (
    <div>
      <p>your click: {count}</p>
      <button onClick={btnclick}>++</button>
      <CountCContext.Provider value={count}>//使用传值
        <Counter/>//子组件
      </CountCContext.Provider>

    </div>
  )
}

export default AppRouter;
```

## useReducer

```
const [count, dispath] = useReducer((state, action) => {
   switch(action){
       case 'add':
       	return state + 1
       case 'sub':
       	return state - 1
       default:
       	return state
   } 
},0)


dispatch('add')
dispatch('sub')
```

## useReducer和ruseContext代替redux

```
import React,  { useContext } from 'react'
import {ColorContext } from './Color'

export default function PText(){
  const {color} = useContext(ColorContext);
  return (
    <h2 style={{color:color}}>文本颜色为{color}</h2>
  )
}



import React, { createContext, useReducer } from 'react'

export const ColorContext = createContext({});

export const UPDATE_COLOR="UPDATE_COLOR";

const reducer = (state,action) => {
  switch(action.type){
    case UPDATE_COLOR:
      return action.color
    default:
      return state
  }
}

export function Color(props){

  const [color,dispatch] = useReducer(reducer,'blue')

  return (
    <ColorContext.Provider value={{color,dispatch}}>
      {props.children}
    </ColorContext.Provider>
  )
}


import React, { useContext } from 'react'
import {ColorContext, UPDATE_COLOR} from './Color'
export default function BUT(){
  const {dispatch} = useContext(ColorContext)
  return (
    <div>
      <button onClick={()=>{
        dispatch({
          type:UPDATE_COLOR,
          color:"red"
        })
      }}>红色</button>
      <button onClick={()=>{
        dispatch({
          type:UPDATE_COLOR,
          color:"yellow"
        })
      }}>黄色</button>
    </div>
  )
}



import React from 'react'
import BUT from './BUT';
import PText from './PText.';
import {Color} from './Color'



function AppRouter(){
  return (
    <div>
    <Color>
      <PText/>
      <BUT/>
    </Color>
    </div>
  )
}

export default AppRouter;
```

## useMemo解决子组件重复渲染性能的问题

```
import React, { useMemo } from 'react'


const a = useMemo( ()=> b(name),[name])//name发生变化是才执行b
```

## useRef获取DOM和保存变量

```
import React, { useRef } from 'react'
const inputEl = useRef(null)

inputEl.current.value = 'hello'//控制input的value

<input ref={inputEl} />
```

```
const [text,setText] = useState('hello')

const textRef = useRef()

textRef.current = text  //保存text


console.log(textRef.current)//hello
```

