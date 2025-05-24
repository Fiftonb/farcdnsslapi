# SSL证书管理接口文档

## 📋 目录
- [基础信息](#基础信息)
- [认证说明](#认证说明)
- [接口列表](#接口列表)
  - [1. 查询SSL证书信息](#1-查询ssl证书信息)
  - [2. 更新SSL证书信息](#2-更新ssl证书信息)
  - [3. 部署SSL证书信息](#3-创建ssl证书信息)
- [JavaScript测试示例](#javascript测试示例)
- [错误码说明](#错误码说明)
- [注意事项](#注意事项)

---

## 🔧 基础信息

**API Base URL:** `https://open.farcdn.net/api/source`

### 测试环境配置
```javascript
const TEST_CONFIG = {
  baseUrl: 'https://open.farcdn.net/api/source',
  accessKeyId: 'u2ZF6k63dFCOS7It',
  accessKey: 'mTGaNRGUFHj3r3YxMrrg5XSGIXd6rBWG',
  sslCertId: 2106
};
```

### 前端演示
- 测试页面：`ssl.html`

---

## 🔐 认证说明

所有接口请求都需要携带以下认证参数：
- `accessKeyId`: 访问密钥ID
- `accessKey`: 访问密钥

**安全提示：** 请妥善保管您的访问密钥，避免在客户端代码中明文暴露。

---

## 📡 接口列表

### 1. 查询SSL证书信息

#### 📍 接口地址
```
POST /findSSLCertConfig
```

#### 🎯 功能说明
根据证书ID和认证信息查询SSL证书的详细配置信息，包括证书状态、域名绑定、有效期等关键信息。

#### 📥 请求参数

| 参数名        | 类型   | 必填 | 说明                    | 示例值               |
| ------------- | ------ | ---- | ----------------------- | -------------------- |
| sslCertId     | int    | ✅   | 证书唯一标识ID          | 2106                 |
| accessKeyId   | string | ✅   | 访问密钥ID              | u2ZF6k63dFCOS7It     |
| accessKey     | string | ✅   | 访问密钥                | mTGaNRGUFHj3r3YxMrrg5XSGIXd6rBWG',

#### 📤 返回参数

**响应结构：**
```json
{
  "code": 200,
  "data": {...},
  "message": "获取成功"
}
```

**data 字段详细说明：**

| 字段名         | 类型     | 说明                           | 示例值                    |
| -------------- | -------- | ------------------------------ | ------------------------- |
| 证书ID         | int      | 证书唯一标识                   | 2106                      |
| 证书名称       | string   | 证书显示名称                   | "example.com"             |
| 证书描述       | string   | 证书描述信息                   | "主域名SSL证书"           |
| 是否启用       | string   | 证书启用状态                   | "是" / "否"               |
| 是否为CA证书   | string   | 是否为CA根证书                 | "是" / "否"               |
| 是否为ACME证书 | string   | 是否为ACME自动申请证书         | "是" / "否"               |
| 证书生效时间   | string   | 证书生效时间（格式化）         | "2024/6/1 12:00:00"       |
| 证书过期时间   | string   | 证书过期时间（格式化）         | "2025/6/1 12:00:00"       |
| 域名列表       | array    | 证书绑定的所有域名             | ["example.com", "*.example.com"] |
| 通用名称列表   | array    | 证书的通用名称列表             | ["example.com"]           |
| 服务器名称     | string   | 关联的服务器名称               | "web-server-01"           |
| OCSP状态       | string   | OCSP（在线证书状态协议）状态   | "正常" / "未启用"         |
| OCSP过期时间   | string   | OCSP响应过期时间               | "2024/12/1 12:00:00"      |
| OCSP错误信息   | string   | OCSP相关错误信息               | "无" / "连接超时"         |

#### 请求示例
```json
{
  "sslCertId": 2106,
  "accessKeyId": "u2ZF6k63dFCOS7It",
  "accessKey": "mTGaNRGUFHj3r3YxMrrg5XSGIXd6rBWG"
}
```

#### ✅ 成功响应示例
```json
{
  "code": 200,
  "data": {
    "证书ID": 2106,
    "证书名称": "example.com",
    "证书描述": "主域名SSL证书",
    "是否启用": "是",
    "是否为CA证书": "否",
    "是否为ACME证书": "是",
    "证书生效时间": "2024/6/1 12:00:00",
    "证书过期时间": "2025/6/1 12:00:00",
    "域名列表": ["example.com", "www.example.com", "*.example.com"],
    "通用名称列表": ["example.com"],
    "服务器名称": "web-server-01",
    "OCSP状态": "正常",
    "OCSP过期时间": "2024/12/1 12:00:00",
    "OCSP错误信息": "无"
  },
  "message": "获取成功"
}
```

#### ❌ 错误响应示例
```json
{
  "code": 400,
  "message": "缺少必要参数",
  "required": ["sslCertId", "accessKeyId", "accessKey"]
}
```

---

### 2. 更新SSL证书信息

#### 📍 接口地址
```
POST /updateSSLCert
```

#### 🎯 功能说明
更新指定SSL证书的完整配置信息，包括证书内容、私钥、域名绑定、服务器配置等。

#### 📥 请求参数

| 参数名         | 类型      | 必填 | 说明                           | 示例值                           |
| -------------- | --------- | ---- | ------------------------------ | -------------------------------- |
| sslCertId      | int       | ✅   | 证书ID                         | 2106                             |
| accessKeyId    | string    | ✅   | 访问密钥ID                     | u2ZF6k63dFCOS7It                 |
| accessKey      | string    | ✅   | 访问密钥                       | mTGaNRGUFHj3r3YxMrrg5XSG...      |
| isOn           | boolean   | ✅   | 是否启用证书                   | true                             |
| name           | string    | ✅   | 证书显示名称                   | "example.com"                    |
| description    | string    | ✅   | 证书描述信息                   | "主域名SSL证书"                  |
| serverName     | string    | ✅   | 关联的服务器名称               | "web-server-01"                  |
| isCA           | boolean   | ✅   | 是否为CA根证书                 | false                            |
| certData       | string    | ✅   | 证书内容（PEM格式）            | "-----BEGIN CERTIFICATE-----..." |
| keyData        | string    | ✅   | 私钥内容（PEM格式）            | "-----BEGIN PRIVATE KEY-----..." |
| timeBeginAt    | int/long  | ✅   | 证书生效时间（毫秒时间戳）     | 1719830400000                    |
| timeEndAt      | int/long  | ✅   | 证书过期时间（毫秒时间戳）     | 1751366400000                    |
| dnsNames       | string[]  | ✅   | 证书绑定的域名列表             | ["example.com", "*.example.com"] |
| commonNames    | string[]  | ✅   | 证书的通用名称列表             | ["example.com"]                  |

#### 📝 请求示例
```json
{
  "sslCertId": 2106,
  "accessKeyId": "u2ZF6k63dFCOS7It",
  "accessKey": "mTGaNRGUFHj3r3YxMrrg5XSGIXd6rBWG",
  "isOn": true,
  "name": "example.com",
  "description": "主域名SSL证书 - 自动续期",
  "serverName": "web-server-01",
  "isCA": false,
  "certData": "-----BEGIN CERTIFICATE-----\nMIIFXTCCBEWgAwIBAgISBExample...\n-----END CERTIFICATE-----",
  "keyData": "-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBExample...\n-----END PRIVATE KEY-----",
  "timeBeginAt": 1719830400000,
  "timeEndAt": 1751366400000,
  "dnsNames": ["example.com", "www.example.com", "*.example.com"],
  "commonNames": ["example.com"]
}
```

#### ✅ 成功响应示例
```json
{
  "code": 200,
  "message": "证书更新成功"
}
```

#### ❌ 错误响应示例
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

### 3. 创建SSL证书信息

#### 📍 接口地址
```
POST /createSSLCert
```

#### 🎯 功能说明
允许非管理员用户创建新的SSL证书配置，包括证书内容、私钥、域名绑定、服务器配置等。此接口专为非管理员用户设计，无需指定userId参数。

#### 📥 请求参数

| 参数名         | 类型      | 必填 | 说明                           | 示例值                           |
| -------------- | --------- | ---- | ------------------------------ | -------------------------------- |
| accessKeyId    | string    | ✅   | 访问密钥ID                     | u2ZF6k63dFCOS7It                 |
| accessKey      | string    | ✅   | 访问密钥                       | mTGaNRGUFHj3r3YxMrrg5XSG...      |
| isOn           | boolean   | ✅   | 是否启用证书                   | true                             |
| name           | string    | ✅   | 证书显示名称                   | "我的SSL证书"                    |
| description    | string    | ✅   | 证书描述信息                   | "用于example.com的SSL证书"       |
| serverName     | string    | ✅   | 关联的服务器名称               | "example.com"                    |
| isCA           | boolean   | ✅   | 是否为CA根证书                 | false                            |
| certData       | string    | ✅   | 证书内容（PEM格式）            | "-----BEGIN CERTIFICATE-----..." |
| keyData        | string    | ✅   | 私钥内容（PEM格式）            | "-----BEGIN PRIVATE KEY-----..." |
| timeBeginAt    | int/long  | ✅   | 证书生效时间（毫秒时间戳）     | 1640995200000                    |
| timeEndAt      | int/long  | ✅   | 证书过期时间（毫秒时间戳）     | 1672531200000                    |
| dnsNames       | string[]  | ✅   | 证书绑定的域名列表             | ["example.com", "www.example.com"] |
| commonNames    | string[]  | ✅   | 证书的通用名称列表             | ["example.com"]                  |

#### 📝 请求示例
```json
{
  "accessKeyId": "u2ZF6k63dFCOS7It",
  "accessKey": "mTGaNRGUFHj3r3YxMrrg5XSGIXd6rBWG",
  "isOn": true,
  "name": "我的SSL证书",
  "description": "用于example.com的SSL证书",
  "serverName": "example.com",
  "isCA": false,
  "certData": "-----BEGIN CERTIFICATE-----\nMIIC...\n-----END CERTIFICATE-----",
  "keyData": "-----BEGIN PRIVATE KEY-----\nMIIE...\n-----END PRIVATE KEY-----",
  "timeBeginAt": 1640995200000,
  "timeEndAt": 1672531200000,
  "dnsNames": ["example.com", "www.example.com"],
  "commonNames": ["example.com"]
}
```

#### ✅ 成功响应示例
```json
{
  "code": 200,
  "data": {
    "sslCertId": 12345,
    "message": "SSL证书创建成功"
  },
  "message": "创建成功"
}
```

#### ❌ 错误响应示例

**参数错误 (400)：**
```json
{
  "message": "请提供以下必要参数: name, certData",
  "missingFields": ["name", "certData"],
  "description": {
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

**认证失败 (401)：**
```json
{
  "message": "获取访问令牌失败"
}
```

**服务器错误 (500)：**
```json
{
  "message": "服务器错误",
  "error": "详细错误信息"
}
```

#### 💡 重要说明
- **非管理员用户专用**: 此接口专为非管理员用户设计，不需要指定 `userId` 参数
- **时间格式**: `timeBeginAt` 和 `timeEndAt` 需要提供毫秒时间戳，接口会自动转换为秒级时间戳
- **证书数据**: `certData` 和 `keyData` 必须是有效的PEM格式证书和私钥
- **数据编码**: 证书和私钥数据会被自动转换为base64编码后发送到后端API
- **数组格式**: `dnsNames` 和 `commonNames` 必须是字符串数组

### 证书管理
- ✅ 更新证书前请确保新证书的有效性
- ✅ 创建证书前请使用验证工具检查证书数据格式
- 🗓️ 定期检查证书过期时间，及时续期
- 🔍 验证域名列表与实际使用的域名一致
- 💾 建议备份重要的证书和私钥数据
- 🆔 创建成功后请记录返回的证书ID，用于后续管理操作

### 创建证书特别提醒
- 📋 **非管理员专用**: `/createSSLCert` 接口专为非管理员用户设计
- ⏰ **时间戳格式**: 创建时需要提供毫秒时间戳，系统会自动转换
- 🔢 **返回证书ID**: 创建成功后会返回新的 `sslCertId`，请妥善保存
- 📝 **完整验证**: 建议使用 `SSLCertValidator` 类进行完整的数据验证
- 🔄 **关联操作**: 创建成功后可使用其他接口进行查询和更新操作

### 开发建议
- 🧪 建议先在测试环境验证所有功能
- 📝 记录关键操作日志，便于问题排查
- 🔔 实现证书过期提醒机制
- 📊 监控API调用状态和响应时间

---

## 🧪 JavaScript测试示例

### 环境配置
```javascript
// 配置文件 - config.js
const API_CONFIG = {
  baseUrl: 'https://open.farcdn.net/api/source',
  accessKeyId: 'u2ZF6k63dFCOS7It',
  accessKey: 'mTGaNRGUFHj3r3YxMrrg5XSGIXd6rBWG',
  sslCertId: 2106
};
```

### 1. 查询SSL证书信息 - JavaScript示例

#### 使用 Fetch API
```javascript
/**
 * 查询SSL证书信息
 * @param {number} sslCertId - 证书ID
 * @returns {Promise<Object>} 证书信息
 */
async function findSSLCertConfig(sslCertId = API_CONFIG.sslCertId) {
  try {
    const response = await fetch(`${API_CONFIG.baseUrl}/findSSLCertConfig`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify({
        sslCertId: sslCertId,
        accessKeyId: API_CONFIG.accessKeyId,
        accessKey: API_CONFIG.accessKey
      })
    });

    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status} ${response.statusText}`);
    }

    const result = await response.json();
    
    if (result.code === 200) {
      console.log('✅ 查询成功:', result.data);
      return result.data;
    } else {
      console.error('❌ 查询失败:', result.message);
      throw new Error(result.message);
    }
  } catch (error) {
    console.error('🚨 请求异常:', error.message);
    throw error;
  }
}

// 使用示例
findSSLCertConfig()
  .then(certInfo => {
    console.log('证书名称:', certInfo['证书名称']);
    console.log('过期时间:', certInfo['证书过期时间']);
    console.log('域名列表:', certInfo['域名列表']);
  })
  .catch(error => {
    console.error('操作失败:', error);
  });
```

#### 使用 Axios
```javascript
/**
 * 使用 Axios 查询SSL证书信息
 */
async function findSSLCertConfigWithAxios(sslCertId = API_CONFIG.sslCertId) {
  try {
    const response = await axios({
      method: 'post',
      url: `${API_CONFIG.baseUrl}/findSSLCertConfig`,
      headers: {
        'Content-Type': 'application/json'
      },
      data: {
        sslCertId: sslCertId,
        accessKeyId: API_CONFIG.accessKeyId,
        accessKey: API_CONFIG.accessKey
      },
      timeout: 10000 // 10秒超时
    });

    if (response.data.code === 200) {
      return response.data.data;
    } else {
      throw new Error(response.data.message);
    }
  } catch (error) {
    if (error.response) {
      console.error('服务器响应错误:', error.response.data);
    } else if (error.request) {
      console.error('网络请求错误:', error.message);
    } else {
      console.error('请求配置错误:', error.message);
    }
    throw error;
  }
}
```

### 2. 更新SSL证书信息 - JavaScript示例

#### 使用 Fetch API
```javascript
/**
 * 更新SSL证书信息
 * @param {Object} certData - 证书更新数据
 * @returns {Promise<Object>} 更新结果
 */
async function updateSSLCert(certData) {
  // 验证必填字段
  const requiredFields = [
    'sslCertId', 'isOn', 'name', 'description', 'serverName', 
    'isCA', 'certData', 'keyData', 'timeBeginAt', 'timeEndAt', 
    'dnsNames', 'commonNames'
  ];
  
  const missingFields = requiredFields.filter(field => !(field in certData));
  if (missingFields.length > 0) {
    throw new Error(`缺少必填字段: ${missingFields.join(', ')}`);
  }

  // 构建请求数据
  const requestData = {
    ...certData,
    accessKeyId: API_CONFIG.accessKeyId,
    accessKey: API_CONFIG.accessKey
  };

  try {
    const response = await fetch(`${API_CONFIG.baseUrl}/updateSSLCert`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify(requestData)
    });

    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status} ${response.statusText}`);
    }

    const result = await response.json();
    
    if (result.code === 200) {
      console.log('✅ 更新成功:', result.message);
      return result;
    } else {
      console.error('❌ 更新失败:', result.message);
      if (result.missingFields) {
        console.error('缺少字段:', result.missingFields);
      }
      throw new Error(result.message);
    }
  } catch (error) {
    console.error('🚨 更新异常:', error.message);
    throw error;
  }
}

// 使用示例
const certUpdateData = {
  sslCertId: 2106,
  isOn: true,
  name: "example.com",
  description: "更新的SSL证书描述",
  serverName: "web-server-01",
  isCA: false,
  certData: "-----BEGIN CERTIFICATE-----\n您的证书内容\n-----END CERTIFICATE-----",
  keyData: "-----BEGIN PRIVATE KEY-----\n您的私钥内容\n-----END PRIVATE KEY-----",
  timeBeginAt: Date.now(), // 当前时间
  timeEndAt: Date.now() + (365 * 24 * 60 * 60 * 1000), // 一年后
  dnsNames: ["example.com", "www.example.com"],
  commonNames: ["example.com"]
};

updateSSLCert(certUpdateData)
  .then(result => {
    console.log('证书更新完成');
  })
  .catch(error => {
    console.error('更新失败:', error);
  });
```

### 3. 创建SSL证书信息 - JavaScript示例

#### 使用 Fetch API
```javascript
/**
 * 创建SSL证书
 * @param {Object} certData - 证书创建数据
 * @returns {Promise<Object>} 创建结果
 */
async function createSSLCert(certData) {
  // 验证必填字段
  const requiredFields = [
    'isOn', 'name', 'description', 'serverName', 
    'isCA', 'certData', 'keyData', 'timeBeginAt', 'timeEndAt', 
    'dnsNames', 'commonNames'
  ];
  
  const missingFields = requiredFields.filter(field => !(field in certData));
  if (missingFields.length > 0) {
    throw new Error(`缺少必填字段: ${missingFields.join(', ')}`);
  }

  // 构建请求数据
  const requestData = {
    ...certData,
    accessKeyId: API_CONFIG.accessKeyId,
    accessKey: API_CONFIG.accessKey
  };

  try {
    const response = await fetch(`${API_CONFIG.baseUrl}/createSSLCert`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify(requestData)
    });

    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status} ${response.statusText}`);
    }

    const result = await response.json();
    
    if (result.code === 200) {
      console.log('✅ 创建成功:', result.message);
      console.log('新证书ID:', result.data.sslCertId);
      return result.data;
    } else {
      console.error('❌ 创建失败:', result.message);
      if (result.missingFields) {
        console.error('缺少字段:', result.missingFields);
      }
      throw new Error(result.message);
    }
  } catch (error) {
    console.error('🚨 创建异常:', error.message);
    throw error;
  }
}

// 使用示例
const certCreateData = {
  isOn: true,
  name: "example.com",
  description: "新创建的SSL证书",
  serverName: "example.com",
  isCA: false,
  certData: "-----BEGIN CERTIFICATE-----\n您的证书内容\n-----END CERTIFICATE-----",
  keyData: "-----BEGIN PRIVATE KEY-----\n您的私钥内容\n-----END PRIVATE KEY-----",
  timeBeginAt: Date.now(), // 当前时间
  timeEndAt: Date.now() + (365 * 24 * 60 * 60 * 1000), // 一年后
  dnsNames: ["example.com", "www.example.com"],
  commonNames: ["example.com"]
};

createSSLCert(certCreateData)
  .then(result => {
    console.log('证书创建完成，ID:', result.sslCertId);
    // 可以继续查询新创建的证书信息
    return findSSLCertConfig(result.sslCertId);
  })
  .then(certInfo => {
    console.log('新证书信息:', certInfo);
  })
  .catch(error => {
    console.error('操作失败:', error);
  });
```

#### 使用 Axios
```javascript
/**
 * 使用 Axios 创建SSL证书
 */
async function createSSLCertWithAxios(certData) {
  try {
    const response = await axios({
      method: 'post',
      url: `${API_CONFIG.baseUrl}/createSSLCert`,
      headers: {
        'Content-Type': 'application/json'
      },
      data: {
        ...certData,
        accessKeyId: API_CONFIG.accessKeyId,
        accessKey: API_CONFIG.accessKey
      },
      timeout: 15000 // 15秒超时
    });

    if (response.data.code === 200) {
      return response.data.data;
    } else {
      throw new Error(response.data.message);
    }
  } catch (error) {
    if (error.response) {
      console.error('服务器响应错误:', error.response.data);
    } else if (error.request) {
      console.error('网络请求错误:', error.message);
    } else {
      console.error('请求配置错误:', error.message);
    }
    throw error;
  }
}
```

#### 证书数据验证工具
```javascript
/**
 * SSL证书数据验证工具
 */
class SSLCertValidator {
  /**
   * 验证PEM格式证书
   */
  static validateCertData(certData) {
    if (!certData || typeof certData !== 'string') {
      return { valid: false, error: '证书数据不能为空' };
    }
    
    if (!certData.includes('-----BEGIN CERTIFICATE-----') || 
        !certData.includes('-----END CERTIFICATE-----')) {
      return { valid: false, error: '证书格式不正确，必须是PEM格式' };
    }
    
    return { valid: true };
  }

  /**
   * 验证PEM格式私钥
   */
  static validateKeyData(keyData) {
    if (!keyData || typeof keyData !== 'string') {
      return { valid: false, error: '私钥数据不能为空' };
    }
    
    const validKeyHeaders = [
      '-----BEGIN PRIVATE KEY-----',
      '-----BEGIN RSA PRIVATE KEY-----',
      '-----BEGIN EC PRIVATE KEY-----'
    ];
    
    const hasValidHeader = validKeyHeaders.some(header => keyData.includes(header));
    if (!hasValidHeader) {
      return { valid: false, error: '私钥格式不正确，必须是PEM格式' };
    }
    
    return { valid: true };
  }

  /**
   * 验证域名格式
   */
  static validateDomainNames(dnsNames) {
    if (!Array.isArray(dnsNames) || dnsNames.length === 0) {
      return { valid: false, error: 'DNS名称必须是非空数组' };
    }
    
    const domainRegex = /^(\*\.)?[a-zA-Z0-9]([a-zA-Z0-9\-]{0,61}[a-zA-Z0-9])?(\.[a-zA-Z0-9]([a-zA-Z0-9\-]{0,61}[a-zA-Z0-9])?)*$/;
    
    for (const domain of dnsNames) {
      if (!domainRegex.test(domain)) {
        return { valid: false, error: `无效的域名格式: ${domain}` };
      }
    }
    
    return { valid: true };
  }

  /**
   * 验证时间戳
   */
  static validateTimestamps(timeBeginAt, timeEndAt) {
    if (!Number.isInteger(timeBeginAt) || !Number.isInteger(timeEndAt)) {
      return { valid: false, error: '时间戳必须是整数' };
    }
    
    if (timeBeginAt >= timeEndAt) {
      return { valid: false, error: '证书生效时间必须早于过期时间' };
    }
    
    const now = Date.now();
    if (timeEndAt <= now) {
      return { valid: false, error: '证书过期时间不能早于当前时间' };
    }
    
    return { valid: true };
  }

  /**
   * 完整验证证书数据
   */
  static validateCertRequest(certData) {
    const validations = [
      this.validateCertData(certData.certData),
      this.validateKeyData(certData.keyData),
      this.validateDomainNames(certData.dnsNames),
      this.validateTimestamps(certData.timeBeginAt, certData.timeEndAt)
    ];
    
    for (const validation of validations) {
      if (!validation.valid) {
        return validation;
      }
    }
    
    return { valid: true };
  }
}

// 使用验证工具的示例
const certDataToValidate = {
  certData: "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----",
  keyData: "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----",
  dnsNames: ["example.com", "www.example.com"],
  timeBeginAt: Date.now(),
  timeEndAt: Date.now() + (365 * 24 * 60 * 60 * 1000)
};

const validation = SSLCertValidator.validateCertRequest(certDataToValidate);
if (validation.valid) {
  console.log('✅ 证书数据验证通过');
  // 继续创建证书
  createSSLCert(certDataToValidate);
} else {
  console.error('❌ 验证失败:', validation.error);
}
```

### 4. 完整的SSL证书管理类

```javascript
/**
 * SSL证书管理工具类
 */
class SSLCertManager {
  constructor(config = API_CONFIG) {
    this.config = config;
  }

  /**
   * 发送HTTP请求的通用方法
   */
  async request(endpoint, data) {
    const url = `${this.config.baseUrl}${endpoint}`;
    
    try {
      const response = await fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        },
        body: JSON.stringify({
          ...data,
          accessKeyId: this.config.accessKeyId,
          accessKey: this.config.accessKey
        })
      });

      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }

      const result = await response.json();
      
      if (result.code !== 200) {
        throw new Error(result.message || '请求失败');
      }

      return result;
    } catch (error) {
      console.error(`请求 ${endpoint} 失败:`, error.message);
      throw error;
    }
  }

  /**
   * 查询证书信息
   */
  async findCert(sslCertId) {
    const result = await this.request('/findSSLCertConfig', { sslCertId });
    return result.data;
  }

  /**
   * 更新证书信息
   */
  async updateCert(certData) {
    const result = await this.request('/updateSSLCert', certData);
    return result;
  }

  /**
   * 创建SSL证书
   */
  async createCert(certData) {
    const result = await this.request('/createSSLCert', certData);
    return result.data;
  }

  /**
   * 检查证书过期时间
   */
  async checkCertExpiry(sslCertId) {
    const certInfo = await this.findCert(sslCertId);
    const expiryDate = new Date(certInfo['证书过期时间'].replace(/\//g, '-'));
    const now = new Date();
    const daysLeft = Math.ceil((expiryDate - now) / (1000 * 60 * 60 * 24));
    
    return {
      expiryDate: certInfo['证书过期时间'],
      daysLeft: daysLeft,
      isExpiringSoon: daysLeft <= 30,
      isExpired: daysLeft < 0
    };
  }

  /**
   * 批量查询多个证书信息
   */
  async findMultipleCerts(certIds) {
    const promises = certIds.map(id => this.findCert(id));
    const results = await Promise.allSettled(promises);
    
    return results.map((result, index) => ({
      certId: certIds[index],
      success: result.status === 'fulfilled',
      data: result.status === 'fulfilled' ? result.value : null,
      error: result.status === 'rejected' ? result.reason.message : null
    }));
  }
}

// 使用示例
const certManager = new SSLCertManager();

// 查询单个证书
certManager.findCert(2106)
  .then(cert => console.log('证书信息:', cert))
  .catch(err => console.error('查询失败:', err));

// 创建新证书
const newCertData = {
  isOn: true,
  name: "example.com",
  description: "新创建的SSL证书",
  serverName: "example.com",
  isCA: false,
  certData: "-----BEGIN CERTIFICATE-----\n您的证书内容\n-----END CERTIFICATE-----",
  keyData: "-----BEGIN PRIVATE KEY-----\n您的私钥内容\n-----END PRIVATE KEY-----",
  timeBeginAt: Date.now(),
  timeEndAt: Date.now() + (365 * 24 * 60 * 60 * 1000), // 一年后
  dnsNames: ["example.com", "www.example.com"],
  commonNames: ["example.com"]
};

certManager.createCert(newCertData)
  .then(result => {
    console.log('✅ 证书创建成功:', result);
    console.log('新证书ID:', result.sslCertId);
  })
  .catch(err => console.error('❌ 创建失败:', err));

// 检查证书过期情况
certManager.checkCertExpiry(2106)
  .then(expiry => {
    console.log(`证书过期时间: ${expiry.expiryDate}`);
    console.log(`剩余天数: ${expiry.daysLeft}`);
    if (expiry.isExpiringSoon) {
      console.warn('⚠️ 证书即将过期，请及时更新！');
    }
  });

// 批量查询证书
certManager.findMultipleCerts([2106, 2107, 2108])
  .then(results => {
    results.forEach(result => {
      if (result.success) {
        console.log(`证书 ${result.certId}:`, result.data['证书名称']);
      } else {
        console.error(`证书 ${result.certId} 查询失败:`, result.error);
      }
    });
  });
```

### 5. 错误处理最佳实践
```javascript
/**
 * 带重试机制的请求函数
 */
async function requestWithRetry(requestFn, maxRetries = 3, delay = 1000) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await requestFn();
    } catch (error) {
      console.warn(`第 ${attempt} 次尝试失败:`, error.message);
      
      if (attempt === maxRetries) {
        throw new Error(`经过 ${maxRetries} 次重试后仍然失败: ${error.message}`);
      }
      
      // 等待后重试
      await new Promise(resolve => setTimeout(resolve, delay * attempt));
    }
  }
}

// 使用示例
requestWithRetry(() => findSSLCertConfig(2106))
  .then(result => console.log('成功获取证书信息'))
  .catch(error => console.error('最终失败:', error));
```

---

## 📊 错误码说明

| 错误码 | 说明           | 常见原因                         | 解决方案                   |
| ------ | -------------- | -------------------------------- | -------------------------- |
| 200    | 请求成功       | -                                | -                          |
| 400    | 请求参数错误   | 缺少必填参数或参数格式不正确     | 检查请求参数               |
| 401    | 认证失败       | accessKeyId 或 accessKey 错误    | 检查认证信息               |
| 404    | 资源不存在     | 指定的证书ID不存在               | 确认证书ID是否正确         |
| 500    | 服务器内部错误 | 服务器异常                       | 联系技术支持或稍后重试     |
| 503    | 服务不可用     | 服务器维护或负载过高             | 稍后重试                   |

---

## ⚠️ 注意事项

### 安全相关
- 🔐 请妥善保管 `accessKeyId` 和 `accessKey`，避免泄露
- 🚫 不要在前端代码中硬编码认证信息
- 🔒 建议使用 HTTPS 协议进行所有API调用
- 🛡️ 定期轮换访问密钥以提高安全性

### 数据格式
- 📅 时间戳参数使用毫秒级Unix时间戳
- 📜 证书内容必须为标准的PEM格式
- 🔑 私钥内容必须为PEM格式，且与证书匹配
- 📝 证书和私钥会在后端自动进行base64编码处理

### 请求限制
- ⏱️ API调用建议设置合理的超时时间（推荐10-30秒）
- 🔄 建议实现重试机制，处理临时网络异常
- 📊 注意API调用频率限制，避免过于频繁的请求

### 证书管理
- ✅ 更新证书前请确保新证书的有效性
- 🗓️ 定期检查证书过期时间，及时续期
- 🔍 验证域名列表与实际使用的域名一致
- 💾 建议备份重要的证书和私钥数据
- 🆔 创建成功后请记录返回的证书ID，用于后续管理操作

### 创建证书特别提醒
- 📋 **非管理员专用**: `/createSSLCert` 接口专为非管理员用户设计
- ⏰ **时间戳格式**: 创建时需要提供毫秒时间戳，系统会自动转换
- 🔢 **返回证书ID**: 创建成功后会返回新的 `sslCertId`，请妥善保存
- 📝 **完整验证**: 建议使用 `SSLCertValidator` 类进行完整的数据验证
- 🔄 **关联操作**: 创建成功后可使用其他接口进行查询和更新操作

### 开发建议
- 🧪 建议先在测试环境验证所有功能
- 📝 记录关键操作日志，便于问题排查
- 🔔 实现证书过期提醒机制
- 📊 监控API调用状态和响应时间