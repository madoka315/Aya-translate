# Aya-translate

HoshinoBot 翻译插件，基于[有道智云](https://ai.youdao.com/)文本翻译 API。

## 指令

| 指令 | 说明 |
|---|---|
| `翻译英<内容>` | 翻译为英文 |
| `翻译中<内容>` | 翻译为中文 |
| `翻译日<内容>` | 翻译为日文 |
| `翻译<空格><内容>` | 自动判断语种：中文→英文，日文/英文→中文 |

自动判断规则：文本含平/片假名 → 日文；否则含汉字 → 中文；其余 → 英文。

## 配置

1. 到[有道智云](https://ai.youdao.com/)注册账号，创建应用并**绑定「文本翻译」服务**（不绑定会报 errorCode 108）；
2. 把应用的 **应用 ID** 和 **应用密钥** 填入 `translate.py` 顶部的 `APP_KEY` / `APP_SECRET`；
3. 将本目录放入 `hoshino/modules/`（建议命名为 `translate`），并在 `hoshino/config/__bot__.py` 的 `MODULES_ON` 中添加 `'translate'`；
4. 重启 bot。

依赖：`requests`（HoshinoBot 环境自带，无需额外安装）。

## 与原仓库的差异

根据原仓库 [pcrbot/descr_entity_translate](https://github.com/pcrbot/descr_entity_translate) 修复/优化：

- **修复**：解析接口返回改用 `json.loads`——原版用 `eval()`，返回内容含 `null` 时直接 `NameError` 崩溃；
- **修复**：接口返回错误码时给出提示文本，不再抛异常；
- **优化**：所有 HTTP 请求加 `timeout=10`，防止接口无响应时阻塞整个 bot 事件循环；
- **新增**：`翻译 <内容>` 自动语种翻译命令；
- **优化**：译文输出格式化——原版输出 `['译文']` 样式的原始列表，现按行拼接；
- **移除**：名词解释功能（原版 `entity.py` 依赖的腾讯云 `DescribeEntity` 接口已被官方下线，无法使用）。
