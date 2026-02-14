#### 1.0 Apache Log (平均日志大小：89B)

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

表 1.0.1-1：Apache Log（File -> BlackHole / TCP -> BlackHole ）

| 引擎          | 拓扑              | EPS       | MPS    | CPU (Avg/Peak) | MEM (Avg/Peak)  | 性能倍数  |
| :------------ | :---------------- | --------- | ------ | :------------- | :-------------- | --------- |
| **WarpParse** | File -> BlackHole | 1,942,700 | 164.89 | 577% / 586%    | 87 MB / 90 MB   | **3.25x** |
| Vector        | File -> BlackHole | 597,941   | 50.75  | 325% / 342%    | 235 MB / 254 MB | 1.0x      |
| **WarpParse** | TCP -> BlackHole  | 1,686,700 | 143.16 | 603% / 648%    | 797 MB / 820 MB | **1.35x** |
| Vector        | TCP -> BlackHole  | 1,242,500 | 105.46 | 586% / 643%    | 219 MB / 224 MB | 1.0x      |


#### 1.1 Openssh Log (平均日志大小：112B)
表 1.1.1-1：Openssh Log（File -> BlackHole / TCP -> BlackHole ）

| 引擎          | 拓扑              | EPS       | MPS | CPU (Avg/Peak) | MEM (Avg/Peak)  | 性能倍数  |
| :------------ | :---------------- | --------- | --- | :------------- | :-------------- | --------- |
| **WarpParse** | File -> BlackHole | 1,117,600 | 119.37 | 645% / 749%    | 338 MB / 557 MB | **2.92x** |
| Vector        | File -> BlackHole | 382,333   | 40.84  | 491% / 582%    | 267 MB / 306 MB | 1.0x      |
| **WarpParse** | TCP -> BlackHole  | 978,500   | 104.52 | 700% / 768%    | 616 MB / 682 MB | **1.85x** |
| Vector        | TCP -> BlackHole  | 530,100   | 56.62  | 498% / 529%    | 312 MB / 323 MB | 1.0x      |
