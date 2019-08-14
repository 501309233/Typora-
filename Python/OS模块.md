[TOC]

# OS模块

os模块提供了很多与操作系统相关的函数，可以处理文件和目录的操作，比如：显示当前目录下所有文件/删除某个文件/获取当前的绝对路径

| 方法                           | 描述                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| os.name                        | 显示当前使用的平台                                           |
| os.getcwd()                    | 显示当前python脚本工作路径                                   |
| os.listdir('dirname')          | 返回指定目录下的所有文件和目录名                             |
| os.remove('filename')          | 删除一个文件                                                 |
| os.makedirs('dirname/dirname') | 可生成多层递归目录                                           |
| os.rmdir('dirname')            | 删除单级目录                                                 |
| os.rename('oldname','newname') | 重命名文件                                                   |
| os.system()                    | 运行shell命令。仅仅在一个子终端运行系统命令，而不能获取命令执行后返回信息。如果命令行下执行，结果直接打印出来 |
| os.popen()                     | 运行shell命令，该方法不但执行命令还返回执行后的信息对象，好处在于：将返回的结果赋予一变量，便于程序的处理 |
| os.sep                         | 显示当前平台下路径分隔符                                     |
| os.linesep                     | 给出当前平台使用的行终止符                                   |
| os.environ                     | 获取系统环境变量                                             |
| os.path.abspath(path)          | 显示当前绝对路径                                             |
| os.path.dirname(path)          | 返回该路径的父目录                                           |
| os.path.basename(path)         | 返回该路径的最后一个目录或文件，如果path以/或\结尾，那么就会返回空值 |
| os.path.exists(path)           | 如果path存在，返回True                                       |
| os.path.isfile(path)           | 如果path是一个文件，返回True                                 |
| os.path.isdir(path)            | 如果path是一个目录，返回True                                 |
| os.stat()                      | 获取文件或者目录信息                                         |
| os.path.split(path)            | 将path分割成路径名和文件名（如果完全使用目录，它也会将最后一个目录作为文件名而分离，同时它不会判断文件或目录是否存在） |
| os.path.join(path,name)        | 连接目录与文件名或目录，结果为path/name                      |
| os.walk(path)                  | 遍历path,返回一个对象，每个部分都是一个三元组，（'目录x',[目录x下的目录list]，目录x下面的文件） |

## 获取系统信息

![1565697431361](E:\Typora笔记\Python\assets\1565697431361.png)

## 处理文件、目录、路径

前提：当前目录下新建一个qsx.txt文件

功能：判断是否存在文件、再重命名文件、再删除文件

![1565697561431](E:\Typora笔记\Python\assets\1565697561431.png)

示例3：

功能：创建目录，切换目录，删除目录

![1565697612828](E:\Typora笔记\Python\assets\1565697612828.png)

![1565697640088](E:\Typora笔记\Python\assets\1565697640088.png)

## 运行Shell命令

在Windows、Linux平台都可以使用os.system()/os.popen()运行shell命令，但是需要注意Windows和Linux平台的Shell命令不同，例如Windows平台列出当前目录下文件的命令是dir,而Linux平台使用的命令是ls

### os.system()运行shell命令

![1565697821358](E:\Typora笔记\Python\assets\1565697821358.png)

在交互模式下，执行os.system()是可以返回执行结果，但是如果在.py文件中执行os.system('ls qsx*')是不能获取命令执行后的返回信息，如果想要执行shell命令后又获取命令的返回信息，请使用os.popen()

### os.popen()运行shell命令

os.popen()与os.system()使用和功能基本相同，唯一的区别就是os.popen()方法执行系统命令并返回信息对象，注意返回的是信息对象，需要再通过read()、readlines()方法获取到详细返回信息内容

![1565698038043](E:\Typora笔记\Python\assets\1565698038043.png)