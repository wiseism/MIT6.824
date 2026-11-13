# 修复 CMake workflow：仓库无 CMakeLists.txt

## 失败信息
- Run 42817218879 / 42817214798 (CMake on a single platform) — `cmake -B build` 报错：
  `CMake Error: The source directory ... does not appear to contain CMakeLists.txt.`

## 原因分析
- 仓库 src/ 下为 C++ 实现的 MapReduce/Raft/kvRaft/shardKv（buttonrpc + ZeroMQ），
  但 workflow 是通用 CMake starter，而仓库根目录没有 CMakeLists.txt。

## 修复内容
- 新增根 CMakeLists.txt：构建 `mrFunc` 动态库（src/MapReduce/map_reduceFun.cpp）
  与 `master` 可执行文件（src/MapReduce/master.cpp），include 路径含
  buttonrpc-master/depends（zmq.hpp），链接 pthread + zmq。
- workflow 增加 `sudo apt-get install -y libzmq3-dev` 安装 ZeroMQ 依赖。

## 本地验证
- g++ -fsyntax-only 与 g++ -c 均编译通过（仅原有警告）。

## 验证记录
| Run | 结果 |
|---|---|
| 43025306254 / 43025558962 | ✅ success |

## 验证记录
| Run | 结果 |
|---|---|
| 43025306254 (30402cc) | ✅ success |
