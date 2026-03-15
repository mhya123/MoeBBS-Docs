# RSS API 开发者调用指南

---

## 🔗 API 端点总览

| 端点 | 方法 | 描述 | 认证 |
|------|------|------|------|
| `/health` | GET | 服务健康检查 | 无 |
| `/feeds/moebbs` | GET | 获取论坛RSS | 无 |
| `/validate` | GET | 验证RSS URL有效性 | 无 |

---
## API 地址

```
GET https://rss-api.mahiro.work
```
##  API 详细说明

### 1. MoeBBS 论坛 RSS 

**GET** `/feeds/moebbs`


#### 请求参数

| 参数 | 类型 | 必需 | 默认值 | 说明 |
|------|------|------|--------|------|
| `max_items` | integer | ❌ | 10 | 最大返回条目数 (1-100) |
| `timeout` | integer | ❌ | 10 | 请求超时时间 (1-60秒) |

#### 请求示例
```bash
GET /feeds/moebbs?max_items=5&timeout=15
```

#### 响应格式
与通用RSS解析端点相同，返回 `RSSFeedResponse` 格式。

---

### 2. RSS URL 验证

**GET** `/validate`

验证RSS URL是否有效且可访问。

#### 请求参数

| 参数 | 类型 | 必需 | 默认值 | 说明 |
|------|------|------|--------|------|
| `url` | string | ✅ | - | 要验证的RSS URL |
| `timeout` | integer | ❌ | 10 | 请求超时时间 (1-60秒) |

#### 响应模型
```typescript
interface RSSValidationResponse {
  valid: boolean;
  content_type: string | null;
  is_xml: boolean;
  status_code: number | null;
  error: string | null;
}
```

#### 响应示例
```json
{
  "valid": true,
  "content_type": "application/rss+xml; charset=utf-8",
  "is_xml": true,
  "status_code": 200,
  "error": null
}
```

---

## 💻 代码示例

### JavaScript/TypeScript (Fetch API)

```javascript
// 基础请求类
class RSSApiClient {
  constructor(baseUrl = 'http://rss-api.mahiro.work') {
    this.baseUrl = baseUrl;
  }

  async healthCheck() {
    const response = await fetch(`${this.baseUrl}/health`);
    return await response.json();
  }

  async getRSSFeed(url, options = {}) {
    const params = new URLSearchParams({
      url,
      ...options
    });
    
    const response = await fetch(`${this.baseUrl}/feed?${params}`);
    return await response.json();
  }

  async getmoebbsFeed(maxItems = 10) {
    const response = await fetch(
      `${this.baseUrl}/feeds/moebbs?max_items=${maxItems}`
    );
    return await response.json();
  }

  async validateRSSUrl(url, timeout = 10) {
    const params = new URLSearchParams({ url, timeout });
    const response = await fetch(`${this.baseUrl}/validate?${params}`);
    return await response.json();
  }
}

// 使用示例
const api = new RSSApiClient();

// 获取健康状态
const health = await api.healthCheck();
console.log('服务状态:', health.status);

// 获取moebbs最新5条
const moebbsData = await api.getmoebbsFeed(5);
if (moebbsData.success) {
  moebbsData.items.forEach(item => {
    console.log(`${item.title} - ${item.published}`);
  });
}

// 解析自定义RSS
const customFeed = await api.getRSSFeed(
  'https://example.com/rss.xml',
  { max_items: 10, timeout: 15 }
);
```

### Python (requests)

```python
import requests
from typing import Optional, Dict, Any

class RSSApiClient:
    def __init__(self, base_url: str = "http://rss-api.mahiro.work"):
        self.base_url = base_url
        self.session = requests.Session()
    
    def health_check(self) -> Dict[str, Any]:
        """检查服务健康状态"""
        response = self.session.get(f"{self.base_url}/health")
        response.raise_for_status()
        return response.json()
    
    def get_rss_feed(self, url: str, max_items: Optional[int] = None, 
                     timeout: int = 10, format: str = "json") -> Dict[str, Any]:
        """获取RSS订阅源内容"""
        params = {"url": url, "timeout": timeout, "format": format}
        if max_items:
            params["max_items"] = max_items
            
        response = self.session.get(f"{self.base_url}/feed", params=params)
        response.raise_for_status()
        return response.json()
    
    def get_moebbs_feed(self, max_items: int = 10, timeout: int = 10) -> Dict[str, Any]:
        """获取moebbs论坛RSS"""
        params = {"max_items": max_items, "timeout": timeout}
        response = self.session.get(f"{self.base_url}/feeds/moebbs", params=params)
        response.raise_for_status()
        return response.json()
    
    def validate_rss_url(self, url: str, timeout: int = 10) -> Dict[str, Any]:
        """验证RSS URL"""
        params = {"url": url, "timeout": timeout}
        response = self.session.get(f"{self.base_url}/validate", params=params)
        response.raise_for_status()
        return response.json()

# 使用示例
if __name__ == "__main__":
    api = RSSApiClient()
    
    # 健康检查
    health = api.health_check()
    print(f"服务状态: {health['status']}")
    
    # 获取moebbs最新文章
    moebbs_data = api.get_moebbs_feed(max_items=5)
    if moebbs_data["success"]:
        print(f"获取到 {len(moebbs_data['items'])} 篇文章:")
        for item in moebbs_data["items"]:
            print(f"- {item['title']}")
            print(f"  链接: {item['link']}")
            print(f"  时间: {item['published']}")
            print()
    
    # 验证RSS URL
    validation = api.validate_rss_url("https://www.hvhbbs.de/forums/-/index.rss")
    print(f"RSS验证结果: {'有效' if validation['valid'] else '无效'}")
```
### PowerShell

