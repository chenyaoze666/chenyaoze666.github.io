---
   layout: post
   title:   " 利用React的Context完成手机注册登录拉去个人信息"                                                        
   date: 2020-03-27 09:00:00 +0530
   categories: React Context
---
  React Context


# 利用React的Context完成手机注册登录拉去个人信息

* index.js

```react
import React from 'react';
import {render} from 'react-dom';

import App from "./App";

import "./assets/css/index.css";

render(<App/>,document.getElementById("root"));
```

* App.js

```react
import React from 'react';
import {HashRouter,Switch,Route} from "react-router-dom";

import routers from './router/routers';

//user 默认值
let defaultUser = {
    token:window.localStorage.getItem("token"),
    mobile:window.localStorage.getItem("mobile"),
    nickname:window.localStorage.getItem("nickname"),
    avatar_url:window.localStorage.getItem("avatar_url"),
}
console.log(defaultUser);
//第三步 导出Context   
export const AuthContext = React.createContext({
    user:null,
    setUser:()=>{

    },
})


class App extends React.Component{
    constructor(props){
        super(props)

        this.setUser=(ut)=>{
            this.setState({
                auth:{
                    ...this.state.auth,
                    user:ut,
                },
            })
        }

        this.state={
            //传递多个值时需要包裹起来
            auth:{
                user:defaultUser?defaultUser:null,
                setUser:this.setUser,
            }
        }

    }



    render(){
        return(
            <AuthContext.Provider value={this.state.auth}>
            <HashRouter>
                    <Switch>
                        {
                            // 循环路由并传递路由
                            routers.map((item,index)=>{

                                if(item.exact){
                                    return (
                                        
                                        <Route key={index} exact path={item.path} 
                                            render={ props => (<item.component {...props} routers={item.childrenRoutes}/>)}
                                        />
                                    )
                                }else{
                                    return (
                                        <Route key={index} path={item.path}
                                            render={ props => (<item.component {...props} routers={item.childrenRoutes}/>)}
                                        />
                                    )
                                }

                            })
                        }
                    </Switch>
            </HashRouter>
            </AuthContext.Provider>
        )
        
    }

}

export default App;
```

* routes.js

```react
import Home from "../components/home/home";
    import Login from "../components/login/login";
import PhoneRegister from "../components/phoneRegister/phoneRegister";
    import Form from "../components/phoneRegister/components/form/form";
import MyInfo from "../components/myInfo/myInfo";
    import InfoForm from "../components/myInfo/components/infoform/infoform";

let routers = [
    {
        path:"/",
        component:Home,
        exact:true,
        childrenRoutes:[
            {
                path:"/",
                component:Login,
            },
        ]
    },
    {
        path:"/phoneRegister",
        component:PhoneRegister,
        exact:true,
        childrenRoutes:[
            {
                path:"/phoneRegister/",
                component:Form,
            },
            
        ]
    },
    {
        path:"/myInfo",
        component:MyInfo,
        childrenRoutes:[
            {
                path:"/myInfo/",
                component:InfoForm,
            },
            
        ]
    },
]

export default routers;
```

* conponents
* home.js

```react
import React,{Fragment} from 'react';
import {HashRouter,Switch,Route} from "react-router-dom";

class Home extends React.Component{


    render(){
        return(
            <Fragment>
                <HashRouter>
                <Switch>
                        {
                            this.props.routers.map((item,index)=>{
                                
                                if(item.exact){
                                    return (
                                        <Route key={index} exact path={item.path} 
                                            component={item.component}
                                        />
                                    )
                                }else{
                                    return (
                                        <Route key={index} path={item.path}
                                        component={item.component}
                                        />
                                    )
                                }

                            })
                        }
                    </Switch>
                </HashRouter>
            </Fragment>
        )
    }

}

export default Home;
```

* loginTop.js

```react
import React, { Fragment } from 'react';

import "../../../assets/css/registerTop.css";
import "../../../assets/iconfont/iconfont.css";
class LoginTop extends React.Component{


    back=()=>{
        let {props}= this.props;
        props.history.push("/phoneRegister");
    }
    render(){
        return(
            <Fragment>
                <div id="top">
                    <div className={"title"}>
                        <span className="iconfont" onClick={this.back}>&#xe645;</span>
                        <h4>登录</h4>
                    </div>
                    <div className={"tip"}>
                        <span>
                            为了获得更好的体验，建议您立即注册登录
                        </span>
                    </div>
                </div>
            </Fragment>
        )
    }

}

export default LoginTop;
```

