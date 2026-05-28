### 方案一：使用 FontForge 图形界面直接合并（最推荐，无需代码）

这是最直观的方法，完全不需要处理命令行。

1. **正确启动 FontForge**：安装后，请从 **Windows 的开始菜单**中找到 "FontForge" 文件夹，然后点击 **"FontForge (normal user)"** 来启动程序。这样能确保环境正确加载。
2. **用 FontForge 打开第一个字体**：
   - 打开 FontForge 后，点击菜单栏的 **`File`** -> **`Open`**。
   - 在弹出的对话框中，找到并选择你的 **`HarmonyOS_Sans_Naskh_Arabic_Bold.ttf`**（阿拉伯语字体），点击“打开”。
3. **执行合并操作**：
   - 这是最关键的一步。在 FontForge 的主窗口中，点击菜单栏的 **`Element`** -> **`Font Info`**。
   - 在弹出的“Font Info”窗口中，从左侧列表里找到并点击 **`Lookups`**。这一步是为了准备合并环境。
   - 关闭“Font Info”窗口。然后，再次点击菜单栏的 **`File`** -> **`Merge Fonts...`**。
   - 在文件选择对话框中，选择你的 **`HarmonyOS_SansSC_Bold.ttf`**（中文字体）文件。
4. **处理可能出现的提示**：
   - FontForge 可能会弹出一个提示框，告诉你中文字体包含垂直排版表（也就是之前脚本里提到的 `vhea`/`vmtx` 表），询问如何处理。一般来说，选择 **`Keep old table`（保留旧表）**或者 **`Don't add this table`（不添加此表）** 都是安全的。
   - 如果询问关于编码或字符冲突，通常选择 **`Auto`** 或 **`Yes`** 即可。
5. **保存合并后的字体**：
   - 等待合并完成（可能需要几秒钟到一分钟）。
   - 点击菜单栏 **`File`** -> **`Generate Fonts...`**。
   - 在弹出的对话框中，选择输出格式为 **`TrueType`** (`.ttf`)，然后选择一个你喜欢的文件名（例如 `Final_Merged_Font.ttf`）和保存位置，最后点击 **`Generate`** 按钮。
   - 可能会再次弹出确认窗口，直接点击 **`Yes`** 或 **`OK`** 即可。

### 方案二：如果启动 FontForge 仍有问题

如果点击 "FontForge (normal user)" 后无法启动或报错，可以尝试以下两种方式：

- **作为管理员运行**：在开始菜单的 "FontForge (normal user)" 图标上点击右键，选择 **“更多”** -> **“以管理员身份运行”**，这有时能解决权限问题。
- **使用 FontForge 的“一键启动”脚本**：进入 FontForge 的安装目录（通常在 `C:\Program Files (x86)\FontForgeBuilds` 或 `C:\Program Files\FontForge` 下），找到并直接运行 `run_fontforge.exe` 或 `fontforge.bat` 文件，它们会帮你配置好临时的命令行环境。



### 方案三：最终简化版 Python 脚本（如果必须用命令行）

如果你坚持想用命令行，可以试试这个更简单的脚本。创建一个新的文本文件，命名为 `merge_simple.py`，把下面的代码复制进去：

```python
#!/usr/bin/env python
import sys
import os

# 硬编码字体文件路径
ARABIC_FONT = "HarmonyOS_Sans_Naskh_Arabic_Bold.ttf"
CHINESE_FONT = "HarmonyOS_SansSC_Bold.ttf"
OUTPUT_FONT = "Final_Merged_Font.ttf"

# 检查文件是否存在
if not os.path.exists(ARABIC_FONT):
    print(f"错误: 找不到 {ARABIC_FONT}")
    sys.exit(1)
if not os.path.exists(CHINESE_FONT):
    print(f"错误: 找不到 {CHINESE_FONT}")
    sys.exit(1)

# 构建并运行 FontForge 命令
cmd = f'fontforge -lang=ff -c "Open(\'{ARABIC_FONT}\'); MergeFonts(\'{CHINESE_FONT}\'); Generate(\'{OUTPUT_FONT}\');"'
print("正在执行命令:", cmd)
os.system(cmd)
```

然后在 **FontForge 的命令行环境**中运行它。你可以在开始菜单的 FontForge 文件夹里找到 **"FontForge Command Prompt"**，点击打开后，用 `cd` 命令切换到你的字体文件夹，再输入 `python merge_simple.py` 来运行。

### 合并后的验证与下一步

无论你用哪种方法，成功合并出 `Final_Merged_Font.ttf` 后，建议先用 FontForge 打开它，随便看几个中文字符和阿文字符是否都存在，确认无误后，就可以继续用 LVGL 工具生成 `.c` 文件了。在转换时，记得通过 `-r` 参数包含所有需要的 Unicode 范围：

```bash
lv_font_conv --font font_all_24_blod.ttf -r 0x20-0x7F,0x4E00-0x9FFF,0x0600-0x06FF,0x0080-0x00FF,0x0400-0x04FF --size 24 --bpp 4 --format lvgl --output my_multilang_font.c
```



