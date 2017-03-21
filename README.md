# exercise_of_fileUploadorDownload
1. 进行文件上传时，表单需要做的准备：
- 请求方式为post
- 使用file的表单域
- 使用新的编码方式（二进制的形式上传）
  &lt;form action="uploadServlet" method="post" enctype="multipart/form-data"&gt;
2. 如何修改小工具或框架的源代码？
- 原则：能不修改就不修改
- 修改的方法：
	- 修改源代码，替换jar包中对应的class文件
	- 在本地新建相同的包，并在下面建相同的类，在这个类中修改即可
3. 文件的下载
- 设置contentType响应头：设置响应的类型时什么？

	//通知客户端浏览器：这是一个需要下载的文件，不能再按普通的html的方式打开

	response.setContentType("application/x-msdownload");
- 设置Content-Disposition响应头：通知浏览器不再由浏览器来自行处理（或打开）要下载的文件

	//通知客户端浏览器：不再由浏览器来处理该文件，而是交由用户自行处理

	response.setHeader("Content-Disposition","attachment;filename=文件名");
- 具体的文件：可以调用response.getOutputStream的方式，以IO流的方式发送给客户端。
4. mysql语句
```sql
create table if not exists  upload_files(
id int unsigned not null auto_increment primary key,
file_name varchar(20) not null ,
file_path varchar(1000) not null,
file_desc varchar(30) not null
)engine=myisam auto_increment=1 default charset=utf8;
```
