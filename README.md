### tornado-v1.0注释

**IOStream** 对socket的封装，使用IOLoop事件循环时，当读事件时，服务器socket.accept()与客户端建立连接，创建HttpConnection对象(其初始化时，回调self._on_header创建HttpRequest对象，两类相互依赖).
**Application**类对象作为回调，在读取完数据后，被调用处理，调用其__call__方法，对应用初始化时注册的handler进行遍历，根据request.path和注册路径进行正则匹配，从而选择对应的handler类，并调用其method方法(get,post,put, delete等)

socket.recv获取数据
* read_byte：读取指定长度数据， 用来根据http请求头中的content-length读取请求body中数据    
* read_util：读取按照制定字符串结束的所有数据。 使用"\n\r\n\r"作为终止符读取请求数据，再对数据进行"\n\r"分割，即可获取http中的 method, url, http version. 之后的数据安装键值对进行存储。   
  
*Application(tornado.web.Application)** web项目初始化时，添加参数Handler对象。 在handler对象中，self.write或者self.render 将数据保存到self._write_buffer(基类RequestHandler对象变量)中，后面调用self.flush时，将数据通过IOStream传给客户端socket。
self.write调用HttpRequest对象的write方法，HttpRequest调用HttpConnection对象的write，httpConnection对象的write调用IOStream对象的write方法，写如socket中。浏览器在获取socket中的数据后， 呈现在页面上。

