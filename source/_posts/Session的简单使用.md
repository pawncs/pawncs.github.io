---
title: Session和简易登录
date: 2020-07-14 18:24:05
tags: 
- note
---

<style>
.title1{
    font-size:36px;
    color:#e7767f;
    /* 桃红 */

}
.title2{
    font-size:29px;
    color:#176f58;
    /* 祖母绿 */
}
.title3{
    font-size:22px;
    color:#21a675;
    /* 石绿 */
}
.title4{
    font-size:15px;
    color:#a8cd34;
    /* 柳绿 */
}
</style>

<div class="title3">关于获取Session</div>

读和写Session分别借助了HttpServletRequest类和HttpServletResponse类。在控制类中添加其为参数，Spring会自动导入。
其中获得session为`request.getSession()`。

<div class="title3">关于修改Session内容</div>

获得Session的内容用`getAttribute`,显然修改内容用的是`setAttribute`。这里放入Seesion的为对象，则其要求可序列化，即继承了`Serializable`接口。

<div class="title4">简易登录界面</div>

~~~java
// com.pawncs.fourth.control.IndexController
@RequestMapping("index")
    public ModelAndView index(ModelAndView modelAndView,HttpServletRequest request){
        HttpSession session = request.getSession();
        UserLoginInfo userLoginInfo = (UserLoginInfo) session.getAttribute("userLoginInfo");
        if(userLoginInfo == null){
            modelAndView.setViewName("login");
        }else{
            modelAndView.setViewName("index");
            modelAndView.addObject(userLoginInfo);
        }
        return modelAndView;
    }
    //这里login和index都用了freemaker模板，其为模板名。

    @RequestMapping("login")
    @ResponseBody
    public Map login(@RequestParam("username")String username,
                     @RequestParam("password")String password,
                     HttpServletRequest request,
                     HttpServletResponse response){
        Map<String,Object> map = new HashMap<>();
        HttpSession session = request.getSession();
        UserLoginInfo userLoginInfo = new UserLoginInfo();
        userLoginInfo.setUserId(session.getId());
        userLoginInfo.setUserName(username);

        session.setAttribute("userLoginInfo",userLoginInfo);
        map.put("code","200");
        map.put("message","登录成功");
        return map;
    }
~~~

关于freemaker的配置还是要说一下，自己总是忘。在导包导完后，还要在application.yml里写入freemaker的配置
~~~yml
# application.yml
spring:
  freemarker:
    suffix: .ftl                    # 设置模板后缀名
    content-type: text/html                      # 设置文档类型
    charset: UTF-8                               # 设置页面编码格式
    cache: false                                 # 设置页面缓存
    template-loader-path: classpath:/templates   # 设置ftl文件路径
~~~