## 配置Grub

### 1 Grub的配置文件

Grub启动时通常从/boot/grub/grub.cfg读取引导配置

grub.cfg文件以/ect/grub.d为模板，以/etc/default/grub作为配置，被grub.mkconfig命令自动生成

#### 修改配置文件

1. 修改/etc/default/grub中的功能参数
2. 执行：sudo gedit /etc/default/grub
3. 执行：sudo update-grub

### 2 Grub 命令

在系统启动时，在Grub界面按下按键C即可进行命令行模式，控制Grub的行为