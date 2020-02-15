---                                                                         
   layout: post
   title:  "Nodejs"                                                           
   date: 2020-02-12 08:41:00
   categories: Node.js Javescript
---
  
  简单打印实心和空心 正方形、三角形、菱形、梯形、回字

# 打印实心和空心 正方形、三角形、菱形、梯形、回字

## 封装函数

```
 function printTX(draw,content,row,col,hollow){
      switch(draw)
      {
        case "zfx" :
              ZFX(content,row,col,hollow);
        break;
        case "sjx" :
              SJX(content,row,col,hollow)
        break;
        case "lx" :
              LX(content,row,col,hollow)
        break;
        case "hui" :
            HUI(content,row,col,hollow);
        break;
        case "tx" :
            TX(content,row,col,hollow);
        break;
      }
    }
```

## 正方形

```
function ZFX(content,row,col,hollow){
      var tmp = "";
      if(row!=col || row<3){
        console.log("行数不正确或小于3");
        return;
      }
      if(!hollow){
        console.log("打印正方形");
        for(var i=0; i<row; i++)
        {
            tmp="";
            for(var j=0; j<col; j++) 
            {
                tmp+=content;
            }
            console.log(tmp);
        }
      }else{
        console.log("打印空心正方形");
        for(var i=0; i<row; i++)
        {
          tmp="";
          if(i==0 || i==row-1){
            for(var j=0; j<col; j++) 
            {
                tmp+=content;
                tmp+=" ";
            }
          }else{
            tmp+=content;
            tmp+=" ";
            for(var j=0; j<(row-2)*2; j++) 
            {
                tmp+=" "
            }
            tmp+=content;
            tmp+=" ";
          }
            console.log(tmp);
        }
      }      
    }
    console.log('------------- 分隔符 --------------');
```

## 三角形

```
function SJX(content,row,col,hollow){
        if(row!=col || row<4){
          console.log("输入行数错误或行数<4或行列不等");
          return;
        }
        var tmp ="";
        if(!hollow){
            console.log("打印三角形");
            for(var i =0; i<row; i++)
            {
              tmp = "";
              for(var j=0; j<=i; j++){
                  tmp+=content;
                  tmp+=" ";
              }
              console.log(tmp);
            }
        }else{
            console.log("打印空心三角形");
            for(var i=0; i<row; i++)
            {
              tmp = "";
            if(i==0 || i==1 || i==row-1){
                for(var j=0; j<=i; j++){
                  tmp+=content;
                  tmp+=" ";
                }
            }else{
                  tmp+=content;
                  tmp+=" ";
              for(var j=0; j<(i-1)*2; j++){
                 tmp+=" ";
              }
                  tmp+=content;
                  tmp+=" ";
            }
              console.log(tmp);
           }
        }
        
    }
```

## 菱形

```
function LX(content,row,col,hollow){
        var temp ="";
        var tmp;
        var rowtmp=(row-1)/2
        if(!hollow){
          console.log("打印菱形");
          for(var i=-rowtmp; i<=rowtmp; i++){
            temp="";
            tmp = Math.abs(i);
            for(var j = 0; j<tmp;j++){
              temp+=" ";
            }
            for(var k=0; k<row-2*tmp; k++){
              temp+=content;
            }
            console.log(temp);
          }
        }else{
          console.log("打印空心菱形");
          for(var i=-rowtmp; i<=rowtmp; i++){
            temp="";
            tmp = Math.abs(i);
            for(var j = 0; j<tmp;j++){
              temp+=" ";
            }
            if(tmp==rowtmp || tmp ==rowtmp-1){
              for(var k=0; k<rowtmp-tmp+1; k++){
                temp+=content;
                temp+=" ";
              }
            }else{
                temp+=content;
                temp+=" ";
              for(var k=0; k<row-2*tmp-3; k++){
                temp+=" ";
              }
                temp+=content;
                temp+=" ";
            }
            console.log(temp);
          }
        }
      }
```

## 回字

```
function HUI(content,row,col,hollow){
        console.log("打印回");
        var tmp ="";
         for(var i =0; i<row; i++)
         {
            tmp = "";
            if(i==0 || i==row-1){
              for(var j=0; j<row; j++){
                  tmp+=content;
                  tmp+=" ";
              }  
            }else if(i==1 || i==row-2){
                  tmp+=content;
                  tmp+=" ";
              for(var j=0; j<(row-2)*2; j++){
                  tmp+=" ";
              }
                  tmp+=content;
                  tmp+=" ";
            }else if(i==2 || i==row-3){
                  tmp+=content;
                  tmp+=" ";
              for(var j=0; j<2; j++){
                tmp+=" ";
              }
              for(var k=0; k<row-4; k++){
                  tmp+=content;
                  tmp+=" ";
              }
              for(var j=0; j<2; j++){
                tmp+=" ";
              }
                  tmp+=content;
                  tmp+=" ";
            }else{
                  tmp+=content;
                  tmp+=" "; 
              for(var j=0; j<2; j++){
                tmp+=" ";
              }
                  tmp+=content;
                  tmp+=" ";
              for(var k=0; k<(row-6)*2; k++){
                tmp+=" "
              }
                  tmp+=content;
                  tmp+=" ";
              for(var j=0; j<2; j++){
                tmp+=" ";
              }
                  tmp+=content;
                  tmp+=" ";
            }
            console.log(tmp);
         }
    }
```

## 梯形

```
 function TX(content,row,col,hollow){
        if(row<3){
          console.log("请输入行数小于3");
          return ;
        }
        var tmp = "";
        if(!hollow){
          console.log("打印梯形");
          for(var i=0;i<row;i++){
             tmp = "";
             for( var j =0; j<row-1-i; j++){
                tmp+=" ";
             }
             for( var k =0; k<2*i+4; k++){
                tmp+=content;
             }
             console.log(tmp);
          }
        }else{
          console.log("打印空心梯形");
          for(var i=0;i<row;i++){
            tmp = "";
            for( var j =0; j<row-1-i; j++){
                tmp+=" ";
            }
            if(i==0 || i==row-1){
              for( var k =0; k<2*i+4; k++){
                tmp+=content;
              }
            }else{
              tmp+=content;
              for( var k =0; k<2*i+2; k++){
                 tmp+=" ";
              }
              tmp+=content;
            }
            console.log(tmp);
          }
        }
      }

```

## 打印

```
printTX("zfx","#",4,4,false);
    console.log('------------- 分隔符 --------------');
    printTX("zfx","#",4,4,true);
    console.log('------------- 分隔符 --------------');

    printTX("sjx","*",8,8,false);
    console.log('------------- 分隔符 --------------');
    printTX("sjx","*",8,8,true);
    console.log('------------- 分隔符 --------------');

    printTX("lx","#",9,9,false);
    console.log('------------- 分隔符 --------------');
    printTX("lx","#",9,9,true);
    console.log('------------- 分隔符 --------------');

    printTX("hui","x",12,12,false);
    console.log('------------- 分隔符 --------------');

    printTX("tx","#",3,3,false);
    console.log('------------- 分隔符 --------------');
    printTX("tx","#",3,3,true);
    console.log('------------- 分隔符 --------------');
```



