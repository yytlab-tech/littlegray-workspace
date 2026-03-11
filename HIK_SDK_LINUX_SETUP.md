# Hik SDK Linux 部署（历史录像下载）

## 1) 准备文件
把下面两个文件放到 Linux 服务器同一目录：
- `hik_sdk_download_linux.py`
- `libhcnetsdk.so`（来自 Device Network SDK_Linux64 包）

建议目录：
```bash
/opt/hik-sdk/
```

## 2) 安装依赖
```bash
sudo apt update
sudo apt install -y python3 python3-pip
```

## 3) 设置动态库路径
```bash
cd /opt/hik-sdk
export LD_LIBRARY_PATH=$PWD:$LD_LIBRARY_PATH
```

如果 SDK 包里有 `libcrypto.so*`, `libssl.so*` 也放在同目录。

## 4) 运行下载
```bash
cd /opt/hik-sdk
python3 hik_sdk_download_linux.py \
  --sdk-lib ./libhcnetsdk.so \
  --host 10.108.30.252 \
  --port 8000 \
  --username admin \
  --password '你的密码' \
  --channel 1 \
  --start '2026-03-10 10:00:00' \
  --end '2026-03-10 10:10:00' \
  --out ./downloads/history_20260310_1000_101.mp4
```

## 5) 常见错误

### `[ERROR] sdk lib not found`
- 路径不对，确认 `--sdk-lib` 指向真实文件。

### `NET_DVR_Init failed`
- 依赖库缺失，检查目录内是否有 SDK 附带 so。
- 运行 `ldd libhcnetsdk.so` 看缺啥。

### `login failed`
- 检查 `host/port/username/password`
- NVR SDK 端口通常是 `8000`。

### `GetFileByTime failed`
- 通道号不对（试 `1/2/3...`）
- 时间段没有录像。
