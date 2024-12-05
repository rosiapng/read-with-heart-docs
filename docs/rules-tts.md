# 自定义TTS

## 请求处理

### 表达式
| 参数              | 名称   | 说明           |
|:----------------|:-----|:-------------|
| `@{voiceType}`  | 语音标志 | 角色的唯一标识      |
| `@{text}`       | 文本内容 | 需要转换为语音的文字内容 |
| `@{voiceStyle}` | 情感标识 | 角色的情感风格标识 |

### 请求示例

#### GET/POST请求
```javascript
// 请求地址
let url = 'https://www.baidu.com'; 

// 请求参数
let params = {
	type: '@{voiceType}',
	text: '@{text}',
	style: '@{voiceStyle}',
};

// 请求header
let header = {
	token: 'xxx'	
};

// POST请求，具体参考文档 native-to-js
let response = await app.post({
	url,
	params,
	header
});

// 如果responseBody是一个json字符串
let json = app.stringToJson(response.responseBody);

// 获取到语音文件的地址
let voiceUrl = json.url;

return {
	url: voiceUrl, // 语音请求地址
	method: 'GET', // 请求方式
	params: {}, // 携带的参数
	header: {}, // 携带的header
	cookie: {} // 携带的cookie
};
```

#### WebSocket
```javascript
// TODO
```

## 角色配置

### 参数

| 参数        | 名称   | 类型     | 默认值 | 说明           |
|:----------|:-----|:-------|:----|:-------------|
| voiceStyle | 情感标识 | String | -   | 角色的情感风格标识  |
| voiceType | 语音标识 | String | -   | 角色的唯一标识      |
| sex       | 性别   | Number | 0   | 性别 0未知，1男，2女 |
| name      | 名称   | String | -   | 显示的名称        |

### 示例
```json
[
    {
        "voiceStyle" : "gentle", // 情感风格标识
        "voiceType" : "zh-zhangsan", // 语音标识
        "sex" : 1, // 性别 0未知，1男，2女
        "name" : "张三" // 名字
    },
    {
	"voiceStyle" : "sad", // 情感风格标识
        "voiceType" : "zh-erya", // 语音标识
        "sex" : 2, // 性别 0未知，1男，2女
        "name" : "二丫" // 名字
    }
]
```
