# wechat ubuntu搭建小程序环境
git clone https://github.com/cytle/wechat_web_devtools.git
cd wechat_web_devtools
# 自动下载最新 `nw.js` , 同时部署目录 `~/.config/wechat_web_devtools/`
./bin/wxdt install
安装 Wine

sudo apt-get install wine-binfmt
sudo update-binfmts --import /usr/share/binfmts/wine
启动 ide，开发和调试网页

./bin/wxdt # 启动