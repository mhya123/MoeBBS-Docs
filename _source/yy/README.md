# 一言API调用教程

## 概述

通过简单GET请求获取随机经典语句，支持动画/漫画/影视/诗词等多种分类。

- **公开演示网站：** https://hitokoto.fkme.cyou/
- **API文档：** https://api-hitokoto.fkme.cyou/api-docs/

## API地址

```
GET https://api-hitokoto.fkme.cyou/quote/{type}
```

## 参数说明

### 路径参数 (Path Parameters)

**type** *(必填)*

分类代码：

```
a - 动画    b - 漫画    c - 游戏    d - 文学    e - 原创
f - 网络    g - 其他    h - 影视    i - 诗词    j - 网易云
k - 哲学    l - 抖机灵  al - 全类型
```

### 查询参数 (Query Parameters)

**full** *(选填)*

- 设为 `true` 时返回完整对象（包含作者/出处等信息）
- 默认值：`false`

## 请求示例

### cURL 请求

```bash
curl -X 'GET' \
  'https://api-hitokoto.fkme.cyou/quote/i?full=true' \
  -H 'accept: application/json'
```

### Python 请求

```python
import requests

type_code = 'i'  # 诗词类型
response = requests.get(
    f'https://api-hitokoto.fkme.cyou/quote/{type_code}',
    params={'full': True}
)
print(response.json())
```

### JavaScript (Fetch) 请求

```javascript
fetch('https://api-hitokoto.fkme.cyou/quote/h?full=false')
  .then(response => response.json())
  .then(data => console.log(data.hitokoto))
```

### Java 请求（使用HttpURLConnection）

```java
import java.net.HttpURLConnection;
import java.net.URL;
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class HitokotoExample {
    public static void main(String[] args) {
        try {
            String type = "k";  // 哲学类型
            URL url = new URL("https://api-hitokoto.fkme.cyou/quote/" + type + "?full=false");
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");
            conn.setRequestProperty("Accept", "application/json");
          
            BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line); // 输出示例：{"hitokoto":"存在即合理"}
            }
            reader.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### C++ 请求（使用libcurl）

```cpp
#include <iostream>
#include <curl/curl.h>

static size_t WriteCallback(void* contents, size_t size, size_t nmemb, void* userp) {
    ((std::string*)userp)->append((char*)contents, size * nmemb);
    return size * nmemb;
}

int main() {
    CURL* curl;
    CURLcode res;
    std::string readBuffer;
  
    curl = curl_easy_init();
    if(curl) {
        curl_easy_setopt(curl, CURLOPT_URL, "https://api-hitokoto.fkme.cyou/quote/j?full=true");  // 网易云类型
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, &readBuffer);
      
        struct curl_slist* headers = NULL;
        headers = curl_slist_append(headers, "Accept: application/json");
        curl_easy_setopt(curl, CURLOPT_HTTPHEADER, headers);
      
        res = curl_easy_perform(curl);
        if(res == CURLE_OK) {
            std::cout << readBuffer << std::endl; // 输出完整JSON
        }
        curl_easy_cleanup(curl);
    }
    return 0;
}
```

### PHP 请求

```php
<?php
$type = 'l';  // 抖机灵类型
$url = "https://api-hitokoto.fkme.cyou/quote/{$type}?full=false";

$options = [
    'http' => [
        'method' => 'GET',
        'header' => "Accept: application/json\r\n"
    ]
];

$context = stream_context_create($options);
$response = file_get_contents($url, false, $context);

if ($response !== false) {
    $data = json_decode($response, true);
    echo $data['hitokoto'];  // 输出示例：你说的都对，但我选择Ctrl+CV
} else {
    echo "请求失败";
}
?>
```

## 响应示例

### 精简模式（默认）

```json
{
  "hitokoto": "仰天大笑出门去，我辈岂是蓬蒿人。"
}
```

### 完整模式（full=true）

```json
{
  "id": 1024,
  "hitokoto": "人生如逆旅，我亦是行人。",
  "type": "i",
  "from": "苏轼《临江仙·送钱穆父》",
  "creator": "东坡居士",
  "created_at": "2020-08-15T00:00:00Z"
}
```

## 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| id | number | 语句唯一标识 |
| hitokoto | string | 一言内容 |
| type | string | 分类代码 |
| from | string | 出处 |
| creator | string | 作者 |
| created_at | string | 创建时间 |