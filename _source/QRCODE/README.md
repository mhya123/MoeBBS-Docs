# 二维码生成器 API

## 概述

通过简单的 HTTP GET 请求生成自定义二维码图片，支持自定义内容和尺寸。

## API 地址

```
GET https://api.hvhbbs.cc/qrcode/
```

## 参数说明

### 查询参数 (Query Parameters)

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
|--------|------|------|--------|------|
| text | string | ✅ | - | 要生成二维码的文本内容 |
| size | number | ❌ | 300 | 二维码大小（像素） |

## 使用示例

### 基本用法

```
https://api.hvhbbs.cc/qrcode/?text=https://www.hvhbbs.cc/&size=100
```

### 参数详解

- **text=https://www.hvhbbs.cc/** - 生成指向网站的二维码
- **size=100** - 设置二维码尺寸为 100x100 像素

## 请求示例

### 直接访问

```
https://api.hvhbbs.cc/qrcode/?text=Hello%20World&size=200
```

### cURL 请求

```bash
curl -o qrcode.png "https://api.hvhbbs.cc/qrcode/?text=https://www.hvhbbs.cc/&size=100"
```

### Python 请求

```python
import requests
from urllib.parse import quote

# 要编码的内容
text_content = "https://www.hvhbbs.cc/"
size = 200

# 构建请求URL
url = f"https://api.hvhbbs.cc/qrcode/?text={quote(text_content)}&size={size}"

# 发送请求并保存图片
response = requests.get(url)
if response.status_code == 200:
    with open('qrcode.png', 'wb') as f:
        f.write(response.content)
    print("二维码生成成功!")
else:
    print("请求失败")
```

### JavaScript 请求

```javascript
// 生成二维码并显示在页面上
function generateQRCode(text, size = 300) {
    const encodedText = encodeURIComponent(text);
    const url = `https://api.hvhbbs.cc/qrcode/?text=${encodedText}&size=${size}`;
    
    // 创建图片元素
    const img = document.createElement('img');
    img.src = url;
    img.alt = '二维码';
    img.style.border = '1px solid #ccc';
    
    // 添加到页面
    document.body.appendChild(img);
}

// 使用示例
generateQRCode('https://www.hvhbbs.cc/', 150);
```

### HTML 直接使用

```html
<!DOCTYPE html>
<html>
<head>
    <title>二维码生成器示例</title>
</head>
<body>
    <h1>二维码生成器</h1>
    
    <!-- 直接在 img 标签中使用 -->
    <img src="https://api.hvhbbs.cc/qrcode/?text=https://www.hvhbbs.cc/&size=200" 
         alt="网站二维码" 
         style="border: 1px solid #ddd; border-radius: 8px;">
    
    <!-- 文本内容二维码 -->
    <img src="https://api.hvhbbs.cc/qrcode/?text=Hello%20World&size=150" 
         alt="文本二维码">
</body>
</html>
```

### PHP 请求

```php
<?php
function generateQRCode($text, $size = 300) {
    $encodedText = urlencode($text);
    $url = "https://api.hvhbbs.cc/qrcode/?text={$encodedText}&size={$size}";
    
    $imageData = file_get_contents($url);
    
    if ($imageData !== false) {
        // 保存到文件
        file_put_contents('qrcode.png', $imageData);
        echo "二维码生成成功!";
    } else {
        echo "生成失败";
    }
}

// 使用示例
generateQRCode('https://www.hvhbbs.cc/', 250);
?>
```

## 常见用例

### 1. 网站链接二维码

```
https://api.hvhbbs.cc/qrcode/?text=https://www.hvhbbs.cc/&size=200
```

### 2. 文本信息二维码

```
https://api.hvhbbs.cc/qrcode/?text=联系方式：电话123456789&size=150
```

### 3. 微信群二维码

```
https://api.hvhbbs.cc/qrcode/?text=微信群：xxxxxx&size=300
```

### 4. WiFi 连接二维码

```
https://api.hvhbbs.cc/qrcode/?text=WIFI:T:WPA;S:网络名称;P:密码;;&size=250
```

## 响应说明

- **成功响应：** 返回 PNG 格式的二维码图片
- **内容类型：** `image/png`
- **状态码：** 200 OK

## 注意事项

1. **文本编码：** URL中的特殊字符需要进行 URL 编码
2. **尺寸限制：** 建议 size 参数在 50-1000 像素范围内
3. **内容长度：** 二维码内容过长可能影响扫描识别度
4. **缓存策略：** 相同参数的请求可能会被缓存

## 错误处理

如果请求参数有误或服务异常，API 会返回相应的 HTTP 错误状态码。建议在使用时添加适当的错误处理机制。

## 技术支持

如有问题或建议，请访问：https://docs.hvhbbs.cc/