* login.js

```react
import React, { Fragment } from 'react';
import LoginTop from './loginTop/loginTop';
import {Link} from "react-router-dom";

import axios from 'axios';
import qs from 'qs';

import "../../assets/css/login.css";

import {AuthContext} from "../../App"; 

class Login extends React.Component{
    constructor(props){
        super(props);
        this.state={
            mobile:"",
            password:"",
            mobileMessage:"",
            passwordMessage:"",
        }
    }

    //非空
    checkunNull=()=>{
        let flag = true;
        if(this.state.mobile===""){
            this.setState({
                mobileMessage:"手机号不能为空!!"
            })
            flag = false;
        }else if(this.state.password===""){
            this.setState({
                passwordMessage:"密码不能为空!!"
            })
            flag = false;
        }
        return flag;
    }

    setMobile=(e)=>{
        this.setState({
            mobile:e.target.value
        })
    }

    setPassword=(e)=>{
        this.setState({
            password:e.target.value
        })
    }

    checkMobile=()=>{
        let regex = /^1\d{10}$/; 
        if(regex.test(this.state.mobile)){
            return true;
        }else{
            this.setState({
                mobileMessage:"手机号不正确!!"
            })
            return false;
        }
    }


    loginSumbit=(value)=>{
        if(!this.checkunNull() || !this.checkMobile())
            return
        this.getLoginApi(value);
    }


    getLoginApi=(value)=>{
        axios.post("http://xxx/api/user/token/mobile",qs.stringify({
          mobile:this.state.mobile,
          password:this.state.password,
        }))
        .then((result)=>{
            if(result.data.status){
                console.log(result)
                window.localStorage.setItem("token",result.data.data.token);
                this.getUserInfo(value);
            }else{
                this.setState({
                    passwordMessage:"用户名或密码错误",
                })
            }
        }).catch((error)=>{
            console.log(error)    
        })
    }

    //获取信息
    getUserInfo=(value)=>{
        axios.get("http://xxx/api/user/profile",{
            params:{
                token:window.localStorage.getItem("token"),
            }
        })
        .then((result)=>{
            console.log(result)

            window.localStorage.setItem("mobile",result.data.data.mobile);
            window.localStorage.setItem("nickname",result.data.data.nickname);
            window.localStorage.setItem("avatar_url",result.data.data.avatar_url);

            value.setUser({
                token:window.localStorage.getItem("token"),
                mobile:result.data.data.mobile,
                nickname:result.data.data.nickname,
                avatar_url:result.data.data.avatar_url,
            })
            this.props.history.push("/myInfo");
        }).catch((error)=>{
            console.log(error)    
        })
    }
    

    render(){
        return(
            <AuthContext.Consumer>
                {(value)=>{
                    return(
                    <Fragment>
                        <LoginTop props={this.props}/>
                        <div id="Login">
                            <form className={"login"}>
                                <div className="input phone">
                                    <span>+86</span>
                                    <input className={"phone"} type="text" placeholder="请输入手机号"
                                        value={this.state.mobile}
                                        onChange={this.setMobile}
                                        onBlur={this.checkMobile}
                                        onFocus={()=>{
                                            this.setState({
                                                mobileMessage:"",
                                            })
                                        }}
                                    />
                                </div>
                                <div className="messages">
                                    <span className="tip">{this.state.mobileMessage}</span>
                                </div>
                                <div className="input">
                                    <input className={"password"} type="password" placeholder="请输入密码"
                                        value={this.state.password}
                                        onChange={this.setPassword}
                                        onFocus={()=>{
                                            this.setState({
                                                passwordMessage:"",
                                            })
                                        }}
                                    />
                                </div>
                                <div className="messages">
                                    <span className="tip">{this.state.passwordMessage}</span>
                                </div>
                                <input className="loginSumbit" type="button" value="登录"
                                    onClick={()=>{
                                            this.loginSumbit(value)
                                        }
                                    }               
                                />
                            </form>
                            <Link to="/phoneRegister/">注册</Link>
                        </div>
                    </Fragment>
                )
            }}
            </AuthContext.Consumer>
        )
    }

}

export default Login; 
```

