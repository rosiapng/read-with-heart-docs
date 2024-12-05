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

WebSocket和post的最大区别在于WebSocket的返回值是字符串 socket

```javascript
const ws = app.socket('wss://xxxxx');
// socket连接
ws.open(() => {
    console.log('连接socket成功');
    let message = '{"type": "@{voiceType}", "text": "@{text}"}'
    // 发送消息
    ws.send(message);
});

// 获取text文本信息
ws.text((text) => {
    console.log('返回的text数据:', text);
    const json = JSON.parse(text);
    if (json.event === 'TaskFinished') {
        // 告诉app，消息已经接收完毕
        ws.finished();
        
        // 关闭连接
        ws.close();
    }
});

// 获取二进制消息信息
ws.binary((data) => {
    console.log('返回的二进制数据:', data.length);
    // 将获取到的数据处理后重新推送给app，如果不需要处理该data，直接删除该binary方法
    ws.push(data)
});

// 必定返回值，告诉app这是一个websocket
return 'socket';
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
