# ARM64 環境設定指南

## 概述

本文檔說明在 ARM64 架構（如 Apple Silicon Mac）上優化 OpenClaw 運行的環境變數設定建議。

## 推薦環境變數

### 1. UV_THREADPOOL_SIZE

```bash
export UV_THREADPOOL_SIZE=16
```

**說明**: 設定 libuv .thread pool 大小，優化 ARM64 環境下的 I/O 並行處理能力。

### 2. NODE_OPTIONS

```bash
export NODE_OPTIONS="--max-old-space-size=2048 --dns-result-order=ipv4first"
```

**參數說明**:
- `--max-old-space-size=2048`: 設定 V8 堆記憶體上限為 2GB，防止記憶體過度消耗
- `--dns-result-order=ipv4first`: 優先使用 IPv4 DNS 解析，提升網路連線穩定性

## 設定方式

### 方式一：Shell 設定檔

將以下內容加入 `~/.zshrc` 或 `~/.bashrc`:

```bash
# ARM64 OpenClaw 優化設定
export UV_THREADPOOL_SIZE=16
export NODE_OPTIONS="--max-old-space-size=2048 --dns-result-order=ipv4first"
```

### 方式二：Gateway 啟動前設定

在啟動 Gateway 前執行：

```bash
export UV_THREADPOOL_SIZE=16
export NODE_OPTIONS="--max-old-space-size=2048 --dns-result-order=ipv4first"
openclaw gateway start
```

## 注意事項

1. **不建議使用**: `--optimize-for-size` 和 `--force-timer-accuracy`（基於 Subagent V-A2 建議）
2. **測試驗證**: 重啟 Gateway 後，CPU 使用率應下降 10-15%

## 參考資料

- Issue: OC-ARM64-INV-20260505-001
- GitHub Issue: #65375