* infoform.js

```react
import React, { Fragment } from 'react';
import InfoTop from '../infotop/infotop';

import {AuthContext} from "../../../../App";

class InfoForm extends React.Component{
    constructor(props){
        super(props)
        this.state={
            token:""
        }
    }

    componentDidMount=()=>{
    }

    UNSAFE_componentWillMount=()=>{
        
    }
    
    render(){
        return(
            <AuthContext.Consumer>
                {(value)=>{
                    return (
                    <Fragment>
                        <InfoTop props={this.props}/>
                        <div id="infoForm">
                            <div className="img">
                                <div className="left">
                                    头像
                                </div>
                                <div className="right iconfont">
                                    {
                                        value.user.avatar_url?
                                            <img src={value.user.avatar_url} alt=""/>
                                        :
                                            <span>&#xebac;</span>
                                    }
                                </div>
                            </div>
                            <div className="content">
                                <div className="left">
                                    昵称
                                </div>
                                <div className="right iconfont">
                                    {
                                        value.user.nickname?
                                            <span>{value.user.nickname}&#xe64c;</span>
                                        :
                                            <span>气质胜过一切&nbsp;&#xe64c;</span>
                                    }
                                </div>
                            </div>
                            <div className="content">
                                <div className="left">
                                    性别
                                </div>
                                <div className="right iconfont">
                                    &#xe64c;
                                </div>
                            </div>
                            <div className="content">
                                <div className="left">
                                    地区
                                </div>
                                <div className="right iconfont">
                                    &#xe64c;
                                </div>
                            </div>
                            <div className="content">
                                <div className="left">
                                    个人简介
                                </div>
                                <div className="right iconfont">
                                    &#xe64c;
                                </div>
                            </div>
                        </div>
                    </Fragment>
            )
        }}
            </AuthContext.Consumer>
        )
    }

}

export default InfoForm;
```

* infoTop.js

```react
import React, { Fragment } from 'react';


import "../../../../assets/css/registerTop.css";
import "../../../../assets/iconfont/iconfont.css";
class InfoTop extends React.Component{
    constructor(props){
        super(props);            
        this.state={
            
        }
    }

    back=()=>{
        window.localStorage.removeItem("token");
        window.localStorage.removeItem("mobile");
        window.localStorage.removeItem("nickname");
        window.localStorage.removeItem("avatar_url");

        let {props}= this.props;
        props.history.push("/");
    }

    render(){
        return(
            <Fragment>
                <div id="top">
                    <div className={"title"}>
                        <span className="iconfont" onClick={this.back}>&#xe645;</span>
                        <h4>个人信息</h4>
                    </div>
                </div>
            </Fragment>
        )
    }

}

export default InfoTop;
```

* myinfo.js

```react
import React,{Fragment} from 'react';

import {Switch,Route} from 'react-router-dom';

import "../../assets/css/myInfo.css";
import "../../assets/iconfont/iconfont.css";
class MyInfo extends React.Component{

    render(){
        return(
            <Fragment>
                <div id="myInfo">
                <Switch>
                        {
                            this.props.routers.map((item,index)=>{
                                
                                if(item.exact){
                                    return (
                                        <Route key={index} exact path={item.path} 
                                            component={item.component}
                                        />
                                    )
                                }else{
                                    return (
                                        <Route key={index} path={item.path}
                                        component={item.component}
                                        />
                                    )
                                }

                            })
                        }
                    </Switch>
                </div>
            </Fragment>
        )
    }

}

export default MyInfo;
```

* phoneRegister.js

```react
import React,{Fragment} from 'react';
import {Switch,Route} from "react-router-dom";



class PhoneRegister extends React.Component{

    render(){
        return(
            <Fragment>
                <div id="phoneRegister">
                    <Switch>
                        {
                            this.props.routers.map((item,index)=>{
                                
                                if(item.exact){
                                    return (
                                        <Route key={index} exact path={item.path} 
                                            component={item.component}
                                        />
                                    )
                                }else{
                                    return (
                                        <Route key={index} path={item.path}
                                        component={item.component}
                                        />
                                    )
                                }

                            })
                        }
                    </Switch>
                </div>
            </Fragment>
        )
    }

}

export default PhoneRegister;
```

