# KdeButi
KDE美化
arch安装必要软件
# 美化
## 窗口开启关闭动效
https://github.com/Schneegans/Burn-My-Windows
```
wget https://github.com/Schneegans/Burn-My-Windows/releases/latest/download/burn_my_windows_kwin4.tar.gz
mkdir -p ~/.local/share/kwin/effects
tar -xf burn_my_windows_kwin4.tar.gz -C ~/.local/share/kwin/effects
```
可能需要重启
## 虚拟桌面切换
## 右键菜单动效
```
git clone https://github.com/kde-yyds/kwin4_effects_foldpopups/ #clone this repository
mkdir -p ~/.local/share/kwin/effects/
cp -r kwin4_effects_foldpopups/kwin4_effects_foldpopups/ ~/.local/share/kwin/effects/
```
## 光标
安装在 /usr/share/icons/ 或 ~/.icons/ 目录下
