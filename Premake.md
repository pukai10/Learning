# Premake使用
命令行使用：
```
premake5 [action]
```
action如下，可生成对应的项目工程
```
vs2022/vs2019/vs2017/vs2015/vs2013/vs2010/vs2008/vs2005/gmake/gmake2/xcode4/codelite
```
premake5 vs2019    //生成vs2019工程解决方案
premake5 --file=MyProjectScript.lua vs2013   //不适用默认脚本 premake5.lua时，使用--file指定自定义脚本文件

# Workspaces & Projects

## 工作区

每个构建的顶层都会有一个工作区，充当项目的容器。其他工具，特别是Visual Studio,可能会使用术语 解决方案。

工作区定义一组通用的生成配置和平台，用于所有包含的项目，您还可以在此级别指定其他构建设置(定义，包含路劲等),这些设置将由项目类似地继承。

工作区是使用 __workspace__ 函数定义的。大多数构建只需要一个工作区，但如果需要，您可以自由创建更多工作区。构建配置是使用 __configurations__ 功能指定的，并且是必需的。

作为函数参数提供的工作区名称用作生成的工作区文件的默认文件名，因此最好避免使用特殊字符(空格可以)。如果要使用其他名称，请使用 __filename__ 函数来制定它。
```
workspace "HelloWorld"
  filename "Hello"
  configurations {"Debug","Release"}
```

## 项目

工作区的主要用途是充当项目的容器。项目列出了生成一个二进制目标所需的设置和源文件。几乎每个IDE都是用属于"项目"来表示此目的。在Make的世界中，您可以将项目视为一个特定库或者可执行文件的生成文件；工作区是一个元生成文件，可根据需要调用每个项目。

项目是使用 __project__ 函数定义的。必须先创建包含的工作区。
```
workspace "HelloWorld"
  configurations {"Debug","Release"}

project "MyProject"
```

项目名称(如工作区名称)用作生成的项目文件的文件名，因此请避免使用特殊字符，或使用 __filename__ 函数提供不同的值。

每个项目指定一种类型，用于确定生成的输出类型，例如控制台或者窗口化的可执行文件，或者共享库或静态库。__kind__ 函数用于指定此值。

每个项目还指定它使用的编程语言，如C++或C#.。 __language__ 函数用于设置此值。
```
project "MyProject"
	kind "ConsoleApp"
	language "C++"
```

## 地点

默认情况下，Premake会将生成的工作区和项目文件放在与定义它们的脚本相同的目录中，如果您的预制脚本位于其中，则生成的文件也将位于其中。

您可以使用 __location__ 功能更改输出位置
```
workspace "HelloWorld"
	configurations {"Debug","Release"}
	location "build"
project "MyProject"
	location "build/MyProject"
```
与预制件中的所有路径一样，应相对于脚本文件指定 location。


# 范围和继承

您可能已经从前面的示例中注意到，Premake使用伪声明性语法来指定项目信息。您可以为设置指定范围(即工作区或项目)，然后指定要放置在该范围内的设置。

范围具有层次结构：包含工作区的全局范围，而工作区又包含项目。放置在外部作用域中的值由内部作用域继承，因此工作区继承储存在全局作用域的值，项目继承储存在工作区中的值。

有时，返回并将值添加到向前声明的范围可能会有所帮助。您可以按照最初声明它的方式执行此操作:使用相同的名称调用 __workspace__ 或 __project__
```
--全局范围，所有工作区都接受这些值
defines {"GLOBAL"}

workspace "HelloWorld"
	--工作区范围继承全局范围，定义的列表值将有{"GLOBAL","WORSPACE"}
	defines {"WORKSPACE"}

project "MyProject"
	--项目范围继承工作区范围，定义的列表值将有{"GLOBAL","WORKSPACE","PROJECT"}
	defines {"PROJECT"}

workspace "HelloWorld"
	defines {"WORKSPACE2"}  --列表定义的值有{"GLOBAL","WORKSPACE","WORKSPACE2"}
```

还可以使用特殊的"*"名称选择当前范围的父级或容器，而不必知道其名称
```
-- 定义myworkspace
workspace "MyWorkspace"  
	defines { "WORKSPACE1" }  
  
-- 定义一个或两个project  
project "MyProject"  
	defines { "PROJECT" }  
  
-- 重新选择myworkspace并添加设置  
project "*"  
	defines { "WORKSPACE2" } -- 工作区定义 { "WORKSPACE1", "WORKSPACE2" }  
  
-- 重新选择全局范围  
workspace "*"
```

# 添加源文件
您可以使用 __files__ 函数将文件(源代码，资源等)添加到项目中。
```
files{
	"Hello.h",
	"*.c",
	"**.cpp"
}
```

您可以在文件模式中使用通配符来匹配一组文件。通配符 __*__ 将匹配一个目录中的文件；通配符 __**__ 将匹配一个目录中的文件，并将递归到任何子目录中。

位于其他目录中的文件应相对于脚本文件指定。例如:
```
files{"../src/*.cpp"}
```
路径应始终使用正斜杠(/)作为分隔符；预制件将根据需要转换为相应的特定平台的分隔符
## 排除文件
有时您希望添加目录中的大多数(但不是全部)文件，在这种情况下，请使用 __removefiles()__ 函数来排除指定文件
```
files { "*.c" }
removefiles { "a_file.c","another_file.c"}
```
排除也可以使用通配符
```
files { "**.c" }
removefiles { "tests/*.c" }
```
有时您可能希望排除特定目录中的所有文件，但不确定改目录在源树中的位置；
```
files {"**.c"}
removefiles{"**/win32Specific/**"}
```

# 链接(Link)
链接到外部库是通过 __links__ 功能完成的
```
links{"png","zlib"}
```
指定库时，应省略特定于系统的修饰，例如前缀或文件扩展名，预制件将根据目标平台自动合成正确的格式。
该规则的一个例外时Mac OS X框架，其中需要文件扩展名来识别它。
```
links{ "Cocoa.framework" }
```
要链接到同级项目(同一工作区中的项目)，请使用 __项目名称__。预制件将根据当前平台和配置推断出正确的库路径和名称。
```
workspace "MyWorkSpace"

	project "MyLibraryProject"

	project "MyExecutableProject"
		links {"MyLibraryProject"}
```

## 查找库
您可以告诉premake在哪里， 使用 __libdis__ 函数搜索库
```
libdirs {"libs","../mylilbs"}
```

如果需要发现库的位置，请使用 __os.findlib__ 函数
```
libdirs {os.findlib("X11")}
```

# 配置和平台
配置是要应用于构建的设置集合，包括标志和开关、头文件和库搜索目录等。每个工作区定义自己的配置名称列表；大多数IDE提供默认值"Debug"和"Release"。
## 构建配置
前面的示例演示了如何指定生成配置
```
workspace "MyWorkSpace"
	configurations {"Debug", "Release"}
```
