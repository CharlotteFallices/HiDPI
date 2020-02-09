# HiDPI

## 说明

HiDPI可以在无需RDM软件的前提下为中低分辨率屏幕启用HiDPI选项,并且可以使用原生的HiDPI设置.<br>
macOS的DPI机制不同于Windows,例如,在Windows下1080p屏幕具有125％和150％缩放比例选项，而同一屏幕只能在macOS下调整分辨率，这使得默认分辨率Fonts和UI看起来很小,但如果降低分辨率则会显得模糊,HiDPI可以解决这些问题.

同时，HiDPI还可以通过注入已修复的EDID来修复闪屏,或者在睡眠和唤醒后修复启动屏幕,当然,这种修复可能没有必要.

在启动的第二阶段中,徽标始终会稍微放大,因为分辨率是伪造的.

## 使用方法

在`bash`终端输入以下命令即可:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/CharlotteFallices/one-key-hidpi/master/hidpi.sh)"
```

## 恢复

### 命令恢复

请在`bash`终端运行命令并选择`选项3`以关闭HiDPI.

### 恢复模式(不建议使用)

如果使用此脚本后无法进入系统,请进入恢复模式或使用`clover -x`以安全模式进入系统.
然后打开终端并:

1. 快捷恢复
    
```bash
ls /Volumes/
cd /Volumes/{系统盘盘符}/System/Library/Displays/Contents/Resources/Overrides/HIDPI

./disable
```

2. 手动恢复

使用终端删除 `/System/Library/Displays/Contents/Resources/Overrides` 下显示器`VendorID`对应的文件夹.
请复制`HIDPI/backup`文件夹至外部设备并请关闭显示器.
最后请执行以下命令:

```bash
ls /Volumes/
cd /Volumes/{系统盘盘符}/System/Library/Displays/Contents/Resources/Overrides
EDID=($(ioreg -lw0 | grep -i "IODisplayEDID" | sed -e "/[^<]*</s///" -e "s/\>//"))
Vid=($(echo $EDID | cut -c18-20))
rm -rf ./DisplayVendorID-$Vid
cp -r ./HIDPI/backup/* ./
```

## 附录

借鉴了以下内容的思路:
[[Solved]: Black Screen with GTX 1070, LG Ultrafine 5k, Sierra 10.12.4](https://www.tonymacx86.com/threads/solved-black-screen-with-gtx-1070-lg-ultrafine-5k-sierra-10-12-4.219872/page-4#post-1644805)

[syscl/Enable-HiDPI-OSX](https://github.com/syscl/Enable-HiDPI-OSX)

