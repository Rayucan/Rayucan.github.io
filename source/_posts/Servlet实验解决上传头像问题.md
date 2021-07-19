---
title: Servlet实验解决上传头像问题
date: 2021-04-09 11:13:35
tags: Web
categories: 问题解决
---

在使用 JSP/Servlet 做注册登录功能时，信息填写完成点击注册后页面出现500异常

<!-- more -->

经过调试后发现Servlet不能正常获取上传头像图片的photoName值，即`req.getParameter("photo")`为null

上网搜索资料后整理解决方案：

* JSP部分

  **1是提交方式必须是post**

  **2是enctype设置为"multipart/form-data"**

  ```jsp
  <form action="up" method="post"  enctype="multipart/form-data">
      <input type="file" name="myfile">
      <input type="submit" value="上传文件">
  </form>
  ```

  

* Servlet部分

  photoName可以通过request获取Part对象，Part对象的Head中Content-Disposition包含有上传文件名，截取后缀+生成随机UUID字符串拼接成完整的文件名（也是为了防止存储时重名）

  **另外需要加上@MultipartConfig注解**

  ```java
  @WebServlet("/up")
  @MultipartConfig  //使用MultipartConfig注解标注改servlet能够接受文件上传的请求
  public class UploadServlet extends HttpServlet {
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp)
              throws ServletException, IOException {
          Part part = req.getPart("myfile");
          String disposition = part.getHeader("Content-Disposition");
          String suffix = disposition.substring(disposition.lastIndexOf("."),disposition.length()-1);
            //随机的生存一个32的字符串
          String filename = UUID.randomUUID()+suffix;
            //获取上传的文件名
          InputStream is = part.getInputStream();
          //动态获取服务器的路径
          String serverpath = req.getServletContext().getRealPath("upload");
          FileOutputStream fos = new FileOutputStream(serverpath+"/"+filename);
          byte[] bty = new byte[1024];
          int length =0;
          while((length=is.read(bty))!=-1){
              fos.write(bty,0,length);
          }
          fos.close();
          is.close();
      }
  }
  ```



## 自己的代码

```jsp
<form class="form col-md-12 center-block" action="register.do" method="post" enctype="multipart/form-data">
    <div class="form-group">
        <label>用户名</label>
        <input type="text" name="userName" class="form-control input-lg" placeholder="用户名">
    </div>
    <div class="form-group">
        <label>密码</label>
        <input type="password" name="pwd" class="form-control input-lg" placeholder="密码">
    </div>
    <div class="form-group">
        <label>性别</label>
        <div class="radio">
            <label> <input type="radio" name="gender"
                           id="maleRadios" value="male" checked>男
            </label>
        </div>
        <div class="radio">
            <label> <input type="radio" name="gender"
                           id="femaleReadios" value="female">女
            </label>
        </div>
    </div>
    <div class="form-group">
        <label>用户类型</label>
        <select id="userType" class="form-control" name="category">
            <option value="vip"> VIP用户</option>
            <option value="normal"> 普通用户</option>
        </select>
    </div>
    <div class="form-group">
        <label>兴趣</label>
        <div class="checkbox">

            <input type="checkbox" name="interest" value="sports">运动
        </div>
        <div class="checkbox">
            <input type="checkbox" name="interest" value="reading">阅读
        </div>
    </div>
    <div class="form-group">
        <label>头像</label>
        <input type="file" name="photo" class="form-control" placeholder="选择头像文件">
    </div>
    <div class="btn-group pull-right">
        <button class="reset btn btn-default">重置</button>
        <button class="submit btn btn-primary">注册</button>
    </div>	
</form>
```

```java
package servlets;

import entity.User;
import model.UserModel;

import javax.servlet.ServletException;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.Part;
import java.io.*;
import java.util.UUID;

@MultipartConfig
@WebServlet(name = "register",value = "/register.do")
public class RegisterServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf8");
        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf8");

        String photoPath = "D:\\IdeaProjects\\Test2\\src\\main\\webapp\\WEB-INF\\upload";
        Part part = req.getPart("photo");

        String disposition = part.getHeader("Content-Disposition");
        String suffix = disposition.substring(disposition.lastIndexOf("."),disposition.length()-1);
        String photoName = UUID.randomUUID() + suffix;

        InputStream inputStream = part.getInputStream();

        FileOutputStream fileOutputStream = new FileOutputStream(photoPath + "/" + photoName);

        int length = 0;
        byte[] bytes = new byte[1024];
        while ((length = inputStream.read(bytes)) != -1){
            fileOutputStream.write(bytes,0,length);
        }

        inputStream.close();
        fileOutputStream.close();


        User user = new User();

        user.setUserName(req.getParameter("userName"));
        user.setPwd(req.getParameter("pwd")); ;
        user.setGender(req.getParameter("gender"));
        user.setCategory(req.getParameter("category"));
        user.setInterest(req.getParameter("interest"));
        user.setPhoto(photoName);

        UserModel userModel = new UserModel();
        boolean result = userModel.registerUser(user);

        if (result){

            PrintWriter out = resp.getWriter();

            out.println("注册成功，3秒后跳转到登录界面");

            resp.setHeader("refresh","3;url=/login.html");
        } else {
            resp.sendRedirect("/register.html");
        }
    }
}

```



参考资料：https://blog.csdn.net/a3226988/article/details/82685094