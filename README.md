# HomeSpan
适用于 Arduino-ESP32 的 HomeKit 库
MIT 开源协议

> 项目地址：https://github.com/HomeSpan/HomeSpan

欢迎使用 HomeSpan！
HomeSpan 是一套健壮、上手极其简单的 Arduino 库，用于在 Arduino IDE 内基于 ESP32 打造你自己的 HomeKit 设备。

HomeSpan 面向单片机实现了苹果 HomeKit 配件协议 HAP-R2，专门为乐鑫 ESP32 系列芯片设计。设备可以直接通过局域网 WiFi 配对接入 HomeKit，**不需要额外桥接器**。你可以充分利用 ESP32 的 IO 能力编写自定义控制程序、搭配硬件，在 iPhone / iPad / Mac 的家庭 App 或者直接用 Siri 控制设备。

> ⚠️ 版本重要说明
> HomeSpan 需要 Arduino-ESP32 开发板核心 **V2 版本**。支持初代 ESP32、ESP32-S2、ESP32-C3、ESP32-S3。
> **暂不兼容 Arduino-ESP32 V3.x**，V3 改动巨大，不向下兼容；目前只能在 V2 核心下编译。

> 📌 重要提醒
> 苹果新版 HomeKit 架构要求必须搭配家庭中枢（HomePod 或 Apple TV）才能完整正常使用 HomeKit 设备，包括 HomeSpan。
> 如果没有家庭中枢，HomeSpan 无法向家庭 App 推送状态通知，按键、传感器等设备无法主动上报数据。**不推荐无中枢使用 HomeSpan**。

## ✨ HomeSpan 亮点
- 框架直观自然，上手门槛低
- 独特的**面向服务（Service-Centric）**开发范式，贴合 HomeKit 原生模型
- 完全基于大众熟悉的 Arduino IDE
- 100% 兼容 HAP-R2 协议
- 内置数十种标准 HomeKit 服务
- 支持两种运行模式：单配件模式 / 桥模式
- 支持数字配对码、二维码扫码配对

## 👨‍💻 面向开发者
- 深度复用 Arduino 串口监视器
- 实时、易懂的诊断日志
- 底层 HAP 交互完全透明，可以看到每一次读写请求与响应
- 内置命令行 CLI，支持信息查看、调试、配置修改
- 内置 HAP 数据库校验，提前捕获不符合协议的配置错误
- 专用类封装 ESP32 16通道硬件PWM：
  - LED 亮度调节（自带平滑渐变）
  - 舵机控制
- 内置按键/触摸驱动：单击、双击、长按
  - 普通IO按键（接地/接VCC）
  - ESP32 触摸按键（支持TouchPad的型号）
- 直接调用 ESP32 RMT 外设，方便发射红外 IR、射频 RF 遥控信号
- 专用类驱动单总线/双总线可寻址 RGB / RGBW 灯带
- 步进电机驱动类：后台平稳运行，不阻塞 HomeSpan 主循环
- **SpanPoint：基于 ESP-NOW 的 ESP32 点对点双向通信**（本库王牌功能）
- 内置网页日志 Web Log，支持自定义日志输出
- 大量注释详尽的示例教程，从入门到高级 HomeKit 用法
- 额外实战项目示例
- 完整 API 文档覆盖全部接口

## 🧑‍🔧 面向终端使用者
- 自带 WiFi AP 热点配网网页界面，不懂代码也可以配置：
  - 设置 WiFi 账号密码
  - 修改 HomeKit 配对码
- 状态指示灯 + 功能按键：
  - 强制解除 HomeKit 配对
  - 恢复出厂设置
  - 开启配网热点
- 独立完整的终端用户手册

---
## 🆕 更新日志：HomeSpan 1.9.1（2024-07-03）
> 当前版本重点更新：
1. **新增 TLV8 特征支持**
   - 完整 TLV8() 类，创建、管理 TLV8 对象
   - 支持字节流、字符串、数值、空标签、嵌套TLV
   - C++ 迭代器方便读写TLV记录
   - `getTLV()` / `getNewTLV()` / `setTLV()`、`CUSTOM_CHAR_TLV8()` 自定义TLV特征
   - 教程22专门演示TLV8用法
2. DisplayOrder TLV8 特征：电视服务输入源在家庭 App 里的显示顺序自定义
3. AccessoryIdentifier 配件识别教程：配对时触发设备识别
4. Pixel 灯带类升级：
   - 新构造函数支持指定色序：RGB / GRB / BGR / RGBW / GRBW 等
   - 旧构造标记弃用，编译会告警
   - `isRGBW()` 判断灯带是否带白色通道
   - PixelTester 示例：快速识别灯带色序
5. 支持读取、修改服务与特征的 IID
   - `getIID()` 获取服务/特征IID
   - `resetIID()` 重置IID序号计数器
6. 读取控制器配对信息（高级用途）
   - 遍历所有已配对控制器
   - 获取控制器ID、长期公钥、判断是否管理员
   - `setControllerCallback()`：控制器新增/删除/权限变更回调
7. 支持 HomeKit WR（write-response）写响应协议，用于自定义特征开发
8. ServoPin 新增 `set(NAN)`：停止PWM输出，舵机可以自由转动；写入角度恢复驱动
9. 客户端连接管理重构，节省内存，为以太网支持做准备
10. 修复多处内存泄漏、SpanPoint、配对码保存等 Bug

---
## 📚 HomeSpan 配套文档列表
- 入门指南：搭建 HomeSpan 开发环境
- API 总览：手把手写第一个 HomeSpan 程序
- 教程合集：配套示例讲解
- 服务与特征清单：库支持的全部 HAP 服务、特征
- 配件分类 Category：HomeKit 配件类型定义
- CLI 命令手册：串口命令配置、诊断
- 用户手册：成品设备配网、改配对码、配对HomeKit（无需电脑）
- API 参考手册
- QR 二维码配对文档
- OTA 无线升级：Arduino IDE 无线烧录
- PWM：LED、舵机驱动说明
- RFControl：红外射频发射
- Pixels：灯带驱动
- Stepper：步进电机
- SpanPoint：ESP-NOW 点对点通信
- Television：电视服务（HomeKit 电视输入源）
- Message Logging：串口+网页日志
- TLV8 特征开发
- Device Cloning：设备克隆，换硬件不用重新配对、不丢失自动化
- Projects：实战项目案例
- FAQ
- 常见问题排错
- Reference Sketches：复杂设备参考代码（温控、灌溉等）

> 文档跟随分支版本管理，master 分支一般是正式发布版，dev 是开发版。

## 🔗 外部参考资源
如果你刚接触 HomeKit 开发，可以查阅苹果 HAP-R2（非商业版）第8、9章。这份文档苹果已经不再提供更新，网上可以找到存档版本。苹果开源的 HomeKit ADK 也只基于原版 HAP-R2。

## 💬 反馈与提问
- Bug、异常问题：提交到 Issues
- 使用疑问、功能建议、讨论：进入 Discussions
- 通用咨询：邮箱 homespan@icloud.com

## 🙋 关于作者
HomeSpan 由 Gregg Berman 开发并持续维护。最初开发初衷：想用 Siri 控制家里射频油烟机。这是他第二个大型开源项目，第一个是 DCC++ 火车模型控制器。

## 标签
home-automation, esp32, arduino-ide, arduino-library, homekit, homekit-server, homespan
