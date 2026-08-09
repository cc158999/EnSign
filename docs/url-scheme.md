# URL Scheme 使用指南

[← 返回首页](../README.md) · [English](url-scheme.en.md)

易能签支持通过 `enq-app://` 从网页、快捷指令或其他 App 发起指定操作。当前公开用于添加软件源、下载资源和一键导入签名证书。

> App 内也能查看示例：**设置 → 关于我们 → URL Scheme**。

---

## 支持的功能

| 功能 | 格式 | 适合场景 |
| --- | --- | --- |
| 添加软件源 | `enq-app://source/[软件源地址]` | 从源网站一键订阅 |
| 下载资源 | `enq-app://install/[资源地址]` | 把 IPA / TIPA 交给易能签下载 |
| 导入证书 | `enq-app://import-certificate?...` | 从证书服务页面一键导入 |

URL Scheme 需要在已安装易能签的 iPhone 或 iPad 上打开。网页中必须由用户点击按钮或链接触发，页面加载后自动跳转可能会被 iOS 或浏览器拦截。

---

## 添加软件源

把 HTTPS 软件源地址直接放在 `/source/` 后面：

```text
enq-app://source/https://example.com/appstore
```

点击后会打开易能签并进入软件源添加流程。这里使用完整的 HTTPS 地址，不要把 `https://` 百分号编码成 `https%3A%2F%2F`。

---

## 下载资源

把资源地址放在 `/install/` 后面：

```text
enq-app://install/https://example.com/MyApp.ipa
```

- `.ipa` / `.tipa` 会创建下载任务
- 其他网页或资源链接会在易能签内置浏览器中打开
- 此格式是兼容入口；下载、签名和安装仍会按 App 内的正常流程进行

和软件源入口一样，资源部分应保留完整的 `https://` 地址。

---

## 导入证书（重点）

`enq-app://import-certificate` 可以让证书服务页面把证书直接交给易能签。用户点击后，易能签会显示来源确认，用户同意后才会下载和导入。

### 推荐：导入 ZIP 整包

ZIP 中需要同时包含 `.p12` 与 `.mobileprovision`，文件可以位于子目录：

```text
enq-app://import-certificate?url=https%3A%2F%2Fexample.com%2Fcertificate.zip&password=YOUR_PASSWORD
```

### 分别提供证书和描述文件

```text
enq-app://import-certificate?p12=https%3A%2F%2Fexample.com%2Fcertificate.p12&mobileprovision=https%3A%2F%2Fexample.com%2Fprofile.mobileprovision&password=YOUR_PASSWORD
```

`mobileprovision` 也可以写成 `provision`。

### 内嵌 Base64

```text
enq-app://import-certificate?p12=[P12_BASE64]&mobileprovision=[PROFILE_BASE64]&password=YOUR_PASSWORD
```

Base64 只适合作为小文件兼容方案。长链接很容易被 Safari、微信、短信或中转页面截断，默认应使用远程文件地址。

### 参数说明

| 参数 | 必填 | 说明 |
| --- | --- | --- |
| `url` | 与 `p12` 二选一 | ZIP 整包的 HTTP(S) 地址 |
| `p12` | 与 `url` 二选一 | `.p12` 地址或 Base64 内容 |
| `mobileprovision` | 使用 `p12` 时必填 | 描述文件地址或 Base64 内容；别名为 `provision` |
| `password` | 视文件而定 | p12 密码；不是 Base64。ZIP 整包需要提供 |
| `name` | 否 | 导入后的证书备注名 |
| `default` | 否 | 使用 `1`、`true` 或 `yes` 将其设为默认证书 |

如果同时提供 `url` 和分离文件参数，易能签会优先使用 `url`。

### 正确编码参数

证书导入使用查询参数，所以**每个参数值都必须进行百分号编码**。例如：

```javascript
const bundleURL = "https://example.com/certificate.zip";
const password = "YOUR_PASSWORD";
const schemeURL = `enq-app://import-certificate?url=${encodeURIComponent(bundleURL)}&password=${encodeURIComponent(password)}`;

document.querySelector("#import-certificate").addEventListener("click", () => {
  window.location.href = schemeURL;
});
```

页面上提供一个由用户点击的按钮：

```html
<button id="import-certificate" type="button">导入到易能签</button>
```

不要在页面加载时自动跳转，也不要尝试跳过易能签的确认弹窗。

### 用户会看到什么

1. 网页提示打开易能签
2. 易能签显示证书来源并请求确认
3. 用户确认后开始下载和导入
4. 导入完成后显示成功或失败原因

远程导入需要用户先同意易能签的隐私协议。密码缺失或不正确时，App 会在可恢复的情况下提示用户重新输入。

### 服务端建议

- 下载地址使用一次性令牌和较短有效期，用完后立即失效
- 使用 HTTPS，并返回正确的文件名与扩展名
- 如果不希望密码进入 URL，使用分离文件形式并让用户在 App 内补录密码
- 远程文件应小于 8 MB；内嵌 Base64 解码后应小于 4 MB
- 不要把证书、密码或完整导入链接写入公开日志

### 常见问题

| 现象 | 建议检查 |
| --- | --- |
| 提示链接不完整 | 是否缺少必要参数，参数名是否拼写正确 |
| 点击没有反应 | 是否由用户点击触发，链接是否被聊天工具截断 |
| 下载失败 | HTTPS 地址是否仍有效，是否返回 2xx，临时链接是否过期 |
| 找不到证书文件 | ZIP 内是否同时包含 `.p12` 和 `.mobileprovision` |
| 密码错误 | 确认使用的是 p12 原始密码，不要先做 Base64 |

---

## 使用 AI 生成接入代码

在易能签中打开 **设置 → 关于我们 → URL Scheme → 导入证书**，点击**复制 AI 对接文档**，再粘贴给 ChatGPT 或 Claude，并说明网站使用的语言或框架。复制内容包含完整参数规则、安全要求和代码输出要求。

[← 返回首页](../README.md)
