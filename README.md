PLC Flow Control PoC
Status: Demo / PoC — 小规模工业流控监控技术验证，尚未部署生产环境。
1. 核心技术栈
	•	PLC：Siemens S7-1200
	•	Backend：Python 3.11 + FastAPI
	•	Frontend：Vue 3 + Vite
	•	PLC Communication：python-opcua（OPC UA）、Modbus TCP
	•	Realtime：WebSocket
	•	Deployment：Docker Compose（规划）
2. 预设采集点位
点位名称
数据类型
描述
Flow_PV
Float32
实时流量反馈
Pressure_PV
Float32
实时压力反馈
Valve_Opening_SP
Float32
阀门开度设定值（0–100%）
PLC_Heartbeat
Boolean
PLC 通信心跳
Device_Status
UInt16
设备运行状态
点位地址、采样周期和单位将在实际 PLC 联调阶段确认。
3. 断线重连与异常告警
采用异步通信采集和状态机管理：
	•	连接超时：3 秒
	•	重试机制：指数退避，1s → 2s → 4s → 8s → 16s
	•	最大退避间隔：16 秒，持续重试
	•	告警阈值：连续 3 次通信失败触发告警
	•	数据保护：断线后标记采集数据为 STALE，不再作为有效实时值
	•	控制保护：通信异常期间禁止 PLC 写入操作
	•	恢复策略：连接恢复后重新订阅或读取点位，校验数据有效性，成功后恢复正常监控
	•	日志审计：记录断线时间、重试次数、异常类型及恢复时间
4. Devin Bot 开发职责
Devin Bot 负责通信适配器、FastAPI 接口、Vue 3 监控面板、异常告警逻辑、自动化测试和容器部署配置。
PLC 安全联锁、底层控制逻辑及现场操作由人工审核。
5. 当前规模
项目处于 Demo 阶段，计划采用 1～2 台 PLC 或模拟设备验证数据采集、接口稳定性及故障恢复机制。
所有参数均为 PoC 预设设计，尚未完成现场实测验证。
