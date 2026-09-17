# ai_house 板块全景｜高德密钥配置说明

本说明用于部署 ai_house 板块全景的高德地图配置。实际 Key 与安全密钥不写入代码、PRD、原型文件或公开仓库。

## 1. 配置项

| 配置项 | 配置名 | 用途 | 保管位置 |
| --- | --- | --- | --- |
| Web JS API Key | `AMAP_WEB_JS_KEY` | 加载高德地图 JS API 2.0 | 部署环境变量或密钥管理服务 |
| JS API 安全密钥 | `AMAP_SECURITY_JS_CODE` | 高德 JS API 鉴权 | 仅服务端密钥管理服务 |
| 地图服务代理地址 | `AMAP_SERVICE_HOST` | 服务端转发高德服务请求 | 部署环境变量 |
| 生产域名白名单 | `AMAP_ALLOWED_DOMAINS` | 限制 Key 的 Web 调用域名 | 高德开放平台应用配置 |

## 2. 高德开放平台配置

1. 在同一个高德应用中创建或使用“Web 端（JS API）”类型的 Key。
2. 在域名白名单中登记实际生产域名；域名仅填写主机名，不填写协议、路径或端口。
3. 当前公开原型使用的域名为 `jzt-ai.github.io`，如需在原型页加载高德底图，该域名必须在对应 Key 的白名单中。
4. 2021-12-02 后创建的 Key 必须同时配置 JS API 安全密钥。
5. Key 更换、作废或域名变更后，更新部署环境变量并重新发布前端服务；不修改业务代码中的明文值。

## 3. 生产部署方式

生产环境通过服务端代理保存安全密钥，前端加载地图前设置代理地址：

```js
window._AMapSecurityConfig = {
  serviceHost: process.env.AMAP_SERVICE_HOST,
};

AMapLoader.load({
  key: process.env.AMAP_WEB_JS_KEY,
  version: '2.0',
  plugins: ['AMap.Scale'],
});
```

服务端代理将高德地图请求转发到高德服务，并在服务端附加安全密钥。安全密钥不可下发到浏览器、写入 HTML、提交 Git 或输出到浏览器控制台。

## 4. 发布前检查

- Key 类型为“Web 端（JS API）”。
- 实际访问域名已加入高德域名白名单。
- 服务端代理可访问高德地图服务。
- 地图 JS API、真实底图、电子围栏绘制与“定位全部”均可用。
- 浏览器控制台不出现 Key、安全密钥或完整代理鉴权参数。

官方说明：[申请 Key](https://lbs.amap.com/api/javascript-api-v2/prerequisites)、[JS API 加载](https://lbs.amap.com/api/javascript-api-v2/guide/abc/load)、[安全密钥使用](https://lbs.amap.com/api/javascript-api-v2/guide/abc/jscode)。
