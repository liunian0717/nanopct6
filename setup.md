step1更换软件源头为国内镜像
sudo cp /etc/apt/sources.list /etc/apt/sources.list.org
sudo sed -i -e 's/ports.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
sudo apt-get update

step2安装pip
sudo apt install python3-pip
step3:pip换源 注意使用，这里没有sudo
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/
pip config set install.trusted-host pypi.tuna.tsinghua.edu.cn

step4:安装virtualenvwrapper,并设置workon_home目录
pip3 install virtualenvwrapper
mkdir /py_venv
sudo apt install nano
nano ~/.bashrc
在末尾添加以下内容保存退出
export WORKON_HOME=/home/pi/py_venv
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
VIRTUALENVWRAPPER_VIRTUALENV=/home/pi/.local/bin/virtualenv
source /home/pi/.local/bin/virtualenvwrapper.sh
在bash下申明环境变量 
source ~/.bashrc

step5:apt安装vscode
安装必要的依赖工具：
sudo apt install curl gpg dirmngr software-properties-common apt-transport-https
导入微软的 GPG 密钥：
curl -fsSL https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor | sudo tee /usr/share/keyrings/vscode.gpg > /dev/null
添加 Visual Studio Code 仓库：
echo "deb [arch=amd64,arm64,armhf signed-by=/usr/share/keyrings/vscode.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null
运行apt update
sudo apt update
根据需要安装不同版本的 Visual Studio Code
sudo apt install code          # 安装稳定版
sudo apt install code-insiders # 安装预览版

step6:安装中文包
sudo apt-get install language-pack-zh-hans
然后，你需要将系统的默认语言设置为中文，可以在终端执行下面的命令：
sudo update-locale LANG=zh_CN.UTF-8
最后，重启系统即可使用中文：
sudo reboot
登陆 Ubuntu 2204 系统的 GNOME 桌面你会发现应用程序的名称和应用程序的菜单还都是英文，因为还没有安装 GNOME 桌面对应的语言包。
sudo apt install language-pack-gnome-zh-hans-base language-pack-gnome-zh-hans 
默认情况下 GNOME 文档没有中文版，使用如下命令安装 GNOME 中文用户文档：
sudo apt install gnome-user-docs-zh-hans
通过以上设置 Ubuntu 2204 系统以及 GNOME 界面和程序被更改成了中文，但是 Ubuntu 2204 系统中安装的非 GNOME 程序程序使用的语言依旧是英文。
sudo apt-cache search --names-only program-name | grep zh
解释说明：
program-name：需要安装简体中文语言包的程序名
grep zh：有的程序的简体中文语言包后缀为 zh-hans，有的为 zh-cn，因此获取包含 zh 的软件包
例如输入：
sudo apt-cache search --names-only libreoffice | grep zh
显示信息：
libreoffice-help-zh-cn - office productivity suite -- Chinese_simplified help
libreoffice-help-zh-tw - office productivity suite -- Chinese_traditional help
libreoffice-l10n-zh-cn - office productivity suite -- Chinese_simplified language package
libreoffice-l10n-zh-tw - office productivity suite -- Chinese_traditional language package
然后
sudo apt install libreoffice-l10n-zh-cn

step7:安装Google拼音
sudo apt-get install fcitx-googlepinyin
安装设置完成后发现全家桶里的 chromium-browser 不能使用
此时卸载 chromium-browser 
重新安装即可，但是安装时间比较长，要稍稍有耐性。

linux下固定全部apt软件版本的方案
可以使用apt-mark命令或编辑/etc/apt/preferences文件来固定软件包版本，防止自动更新。apt-mark允许标记软件包保持当前版本，而preferences文件则可精确控制每个软件包的版本和优先级。需谨慎操作，尤其是涉及系统核心组件时，并建议在更改前备份系统。
sudo apt-mark hold $(apt list --installed | cut -d/ -f1)
这将锁定当前安装的所有软件包的版本，防止他们在升级时被更新。
如果要接触锁定
sudo apt-mark unhold package_name

step8:重新安装的chrome没有硬件加速功能，这里可以安装panfork驱动
sudo add-apt-repository ppa:liujianfeng1994/panfork-mesa
sudo add-apt-repository ppa:liujianfeng1994/rockchip-multimedia
sudo apt update
sudo apt dist-upgrade
sudo apt install mali-g610-firmware rockchip-multimedia-config

filezilla visit pc server,when the file is code in chinese,can recgnized,
console_method:when create new station ,look right font-set, use utf-8
