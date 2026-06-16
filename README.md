# muduo 环境搭建

## 环境信息

| 项目 | 版本 |
|------|------|
| OS | Ubuntu 24.04 |
| 编译器 | g++ 13.3.0 |
| 构建工具 | CMake 3.28.3, GNU Make 4.3 |
| muduo 源码 | https://github.com/chenshuo/muduo (2013, 陈硕) |
| 安装日期 | 2026-06-08 |

## 安装步骤

```bash
# 1. 克隆源码
git clone https://github.com/chenshuo/muduo.git
cd muduo

# 2. 编译
./build.sh

# 3. 安装（集中到统一目录）
./build.sh install
```

## 关键路径

```
~/muduo/                                    ← 源码
~/muduo/build/release-cpp11/                ← 编译中间产物
~/muduo/build/release-cpp11/bin/            ← 可执行文件
~/build/release-install-cpp11/include/      ← 头文件
~/build/release-install-cpp11/lib/          ← 静态库
```

## 编译产物

### 静态库

| 文件 | 用途 |
|------|------|
| `libmuduo_base.a` | 基础工具（日志、线程、时间戳等） |
| `libmuduo_net.a` | 网络核心（TcpServer, EventLoop, Buffer 等） |
| `libmuduo_http.a` | HTTP 服务 |
| `libmuduo_protobuf_codec.a` | Protobuf 编解码 |
| `libmuduo_curl.a` | Curl 封装 |

### 示例程序（对应《Linux 多线程服务端编程》第 7 章）

| 可执行文件 | 说明 | ChatRoom 关联 |
|------|------|------|
| `simple_echo` | echo 回显服务 | **ChatRoom 骨架** |
| `simple_daytime` | 发送时间后断开 | 练手 |
| `simple_discard` | 接收并丢弃数据 | 练手 |
| `simple_chargen` | 持续推送字符流 | 练手（消息推送模式） |
| `pingpong_server` / `pingpong_client` | 收发往返测试 | 心跳检测雏形 |
| `filetransfer_download` | 文件传输 | 文件发送功能 |
| `hub` | 发布/订阅中心 | 群组消息分发 |

### 快速测试

```bash
# 终端1：启动 echo 服务
~/muduo/build/release-cpp11/bin/simple_echo

# 终端2：连接测试
nc localhost xxxx (具体端口查看具体代码)
# 输入任意文字，服务器回显相同内容
```

## 编写自己的 muduo 程序：编译命令模板

```bash
g++ -std=c++11 \
    -I ~/build/release-install-cpp11/include \
    -L ~/build/release-install-cpp11/lib \
    your_program.cpp \
    -lmuduo_net -lmuduo_base \
    -lpthread \
    -o your_program
```
