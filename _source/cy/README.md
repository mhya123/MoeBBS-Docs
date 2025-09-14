# MoeBBS API 开发者文档

## 概述
本文档详细说明如何调用 MoeBBS API 接口，包括请求格式、参数说明、响应示例和错误处理。

## 基础信息

### 接口地址
```
https://api.hvhbbs.cc/
```

### 请求方式
- GET: 用于查询数据
- POST: 用于发送数据（如发送警报）

### 响应格式
所有接口统一返回 JSON 格式数据，编码为 UTF-8。

## API 接口详情

### 1. 获取主题信息

**接口说明**: 根据主题 ID 获取主题的详细信息。

**请求方式**: `GET`

**请求 URL**:
```
GET /?action=get_thread&thread_id={thread_id}
```

**请求参数**:
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| action | string | 是 | 固定值: `get_thread` |
| thread_id | integer | 是 | 主题 ID，必须为正整数 |

**请求示例**:
```bash
# cURL 示例
curl -X GET "https://api.hvhbbs.cc/?action=get_thread&thread_id=123"

# JavaScript fetch 示例
fetch('https://api.hvhbbs.cc/?action=get_thread&thread_id=123')
  .then(response => response.json())
  .then(data => console.log(data));

# PHP 示例
$url = 'https://api.hvhbbs.cc/?action=get_thread&thread_id=123';
$response = file_get_contents($url);
$data = json_decode($response, true);
```

**成功响应示例**:
```json
{
    "success": true,
    "message": "Success",
    "data": {
        "thread_id": 123,
        "title": "主题标题",
        "user_id": 456,
        "username": "用户名",
        "post_date": 1234567890,
        "reply_count": 10,
        "view_count": 100,
        "is_sticky": false,
        "is_locked": false
    },
    "timestamp": 1694678400
}
```

---

### 2. 获取资源版本信息

**接口说明**: 根据资源版本 ID 获取资源版本的详细信息。

**请求方式**: `GET`

**请求 URL**:
```
GET /?action=get_resource_version&resource_version_id={version_id}
```

**请求参数**:
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| action | string | 是 | 固定值: `get_resource_version` |
| resource_version_id | integer | 是 | 资源版本 ID，必须为正整数 |

**请求示例**:
```bash
# cURL 示例
curl -X GET "https://api.hvhbbs.cc/?action=get_resource_version&resource_version_id=456"

# JavaScript fetch 示例
fetch('https://api.hvhbbs.cc/?action=get_resource_version&resource_version_id=456')
  .then(response => response.json())
  .then(data => console.log(data));

# Python requests 示例
import requests
response = requests.get('https://api.hvhbbs.cc/?action=get_resource_version&resource_version_id=456')
data = response.json()
```

**成功响应示例**:
```json
{
    "success": true,
    "message": "Success",
    "data": {
        "resource_version_id": 456,
        "resource_id": 789,
        "version_string": "1.0.0",
        "release_date": 1234567890,
        "download_count": 50,
        "file_size": 1024000,
        "changelog": "版本更新说明"
    },
    "timestamp": 1694678400
}
```

---

### 3. 根据邮箱查找用户

**接口说明**: 根据邮箱地址查找用户信息。

**请求方式**: `GET`

**请求 URL**:
```
GET /?action=find_user_by_email&email={email}
```

**请求参数**:
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| action | string | 是 | 固定值: `find_user_by_email` |
| email | string | 是 | 用户邮箱地址，必须为有效的邮箱格式 |

**请求示例**:
```bash
# cURL 示例
curl -X GET "https://api.hvhbbs.cc/?action=find_user_by_email&email=tuder1218@gmail.com"

# JavaScript fetch 示例（注意 URL 编码）
const email = encodeURIComponent('tuder1218@gmail.com');
fetch(`https://api.hvhbbs.cc/?action=find_user_by_email&email=${email}`)
  .then(response => response.json())
  .then(data => console.log(data));

