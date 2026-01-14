# 调研小结及画布引擎可能的优化方向

1.常用的功能可以固定为节点：如意图识别、重要信息储存等

2.可以增加敏感词审查功能，api审查或关键词审查，为意图识别提示词减负

3.可以支持工作流作为节点导入，分块编排，提高复杂工作流搭建效率

4.在秒回中支持高质量对话标注，避免导出所有对话作为知识库

5.优化试运行结果展示方式

6.对”历史消息“会话属性进行数据提取（也可以手动使用js代码块）

7.增加user消息类型，提高模型兼容性 （以上部分建议在正文中有标黄处理）

# Dify、coze、句子功能对比表格

- Coze的tob版本-Hiagent商业化：私有化部署，年付80万/年起、买断200万起，与coze.cn专业版功能上暂无较大区别。
    

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|功能|子功能|[dify.ai](http://dify.ai)|[coze.com](http://coze.com)|[coze.cn](http://coze.cn)|句子|
|LLM|支持模型|社区版：市面上的开源/闭源模型基本都支持，需要自行配置key 付费订阅：OpenAI/Anthropic/Llama2/Azure OpenAI/Hugging Face/Replicate(部署开源模型的云计算平台)|Claude、Gemini、Openai三大国际主流模型api|普通版：豆包、通义千问、智谱、kimi、百川、minimax的api 专业版：支持调用火山方舟模型资源（自部署豆包、kimi、智谱开源模型、mistral、llama，支持微调） 「豆包模型系列」：有角色扮演、声音克隆、语音识别、语音生成、文生图、图生图、embedding、Function Call等系列|国内国际主流模型  <br>国际：openai全系列模型、Claude-3-haiku、Claude-3-sonnet、Claude-3.5-sonnet 国内：智谱、百川、minimax、kimi、文心一言、混元、通义千问、deepseek 我们没有、别人有的：Gemini、豆包 大家都没有的：零一万物、阶跃星辰|
|插件||dify官方和社区开发者提供（数量有限）|插件商店（coze官方和开发者上传，非常丰富）|插件商店（coze官方和开发者上传，非常丰富）|PE定制编写|
|agent分发||webapp、iframe、api|bot商店、api调用、websdk、discord、telegram、messenger、line、ins、slack、lark|bot商店、api调用、websdk、豆包、飞书、抖音、微信客服、小程序、公众号、掘金、飞书多维表格|企微、whatsapp、微信客服、小程序、公众号、飞书多维表格|
|多agent||❌|✅（将agent视为节点进行编排）|✅（将agent视为节点进行编排）|❌（将要做）|
|开场白||✅|✅|✅|❌（chatbotui使用场景较少）|
|快捷指令||✅（和开场白一块）|✅（常驻输入栏上方）|✅（常驻输入栏上方）|✅（可驻输入栏上方）|
|下一步问题建议||✅|✅|✅|❌（chatbotui使用场景较少）|
|触发器||❌|✅在分发平台触发（定时触发、事件触发（webhook，外部向url发送请求时触发）|✅在分发平台触发（定时触发、事件触发（webhook，外部向url发送请求时触发）|✅结合企微（新加好友、事件、标签变更）|
|工作流商店||✅dify官方、开发者上传|✅|✅|❌（非to c，无相关设计）|
|图片流商店||❌|❌|✅（抠图，添加文字、文生图等）|❌（非to c，无相关设计）|
|知识库||文本类型|文本、表格、图片（语义描述匹配）|文本、表格、图片（语义描述匹配）|文本（可以上传表格，但是是转为文本段落）|
|节点|工作流作为节点|✅|✅|✅|❌（将要做）|
|迭代/循环|✅|✅|✅|❌（将要做）|
|意图识别|✅|✅|✅|✅（可以通过大模型+规则中心，PE手写实现）|
|文本格式处理|✅统一格式为字符串|✅拼接内容为字符串；或分离字符串为数组|✅拼接内容为字符串；或分离字符串为数组|✅（可借助js代码块处理）|
|问答节点|❌|✅预设问题，大模型在对话中向用户提问，收集用户喜好|✅（预设问题，大模型在对话中向用户提问，收集用户喜好）|✅（PE手写prompt实现）|
|记忆|✅（变量）|✅变量、数据库（以表格结构存储数据，NL2SQL）、总结聊天历史、filebox增删查改「通过自然语言修改文件名、移动文件、检索多模态」|✅变量、数据库、总结聊天历史、filebox增删查改「通过自然语言修改文件名、移动文件、检索多模态」|✅（可以通过赋值实现记忆，支持数字，字符串，布尔三种类型）|
|流式输出||✅|✅|✅|❌（主要场景在企微，意义较小）|
|输入格式||图片、文字、（将支持文件）|文字、文件（图像、PDF、DOCX、Excel、CSV 或音频。）|文字、文件（图像、PDF、DOCX、Excel、CSV 或音频）|文字、图像、PDF、DOCX、Excel、CSV|
|批处理||✅webapp批处理，工作流迭代节点|✅llm批处理节点，循环节点|✅llm批处理节点，循环节点|✅测试中心可以批运行、但需逐个导入|
|背景图像||✅源代码定制前端|✅c端客户友好、可自由添加背景图|✅c端客户友好、可自由添加背景图|✅非toc，可定制|
|文本转语音/语音转文本||✅配置声音模型api-key|✅字节的自有声音模型，可上传音频，可电话|✅字节的自有声音模型，可上传音频，可电话|✅（非toc。PE可用语音转文本API处理音频。客户不可用）|
|商业化||订阅制（专业版、团队版）商业版（定制服务）|订阅制（9、19、39美元档次，限额不同）、单独购买消息次数、api调用tokens付费|免费版（限额） 专业版（bot调用次数、方舟模型token消耗费用，SaaS） 专业版： 团队空间：团队数量10个、团队人数100人 知识库空间：空间10GB Bot as API：按量计费支持专业级SLA（服务等级协议）支持调用方舟模型资源 智能体调用：扣子专业版按量-小时结|SaaS、api的 tokens调用消耗|
|运行限制||社区版有负载均衡限制、坐席限制|bot运行时间限制2分钟(同步运行)|bot运行时间限制2分钟(同步运行)、图像流可以设置为异步运行|后台对上下文窗口、输出长度有限制（防止浪费）可以调整|

1. # Dify节点概览
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2NhMTNlYjBlYTFlZTg2N2IyYTdmNzkxOGIyYWE3MDFfWklxdnpTcGNNZ2ZKbERZR080ZnJrRnlTTlppQU5MSFRfVG9rZW46UjRLN2JBZWR3b1FvU1J4WWFlSWN4WTJDbmZjXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

  

- 知识检索
    

支持手动对文本分段后embedding和大模型生成QA对，可设置Rerank模型对召回内容重排

- 问题分类器
    

“意图识别”固定成节点

- 迭代
    

对数组执行多次步骤输出所有结果。

- 代码执行
    

支持python3和node.js

- 模板转换
    

使用jinjia把数据转化为字符串

- 变量聚合器
    

将多路分支变量聚合成一个变量

- 变量赋值
    

储存重要信息

- 参数提取器
    

使用大模型从文本中提取结构化数据

- HTTP请求
    

通过服务器发送HTTP请求

1. ## LLM节点
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MmQyNDNmYzhhMGQzOTQ1YWVmMGVkZDdiNTBjOGYwZTJfZkJKY2ZNRDBSNnByZmk3cDUxeGFvdVdNcEVzVG16eWRfVG9rZW46VjJDWWJGdHE4b1hGaEp4MHpPT2NhUXp0bmNjXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

- 上下文
    

将上游变量赋值给“上下文”节点，在提示词内引用

- 消息类型
    

system、user、assistant

支持设置并自由添加3种类型的消息类型

句子的画布引擎提示词栏提示词类型为system，而部分大模型厂商的模型api调用方式必须携带user类型，所以画布引擎的部分模型使用报错。

- 记忆
    

即携带多少条（user，assistant）历史消息

句子的画布引擎的引用历史=dify的记忆

其中句子有个叫历史消息的会话属性，是以json格式传递给提示词栏，还携带了时间数据，例如 [{"role":"user","content":"hello","time":"2024-08-16T09:58:40.380Z"},{"role":"assistant","content":"Hello! How can I assist you today?","time":"2024-08-16T09:58:42.462Z"}]

个人觉得杂乱的json数据可能影响模型推理能力，且不便于阅读，或许可以提取json数据为如下格式作为历史消息。 User: Assistant:

  

2. ## 知识检索
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDE3ZTgwZDhiZGM4MDhkNDljY2Y1ZGEyMTlmOTQ4MTFfY2xWUllzWWhHb2JlZnByV2RTWEhYR3hYaGR4VnhCUFZfVG9rZW46R0xmeGIycU9Kb0h2b0N4RzdOTmNpWnpCbllmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NjdjZDYxZGJkNzczMzRkZTAzNTExMzVmMjAzZDIxMzVfbGE5aUtYVHJoZW5mMlhhNnJRc0ROUHhoemNKSlRvaDZfVG9rZW46UGY5TmJEZUZrb2NxdXZ4SWVXUGNsWFlxbkF1XzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

  

文本导入

- 导入文本：支持 TXT、 MARKDOWN、 PDF、 HTML、 XLSX、 XLS、 DOCX、 CSV
    
- 同步Notion内容
    
- 同步web站点：Firecrawl爬虫
    

  

  

  

文本处理

- 支持手动设置处理规则，机械分段
    
- 支持使用大模型对导入文本生成QA对
    

  

  

  

  

  

  

  

  

  

  

  

检索方式

- 向量检索（索引段落）
    
- 全文检索（索引词汇）
    
- 混合检索（索引段落+索引词汇）
    
- 可以配置Rerank模型，计算召回文本和问题的语义匹配度后进行重排序
    

3. ## 问题分类器
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NzM3NTc1NDU4YWFhZjlkYTdiYzQyNmYyNDgyZTE5MDNfbGZab2Zobll6bGN3MGFWVVk3dG9wTkFBcWlYUjJrUFdfVG9rZW46TloxWmI5Rndyb1JzQnB4M2FUaWMySUtNbnNmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

- 把意图识别固定成节点
    
- 可设置提示词指导分类
    

  

4. ## 迭代
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NzY0MTUyMmRjNjc3ZTdmZmY0NTMxN2Q0MDQ0ZWI2ZGNfSk1UdXVRU3ZaWlZBd1dKUHV3aFF4VzdrcUlmeTZ1T2ZfVG9rZW46S0ZZRGJ5N3ZFb1ZyV0F4dGlFNWNVSHFKbktmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ODc0NzU3YjkwOTExMzUyZDgyNzdjYzVmZTJiOTdjMjZfR3phb1hPOUFZWUNHRWVud0FScXp0NnRBUGtzM0sydjRfVG9rZW46STNRd2JhNVlQb2FyY1R4ZHFFRGNuenZRbnVkXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

- 输入数组变量，对数组执行多次步骤直至输出所有结果。迭代步骤在列表中的每个条目（item）上执行相同的步骤。
    
- 可以用于长文本写作场景
    

  

5. ## 数据转换类节点
    

6. ### 代码执行
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NDQ5YmFlYWFlZGE1OGM2OGJjNTQ1MWM1Nzg2YjMzMjBfZWc4R3lYUEpWVWFPTzJYVmtia29IVUdwYldSUXVFYVdfVG9rZW46WFU5VGI2V3VNb29uczV4ek5nTGNpNGF2bkczXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

2. ### 模板转化
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MjQ0ODQwY2NiYzYwYTRiYWE3N2FmYjE4MWNmMTUwYzZfVFdOY1JFOXlZRUtiWks2VHJVbzdzUExRRGxteVRDdndfVG9rZW46VnFLNWI0SkdNb1JsazZ4VXBlMmNuUlhiblliXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

3. ### 变量聚合器
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NjgzM2MwZDk3OWFhMmU1YjJmMzJhNzM3ZTAwMzFjOGJfbU5ySTR3b2hrdGxpamRnb0Q4amNYNnFkV3puYlMzRWtfVG9rZW46WFdyS2J4VDZYb1FqODd4NHNnb2NaZVFVbjlmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

4. ### 变量赋值
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=YTMzZmIyNTRiMzY1OThlZmFlMmZiODM5NjBmOWQ3MjRfYkRlU3NsWnVpMnlJYXlzbW1RNmpnM2ZWa1ZTT0xsWGJfVG9rZW46SjFCZ2JwaXE3b2FiTXZ4bnRCcWNPeGNqbkJjXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MWFiZjIyZGIzYmI2NjQ3ZDYyODM4YzQxMDUyNWNjMjBfMlRRMkxtSWttZ2tqYnZ1eG1CSVlRUGo0YUlHVVpHTVlfVG9rZW46QndZYmJXUnFBb1YzN054ek5uNGNFNmR0bk1oXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

5. ### 参数提取器
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ODc5ODczZTdlODFiNGJhNDc3OGZmZGI0N2IxNmY0ODlfOHc2R3ZpY1Q5VEVEWHZDdlNkZ2w1MWxyZ0d0dTlXd0tfVG9rZW46WEdGTmJXMUNmb3BFR3l4bjR5S2MwMjJqbk5mXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

  

- 支持python3、nodejs两种语言
    

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

- 使用 Jinja 模板语法将数据转换为字符串
    

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

- 可以分类聚合上游变量，便于下游节点配置
    

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

- 预先设置变量，可以提前写入信息，或用于储存工作流中生成的信息：如用户偏好，收集用户数据、总结的对话历史等。可读可写。
    
- 例如收集病人信息，如下，通过提取结构化数据后，写入年龄，性别、病症等数据储存
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NTg0MTNiYTkxYmU4ZWU3OTNjZTRiNTNhODM2YzM4YzNfWnBrWDJGUmFHRVl3bmNuRnBwMUdBQkxUUDhzd05YY25fVG9rZW46S24zNWJmRWpOb2FLc3J4a25xaGNjZGZPbjBmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

  

  

  

  

  

  

  

  

  

- 把结构化数据处理固定为节点。
    
- 利用 LLM 从非结构化文本提取出结构化参数
    

  

  

6. ## HTTP请求
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDgwOGQxNTk4YTFkOTljM2NhZTA2MGJhOTZiMDY1ZGRfb0gzT0VlZmZoZE1vNFlpT1poMDMyc1daNGk2aTdIdEZfVG9rZW46R3lrT2IwYlRLb3ZJcGp4Z29XV2M4dVJ4bjR4XzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

- 允许通过 HTTP 协议发送服务器请求，适用于获取外部数据、webhook、生成图片、下载文件等情景。
    

  

2. # Dify应用类型
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=Zjk3Yzg2M2E0N2FkMGEzOGQ1NDJjMjUxNmZjMTE4YjlfUGpsbVRlOXFYdlNiSGxtMlFPZkFWekhkM21KODlOODhfVG9rZW46V0NrMGJWeXp3b3RwdHR4eDQxN2M2Zk1vbnJoXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

1. ## 聊天助手
    
    1. ###   基础编排
        
    
    ![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MmY5OTdlYmYyMzE3ODUxZGQwMTJmZjlkMDdmMDk1MGRfNGQ1RjFmR21aMDYwcDEzTDRuaThPckFNWlQ1ZXRXZWpfVG9rZW46R1psTWI5UkwzbzlnZGh4aHdSN2M3Z0pObmdiXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)
    
    2. ###   工作流编排
        
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NWI2Y2UyNGJlZjg4NWRiODNlMzFhMDg1OWI0OGE3MjdfRWN2TnZWRGdYaHdYelpRTkxXMzlqRjJDVHNnenBucVNfVG9rZW46UUNRZmJkY2R3b1k0UWJ4TnF4R2NLVXFhblJiXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=YWQzZDhjNDVmYmJmMzViMDhlZDY5YTk2YzE4ZjhjNjBfaVJWUWl0bWtDejdYSk0yTlYxWFJ2V1hFN1pEWWJETTBfVG9rZW46WXRZZGJoQ3Q5b0l2NDR4ODFDN2NFV1JCbmplXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

2. ## 文本生成应用
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NDRiZjc4ZjlkODM2NDc5NWYxYzk0ZTU3YjNhMjYzZWZfZnJGa2FZeGFjY01LSFFOSWNDQ1hsMU9YYTVvNGhGRWhfVG9rZW46U3FJc2JZV0hjb2dHWTZ4S2hBN2NBWlFjbjhnXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

3. ## Agent
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MmYxOTQ1ZmJkMWZhMmZiMjA0MWFkMTIzYmZmZjMxMTdfR0x5OW8yMmFWTlJ6anBuTFNYSXRHTWNncG9rY0NuWTdfVG9rZW46T1Z4OGJ1RGRBb2daYlB4QjV4MWNOVWJYbnRoXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

4. ## 工作流
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGU4MWY2NzM3Yzc4OGVjMmQ1OWMxMjM4Y2Q3MzIyMzlfQjdaMWN5eWR5bFRSckE3RXRKU3UwVFhQYUlFVWl5QkZfVG9rZW46WmxHZWJidVgyb080Mmt4ZzVhZWNHdjJkbnBlXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MDNhMzFlYzQ1N2NlM2NhYzEyNmFiYTBkMDRhNGIyNTRfR0VUcjdNVFFyclJCMXJreUlCc21TWjVEYW9wY0F2VUxfVG9rZW46S2dnWGJERWZ1b01ERWF4a2lBTGNCcHRabmNlXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=N2Q4MzJmOTZlNDdiMDYxZjhkMjk2ZDZkZDFkZmExYjNfcURGeFdrUGFFUW1FVGlPUkNOZFhxMHR6MXhuS2RVUVdfVG9rZW46RUVBa2JEeEFpb0d3bDV4ZzNvZ2NEVnFUbmNmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NDIzOTE4MzdkYjUwYWNlOTQyYTE5NTc2NTM4MTBhYjNfMlBRWEliRDRyWjN0RDhLbGdBa1d2dkI3U0ZvV3BLMldfVG9rZW46RXNNQ2JEcVpLb29xSjZ4NlM2UmM2eGlmbnVoXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

5. ## 应用功能
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NWEwOWU0YWUzZDkyMmEwNjU5ODZhYTQ2ZDg5ZDljOWZfRmNOdzRXT1BXZHhCVlhYU3NjU3pqOVdCVVFYQ2RNVk9fVG9rZW46TDIzSWJ0emJhb081Ymt4RFdkNWNFRWpIbmhjXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

- 基础编排
    

最简单的chatbot

  

  

  

  

  

  

  

  

- 工作流编排
    

将工作流嵌入chatbot，支持多轮对话，每次交互均调用工作流

  

  

  

  

  

  

  

  

- 对话日志
    

  在侧边栏追踪运行工作流的运行详情和结果，折叠式的设计，节省屏幕空间，提高可视化效果

- 预览
    

在对话框内进行试运行，可以追踪运行过程和结果

- 或许画布引擎可以参考这种预览方式设计
    

  

  

  

  

  

  

  

  

  

  

  

  

- 文本生成应用
    

基础的提示词chatbot，可以在提示词中插入变量，只能单次交互，不能多轮对话

  

  

  

  

  

  

  

  

- 没有工作流的agent，由大模型依据提示词选择工具调用
    

  

  

  

  

  

  

  

  

  

  

  

- 设置若干入参和出参，不支持多轮对话。
    

  

  

  

  

  

  

  

  

  

  

- 可以发布为工具，从而嵌入其他工作流或聊天助手。
    

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

- 如果在句子画布引擎中添加内容审查功能，可以缩减意图识别中对"敏感"内容不回复的提示词。
    

  

- 标注回复，对对话中的高质量问答对进行增量embedding。或许可以在秒回中加入这一场景，由客户选择高质量对话进行标注。
    

  

  

  

  

  

  

  

  

  

  

  

  

3. # 应用发布
    

4. ## webapp发布
    

5. ### 对话型应用
    

Agent、聊天助手等需要多轮对话的webapp

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MWMyNDY5NzE3YjZiZWFmYzI1MWZiMzMxMDc5OTgxYjBfZk1WZmRsREd4Znd3QWp4WEJZTVlaMlVsS09jZGZPSWRfVG9rZW46RkZvMmJEVGlEb3dRNXd4U21hcmNNVDZNbnZuXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

2. ### 非对话型应用（可批量运行）
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NjFlYTJlZDEzYTk0Y2Q4MDdmZjg4YzUwMmU1ZTEwYTJfTktVOXhUM1ZLNm1vR0haSWY4UUhOaE1MdjdCSVgwVm9fVG9rZW46RGhwTGJvdnp3b1NLeTR4cjBpdmNSZnpGbmtkXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

可以通过csv表格导入数据进行批处理

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjI4M2MyMmM3MjVjNWYzZjU5YzAwMDY0OWIyNDkyMTJfalJaNUdSQXRSeWhySktMYW1YWU5rbm5HdURnbHZkWmFfVG9rZW46Q3h1d2Jab0U4bzI0VzN4eEtSS2NpZXlJbm1lXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

3. ### 源代码定制前端
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MGNkNjAxMDA0MDAwYmRkMzZkOWM0MmNkNzMzNWU5OGVfaHlHRU5tVXVjaUxJSFNhS2NTNEdGeUVqRnozZ3h0aGdfVG9rZW46SzBrZGJXMW5Ib1h1VHd4cWEzamNzczZ4bkhiXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

2. ## API发布
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=YWNkNjY5Mjc4ZmE5ZTkxZDRmYmRhZTlmNjc0YjUzYTdfQ3FwSHBhMGZRNE1NM1A5WXBWcUV1bzlIenpzbERMVjlfVG9rZW46Q1RMbWJHb3F2b3UwbU14aTRhVGNWalg3bkllXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

可以发布为api作为后端调用

  

3. ## 嵌入网站
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NjYxNmUwYzNiZTZhMzIzYWNlY2I2MWMwMWUzNTQ2OWJfVVV3cWNQT0NFdmpsS0xudkEwZHhDaEdqTlVyUkVUUldfVG9rZW46UFBJamJkeVdlb2xpeW94a0ViT2M4UHV0bk9lXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

4. ## 应用检测
    

- 检测分析应用使用数据
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MDg5MThhODVmYjg3OGMzZGM4ZjQwYmFlNWZiYTM3NWNfeTVHR1F4MnA5NjVMZUdJSGw0R3NON3FtNlV2eVRSWlRfVG9rZW46R2dMT2JzaVM5b2xad2N4RUUzcmNrd0xpbnNlXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

5. ## 应用日志
    

- 可视化的 LLM 应用运营
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWVlYzI5ZDVlMDc4OWU2MDQwNDFhMmJmMDQ4NDM4MTRfaUkyclJCbW1qcVdHM01TNnZxeUlxQVdjQW1nMnlWYTNfVG9rZW46SGIzQWJleEJZbzI5Yjd4elRJNmNZOE13bmZnXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

  

4. # 工具
    
    1. ##   搜索类
        
    
    ![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NGQzNzU2ODU3ZmFhZWUyZTlmMzBiMjk2Zjc4YWRiNWNfM1VZZkh4Nzk2QTdkaHlLTHN2R3hpdUFyRW5JZVpQakJfVG9rZW46Uzl1TGJobDFsb21BV3J4ellnSGN6bEZpbmplXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)
    

5. ##   图片类
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTYzYzg5ZTVmNGEzYjg0ZjgyZjE3NDBhZmI2YjI4MzlfREw4dmM4eXg2ZHBiTzRrQzVYSWNrYWthcFpONjl0bzdfVG9rZW46UFpYMmJLa3J5b1o4S2x4TzJnQmNjWG9lbnBiXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

3. ##   视频类
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MmUzYzk0YjJhYzVmOTNhNDFmYWZlMjM3YTg2MzYwN2NfQ0Q5VmIwaVVPU0llMlV0Rkk5WmkwZWdqUlVNSHVTOWZfVG9rZW46V2g1eWJJT0lKbzRqSUh4VnJta2NFZXhlbjVmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

4. ##   天气、旅行
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NDBkZmQ2MjBjNDNkNzlmNjgxMmNhMjU4MTZjN2ZiNDdfeVR4bVB6WHFRVWJRVXFHQ2I0M0ZzMnVUVDZ4Q2hsSUZfVG9rZW46TkR5eGJITWdKb3FWTEt4bTk0aGNLZDFXbklmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

5. ##   新闻
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=YTM3NTA1NTdkMjEyZDMyOWQ1Njk1NGE0YzNiZGE5ZjRfN0JKdWVPMVlDSHdVcEZCdGNRVXBsZkdIUWpCazNPNzFfVG9rZW46RnU0RWJjZExVb0xBbXh4WjVjMmNMNk5Hbk5mXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

6. ##   设计
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=YWE2NGUyZWM4NjcyNjlhZjYzN2ZlNjczZTJhNTEzMWRfdm9zWXNwTnNWdXFZQU51V1hVQ2FKODBXMU83U2dmMHZfVG9rZW46UXJrMGJsNkpJb3ozajJ4ME9EbWNuSVRwblZiXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

7. ##   社交
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=Yzk5Zjc0NGY3YjliMzZiZDNhM2ExOTNkMmIxZTg2YjhfeUNLQTFyTG5HemRBcmJEa29tbHRJYjJEbDM1MGh3M2VfVG9rZW46TlBvZmJRN0pqb3Z5QXZ4WjFBeGNYYlhFbmVoXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

8. ##   生产力工具
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MmJmNjI1MTA4YTMwNDQ4MmY0MWRjNGVkNGQxNWRmOTZfem9JUjBiaGJaZ3p3eXRYY21aekJsMzZod0hqaVdYTkNfVG9rZW46TlRiUGJTUGsxb0J0YkJ4dUhjdGM4MEZtbmZlXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=YjE5Y2Y1YmMyMmZmMDM5OGQyMGU3OTcxZjI4ZjJhMDZfdzh6dWNlNUNNR2dvWVExaGJGRldJSDhJVGFEOFAzZ3pfVG9rZW46T2ZVVWJIeVdTb01jOXV4MTZQdWN1SVF2bmRiXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MGM1ZTY1ZWViMWQ5YTk1MzJjZjkyZDZiMjE3ZmFmMGJfZGNyRUhKS2NwYjd6anFzNHE5Qm5HWTBReWVvY1NneTFfVG9rZW46SEw1RGJvWnl5b3BsUjN4Q2RLTWNSVEVYblJiXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

9. ## 自定义工具
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ODlmOTUzN2ZhM2MxMjMwNzU4NDkyMzdhYTI3ZDUwYTVfNEY1ZGtjdWloZUxDeDE2eERrVTNvWmVMaWdSTzlKU3pfVG9rZW46Tkt6ZWJkTUJlb29PT3N4M21mc2MyUDdVbmVlXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

- 类比于句子画布引擎的插件，以调用第三方api的形式或内置工具
    

  

  

- 通用搜索引擎、垂类领域搜索引擎（学术、论坛、医学、地图）、爬虫（返回结构化数据）
    

  

  

  

  

  

  

- 文生图、图片格式处理
    

  

  

  

  

  

  

  

- 获取油管频道视频统计数据、数字人
    

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

- aippt、图表生成
    

  

  

  

  

  

  

- 维基百科检索
    
- 飞书多维表格增删查改
    
- 社交平台消息分发
    

  

  

  

  

  

- 代码解释器、数学计算器、Wolfram（计算和知识平台）
    

  

- GitHub仓库搜索、获取时间、二维码生成、json数据处理、在线代码执行平台、翻译
    

  

  

  

  

  

  

  

  

  

  

  

- 表格处理工具
    

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

- 目前支持 OpenAPI / Swagger 和 OpenAI Plugin 规范
    

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

5. # 拓展
    

6. ## 使用API 扩展
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=YzA3ZGZmNGNkNGM4ZjdkMWVhYjY3MmVlMTYyM2Q1Y2VfZG01WnhpbFZqVW5uTW9wZE1nS1NvMWp3aTdsNDJmb0tfVG9rZW46UDFoMWJHNnhyb0g4NUd4RmZLamNneWt0blJmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

2. ## 修改源代码扩展（略）
    

  

  

  

支持以下两种拓展

- `moderation` 敏感内容审计
    
- `external_data_tool` 外部数据工具（例如天气查询）
    

6. # 其他特性
    

7. ## 导出DSL
    

文件格式为 YML，支持分享

2. ## 单步调试
    

对单个节点进行调试，查看运行状态、输入/输出、元数据信息。

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDk2MGRkNTRjYWFlNThhNDI3OTUwOTUyYTliNDcxMTRfT2FEQ1JBekZNOVlibnhSSXN6ZFZYYUtqN0lnNU0yVnFfVG9rZW46VDJsVmI3eXRob1VGY1V4RmhhSmN5NVpDblFnXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

3. ## 运行历史
    

查看当前工作流历史调试的运行结果和日志信息。

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MDNjN2Y3NDU5NDVlMDc2ODdkMmRlNzk5MzdmYmVkNWJfampsWFJtSFpaYjQxOGZ0RVo5ZkhCM0dWTmFqazU4QmdfVG9rZW46SFZaSWJTdGVGbzUyZEN4bzJGUmNaSllvbnVsXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

4. ## 召回测试
    

测试RAG召回效果

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=OGFlM2I5NTViNmZhNWIxMDkyNDI0Yjc3MjdmMGIyYmRfR1c4ODhnaXdrSzdFSDNNSEYyWUdlNlRWOHZtRlY4UjBfVG9rZW46T0EzcWI0YVRUb25BMzd4OVJxWWNIYUNlblBFXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

5. ## 引用归属
    

查看到具体的引用段落信息，包括**原始分段文本、分段序号、匹配度**等。

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=MWM3MzczN2E5MWMzOWJhM2Q5YzBmMWYwZDQwYjBjZjFfYWFCbEZMRFlodGhRUW1pZjFJQ2JkQkJJejNZOENmUkpfVG9rZW46REsxemJ2WGp3b2NkelV4dHlnZ2NzOEx1bmdmXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

6. ## Dify近期更新方向
    

- 功能更新
    
    - 增加工作流功能
        
    - 新增模型支持
        
    - 新增工具支持
        
    - 新增向量数据库支持
        
    - api服务基础设施优化
        
- 用户体验优化
    
    - 使用全新图标
        
    - MiniMax 模式下支持屏蔽敏感信息
        
    - 优化了知识检索性能
        
- 问题修复等
    
- 弃用 pip 作为 API 服务的主要包管理工具，使用 Poetry
    

7. ## Dify文档
    

## https://docs.dify.ai/v/zh-hans

本人如有阐释不清之处，可参阅官方文档

7. # Dify和句子对比
    

# Dify

# 句子

1. ## 分发渠道
    

2. 发布为webapp，以API形式提供后端支持（坚持后端即服务的理念）、_**使用HTML的**_ _**`<iframe>`**_ _**标签嵌入网站**_ （更正表述：<iframe>是HTML的一个标签，作用是把其他网页的内容嵌入本网页，dify的工作流搭建完成之后，可以生成一段`<iframe>` 标签，只需加入所需网页的代码中即可。）
    
3. 没有自己的生态，给自己定位为中间件。但会有社区开发者扩展生态，帮其接入社交平台，以微信为例https://github.com/hanfangyuan4396/dify-on-wechat
    

(需要安装特定版本的企业微信)

支持多种分发渠道，其中深度融合企微生态，句子有成熟的企微托管系统，而企微是私域营销的主要平台，场景

  

2. ## 销售渠道
    

主要面向海外，在discord、推特均有运营社媒账号。其中在日本较受欢迎，有专门的日文账号。

创始人张路宇理由：国内个人用户、企业客户SaaS付费意愿低

  

主要面向国内，发掘的新客户，原rpa的老客户

3. ## 产品使用
    

个人使用者、开发者友好、开箱即用

UI，交互都非常流畅美观

企业内部（PE使用）、部分功能特性需结合企微环境、如会话属性、事件。

4. ## 知名度
    

Dify作为开源产品知名度较高，GitHub超过4万星，在AI玩家圈耳熟能详。

句子也是开源项目起家，过去做RPA知名，在AI这块知名度小一点。

这个维度会影响获客来源。

5. ## 更新
    

Dify是一款社区共建的产品，据说有300个社区开发者参与，对于AI最新特性都会较快的提供支持。

句子主要是围绕自己的企微生态和客户需求进行更新

  

# FAQ

  

@陈曦 ：

- iframe是否支持点赞点踩、用户问问题后展示一个loding的状态？都支持哪些装修？
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NjdiN2Q3MDU4MTJjOWM3MjY0NWM4N2M3OWYwMDNkOTZfN1ZVdFpPcHQ4Q296c2JjaWRFQ2U5ZGtlTW5nT01GNjFfVG9rZW46UUlsTGJmTFdqb3VqNnd4MXd3WmNWV0d1bk5kXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

  

- 点赞点踩：被嵌入的网页支持即可（需要 **JavaScript代码** 和 **后端服务** 支持）
    
- Loading：有一个省略号跳动的动态
    
- **装修**: iframe 的 "装修" 非常有限，通常只能控制：尺寸 (宽度、高度)、iframe 的边框、滚动条
    

  

  

# TODO

- Dify 的价格的中文版： https://dify.ai/pricing 和句子的产品定价做分析
    

![](https://juzihudong.feishu.cn/space/api/box/stream/download/asynccode/?code=NWQ4ZTUxMGU5MmI5ZGZjNjU3MTA3M2M3ZDQ1NjExNTJfbkhlSUhobmU0NXZZZ0pUbDRPaEpjMFhubllDRkJwNktfVG9rZW46RTRUdmJ6OERYb3l4VEZ4djZIRGNlNFpjblhnXzE3NjgzNzg1Mjk6MTc2ODM4MjEyOV9WNA)

  

|   |   |   |   |   |
|---|---|---|---|---|
|功能/服务|沙盒计划（SANDBOX）|专业计划（PROFESSIONAL）|团队计划（TEAM）|企业计划（ENTERPRISE）|
|价格|免费|59美元/月|159美元/月|联系销售|
|免费消息调用次数（调用 open AI API）|200次试用|-|-|-|
|消息配额（调用 open AI API）|-|5000条/月|10000条/月|无限|
|工作区数量|-|多个|多个|多个|
|单点登录(SSO)支持|-|SAML和OIDC|SAML和OIDC|SAML和OIDC|
|支持的模型提供商|OpenAI/Anthropic/Llama2/Azure|OpenAI/Anthropic/Llama2/Azure|OpenAI/Hugging Face/Replicate|OpenAI/Hugging Face/Replicate|
|部署协助|-|有|有|有|
|团队成员数量|1|3|无限|无限|
|白标服务|有限|扩展|广泛|广泛|
|应用构建|有（10个）|有（50个）|有（无限）|有（无限）|
|向量存储空间|5MB|200MB|1GB|无限|
|文档上传限额|50个|500个|1000个|无限|
|文档批量上传功能|不支持|支持|支持|支持|
|自定义角色|不可用|支持|支持|支持|
|支持服务|社区论坛、智能体模式、工作流搭建|电子邮件支持、webapp标志转换、大模型负载平衡、知识库API请求|优先电子邮件和聊天支持、SSO鉴定|优先电子邮件和聊天支持|
|文档处理优先级|标准|优先|优先|优先|
|每日消息请求限制|500次/日|无限|无限|无限|
|注释配额限制|10|2000|5000|无限|
|日志历史|15天|无限|无限|无限|
|自定义工具数量|不可用|10|无限|无限|

1. 企微生态的打通，根据标签触发等动作
    
2. 方法论优势，结合私域运营的优势，主动触达（跟进、触达）
    

  

  

## 补充的产品之间的关系

- 句子互动 = 句子秒回 （RPA) + AI Agent 流程引擎（insight的后台）
    
- AI Agent 流程引擎（insight的后台） = coze = dify
    
- 句子秒回（RPA） = 企微宝
    

8. # 句子、Dify、Coze功能对比
    

|## 功能|## 子功能|## Dify|coze.com|coze.cn|## 句子|
|---|---|---|---|---|---|
|LLM|支持模型|市面上的开源/闭源模型基本都支持|Claude、Gemini、Openai三大主流模型|国内主流模型|国内国际主流模型|
||消息类型|system、user、assistant|system、user|system、user|system|
|插件||dify官方和社区开发者提供|插件商店|插件商店|PE编写|
|agent分发||webapp、iframe、api|bot商店、api、websdk、discord、telegram、messenger、line、ins、slack、lark|bot商店、api、websdk、豆包、飞书、抖音、微信客服、小程序、公众号、掘金、飞书多维表格|企微等|
|多agent||❌|✅|✅|❌|
|开场白||✅|✅|✅|❌|
|下一步问题建议||✅|✅|✅|❌|
|触发器||❌|✅（定时触发、事件触发）|✅|✅|
|快捷指令||✅（和开场白一块）|✅（常驻输入栏上方）|✅（常驻输入栏上方）|❌|
|工作流商店||❌|✅|✅|❌|
|图片流商店||❌|❌|✅|❌|
|节点|工作流作为节点|✅|✅|✅|❌|
||迭代|✅|✅|✅|❌|
||意图识别|✅|✅|✅|❌|
||文本格式处理|✅统一为字符串|统一为字符串或分离为数组||❌|
||预设问题，在对话中向用户提问|❌|✅|✅|❌|
||记忆|✅（变量）|✅（变量、数据库、总结聊天历史、处理文件）|✅|❌（可以通过赋值实现记忆）|
|输入格式||图片、文字（将支持文件）|文字、文件（图像、PDF、DOCX、Excel、CSV 或音频。）|文字、文件（图像、PDF、DOCX、Excel、CSV 或音频）|文字、图像、PDF、DOCX、Excel、CSV|
|批处理||✅（webapp，迭代）|✅（llm批处理节点，迭代）|✅（llm批处理节点，迭代）|❌|
|背景图像||❌（可以通过定制webapp实现）|✅|✅|❌|
|tts，stt||✅|✅|✅|❌|