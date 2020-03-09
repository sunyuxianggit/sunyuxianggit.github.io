# Python文件打包成可执行文件

Python是一个脚本语言，被解释器解释执行。它的发布方式：

## .py 文件

没什么好讲的，开源项目或者个人练习，直接提供源码最简单粗暴，需要使用者自行安装Python并且安装依赖的各种库。

## .pyc 文件

如果觉得源码写的差劲不好意思被别人看到，或者出于保密等不愿意源码被运行者看到，可以使用pyc文件发布，pyc文件是Python解释器可以识别的二进制码，故发布后也是跨平台的，需要使用者安装相应版本的Python和依赖库。
<!-- more -->
```
#代码
import py_compile
py_compile.compile("D:\Python\main.py")            # 相对路径或绝对路径

#命令行下
python -m py_compile test.py
#会在相同路径里面创建__pycache__文件夹，编译过的pyc文件就在里面

#多个文件
import compileall
compileall.compile_dir("存放海量py的目录")
```

## 可执行exe文件

- pyInstaller
	  1.安装pyInstaller

	```
	$ pip install pyinstaller #安装
	$ pyinstaller --version #查看版本
	```

    2.如果查看版本报错

    ```
    'pyinstall' is not recognized as an internal or external 		command,operable program or batch file.#需要系统变量里的Path变量下添加其所在目录，然后重启命令行即可.
    ```

    3.使用pyInstaller：
  ```pyinstaller -F helloworld.py```
  
  
  
- py2exe

	1. 命令行``pip install py2exe``安装
	
	1. 在命令行内测试你的程序确定可以运行
	
	   ``python helloworld.py``
	
	2. 创建你自己的执行脚本 (setup.py)
	
	   ```
	   from distutils.core import setup
	   
	   import py2exe
	   
	        
	   setup(console=["helloworld.py"]) #这里helloworld.py替换成你的脚本
	   ```
	
	 3. 在命令行Run your setup script
	
	    ``python setup.py py2exe``
	 4. 然后再dist文件夹下就会看到生成的.exe 文件了
	
	5. 如果出现``IndexError: tuple index out of range``的话是因为py2exe停止支持3.4以上版本，可以换用这个地方的py2exe, 但是好像也是有问题
	
	   ref：https://stackoverflow.com/questions/41578808/python-indexerror-tuple-index-out-of-range-when-using-py2exe
	
	   ref：https://github.com/albertosottile/py2exe
	
	
	
- cx_Freeze

    1. ``$ pip install cx_Freeze``安装

    2. ```$ cxfreeze hello.py --target-dir dist```生成执行文件，如果报错无法识别就参考这个解决

       ref:https://stackoverflow.com/questions/25242860/cxfreeze-command-not-found-in-windows/25243419#25243419 

- auto-py-to-exe 2.6.6
  1. ``$ pip install auto-py-to-exe`` 安装
  2. ````$ auto-py-to-exe`` 使用

