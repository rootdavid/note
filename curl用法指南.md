## curl用法指南

### -A

`-A` 参数指定客户端的用户代理标头，即 `User-Agent`。curl 的默认用户代理字符串是curl/[version]

```bash
curl -A 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36' https://google.com
```

```bash
curl -A '' https://google.com
```

也可以通过`-H`参数直接指定标头，更改`User-Agent`

### -b

`-b` 参数用来向服务器发送 Cookie。

```bash
curl -b 'foo=bar' https://google.com
```

### -c

`-c ` 参数将服务器设置的 Cookie 写入一个文件

### -d

`-d` 参数用于发送 POST 请求的数据体

```bash
curl -d 'login=emma' -d 'password=123' -X POST  https://google.com/login
```

```bash
curl -d 'login=emma＆password=123' -X POST https://google.com/login
```

`-d` 参数可以读取本地文本文件的数据，向服务器发送

```bash
curl -d '@data.txt' https://google.com/login
```

### --data-urlencode

`--data-urlencode `参数等同于`-d`，发送 POST 请求的数据体，区别在于会自动将发送的数据进行 URL 编码

```bash
curl --data-urlencode 'comment=hello world' https://google.com/login
```

### -e

`-e` 参数（或 `--referer`）用于在请求头中添加 `Referer` 字段，表示从哪个页面发起了该请求

```bash
curl -e http://example.com http://httpbin.org/headers
```

### -F

基础用法

```bash
curl -F 'file=@xxx' https://httpbin.org/post
```

添加其他表单字段

```bash
curl -F 'file=@xxx' -F 'name=xxx' -F 'desc=xxx' https://httpbin.org/post
```

指定文件类型

```bash
curl -F 'file=@xxx;type=text/plain' https://httpbin.org/post
```

### -G

`-G` 参数用来构造 URL 的查询字符串

```bash
curl -G -d 'a=1' -d 'b=2' https://httpbin.org/get
```

也可以使用 `--data-urlencode` 编码

```bash
curl -G --data-urlencode 'comment=hello world' https://httpbin.org/get
```

### -H

`-H` 参数添加 HTTP 请求的标头

```bash
curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' https://google.com
```