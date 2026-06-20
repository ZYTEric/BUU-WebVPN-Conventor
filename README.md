# BUU Wengine WebVPN URL 转换器

> 北京联合大学（BUU） WebVPN (Wengine) 的 URL 加密/解密工具

将内网域名与 WebVPN 加密 URL 进行双向转换。支持交互式、命令行单次、批量转换三种模式。

## 快速开始

```bash
pip install pycryptodome
python wengine_url.py
```

## 使用示例

### 内网地址 → WebVPN 链接（加密）

```bash
python wengine_url.py my.buu.edu.cn
# → https://wvpn.buu.edu.cn/https/77726476706e69737468656265737421fdee0f9e322526557a1dc7af96/

python wengine_url.py "http://xgxt.buu.edu.cn/mobile/comprehensive/myBaseScores"
# → https://wvpn.buu.edu.cn/http/77726476706e69737468656265737421e8f0598869327d45450d8db9d6562d/mobile/comprehensive/myBaseScores
```

### WebVPN 链接 → 内网地址（解密）

```bash
# 自动识别（直接丢进去）
python wengine_url.py "https://wvpn.buu.edu.cn/https/77726476706e69737468656265737421e8f0598869327d45450d8db9d6562d/mobile/..."

# 或指定 --decrypt
python wengine_url.py --decrypt "https://wvpn.buu.edu.cn/https/77726476706e69737468656265737421e8f0598869327d45450d8db9d6562d/mobile/..."

# 或只解密 hex 串
python wengine_url.py --decrypt 77726476706e69737468656265737421fdee0f9e322526557a1dc7af96
# → my.buu.edu.cn
```

### 批量转换

```bash
# 准备 domains.txt，每行一个域名
echo "my.buu.edu.cn" >> domains.txt
echo "jwxt.buu.edu.cn" >> domains.txt

python wengine_url.py --batch domains.txt
```

### 交互模式

```bash
python wengine_url.py
```

```
>>> my.buu.edu.cn
  → https://wvpn.buu.edu.cn/https/77726476706e69737468656265737421fdee0f9e322526557a1dc7af96/

>>> https://wvpn.buu.edu.cn/https/77726476706e69737468656265737421fdee0f9e322526557a1dc7af96/
  → my.buu.edu.cn
```

## 原理

| 项目 | 说明 |
|------|------|
| 算法 | AES-256-CFB (segment_size=128) |
| 默认密钥 | `wrdvpnisthebest!`（各高校可能不同） |
| 默认 IV | `wrdvpnisthebest!`（与密钥相同） |

## 配置

如果你的学校使用不同的密钥，修改脚本顶部的配置区：

```python
WVPN_HOST = "wvpn.yourschool.edu.cn"  # WebVPN 域名
DEFAULT_KEY = "your-school-key"        # 加密密钥
DEFAULT_IV = "your-school-iv"          # 初始化向量
```

## 适用学校

- **北京联合大学**（默认配置）
- 其他使用 **Wengine SSL VPN** 的高校（修改密钥即可）

## 依赖

```
pycryptodome
```

## ⚠️ 免责声明

1. **仅限教育研究**：本工具仅用于密码学学习和技术研究目的
2. **合法授权**：请在拥有合法访问权限的系统上使用
3. **尊重隐私**：不得用于解密未授权的 URL 或侵犯他人隐私
4. **遵守校规**：使用本工具应遵守所在学校的相关规定和政策
5. **责任自负**：使用者对使用本工具产生的后果自行负责

## 许可证

MIT License - 详见 [LICENSE](LICENSE)

## 参考与致谢

- 密钥捕获思路参考 [xiaobei97/wengine-vpn-decryptor](https://github.com/xiaobei97/wengine-vpn-decryptor)