# Java 示例
String email = URLEncoder.encode("tuder1218@gmail.com", "UTF-8");
String url = "https://api.hvhbbs.cc/?action=find_user_by_email&email=" + email;
// 使用 HttpClient 发送请求
```

**成功响应示例**:
```json
{
    "success": true,
    "message": "Success",
    "data": {
        "user_id": 123,
        "username": "username",
        "email": "tuder1218@gmail.com",
        "register_date": 1234567890,
        "last_activity": 1694678400,
        "message_count": 50,
        "trophy_points": 100,
        "is_banned": false,
        "user_group_id": 2
    },
    "timestamp": 1694678400
}
```

---

### 4. 发送警报（暂不开放）

**接口说明**: 向指定用户发送系统警报通知。

**请求方式**: `POST`

**请求 URL**:
```
POST /?action=
```

**请求头**:
```
Content-Type: application/x-www-form-urlencoded
```

**请求参数**:
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| action | string | 是 | 固定值: `` |
| to_user_id | integer | 是 | 接收警报的用户 ID，必须为正整数 |
| alert | string | 是 | 警报内容，不能为空 |
| from_user_id | integer | 否 | 发送警报的用户 ID，默认为 0（系统） |
| link_url | string | 否 | 相关链接地址，必须为有效 URL 格式 |
| link_title | string | 否 | 链接标题 |

**请求示例**:
```bash
# cURL 示例
curl -X POST "https://api.hvhbbs.cc/?action=" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "to_user_id=123&alert=您有新的消息&from_user_id=456&link_url=https://example.com&link_title=查看详情"

# JavaScript fetch 示例
fetch('https://api.hvhbbs.cc/?action=', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
  },
  body: new URLSearchParams({
    to_user_id: '123',
    alert: '您有新的消息',
    from_user_id: '456',
    link_url: 'https://example.com',
    link_title: '查看详情'
  })
})
.then(response => response.json())
.then(data => console.log(data));

# PHP 示例
$data = [
    'to_user_id' => 123,
    'alert' => '您有新的消息',
    'from_user_id' => 456,
    'link_url' => 'https://example.com',
    'link_title' => '查看详情'
];

$options = [
    'http' => [
        'header' => "Content-type: application/x-www-form-urlencoded\r\n",
        'method' => 'POST',
        'content' => http_build_query($data)
    ]
];

$context = stream_context_create($options);
$result = file_get_contents('https://api.hvhbbs.cc/?action=', false, $context);
$response = json_decode($result, true);
```

**成功响应示例**:
```json
{
    "success": true,
    "message": "Success",
    "data": {
        "alert_id": 789,
        "to_user_id": 123,
        "from_user_id": 456,
        "alert_date": 1694678400,
        "status": "sent"
    },
    "timestamp": 1694678400
}
```

---

## 错误处理

### 错误响应格式
```json
{
    "success": false,
    "error": "错误描述",
    "code": 400,
    "details": null,
    "timestamp": 1694678400
}
```

### 常见错误代码
| 状态码 | 错误类型 | 说明 |
|--------|----------|------|
| 400 | Bad Request | 请求参数错误或缺失 |
| 401 | Unauthorized | API 密钥无效或缺失 |
| 403 | Forbidden | 无权限访问 |
| 404 | Not Found | 请求的资源不存在 |
| 405 | Method Not Allowed | 不支持的请求方法 |
| 429 | Too Many Requests | 请求频率超过限制 |
| 500 | Internal Server Error | 服务器内部错误 |

### 错误示例
```json
{
    "success": false,
    "error": "Invalid thread_id parameter",
    "code": 400,
    "details": null,
    "timestamp": 1694678400
}
```

---

## 最佳实践

### 1. 错误处理
```javascript
async function callApi(url) {
    try {
        const response = await fetch(url);
        const data = await response.json();
        
        if (data.success) {
            return data.data;
        } else {
            throw new Error(`API Error: ${data.error} (Code: ${data.code})`);
        }
    } catch (error) {
        console.error('API call failed:', error);
        throw error;
    }
}
```

### 2. 参数验证
```php
function validateUserId($userId) {
    if (!is_numeric($userId) || $userId <= 0) {
        throw new InvalidArgumentException('Invalid user ID');
    }
    return (int)$userId;
}

