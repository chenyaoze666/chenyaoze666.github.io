---
   layout: post
   title:   " React上下文Context使用"                                                        
   date: 2020-03-25 09:00:00 +0530
   categories: React Context
---
  React Context


* App.js

```react
import React from 'react';

import {HashRouter,Switch,Route} from 'react-router-dom';
import Home from './components/home/home';
import Login from './components/login/login';
import Register from './components/register/register';

// 第一步定义Context
// const AuthContext = React.createContext({
//     user:null
// })

//接第二步 传递函数
// const AuthContext = React.createContext({
//     user:null,
//     setUser:()=>{

//     },
// })

//user 默认值
let defaultNickName = window.localStorage.getItem("nickname")
console.log(defaultNickName);
//第三步 导出Context   --->Nav及其他子组件
export const AuthContext = React.createContext({
    user:null,
    setUser:()=>{

    },
})

class App extends React.Component{
    constructor(props){
        super(props);

        //函数需要设置方法
        this.setUser=(u)=>{
            this.setState({
                auth:{
                    ...this.state.auth,
                    user:u,
                },
            })
        }

        this.state={
            //传递多个值时需要包裹起来
            auth:{
                user:defaultNickName?{nickname:defaultNickName}:null,
                setUser:this.setUser,
            }
        }
    }
    

    render(){
        return (
            //第二步 提供 生产者   提供给子组件 value 
            <AuthContext.Provider value={this.state.auth}>
                <HashRouter>
                    <Switch>
                        <Route exact path="/" component={Home}></Route>
                        <Route path="/login" component={Login}></Route>
                        <Route path="/register" component={Register}></Route>
                    </Switch>
                </HashRouter>
            </AuthContext.Provider>
        )
    }
}

export default App;


```

* home.js

```react
import React, { Fragment } from 'react';

import "../../assets/css/home.css";
import Nav from './nav/nav';


class Home extends React.Component{
    constructor(props){
        super(props);
        this.state={

        }
    }

    render(){

        return(

        <Fragment>
            <div id="home">
                <Nav/>
            </div>  
        </Fragment>

        )

    }



}

export default Home;
```

* nav.js

```react
import React, { Fragment } from 'react';
import {Link} from "react-router-dom";

//第四步 导入 Context
import {AuthContext} from "../../../App"; 

class Nav extends React.Component{
    



    render(){
        
        return (
            //第五步 生成消费者
            <AuthContext.Consumer>

                {/* 第六步接收value值 */}
                {(value)=>{
                    console.log(value)
                    return (
                        <Fragment>
                            {value.user ?
                                    <div>当前用户:{value.user.nickname} <span onClick={()=>{
                                        value.setUser(null)
                                        window.localStorage.removeItem("nickname");
                                    }}> 退出</span></div>
                                :
                                    <Link to="/login">请登录</Link>
                            }
                        </Fragment>
                    )
                }}

            </AuthContext.Consumer>
        )
    }
}

export default Nav;
```

* login.js

```react
import React, { Fragment } from 'react';
import {Link} from "react-router-dom";
import axios from 'axios';
import qs from 'qs';

//导入Context
import {AuthContext} from "../../App"; 

import "../../assets/css/login.css";
class Login extends React.Component{
    constructor(props){
        super(props);
        console.log(this.props)
        this.state={
            axiosUrl:"http://xxx/login",
            routerHistory:"/",
            messages:"",
            username:'',
            password:'',
        }
    }



    componentDidMount(){
       
    }

    changeUsername=(e)=>{

        this.setState({
            username:e.target.value,
        })
    }

    changePassword=(e)=>{
        this.setState({
            password:e.target.value,
        })
    }

    clearMessages=()=>{
        this.setState({
            messages:"",
        })
    }
    

    loginSubmit=(auth)=>{
        axios.post(this.state.axiosUrl,qs.stringify({
            username:this.state.username,
            password:this.state.password,
        }),{headers:{"Content-Type":"application/x-www-form-urlencoded"}}).then((result)=>{
            if(result.data.status){
                window.localStorage.setItem("token",result.data.data.token);
                window.localStorage.setItem("nickname",this.state.username);
                auth.setUser({
                    nickname:this.state.username,
                })
                alert("登录成功");
                this.props.history.push(this.state.routerHistory);
            }else{
                this.setState({
                    messages:result.data.data
                })
            }
        }).catch((error)=>{
            alert(error);
        })
    }

    onKeyDown=(e,auth)=>{
        if(e.keyCode===13)
            this.loginSubmit(auth);
    }


    render(){

        return(

        <Fragment>
            <AuthContext.Consumer>

                {(auth)=>{
                    
                    return(
                        <div id="login">
                            <div className="loginBox">
                                <h2>登录更加精彩</h2>
                                <div className="messageGroup">
                                    <span>{this.state.messages}</span>
                                </div>
                                <div className={"inputGroup"}>
                                    <form>
                                        <input type="text" placeholder="用户名"
                                            value={this.state.username}
                                            onChange={this.changeUsername}
                                            onFocus={this.clearMessages}
                                            onKeyDown={()=>{
                                                    let e = window.event;
                                                    this.onKeyDown(e,auth);
                                            }}
                                        />
                                        <input type="password" placeholder="密码"
                                            value={this.state.password}
                                            onChange={this.changePassword}
                                            onFocus={this.clearMessages}
                                            onKeyDown={()=>{
                                                let e = window.event;
                                                this.onKeyDown(e,auth);
                                            }}
                                        />
                                    </form>
                                </div>
                                <div className="linkGroup">
                                    <Link to="/register">注册</Link>
                                    <Link to="">忘记密码</Link>
                                </div>
                                <div className="buttonGroup">
                                    <button 
                                        onClick={()=>{      
                                            this.loginSubmit(auth);
                                        }}
                                    >登录</button>
                                </div>
                            </div>
                        </div>
                    )
        }}
            </AuthContext.Consumer>
        </Fragment>

        )

    }



}

export default Login;
```