```powershell
# RSS API客户端类
class RSSApiClient {
    [string]$BaseUrl
    
    RSSApiClient([string]$baseUrl = "http://rss-api.mahiro.work") {
        $this.BaseUrl = $baseUrl
    }
    
    [PSCustomObject] HealthCheck() {
        $uri = "$($this.BaseUrl)/health"
        return Invoke-RestMethod -Uri $uri -Method GET
    }
    
    [PSCustomObject] GetRSSFeed([string]$url, [int]$maxItems = 0, [int]$timeout = 10) {
        $params = @{
            url = $url
            timeout = $timeout
        }
        if ($maxItems -gt 0) {
            $params.max_items = $maxItems
        }
        
        $uri = "$($this.BaseUrl)/feed"
        return Invoke-RestMethod -Uri $uri -Method GET -Body $params
    }
    
    [PSCustomObject] GetmoebbsFeed([int]$maxItems = 10) {
        $params = @{ max_items = $maxItems }
        $uri = "$($this.BaseUrl)/feeds/moebbs"
        return Invoke-RestMethod -Uri $uri -Method GET -Body $params
    }
}

# 使用示例
$api = [RSSApiClient]::new()

# 健康检查
$health = $api.HealthCheck()
Write-Host "服务状态: $($health.status)"

# 获取MoeBBS RSS
$moebbsData = $api.GetmoebbsFeed(5)
if ($moebbsData.success) {
    Write-Host "获取到 $($moebbsData.items.Count) 篇文章:"
    foreach ($item in $moebbsData.items) {
        Write-Host "- $($item.title)"
        Write-Host "  链接: $($item.link)"
        Write-Host "  时间: $($item.published)"
        Write-Host ""
    }
}
```

---

## ⚠️ 错误处理

### HTTP 状态码

| 状态码 | 说明 | 处理建议 |
|--------|------|----------|
| 200 | 请求成功 | 正常处理响应数据 |
| 400 | 请求参数错误 | 检查参数格式和取值范围 |
| 422 | 参数验证失败 | 检查必需参数是否提供 |
| 500 | 服务器内部错误 | 稍后重试或联系管理员 |

### 错误响应格式

```json
{
  "detail": "错误描述信息"
}
```

### 业务错误处理

即使HTTP状态码为200，也需要检查响应中的 `success` 字段：

```javascript
const response = await fetch('/feeds/moebbs?max_items=5');
const data = await response.json();

if (data.success) {
  // 处理成功数据
  console.log(`获取到 ${data.items.length} 条记录`);
} else {
  // 处理业务错误
  console.error(`RSS解析失败: ${data.error}`);
}
```

---

## 🚀 性能优化建议

### 1. 缓存策略
```javascript
class CachedRSSClient extends RSSApiClient {
  constructor(baseUrl, cacheTTL = 300000) { // 5分钟缓存
    super(baseUrl);
    this.cache = new Map();
    this.cacheTTL = cacheTTL;
  }
  
  async getRSSFeed(url, options = {}) {
    const cacheKey = `${url}:${JSON.stringify(options)}`;
    const cached = this.cache.get(cacheKey);
    
    if (cached && Date.now() - cached.timestamp < this.cacheTTL) {
      return cached.data;
    }
    
    const data = await super.getRSSFeed(url, options);
    this.cache.set(cacheKey, {
      data,
      timestamp: Date.now()
    });
    
    return data;
  }
}
```

### 2. 并发请求
```javascript
// 同时获取多个RSS源
const urls = [
  'https://example1.com/rss.xml',
  'https://example2.com/rss.xml',
  'https://example3.com/rss.xml'
];

const promises = urls.map(url => api.getRSSFeed(url, { max_items: 5 }));
const results = await Promise.allSettled(promises);

results.forEach((result, index) => {
  if (result.status === 'fulfilled' && result.value.success) {
    console.log(`RSS源 ${index + 1} 获取成功:`, result.value.items.length);
  } else {
    console.error(`RSS源 ${index + 1} 获取失败:`, result.reason);
  }
});
```

### 3. 超时和重试
```javascript
class RobustRSSClient extends RSSApiClient {
  async getRSSFeedWithRetry(url, options = {}, maxRetries = 3) {
    for (let i = 0; i < maxRetries; i++) {
      try {
        const result = await this.getRSSFeed(url, {
          ...options,
          timeout: Math.min(10 + i * 5, 30) // 递增超时时间
        });
        
        if (result.success) {
          return result;
        }
        
        if (i === maxRetries - 1) {
          throw new Error(result.error);
        }
      } catch (error) {
        if (i === maxRetries - 1) {
          throw error;
        }
        
        // 指数退避
        await new Promise(resolve => 
          setTimeout(resolve, Math.pow(2, i) * 1000)
        );
      }
    }
  }
}
```

---

## API 限制

| 限制项 | 值 | 说明 |
|--------|-----|------|
| 最大条目数 | 100 | 单次请求最多返回100条RSS条目 |
| 超时时间 | 60秒 | 单次请求最长等待时间 |
| 并发连接 | 无限制 | 暂无并发限制 |
| 请求频率 | 无限制 | 暂无频率限制 |

---

## 🤝 支持

- **API文档**: https://docs.mahiro.work
- **邮箱支持**: mhya123@lovemh.me

---

**Happy Coding! 🎉**