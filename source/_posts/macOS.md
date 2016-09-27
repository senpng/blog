title: macOS Reconfigure
date: 2016-9-27
---

## 应用
### 基本应用
* QQ
* MWeb
* Xcode8
* SQLiteProfessional
* xScope
* Cornerstone
* iTranslate

### Cocoapods
```bash
gem sources --remove https://rubygems.org/
gem sources -a https://ruby.taobao.org/
sudo gem install cocoapods --pre
```

### Brew 安装应用
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew analytics off
```

#### Git SSH-Key
```bash
ssh-keygen -t rsa -b 4096 -C "senpng@qq.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

#### NVM
```bash
brew install nvm
nvm install node && node alias default node //安装最新node并设置为默认的版本
mkdir ~/.nvm
```
 添加到`~/.zshrc`:
```
export NVM_DIR="$HOME/.nvm"
. "$(brew --prefix nvm)/nvm.sh"
```

#### ReactNative
```bash
brew install watchman
npm install -g react-native-cli
```

#### Proxifier
```bash
brew cask install proxifier
```
<!--注册码：`P427L-9Y552-5433E-8DSR3-58Z68`
[ppx](/media/SenPng.ppx)-->

#### Sublime Text3
```bash
brew cask install sublime-text
```
安装Package Control
```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```
配置
```bash
cd /Users/SenPng/Library/Application\ Support/Sublime\ Text\ 3/Packages/User
git init
git remote add origin git@github.com:senpng/SublimeText3-User.git
git pull origin master
git branch --set-upstream-to=origin/master master
git pull
```
注册码
```
注册码
—– BEGIN LICENSE —–
Michael Barnes
Single User License
EA7E-821385
8A353C41 872A0D5C DF9B2950 AFF6F667
C458EA6D 8EA3C286 98D1D650 131A97AB
AA919AEC EF20E143 B361B1E7 4C8B7F04
B085E65E 2F5F5360 8489D422 FB8FC1AA
93F6323C FD7F7544 3F39C318 D95E6480
FCCC7561 8A4A1741 68FA4223 ADCEDE07
200C25BE DBBC4855 C4CFB774 C5EC138C
0FEC1CEF D9DCECEC D3A5DAD1 01316C36
—— END LICENSE ——
```

#### Clean My Mac3
```bash
brew cask install cleanmymac
```
<!--激活码：`id792986135929uks`-->

#### iStat Menus
```bash
brew cask install istat-menus
```
<!--注册码：`9665-5955-6856-2071-0000`-->

#### Android Studio
```bash
brew cask install java android-sdk android-studio
```
添加环境变量到`~/.zshrc`
```
export ANDROID_HOME=/usr/local/opt/android-sdk
```

#### Archiver
需要下载`2.x`版本
<!--注册码：`ARCA-7ME5-Z2W5-U3JH-323P`-->

#### NTFS14
```bash
brew cask install paragon-ntfs
```
<!--激活文件放入`/Library/Application Support/Paragon Software/`
[NTFS14](/media/NTFS14.)-->

#### Sketch
```bash
brew cask install sketch
```

#### Dash
```bash
brew cask install dash
```
<!-- [license](/media/license.dash-license) -->

#### Reveal
```bash
brew cask install reveal
```
<!-- [macreveal-Reveal-iOS](/media/macreveal-Reveal-iOS.reveallicense) -->

#### Other
```bash
xcode-select --install
brew cask install google-chrome java android-studio shadowsocksx thunder paw
```
```bash
npm i -g cnpm http-server
npm i -g hexo-cli
```
### ZSH
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
#### 配置
##### 主题
```
ZSH_THEME="amuse"
```
修改`vim ~/.oh-my-zsh/themes/amuse.zsh-theme` --> 🕒
##### 插件
###### autojump
```bash
brew install autojump
```
修改`~/.zshrc`配置
```bash
plugins=(git autojump zsh_reload)
```
###### powerline
安装`powerline`
```bash
brew install python
pip install git+git://github.com/powerline/powerline

git clone https://github.com/powerline/fonts.git
cd fonts
./install.sh
cd .. && rm -rf fonts 
```
设置终端字体
```
Meslo LG S for Powerline
```
修改`~/.zshrc`配置
```
. /usr/local/lib/python2.7/site-packages/powerline/bindings/zsh/powerline.zsh
```
###### macvim
安装`macvim`
```bash
brew install macvim --env-std --with-override-system-vim
```
配置Vim `~/.vimrc`
```
set rtp+=/usr/local/lib/python2.7/site-packages/powerline/bindings/vim #这里需要改成自己的安装路径
set guifont=Meslo\ LG\ S\ for\ Powerline
set laststatus=2
let g:Powerline_symbols = 'fancy'
set encoding=utf-8
set t_Co=256
set number
set fillchars+=stl:\ ,stlnc:\
set term=xterm-256color
set termencoding=utf-8
```
安装`Pathogen`
```bash
mkdir -p ~/.vim/autoload ~/.vim/bundle && \
cd ~/.vim/
git clone https://github.com/tpope/vim-pathogen.git
mv vim-pathogen/autoload/* ./autoload
rm -rf vim-pathogen
```
在 `~/.vimrc` 中添加
```
execute pathogen#infect()
```
下载颜色配置
```bash
cd ~/.vim/bundle
//git clone git://github.com/altercation/vim-colors-solarized.git #solarized颜色主题
git clone https://github.com/endel/vim-github-colorscheme.git #github颜色主题
```
修改 `.vimrc`，在 `execute pathogen#infect()` 下面添加
```
syntax enable
set background=light
colorscheme github
```

## 设置
### Finder
* 桌面显示的项目：全去掉
* 边栏
* 高级：搜索当前文件夹
* 显示资源文件夹

### 系统偏好设置
#### 开启**任何来源**
```bash
sudo spctl --master-disable
```

#### Mission Control
* 关闭Dashboard
* 触发角

#### Docker
* 大小
* 关闭应用程序显示指示灯

#### 安全与隐私
* 打开防火墙

#### Spotlight

#### 触摸板

#### iCloud
* 照片：开启iCloud和照片流

#### 用户与群组
* 密码提示：当你看到这行字，说明你离我电脑太近了！
* 关闭客户登录

### 其它
下载图标


