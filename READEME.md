智能法律咨询机器人
基于 Flask + DeepSeek API 的法律咨询 Web 应用，支持普通用户咨询与专业律师模式，具备智能场景识别、风险等级评估、法律文书生成、对话历史管理等功能。

项目介绍
本项目是一个智能法律咨询助手，旨在为用户提供便捷、专业的法律问题解答。系统通过调用 DeepSeek 大语言模型，结合精心设计的提示词模板，能够分别面向普通民众和法律从业者提供不同深度的法律意见。同时集成了自动场景识别（民事/刑事/劳动）、风险评估、典型案例库、普法视频、法律文书自动生成等特色功能。

主要功能
1. 双模式对话
普通模式：面向普通用户，回答通俗易懂，附免责提示和后续问题推荐。

专业模式：面向律师、法务等专业人士，采用法言法语，按结构化格式输出（法律依据、举证责任、诉讼策略、类案观点等）。

2. 智能场景识别
根据用户输入的关键词自动判断属于 民事纠纷、刑事法律 还是 劳动仲裁 领域，并加载对应的提示词模板。

3. 风险等级评估
基于内置的关键词库，自动评估当前问题的风险等级（高/中/低），并在回答中显示相应的风险提醒。高风险问题会优先提示用户注意人身安全或紧急法律程序。

4. 法律文书生成
根据多轮对话内容，可一键生成以下法律文书草稿：

民事起诉状

民事答辩状

劳动仲裁申请书

合同条款建议

生成的文书支持导出为 TXT 或 Word (DOCX) 格式。

5. 对话历史管理
自动保存每次对话记录，支持多会话管理。

普通模式与专业模式的历史记录相互隔离。

支持查看历史会话、清空历史。

6. 典型案例库与普法视频
内置民事、劳动、刑事领域的典型案例，点击即可快速提问。

嵌入 B 站普法视频，方便用户学习法律知识。

7. 性能压测接口
提供 /stress-test 接口，可模拟并发请求测试 API 的响应性能。

技术栈
层	技术
后端	Python + Flask + SQLAlchemy (SQLite)
前端	HTML5 + CSS3 + JavaScript (Axios)
大模型 API	DeepSeek API
分词处理	jieba（用于关键词匹配，非核心）
文档导出	python-docx
环境要求
Python 3.8+

申请 DeepSeek API Key（platform.deepseek.com）

安装与运行
1. 克隆代码
将 app.py 和 index.html 放置于同一目录下，目录结构建议：

text
legal-chatbot/
├── app.py
├── templates/
│   └── index.html
├── chat.db          (运行后自动生成)
└── requirements.txt
2. 安装依赖
创建 requirements.txt：

txt
Flask>=2.0.0
Flask-SQLAlchemy>=3.0.0
requests>=2.28.0
jieba>=0.42.1
python-docx>=0.8.11
执行安装：

bash
pip install -r requirements.txt
3. 配置 API Key
打开 app.py，找到以下行并填入你自己的 DeepSeek API Key：

python
DEEPSEEK_API_KEY = "sk-efb0b7c994c84df6aab93c6a66a1ad1f"   # 请替换为你自己的 key
提示：建议使用环境变量 DEEPSEEK_API_KEY 替代硬编码，提高安全性。

4. 启动服务
bash
python app.py
默认运行地址：http://0.0.0.0:5003

在浏览器中访问该地址即可使用。

主要 API 接口
路径	方法	说明
/	GET	聊天页面
/chat	POST	普通模式聊天（自动风险评估）
/chat-pro	POST	专业模式聊天
/generate-document	POST	根据对话生成法律文书
/export-document	POST	导出文书（支持 txt/docx）
/pro-stat	GET	获取当日专业模式统计数据
/risk-logs	GET	查询高风险/中风险聊天记录
/stress-test	POST	压力测试（并发请求模拟）
/test-result	GET	获取最近一次压测结果
/clear-test-result	POST	清空压测结果
配置与定制
修改风险评估关键词
在 app.py 中可以直接修改以下字典：

CIVIL_HIGH_RISK_KEYWORDS

CRIMINAL_HIGH_RISK_KEYWORDS

LABOR_HIGH_RISK_KEYWORDS

以及中风险关键词列表。

调整提示词模板
普通模式模板：LEGAL_PROMPT_TEMPLATES

专业模式模板：PRO_PROMPT_TEMPLATES

可根据需要修改输出格式、字数限制等。

修改场景识别关键词
调整 CIVIL_KEYWORDS、CRIMINAL_KEYWORDS、LABOR_KEYWORDS 三个列表。

注意事项
API 费用：调用 DeepSeek API 会产生费用，请合理使用。

法律免责：本应用生成的内容仅供参考，不构成正式法律意见。复杂案件请咨询执业律师。

数据存储：所有对话记录存储在 SQLite 文件 chat.db 中，请定期备份。

并发限制：默认 DeepSeek API 有速率限制，压测时请勿过高并发。

示例截图（略）
常见问题
Q: 为什么普通模式回答还是会显示风险评估？
A: 风险评估是独立于模型回答的，会在回答末尾附加风险提示，符合安全规范。

Q: 生成文书时提示“服务暂时不可用”？
A: 请检查 DeepSeek API Key 是否有效，网络是否通畅。

Q: 如何清空所有历史记录？
A: 点击左侧边栏的“清空当前模式历史”按钮即可。也可手动删除 chat.db 文件重新初始化。

扩展建议
接入向量数据库实现长期记忆和案例检索。

增加用户登录与权限管理。

支持更多法律文书模板（如辩护词、代理词）。

集成语音输入功能。