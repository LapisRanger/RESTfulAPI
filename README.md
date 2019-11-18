# 博客网站API设计

## 基本访问路径(Root Endpoints)

- 个人所有文章.```https://api.blog.com/users/用户名/articles```会得到一个用户的所有博客的json格式列表
- 查看一篇博客内容```https://api.blog.com/users/用户名/articles/博客id```,返回一篇博客的具体内容,包括标题,文本等.
- 获取博客中所有的comments列表,`https://api.blog.com/users/用户名/articles/博客id/comments`
- 获取某一条comment详情,`https://api.blog.com/users/用户名/articles/博客id/comments/评论id`

## 查询参数(Parameters)

在上面基本链接中加入查询条件,返回数据就是filtered的,如分页功能

格式是`?page=页数&per_page=每页包含数量`

如`https://api.blog.com/users/Peter/articles?page=2&per_page=3`

## HTTP方法

### 上传博客

- 传输方法: POST

- 访问路径:`https://api.blog.com/users/用户名/write`

- json格式:

  ```json
  {
  	"title":"First Blog",
  	"text":"Hello,World!"
  }
  ```

- 返回信息:

  ```json
  {
      "Status":"201 Created",
      "Date":"Mon, 01 Jul 2013 17:27:06 GMT",
      "Message":"You have posted an article",
      "BlogID":"1"
  }
  ```

### 更新博客

- 传输方式:PUT

- 访问路径:`https://api.blog.com/users/用户名/update/博客id`

- json格式:

  ```json
  {
      "message":"update from Peter",
      "content":"...modified content...",
  }
  ```

- 未填必填项,返回422错误信息:

  ```json
  {
      "Status":"422 Unprocessable Entity",
      "Date":"Mon, 01 Jul 2017 17:27:06 GMT",
      "Message":"Invalid request,missing content"
  }
  ```

  无误则返回200完成更新

  ```json
  {
      "Status":"200 OK",
      "Date":"Mon, 01 Jul 2017 17:27:06 GMT",
      "Message":"Blog 1 has been updated by Peter"
  }
  ```

  

### 删除博客

- 传输方法:DELETE

- 访问路径:`https://api.blog.com/users/用户名/delete/博客id`

- json格式:

  ```json
  {
      "message":"delete an article",
      "blogID":"1"
  }
  ```

- 删除成功,返回200:

  ```json
  {
      "Status":"200 OK",
      "Date":"Mon, 01 Jul 2017 17:27:06 GMT",
      "Message":"Blog 1 has been deleted by Peter"
  }
  ```

### 增删改comments

#### 增加comment

- 传输方法:POST

- 访问路径:`https://api.blog.com/users/用户名/articles/博客id/comments`

- json格式:

  ```json
  {
      "body":"Creat a comment from API"
  }
  ```

#### 更改comment

- 传输方法:PATCH

- 访问路径:`https://api.blog.com/users/用户名/articles/博客id/comments/评论ID`

- json格式:

  ```json
  {
      "body":"Updated a comment from API"
  }
  ```

#### 删除comment

- 传输方法:DELETE
- 访问路径:`https://api.blog.com/users/用户名/articles/博客id/comments/评论ID`
- 无传输数据
