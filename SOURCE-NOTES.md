# 设计参考与来源

本规范参考了 Agent ID Sniper 的只读设计审查，没有复制或运行其交易程序，也没有把它的项目案例、参数或宣传描述当作收益证明。

- 源仓库：`nancyc0813-maker/agentid-sniper`
- 静态审查提交：`19061915c6626537ec0e9db4a34493fa1cc0a09c`
- 审查日期：2026-10-31
- 规则整合基线：本项目提交 `e8d9ca6bc3d1a338eb2a714cb8c6f3d9352c7857`

| 借鉴方向 | 原始代码定位 | 本规范处理 |
|---|---|---|
| 铸造事件发现及反查调用目标 | [mintScan / checkMinted](https://github.com/nancyc0813-maker/agentid-sniper/blob/19061915c6626537ec0e9db4a34493fa1cc0a09c/watch.mjs#L358) | 只进入候选队列；事件、资格和项目真实性分别核实 |
| 调用聚集与研究顺序 | [countHot](https://github.com/nancyc0813-maker/agentid-sniper/blob/19061915c6626537ec0e9db4a34493fa1cc0a09c/watch.mjs#L331) | 只读排序；不导入人数买入阈值，不把接收者当成主动参与者 |
| 新部署及工厂关系 | [scanNew](https://github.com/nancyc0813-maker/agentid-sniper/blob/19061915c6626537ec0e9db4a34493fa1cc0a09c/watch.mjs#L461) | 分开记录创建交易发送人、实际创建者、owner 与其他角色 |
| 节点轮换及扫描游标 | [logsRpc](https://github.com/nancyc0813-maker/agentid-sniper/blob/19061915c6626537ec0e9db4a34493fa1cc0a09c/watch.mjs#L533) | 增加缺口区间、持久化后推进、重组及冲突处理；不照搬跳块 |
| 原子快照及后台工作 | [save](https://github.com/nancyc0813-maker/agentid-sniper/blob/19061915c6626537ec0e9db4a34493fa1cc0a09c/watch.mjs#L170)、[工作队列](https://github.com/nancyc0813-maker/agentid-sniper/blob/19061915c6626537ec0e9db4a34493fa1cc0a09c/watch.mjs#L459) | 只采纳设计原则；保存失败必须可见，不宣称已经实现 |

未采纳：近乎免费即自动 mint、forceMint 旁路、猜测并重播 calldata、买入后才提示权限风险、使用 owner 替代历史部署者、用字节码首尾片段替代完整哈希，以及低价值退出失败后丢弃持仓跟踪。

以上来源只用于解释设计缘由，不构成对第三方工具安全性、收益或当前可用性的背书。
