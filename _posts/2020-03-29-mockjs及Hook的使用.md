---
   layout: post
   title: "mockjs及Hook的使用"                                                        
   date: 2020-03-29 09:00:00 +0530
   categories: React Mockjs Hook
---
  React Mockjs Hook


# mockjs及Hook的使用

* article.js

```
import Mock from "mockjs";

Mock.mock(/\/api\/article\/details/,'get',{
    status:true,
    data:[
        {
            id:1,
            title:"既不中看也不中用——二战德国大型战舰的那些失败设计",
            content:"@cparagraph(5)",
            detail:"@cparagraph(50)",
            time:"神大人麦克君 2020-03-28 23:07:26",
        },
        {
            id:2,
            title:"健康中国",
            content:"@cparagraph(3)",
            detail:"@cparagraph(50)",
            time:"国家卫生健康委员会官方账号 2020-03-29 07:52:02",
        },
        {
            id:3,
            title:"一线｜疫情之中一名美国华商的烦恼",
            content:"@cparagraph(6)",
            detail:"@cparagraph(50)",
            time:"中新经纬 2020-03-29 10:05:45",
        },
        {
            id:4,
            title:"国家卫健委：昨日新增确诊45例，其中境外输入44例，本土病例1例（河南1例）",
            content:"@cparagraph(2)",
            detail:"@cparagraph(50)",
            time:"红星新闻 2020-03-29 07:49:35",
        },
        {
            id:5,
            title:"中共中央政治局常务委员会召开会议 习近平主持",
            content:"@cparagraph(4)",
            detail:"@cparagraph(50)",
            time:"2020-03-19 09:04",
        },
        {
            id:6,
            title:"省十三届人大常委会第十九次会议举行",
            content:"@cparagraph(8)",
            detail:"@cparagraph(50)",
            time:"2020-03-28 07:49",
        }            
    ]
})

Mock.mock(/\/api\/article\/comment/,'get',{
    status:true,
    data:[
        {
            id:1,
            username:"一辆自行车1988",
            time:"11小时前",
            Comment:"有法可依,依法办事",
            img:"@image()",
        },
        {
            id:2,
            username:"热心网友",
            time:"15分钟前",
            Comment:"加油干",
            img:"@image()",
        },
        {
            id:3,
            username:"psm岁月",
            time:"1小时前",
            Comment:"努力!!",
            img:"@image()",
        },
        {
            id:4,
            username:"神评论",
            time:"2小时前",
            Comment:"gogogo",
            img:"@image()",
        },
        {
            id:5,
            username:"一品",
            time:"5小时前",
            Comment:"可以的",
            img:"@image()",
        },
        {
            id:6,
            username:"热搜",
            time:"8小时前",
            Comment:"加把劲",
            img:"@image()",
        }
    ]
})




Mock.mock(/\/api\/article/,'get',{
    status:true,
    data:[
        {
            id:1,
            title:"二战德国大型战舰的那些失败设计",
            time:"神大人麦克君 2020-03-28 23:07:26",
            imgUrl:"@image()",

        },
        {
            id:2,
            title:"健康中国",
            time:"官方账号 2020-03-29 07:52:02",
            imgUrl:"@image()",

        },
        {
            id:3,
            title:"一线｜疫情之中一名美国华商的烦恼",
            time:"中新经纬 2020-03-29 10:05:45",
            imgUrl:"@image()",
        },
        {
            id:4,
            title:"国家卫健委：昨日新增确诊45例",
            time:"红星新闻 2020-03-29 07:49:35",
            imgUrl:"@image()",
        },
        {
            id:5,
            title:"中共中央政治局常务委员会召开会议 习近平主持",
            time:"2020-03-19 09:04",
            imgUrl:"@image()",
        },
        {
            id:6,
            title:"省十三届人大常委会第十九次会议举行",
            time:"2020-03-28 07:49",
            imgUrl:"@image()"
        }            
    ]
})
```

* ArticleComments.js
* 是哦也能够hook改造

```
import React, { Fragment, useState, useEffect } from 'react';

import axios from 'axios';

import "../../../assets/css/atricleComment.css";
function ArticleComments(props){

    let [comment,setComment] = useState("");
    
    let [write,setWrite] = useState(false);


    let [commentList,setCommentList] = useState([]);
    
    useEffect(()=>{
            axios.get("/api/article/comment")
            .then((result)=>{
                console.log(result)
                if(result.data.status){
                    setCommentList(result.data.data);
                }else{
                    console.log("出错啦")
                }
            })
            .catch((error)=>{
                console.log(error);
            })
    },[]);


    function commentChange(e){
        setComment(e.target.value);
        setWrite(true);
    }

    function commentOnFocus(){
        setWrite(true);
    }

    function commentOnBlur(){
        setWrite(false);
    }

    

    function onMyKeyDown(e){
        if(e.keyCode===13){
            if(!window.localStorage.getItem("username")){
                alert("请登录!!")
            }else{
               
            }
        }
    }

    function closeComments(){
        props.history.push("/article");
    }



    return(
        <Fragment>
            <div id="articleComments">
                <div className="commentsBox">
                    <div className="top">
                        <span className="iconfont"
                            onClick={closeComments}
                        >&#xe695;</span>
                    </div>
                    <div className="commentsItemBox">
                        <div className="commentsItem">
                            <div className="tip">
                                最新评论
                                <div className="right">
                                    
                                </div>
                            </div>
                            {
                                commentList.map((item)=>{
                                    return(
                                        <div key={item.id} className="items">
                                            <div className="top">
                                                <div className="left">
                                                    <img src={item.img} alt=""/>
                                                </div>
                                                <div className="right">
                                                    <div className="top">
                                                        {item.username}
                                                    </div>
                                                    <div className="bottom">
                                                        <div className="leftTip">新浪微博</div>
                                                        <div className="iconfont">&#xe67c; {item.time}}</div>
                                                    </div>
                                                    <div className="content">{item.Comment}</div>
                                                    <div className="supper">
                                                        <div className="iconfont">&#xe6bf;0</div>
                                                        <div className="iconfont">&#xe603;0</div>
                                                        <div className="iconfont">&#xe7d8;0</div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    )
                                })
                            }
                            
                            <div className={write?'write active':'write'}>
                                <input type="text" placeholder="写评论..."
                                    value={comment}
                                    onChange={commentChange}
                                    onKeyDown={onMyKeyDown}
                                    onFocus={commentOnFocus}
                                    onBlur={commentOnBlur}
                                />
                            </div>
                        </div>
                    </div>
                </div>
            </div>
           
        </Fragment>
    )

}

export default ArticleComments;
```

