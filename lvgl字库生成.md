# **lvgl字库生成**

主要用于未挂载文件系统或内存空间小的嵌入式系统

.bin文件生成

git bash中
lv_font_conv \
--font SourceHanSansCN_Medium.ttf \
--size 24 \
--bpp 4 \
--format bin \
--range "0x0000-0x007F,0x00C0-0x017F,0x0400-0x04FF,0x2000-0x206F,0x2200-0x22FF,0x4E00-0x9FFF" \				
--no-compress \
--output font_medium_24.bin



## **指令说明：**

--font 使用改字库字模生成字库文件

--size 字符大小（高度）

--bpp 抗锯齿位数（清晰度与体积平衡）

--format 输出格式

--range Unicode字符所在范围

--no-compress 禁用压缩

-- output 文件名

## **生成条件：**

### 软件：

Node.js

git bash

### 软件内部工具：

```
lv_font_conv
```

### 检查工具步骤：

```
node -v
npm -v			//检查node.js版本
which lv_font_conv //检查lv_font_conv安装
```