* top.js

```react
import React, { Fragment } from 'react';


import "../../../../assets/css/registerTop.css";
import "../../../../assets/iconfont/iconfont.css";
class Top extends React.Component{


    back=()=>{
        let {props}= this.props;
        props.history.push("/");
    }
    

    render(){
        return(
            <Fragment>
                <div id="top">
                    <div className={"title"}>
                        <span className="iconfont" onClick={this.back}>&#xe645;</span>
                        <h4>完善手机号</h4>
                    </div>
                    <div className={"tip"}>
                        <span>
                            为了获得更好的体验，建议您立即完善手机号
                        </span>
                    </div>
                </div>
            </Fragment>
        )
    }

}

export default Top;
```

* form.js

```react
import React, { Fragment } from 'react';
import axios from 'axios';
import qs from 'qs';

import Top from "../top/top";


import "../../../../assets/css/registerform.css";
import TestVerify from '../testVerify/testVerify';

import {AuthContext} from "../../../../App"; 

class Form extends React.Component{
    constructor(props){
        super(props);
        this.state={
            mobile:"",
            verify:"",
            password:"",
            testVerify:true,
            mobileMessage:"",
            verifyMessage:"",
            passwordMessage:"",
            registerMobile:"",
            captcha_code:"",
            captcha_key:"",
            codeTip:"获取验证码",
            codeSeconds:"",
            resultCodeData:"",
            registerId:"",
        }

    }

    //清除计时器
    componentWillUnmount=()=>{
        clearInterval(this.timer);
    }

    //设置验证成功后返回的data
    setResultCodeData=(val)=>{
        this.setState({
             resultCodeData:val,
        })
    }

    //设置计时器
    getCode=()=>{
        if(this.state.codeTip===""){
            let count = 59;
            this.timer=setInterval(() => {
                this.setState({
                    codeSeconds:count+"s",
                })
                if(count>0){
                    count--;
                }else{
                    clearInterval(this.timer);
                    this.setState({
                        codeTip:"获取验证码",
                        codeSeconds:"",
                        resultCodeData:"",
                    })
                }
            }, 1000);
        }
    }

    //获取验证码框
    setCodeTip=(val)=>{
        this.setState({
            codeTip:val,
        },()=>{
            this.getCode();
        })
    }

    //计时器框
    setCodeSeconds=(val)=>{
        this.setState({
            codeSeconds:val,
        })
    }

    //设置返回的key
    setCaptcha_key=(val)=>{
        this.setState({
            captcha_key:val,
        })
    }

    //设置返回的code
    setCaptcha_code=(val)=>{
        this.setState({
            captcha_code:val,
        })
    }

    //设置安全码框的创建状态
    setTestVerify=(val)=>{
        this.setState({
            testVerify:val,
        })
    }

    //手机获取焦点
    onPhoneFocus=()=>{
        this.setState({
            mobileMessage:"",
        })
    }

    //验证码获取焦点
    onVerifyFocus=()=>{
        this.setState({
            verifyMessage:"",
        })
    }

    //密码获取焦点
    onPasswordFocus=()=>{
        this.setState({
            passwordMessage:"",
        })
    }

    //设置手机号  解决手机变更
    setMobile=(e)=>{
        this.setState({
            mobile:e.target.value
        },()=>{
            if(this.state.mobile!==this.state.registerMobile)
            {
                this.setState({
                    codeTip:"获取验证码",
                    codeSeconds:"",
                    resultCodeData:"",
                })
                clearInterval(this.timer);
            }
        })
    }

    //设置验证码
    setVerify=(e)=>{
        this.setState({
            verify:e.target.value
        })
    }

    //设置密码
    setPassword=(e)=>{
        this.setState({
            password:e.target.value
        })
    }

    //非空
    checkunNull=()=>{
        let flag = true;
        if(this.state.mobile===""){
            this.setState({
                mobileMessage:"手机号不能为空!!"
            })
            flag = false;
        }else if(this.state.verify===""){
            this.setState({
                verifyMessage:"验证码不能为空!!"
            })
            flag = false;
        }else if(this.state.password===""){
            this.setState({
                passwordMessage:"密码不能为空!!"
            })
            flag = false;
        }
        return flag;
    }

    //校验手机
    checkMobile=()=>{
        
        if(this.state.codeTip!=="获取验证码"){
            return;
        }
        this.setState({
            mobileMessage:"",
        })
        
        let regex = /^1\d{10}$/; 
        if(regex.test(this.state.mobile)){
            this.setTestVerify(false);
            this.setState({
                registerMobile:this.state.mobile,
            })
        }else{
            this.setState({
                mobileMessage:"手机号不正确!!"
            })
        }
    }

    //获取信息
    getUserInfo=(value)=>{
        axios.get("http://xxx/api/user/profile",{
            params:{
                token:window.localStorage.getItem("token"),
            }
        })
        .then((result)=>{
            window.localStorage.setItem("mobile",result.data.data.mobile);
            window.localStorage.setItem("nickname",result.data.data.nickname);
            window.localStorage.setItem("avatar_url",result.data.data.avatar_url);
            value.setUser({
                token:window.localStorage.getItem("token"),
                mobile:result.data.data.mobile,
                nickname:result.data.data.nickname,
                avatar_url:result.data.data.avatar_url,
            })
            this.props.history.push("/myInfo");
        }).catch((error)=>{
            console.log(error)    
        })
    }

    //用户注册
    userRegister=(value)=>{
        axios.post("http://xxx/api/user/register",qs.stringify({
            mobile:this.state.registerMobile,
            verify:this.state.verify,
            password:this.state.password,
        }))
        .then((result)=>{
            console.log(result)
            if(result.data.status){
                window.localStorage.setItem("token",result.data.data.token);
                this.setState({
                    registerId:result.data.data.id,
                    resultCodeData:"注册成功!!!"
                })
                this.getUserInfo(value);
            }else{
                this.setState({
                    verifyMessage:result.data.data,
                })
            }
        }).catch((error)=>{
            console.log(error)    
        })
    }

    //表单提交
    onRegisterSumbit=(e,value)=>{
        e.preventDefault();
        
        if(!this.checkunNull())
            return;
        if(this.state.codeSeconds===""){
            this.setState({
                verifyMessage:"请重新获取验证码"
            })
            return
        }
        if(this.state.codeSeconds!=="" && (this.state.registerMobile!==this.state.mobile)){
            this.setState({
                verifyMessage:"请"+this.state.codeSeconds+"后重新发送"
            })
            return
        }
        this.userRegister(value);
    }

    //创建安全码组件
    createTestVerify=()=>{
        if(!this.state.testVerify)
        {
            return <TestVerify 
                        setTestVerify={this.setTestVerify}
                        setCaptcha_key={this.setCaptcha_key}
                        setCaptcha_code={this.setCaptcha_code}
                        setCodeTip={this.setCodeTip}
                        setCodeSeconds={this.setCodeSeconds}
                        registerMobile={this.state.registerMobile}
                        setResultCodeData={this.setResultCodeData}
                    />
        }
    }


    render(){
        return(
            <AuthContext.Consumer>
            {(value)=>{
                return(
            
            <Fragment>
                <Top props={this.props}/>
                <div id={"form"}>
                    <form className={"register"}>
                        <div className="input">
                            <span className={"numberHome"}>手机号归属地</span>
                            <span className="iconfont">中国 &#xe64c;</span>
                        </div>
                        <div className="input phone">
                            <span>+86</span>
                            <input className={"phone"} type="text" placeholder="请输入手机号"
                                value={this.state.mobile}
                                onChange={this.setMobile}
                                onFocus={this.onPhoneFocus}
                                onBlur={this.checkMobile}
                            />
                        </div>
                        <div className="messages">
                            <span className="tip">{this.state.mobileMessage}{this.state.resultCodeData}</span>
                        </div>
                        <div className="input code">
                            <input type="text" placeholder="请输入验证码"
                                value={this.state.verify}
                                onChange={this.setVerify}
                                onFocus={this.onVerifyFocus}
                            />
                            <span onClick={()=>{
                                
                                    this.checkMobile();
                                
                            }}>{this.state.codeTip}<span className="seconds">{this.state.codeSeconds}</span></span>
                        </div>
                        <div className="messages">
                            <span className="tip">{this.state.verifyMessage}</span>
                            <span className="voice">你也可以接听<span>语音验证码</span></span>
                        </div>
                        <div className="input">
                            <input className={"password"} type="password" placeholder="请输入密码"
                                value={this.state.password}
                                onChange={this.setPassword}
                                onFocus={this.onPasswordFocus}
                            />
                        </div>
                        <div className="messages">
                            <span className="tip">{this.state.passwordMessage}</span>
                        </div>
                        <input className="registerSumbit" type="button" value="完成"
                            onClick={()=>{
                                let e = window.event;
                                this.onRegisterSumbit(e,value);
                            }}                        
                        />
                    </form>
                </div>
                {
                    this.createTestVerify()
                }
            </Fragment>
                )
            }}
            </AuthContext.Consumer>
        )
    }

}

export default Form;
```

