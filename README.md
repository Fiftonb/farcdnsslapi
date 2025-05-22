# SSL证书管理接口文档

```
对接api url: https://open.farcdn.net/api/source

测试用例数据
accessKeyId: u2ZF6k63dFCOS7It
accessKey: mTGaNRGUFHj3r3YxMrrg5XSGIXd6rBWG
sslCertId: 2106

测试前端演示: ssl.html
```


## 1. 查询SSL证书信息接口

### 接口地址

```
POST /findSSLCertConfig
```

### 功能说明

用于根据证书ID和认证信息查询SSL证书的详细配置信息。

### 请求参数

| 参数名        | 类型   | 必填 | 说明           |
| ------------- | ------ | ---- | -------------- |
| sslCertId     | int    | 是   | 证书ID         |
| accessKeyId   | string | 是   | 访问密钥ID     |
| accessKey     | string | 是   | 访问密钥       |

#### 请求示例

```json
{
  "sslCertId": 123,
  "accessKeyId": "your-access-key-id",
  "accessKey": "your-access-key"
}
```

### 返回参数

- code: 状态码（200为成功）
- data: 证书详细信息（见下表）
- message: 提示信息

#### data 字段说明

| 字段名         | 类型     | 说明                 |
| -------------- | -------- | -------------------- |
| 证书ID         | int      | 证书ID               |
| 证书名称       | string   | 证书名称             |
| 证书描述       | string   | 证书描述             |
| 是否启用       | string   | "是"或"否"           |
| 是否为CA证书   | string   | "是"或"否"           |
| 是否为ACME证书 | string   | "是"或"否"           |
| 证书生效时间   | string   | 格式化时间           |
| 证书过期时间   | string   | 格式化时间           |
| 域名列表       | array    | 证书绑定的域名列表   |
| 通用名称列表   | array    | 通用名称列表         |
| 服务器名称     | string   | 服务器名称           |
| OCSP状态       | string   | OCSP状态或"未启用"   |
| OCSP过期时间   | string   | OCSP过期时间         |
| OCSP错误信息   | string   | OCSP错误信息         |

#### 返回示例

```json
{
  "code": 200,
  "data": {
    "证书ID": 123,
    "证书名称": "example.com",
    "证书描述": "示例证书",
    "是否启用": "是",
    "是否为CA证书": "否",
    "是否为ACME证书": "否",
    "证书生效时间": "2024/6/1 12:00:00",
    "证书过期时间": "2025/6/1 12:00:00",
    "域名列表": ["example.com", "www.example.com"],
    "通用名称列表": ["example.com"],
    "服务器名称": "web01",
    "OCSP状态": "未启用",
    "OCSP过期时间": "未设置",
    "OCSP错误信息": "无"
  },
  "message": "获取成功"
}
```

#### 错误返回示例

```json
{
  "code": 400,
  "message": "缺少必要参数",
  "required": ["sslCertId", "accessKeyId", "accessKey"]
}
```

---

## 2. 更新SSL证书信息接口

### 接口地址

```
POST /updateSSLCert
```

### 功能说明

用于更新指定SSL证书的配置信息。

### 请求参数

| 参数名         | 类型      | 必填 | 说明                   |
| -------------- | --------- | ---- | ---------------------- |
| sslCertId      | int       | 是   | 证书ID                 |
| accessKeyId    | string    | 是   | 访问密钥ID             |
| accessKey      | string    | 是   | 访问密钥               |
| isOn           | boolean   | 是   | 是否启用               |
| name           | string    | 是   | 证书名称               |
| description    | string    | 是   | 证书描述               |
| serverName     | string    | 是   | 服务器名称             |
| isCA           | boolean   | 是   | 是否为CA证书           |
| certData       | string    | 是   | 证书内容（PEM格式）    |
| keyData        | string    | 是   | 私钥内容（PEM格式）    |
| timeBeginAt    | int/long  | 是   | 生效时间（毫秒时间戳） |
| timeEndAt      | int/long  | 是   | 过期时间（毫秒时间戳） |
| dnsNames       | string[]  | 是   | 域名列表               |
| commonNames    | string[]  | 是   | 通用名称列表           |

#### 请求示例

```json
{
  "sslCertId": 123,
  "accessKeyId": "your-access-key-id",
  "accessKey": "your-access-key",
  "isOn": true,
  "name": "example.com",
  "description": "示例证书",
  "serverName": "web01",
  "isCA": false,
  "certData": "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----",
  "keyData": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----",
  "timeBeginAt": 1719830400000,
  "timeEndAt": 1751366400000,
  "dnsNames": ["example.com", "www.example.com"],
  "commonNames": ["example.com"]
}
```

### 返回参数

- code: 状态码（200为成功）
- message: 提示信息

#### 成功返回示例

```json
{
  "code": 200,
  "message": "证书更新成功"
}
```

#### 错误返回示例

```json
{
  "code": 400,
  "message": "请提供以下必要参数: certData, keyData",
  "missingFields": ["certData", "keyData"],
  "description": {
    "sslCertId": "SSL证书ID",
    "accessKeyId": "访问密钥ID",
    "accessKey": "访问密钥",
    "isOn": "是否启用",
    "name": "名称",
    "description": "描述",
    "serverName": "服务器名称",
    "isCA": "是否为CA证书",
    "certData": "证书数据",
    "keyData": "密钥数据",
    "timeBeginAt": "证书生效时间",
    "timeEndAt": "证书过期时间",
    "dnsNames": "DNS名称列表",
    "commonNames": "通用名称列表"
  }
}
```

---

## 其他说明

- 所有接口均需携带有效的 accessKeyId 和 accessKey。
- 时间戳参数（timeBeginAt, timeEndAt），后端会自动转换为秒。
- 证书内容和私钥内容会在后端自动进行 base64 编码，无需前端处理。
- 返回的 code 非200时，message 字段会包含详细错误信息。

---
