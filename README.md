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
5. 使用response.setHeader("Content-Disposition","attachment;filename=文件名");在Firefox浏览器中下载文件，文件名中文乱码问题解决。
- RFC 2183规定FILENAME只能为US-ASCII码，然而现代浏览器中许多已经支持UTF-8编码了，但各个浏览器的支持规则不同。在IE、chrome中，可以直接用FILENAME作为下载文件的名称，但是Firefox却不支持这样。
```java
 public void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	// 获取文件路径并创建一个出入流
	String path = this.getServletContext().getRealPath(
			"/WEB-INF/classes/囧雪.jpg");
	FileInputStream fis = new FileInputStream(path);
	// 创建输出流，向客户端输出数据
	ServletOutputStream sos = response.getOutputStream();
	// 获取文件名
	String fileName = path.substring(path.lastIndexOf('\\') + 1);
	// 文件名转码
	fileName = URLEncoder.encode(fileName, "UTF-8");
	// 告诉客户端以什么解码方式打开文件
	// response.setContentType("UTF-8");
	// 告诉客户端下载文件
	if (request.getHeader("User-Agent").toLowerCase().indexOf("firefox") > -1) {
		response.setHeader("Content-Disposition",
				"attachment; filename*=UTF-8''" + fileName);
		System.out.println("firefox");
	} else {
		response.setHeader("content-disposition", "attachment; filename="
				+ fileName);
	}
	// response.setHeader("content-disposition", "attachment; filename=" +
	// fileName);
	response.setHeader("content-type", "img/jpeg");
	// 输出
	byte[] buf = new byte[1024];
	int len = -1;
	while ((len = fis.read(buf)) != -1) {
		sos.write(buf, 0, len);
	}
	// 关流
	sos.close();
	fis.close();
}
```
