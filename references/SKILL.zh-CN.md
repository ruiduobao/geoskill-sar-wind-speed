# SAR海面风场反演 | SAR Sea Surface Wind Retrieval

从 SAR 后向散射系数 σ⁰ 反演海面 10 m 高度风速。采用简化的 CMOD（C-band Model）
经验模型：

    σ⁰_dB(U, φ, θ) = [a0 + a1·θ] + [s0 + s1·θ]·U·M(φ)
    M(φ) = 1 + m1·cosφ + m2·cos2φ

其中 U 为风速 (m/s)，θ 为入射角，φ 为风向与雷达视线夹角，M(φ) 为方位向调制
（迎风 > 侧风 > 顺风）。σ⁰_dB 对 U 单调递增，反演时用向量化二分法逐像元求根，
支持 CMOD5 / CMOD7 两套经验系数。

## 应用场景

- 海面风场监测、台风/气旋风场结构分析
- 海洋动力学与海气相互作用研究
- 海上风电场资源评估

## 依赖

```bash
pip install 'numpy' 'rasterio'
```

## 使用方法

### 示例 1（合成数据，离线）

```bash
python geoskill-sar-wind-speed.py --bbox 121.0 30.0 122.0 31.0 --wind-dir 45 --cmod cmod5 --synthetic --output-dir ./out
```

### 示例 2（CMOD7）

```bash
python geoskill-sar-wind-speed.py --bbox 121.0 30.0 122.0 31.0 --wind-dir 90 --cmod cmod7 --synthetic --output-dir ./out
```

### 示例 3（真实 σ⁰ 影像）

```bash
python geoskill-sar-wind-speed.py --input sigma0_db.tif --wind-dir 225 --incidence-angle 35 --output-dir ./out
```

### 示例 4（指定雷达方位）

```bash
python geoskill-sar-wind-speed.py --input sigma0_db.tif --wind-dir 180 --radar-azimuth 90 --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `wind_speed.tif` | GeoTIFF | 反演风速场 (m/s) |
| `retrieval_params.json` | JSON | 模型系数、风向、入射角等参数 |
| `output-manifest.json` | JSON | 运行清单（含合成模式 RMSE/相关系数 QA） |

## 数据源 / Source

本地 σ⁰ (dB) GeoTIFF，或 `--synthetic` 空间变化风场经 CMOD 正演的模拟场景。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传用户数据。

## License

MIT
