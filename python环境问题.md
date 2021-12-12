#### pip 使用 requirements.txt 安装

	pip install -r requirements.txt

#### pip freeze

使用 `pip freeze` 会输出所有在本地已安装的包（但不包括 `pip`、`wheel`、`setuptools` 等自带包），若需要输出内容与 `pip list` 一致，需使用 `pip freeze -all`。

	pip freeze > requirements.txt

#### pip freeze 导出含有本地路径 (@ file:///) 问题

部分模块显示了 `@ file:///...`，而不是具体的版本号。此时，如果我们直接在其他机器上边使用 `pip install -r requirements.txt` 安装模块时，就会遇到如下错误：

	ERROR: Could not install packages due to an EnvironmentError: [Errno 2] No such file or directory: xxxxxx

遇到此类问题时，如果显示本地路径的包比较少，可以手动将其更改为版本号。
也可以暂时考虑使用如下命令生成 `requirements.txt` 文件：

	pip list --format=freeze > requirements.txt

（有的博客说：“使用上述命令导出的文件中，会包含如下几个包：distribute，pip，setuptools，wheel，建议手动删除！”，但好像并不会包含这几个包）

#### conda环境迁移到另一个服务器

1. 在原服务器的base环境安装**conda-pack**
`pip install conda-pack`
2. 打包环境
`conda pack -n myenv（myenv是环境名）`
打包后的压缩包在当前目录，把它下载后上传到另一个服务器
3. 在上传压缩包的目录创建文件夹
`mkdir -p myenv`
4. 解压到文件夹
`tar -xzf myenv.tar.gz -C myenv`
5. 将 myenv 文件夹移动到 ~/anaconda3/envs/ 下
`mv myenv ~/anaconda3/envs/`
6. 激活环境就可以使用了
`conda activate myenv`

#### pip换源

- pypi 清华大学源：https://pypi.tuna.tsinghua.edu.cn/simple
- pypi 豆瓣源 ：http://pypi.douban.com/simple/
- pypi 腾讯源：http://mirrors.cloud.tencent.com/pypi/simple
- pypi 阿里源：https://mirrors.aliyun.com/pypi/simple/

临时改成国内源：pip install xxxx -i https://pypi.tuna.tsinghua.edu.cn/simple

把国内源设为默认：pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

#### conda换源

https://zhuanlan.zhihu.com/p/87123943

#### conda创建新环境

	conda create -n myenv python=3.8

#### conda删除环境

先`conda deactivate`回到base环境，然后`conda remove -n myenv --all`

