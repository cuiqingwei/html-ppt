# HTML 演示文稿平台

一个轻量级的 HTML 幻灯片演示工具，让技术培训讲义焕发交互魅力。

## 项目简介

本项目用于创建交互式技术培训演示文稿，采用纯 HTML + CSS + JavaScript 实现，无需复杂依赖，直接在浏览器中打开即可演示。

## 当前课程

| 课程 | 简介 | 标签 |
|------|------|------|
| 🎯 OpenClaw × NVIDIA API 配置 | GLM-4.7 + MiniMax M2.1 免费模型 API，国内零门槛直连教程 | 初学者友好、6 章节、免费 |
| 🤖 Claude Code × tmux | 在多窗格终端中解锁 AI 辅助编程的全部潜力 | 中级→高级、3-4h、动手实操 |
| 🖥️ 本地大模型部署实战培训 | 从专业名词到生产级部署，系统掌握消费级 GPU 场景下的 LLM 全链路 | 入门→进阶、6 章节、讲授+实操 |
| 🤖 LM Studio 使用技巧培训 | 界面操作 · 模型管理 · 本地服务 · LM Link 远程推理 | ⭐⭐ 入门、约 90 分钟、演示+实操 |

## 快速开始

### 本地预览

```bash
# 克隆项目
git clone https://github.com/cuiqingwei/html-ppt.git
cd html-ppt

# 用浏览器打开 index.html
# 或启动简单服务器
python3 -m http.server 8000
# 访问 http://localhost:8000
```

### 添加新课程

在 `index.html` 的课程网格中添加新的培训讲义卡片：

```html
<a href="ppts/your-new-presentation.html" class="course-card">
  <div class="course-emoji">🎯</div>
  <div class="course-title">课程标题</div>
  <div class="course-desc">
    课程简介描述，支持多行文字。
  </div>
  <div class="course-tags">
    <span class="tag">标签1</span><span class="tag">标签2</span><span class="tag">标签3</span>
  </div>
</a>
```

卡片字段说明：
- `href`: PPT 文件路径（放在 `ppts/` 目录下）
- `course-emoji`: 课程图标（emoji 表情）
- `course-title`: 课程标题
- `course-desc`: 课程简介
- `course-tags`: 标签（如难度、时长、形式等）

## 演示操作

- **下一页**: `→` 空格键
- **上一页**: `←` 退格键
- **首页**: `Home`
- **末页**: `End`

## 技术栈

- 纯 HTML / CSS / JavaScript
- 无需构建工具
- 响应式设计，支持多设备

## 许可证

MIT License