function validateEmail($email) {
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        throw new InvalidArgumentException('Invalid email format');
    }
    return $email;
}
```

### 3. 频率限制处理
```python
import time
import requests

class ApiClient:
    def __init__(self, base_url):
        self.base_url = base_url
        self.last_request_time = 0
        self.min_interval = 1  # 最小请求间隔（秒）
    
    def make_request(self, endpoint, params=None):
        # 确保请求间隔
        now = time.time()
        if now - self.last_request_time < self.min_interval:
            time.sleep(self.min_interval - (now - self.last_request_time))
        
        response = requests.get(f"{self.base_url}/{endpoint}", params=params)
        self.last_request_time = time.time()
        
        if response.status_code == 429:
            # 处理频率限制
            time.sleep(60)  # 等待1分钟后重试
            return self.make_request(endpoint, params)
        
        return response.json()
```

### 4. 缓存策略
```javascript
class ApiCache {
    constructor(ttl = 300000) { // 默认5分钟
        this.cache = new Map();
        this.ttl = ttl;
    }
    
    get(key) {
        const item = this.cache.get(key);
        if (!item) return null;
        
        if (Date.now() > item.expiry) {
            this.cache.delete(key);
            return null;
        }
        
        return item.data;
    }
    
    set(key, data) {
        this.cache.set(key, {
            data: data,
            expiry: Date.now() + this.ttl
        });
    }
}
```

---

## 测试工具

### 使用 Postman 测试
1. 导入以下 Postman Collection:
```json
{
    "info": {
        "name": "XenForo API",
        "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
    },
    "item": [
        {
            "name": "Get Thread",
            "request": {
                "method": "GET",
                "header": [],
                "url": {
                    "raw": "https://api.hvhbbs.cc/?action=get_thread&thread_id=123",
                    "host": ["api", "hvhbbs", "cc"],
                    "path": [""],
                    "query": [
                        {"key": "action", "value": "get_thread"},
                        {"key": "thread_id", "value": "123"}
                    ]
                }
            }
        }
    ]
}
```

### 使用测试脚本
```bash
#!/bin/bash
# test_api.sh

BASE_URL="https://api.hvhbbs.cc/"

echo "Testing Get Thread API..."
curl -s "$BASE_URL?action=get_thread&thread_id=123" | jq .

echo "Testing Find User API..."
curl -s "$BASE_URL?action=find_user_by_email&email=tuder1218@gmail.com" | jq .

echo "Testing Send Alert API..."
curl -s -X POST "$BASE_URL?action=" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "to_user_id=123&alert=Test message" | jq .
```

```BASH
# test_api.bat

@echo off
set BASE_URL=https://api.hvhbbs.cc/

echo Testing Get Thread API...
curl -s "%BASE_URL%?action=get_thread&thread_id=123"
echo.

echo Testing Find User API...
curl -s "%BASE_URL%?action=find_user_by_email&email=tuder1218@gmail.com"
echo.

echo Testing Send Alert API...
curl -s -X POST "%BASE_URL%?action=" ^
  -H "Content-Type: application/x-www-form-urlencoded" ^
  -d "to_user_id=123&alert=Test message"
echo.
pause

```
---

## 常见问题

### Q: 如何处理中文字符？
A: API 已设置为 UTF-8 编码，确保请求和响应都使用 UTF-8 编码即可。

### Q: 接口有请求频率限制吗？
A: 是的，默认限制为每分钟 60 次请求。可以通过配置调整。

### Q: 如何获取 API 密钥？
A: API 密钥在服务器端配置，客户端无需提供。

### Q: 支持 HTTPS 吗？
A: 是的，建议使用 HTTPS 确保数据传输安全。

### Q: 如何调试 API 问题？
A: 可以使用 `/test.php?action=test` 端点检查 API 状态和配置。
