# Login API 接口文档

## 概述

本文档描述了用户登录认证接口的使用方法。该接口基于 XenForo 论坛系统的认证 API，提供用户身份验证功能。

## 基础信息

- **接口地址**: `https://auth.hvhbbs.cc/?action=login`
- **请求方式**: `POST`
- **Content-Type**: `application/x-www-form-urlencoded`
- **响应格式**: `JSON`

## 接口说明

### 用户登录

用于验证用户的用户名/邮箱和密码，成功后返回用户信息和会话数据。

#### 请求参数

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `login` | string | 是 | 用户名或邮箱地址 |
| `password` | string | 是 | 用户密码 |

#### 请求示例

```bash
curl -X POST "https://auth.hvhbbs.cc/?action=login" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "login=username&password=userpassword"
```

#### JavaScript 示例

```javascript
const loginData = {
    login: 'username_or_email',
    password: 'user_password'
};

fetch('https://auth.hvhbbs.cc/?action=login', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
    },
    body: new URLSearchParams(loginData)
})
.then(response => response.json())
.then(data => {
    console.log('登录结果:', data);
})
.catch(error => {
    console.error('登录错误:', error);
});
```

#### PHP 示例

```php
$loginData = [
    'login' => 'username_or_email',
    'password' => 'user_password'
];

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://auth.hvhbbs.cc/?action=login');
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($loginData));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($ch);
$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

$result = json_decode($response, true);
```

## 响应格式

### 成功响应

当登录成功时，接口将返回用户信息和会话数据：

```json
{
    "success": true,
    "user": {
        "user_id": 123,
        "username": "testuser",
        "email": "user@example.com",
        "user_group_id": 2,
        "display_style_group_id": 2,
        "permission_combination_id": 5,
        "message_count": 10,
        "conversations_unread": 0,
        "register_date": 1609459200,
        "last_activity": 1609545600,
        "Trophy": {
            "trophy_count": 0
        },
        "Profile": {
            "signature": "",
            "website": "",
            "location": ""
        },
        "Option": {
            "show_dob_date": false,
            "show_dob_year": false
        },
        "Privacy": {
            "allow_view_profile": "everyone",
            "allow_post_profile": "members",
            "allow_send_personal_conversation": "members"
        }
    },
    "session": {
        "session_id": "abc123def456",
        "csrf_token": "xyz789"
    }
}
```

### 失败响应

当登录失败时，接口将返回错误信息：

```json
{
    "success": false,
    "error": "登录失败",
    "errors": [
        "用户名或密码错误"
    ]
}
```

### 错误响应

当请求方法错误或其他系统错误时：

```json
{
    "error": "Invalid method, use POST"
}
```

## 状态码说明

| HTTP 状态码 | 说明 |
|-------------|------|
| 200 | 请求成功 |
| 400 | 请求参数错误 |
| 401 | 认证失败 |
| 405 | 请求方法不被允许 |
| 500 | 服务器内部错误 |

## 注意事项

1. **请求方法**: 必须使用 `POST` 方法，其他方法将返回错误
2. **参数格式**: 请求参数必须以 `application/x-www-form-urlencoded` 格式发送
3. **密码安全**: 建议在生产环境中使用 HTTPS 协议传输密码
4. **登录字段**: `login` 参数可以是用户名或邮箱地址


## 常见问题

### Q: 为什么收到 "Invalid method, use POST" 错误？
A: 确保使用 POST 方法发送请求，不支持 GET 方法。

### Q: 登录失败但用户名密码正确？
A: 检查用户账户是否被禁用，或者密码是否包含特殊字符需要正确编码。