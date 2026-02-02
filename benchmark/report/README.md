#### 1.0 Apache Log (平均日志大小：83.5B)

本工程用于完成 LogHub 日志的性能测试与对比评估，覆盖不同引擎与不同拓扑下的吞吐与资源占用指标，形成阶段性 benchmark 基线，便于后续版本与方案的横向/纵向对比。本文档仅描述测试方法与结果，不对生产环境上限作外推。

测试口径与指标说明：
- EPS：每秒处理的日志条数。
- MPS：吞吐量（MiB/s），计算公式：日志大小 * EPS / 1,048,576。
- CPU / MEM：测试进程自身的平均值与峰值，多核累计百分比（例如 800% 约等于 8 核满载）。

测试环境与方法（简要）：
- 平台：Mac mini（Apple M4），macOS 15.5，arm64。
- 资源：10-core CPU，16 GiB 内存，Internal SSD（APFS）。
- 网络：本机回环 127.0.0.1。
- 方法：单机独立运行，各引擎按相同数据规模执行；输入拓扑为 File -> BlackHole 或 TCP -> BlackHole；结果以引擎原生统计接口/采样脚本采集。

表 1.0.1-1：Mixed Log（File -> BlackHole / TCP -> BlackHole ）

| 引擎          | 拓扑              | EPS       | MPS    | CPU (Avg/Peak) | MEM (Avg/Peak)  |
| :------------ | :---------------- | --------- | ------ | :------------- | :-------------- |
| **WarpParse** | File -> BlackHole | 2,027,100 | 161.42 | 759% / 889%    | 263 MB / 279 MB |
| Vector    | File -> BlackHole | 597,941   | 47.62  | 325% / 342%    | 235 MB / 254 MB |
| **WarpParse** | TCP -> BlackHole  | 1,272,100 | 101.30 | 655% / 743%    | 157 MB / 160 MB |
| Vector    | TCP -> BlackHole  | 1,242,500 | 98.94  | 586% / 643%    | 219 MB / 224 MB |