* TestVerify.js

```react
import React from 'react';
import axios from 'axios';
import qs from 'qs';


import "../../../../assets/css/registerTestVerify.css";
class TestVerify extends React.Component{
    constructor(props){
        super(props);
        this.state={
            testVerifyImg:"",
            captcha_key:"",
            captcha_code:"",
            verifyCode:"",
            codeMessages:"",
        }
    }

    //安全码获取焦点 提示消失
    codeFocus=()=>{
        this.setState({
            codeMessages:"",
        })
    }

    //获取安全码图片
    getTestVerifyImg=()=>{
        axios.get("http://jizhang-api-dev.it266.com/api/captcha")
            .then((result)=>{
                console.log(result)
                if(result.data.status){
                    this.setState({
                        testVerifyImg:result.data.data.url,
                        captcha_key:result.data.data.key,
                    })
                }
            }).catch((error)=>{
                console.log(error)    
            })
    }

    //设置验证码
    setVerifyCode=(e)=>{
        this.setState({
            verifyCode:e.target.value,
            captcha_code:e.target.value,
        })
    }

    //发送验证码
    sendCode=()=>{
        // console.log(this.state.captcha_code)
        if(this.state.verifyCode===""){
            this.setState({
                codeMessages:"请输入验证码"
            })
            return;
        }
        axios.post("http://xxx/api/sms/verify",qs.stringify({
            mobile:this.props.registerMobile,
            captcha_key:this.state.captcha_key,
            captcha_code:this.state.captcha_code,
        }))
        .then((result)=>{
            if(result.data.status){
                console.log(result)
                this.props.setTestVerify(true);
                this.props.setCaptcha_key(this.state.captcha_key);
                this.props.setCaptcha_code(this.state.captcha_code);
                this.props.setCodeTip("");
                this.props.setCodeSeconds("60s");
                this.props.setResultCodeData(result.data.data)
            }else{
                this.setState({
                    codeMessages:"验证码错误",
                    verifyCode:"",
                })
                this.getTestVerifyImg();
            }
        }).catch((error)=>{
            console.log(error)    
        })
    }

    //提交
    verifySubmit=(e)=>{
            e.preventDefault();
            this.sendCode();
    }

    //创建即获取
    componentDidMount=()=>{
        this.getTestVerifyImg();
    }
                        
    render(){
        return(
            <div id="bg" >
                <div id="testVerify">
                    <h4>安全验证</h4>
                    <form className="verifyFoem">
                        <div className="verifyImg">
                            <img src={this.state.testVerifyImg} alt=""></img>
                            <span onClick={this.getTestVerifyImg}>换一换</span>
                        </div>
                        <input className="verifyCode" type="text" placeholder="请输入验证码"
                            value={this.state.verifyCode}
                            onChange={this.setVerifyCode}
                            onFocus={this.codeFocus}
                        />
                        <div className={"codeMessages"}><span>{this.state.codeMessages}</span></div>
                        <input className="verifySubmit" type="button" value="确认"
                            onClick={this.verifySubmit}
                        />
                    </form>
                </div>
            </div>
        )
    }
    
}

export default TestVerify;
```

