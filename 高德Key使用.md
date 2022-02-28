# 新版高德地图key和安全密钥的使用说明 

>  需注意：自2021年12月02日升级，升级之后所申请的 key 必须配备安全密钥 jscode 一起使用，即配置key后还需要配合jscode使用。参考文档：https://lbs.amap.com/api/javascript-api/guide/abc/prepare

### 一 、通过代理服务器使用安全密钥【强烈建议】

1. JSAPI key搭配代理服务器并携带安全密钥转发（安全）

	项目入口文件（如：main.js，App.js或index.html）中引入如下代码：

  ```
  window._AMapSecurityConfig = {
  // 例如 ： serviceHost:'您的代理服务器域名或地址/_AMapService',  
   serviceHost:'http://192.168.199.152/_AMapService',   // 其中_AMapService为代理请求固定前缀，不可省略或修改。
  }
  ```

2. 代理服务器的设置

	以 Nginx 反向代理为例，nginx配置中加入自定义地图服务代理配置

  ```
     # 自定义地图服务代理
      location /_AMapService/v4/map/styles {
          set $args "$args&jscode=您的安全密钥"; // $args	这个变量等于GET请求中的参数。例如，foo=123&bar=blahblah;这个变量可以被修改
          proxy_pass https://webapi.amap.com/v4/map/styles;
      }
  ```

> 注解：可以在sever,http,location等标签中使用set命令（非唯一）声明变量，语法如下:
>
> ```
> set $变量名 变量值  （上述示例中即为：set $args "$args&jscode=您的安全密钥"）
> ```
3. 保存相关配置之后需要通过命令`nginx -s reload` 命令重新加载 nginx 配置文件

### 二、直接使用安全密钥【不建议】

> JSAPI key搭配静态安全密钥以明文设置（不安全，建议开发环境用）

项目入口文件（如：main.js，App.js或index.html）中配置密钥：

```
window._AMapSecurityConfig = {
	securityJsCode:'您申请的安全密钥',
}
```
### 示例
此场景需要用到高德自定义地图，引入方式如下

```
window._AMapSecurityConfig = {
  serviceHost:'http://192.168.199.152/_AMapService',  
}

<AMapScene
  option={{}}
  map={{
    style: 'light',
    center: [112, 20],
    token: '4b59c6b4b05d9c4c0cd56062be4763da', //地图密钥，需要平台申请
    style: 'amap://styles/c2a55924fd97612fc6f068228c279c53', // 自定义地图样式
  }}
/>
```





