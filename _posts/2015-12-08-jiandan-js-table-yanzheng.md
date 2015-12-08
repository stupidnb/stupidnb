---
layout:     post
title:      一个简单的js表单验证
keywords: javascript, 表单验证
category : other
tags : [javascript,js对象]
---

*长话短说，刚开始的时候我让这个表单验证整的够呛，虽然用jquery写出来是比较容易，但是用js的面向对象的方法写出来有些吃力，知道后来我听到一句话，javascript里面一切皆是对象。


*说这么多我们就进入正题吧，首先我们写一个表单，就用注册吧，别急这个方法你也可以用在登陆上。

```html

    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <style>
        span{ font-size:12px; }

        body{margin: 0px;}

        .stats1{ color : #ccc; }

        .stats2{ color :black; }

        .stats3{ color :red; }

        .stats4{ color :green; }
        
        #login{
            margin-left:auto;
            margin: 0px;
        }

    </style>


    <form id = "login" method="post" action="reg.php" onsubmit="return regs('click')" >

        用户名：<input type="text" name="username" /><span class="stats1">用户名不能为空</span><br/>

        邮箱：<input type="text" name="email" /><span class="stats1">邮箱不能为空</span><br/>

        密码：<input type="password" name="password" /><span class="stats1">密码不能为空</span><br/>

        确认密码：<input type="password" name="chkpass" /><span class="stats1">密码不能为空</span><br/>

        <input type="submit" />

    </form>


```
*当然我把样式也加里面了，下面就开始写javascript代码。
*首先根据html表我们把提示单独放在一个span标签中，因此我们要将填写的信息与提示信息关联起来。

```javascript

 function spgan(cobj){

            if(cobj.nextSibling.nodeName != 'SPAN'){
                spgan(cobj.nextSibling);
            }else{
                return cobj.nextSibling;
            }
        }
```

*这样就给我们一个返回值，发回的是span标签里面的东西，其实关系不大，但最好写上，接下来我们写一个方法真正操作表单验证的方法，我们定义为check();

```javascript

function check(obj,ifon,fun,click){
             var sp = spgan(obj);

             obj.onfocus = function(){
                sp.innerHTML = ifon;
                sp.className = "stats2";
             }

             obj.onblur = function(){
                if(fun(this.value)){
                    sp.innerHTML = "填写正确";
                    sp.className = "stats4";
                }else{
                    sp.innerHTML = ifon;
                    sp.className = "stats3";
                }
             }

             if(click == "click"){
                obj.onblur();
             }
        }

        onload = res;   //页面加载完之后执行res函数

```

*上面这个方法中，传入四个参数，第一个为我们输入<input>的值，得到之后关联span标签，然后第二个参数为你要提示的值，第三个可以说是一个函数也可以说是一个回调函数。最后一个是点击事件。好了方法写好了最后一步就是实例化，即将每一个空当成一个对象就可以了。

```javascript

function res(click){
            var status = true;
            username = document.getElementsByName('username')[0];
            password = document.getElementsByName('password')[0];
            email = document.getElementsByName('email')[0];
            chkpass = document.getElementsByName('chkpass')[0];

            check(username,"用户名的长度在3-20之间",function fun(val){
                if(val.match(/^\S+$/) && val.length >=3 && val.length <=20){
                    return true;
                }else{
                    return false;
                    status = false;
                }
            },click);

            check(password,"密码必须在6-20位之间",function fun(val){
                if(val.match(/^\S+$/) && val.length >= 6 && val.length <=20){
                    return true;
                }else{
                    return false;
                    status = false;
                }
            },click);

            check(email,"请输入正确邮箱",function fun(val){
                if(val.match(/\w+@\w+\.\w/)){
                    return true;
                }else{
                    return false;
                    status = false;
                }
            },click);

            check(chkpass,"确定密码要和上面一致，规则也要相同",function fun(val){
                if(val.match(/^\S+$/) && val.length >=6 && val.length <=20 && val == password.value){
                    return true;
                }else{
                    return false;
                    status = false;
                }
            },click);
            return status;
        }

```

*这儿的定义status是为了给服务端信息，为了更好的去写下面的东西。这儿就没多大的影响。
好了一个简单的表单验证就写好了，当然这只是其中的一个思路。