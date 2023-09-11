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
您不限于这些名称，还可以使用对您的软件项目和构建环境有意义的任何名称。
```
workspace "MyWorkSpace"
	configurations { "Debug","DebugDLL","Release","ReleaseDLL"}
```
重要的是要注意，这些名称本身没有任何意义，您可以使用任何您喜欢的名称
```
workspace "MyWorkspace"
   configurations { "Froobniz", "Fozbat", "Cthulhu" }
```
生成配置的含义取决于应用于它的设置
```
workspace "HelloWorld"
	configurations {"Debug","Release"}
	filter "configurations:Debug"
		defines {"DEBUG"}
		flags {"Symbols"}
	filter "configurations:Release"
		defines{"NDEBUG"}
		optimiz "On"
```
__filter__ 将后面详细介绍

## 平台
”平台“在这里有点用词不当，我再次遵循Visual Studio命名法。实际上，平台只是另一组构建配置的名称，提供了另一个用于配置项目的轴。
```
configurations {"Debug","Release"}
platforms{"Win32","Win64","Xbox360"}
```

设置后，列出的平台将显示在IDE的平台列表中。因此，您可以选择”Debug Win32“内部版本或”Release Xbox360"内部版本，或这两个列表的任意组合。

就像构建配置一样。平台名称本身没有任何意义。您可以通过使用 __filter__ 功能应用设置来提供含义
```
configurations {"Debug","Release"}
platforms {"Win32","Win64","Xbox360"}

filter {"platforms:Win32"}
	system "Windows"
	architecture "x86"

filter {"platforms:Win64"}
	system "Windows"
	architecture "x86_64"

filter {"platforms:Xbox360"}
	system "Xbox360"
```

与构建配置不同，平台是可以完全可选的，如果不需要它们，只需根本不调用平台函数，就会使用工具集的默认行为。

平台只是构建配置的另一种形式，您可以使用所有相同的设置，并且应用相同的范围规则。您可以在没有平台的情况下使用 __system__ 和 __architecture()__ 设置，也可以在平台配置中使用其他非平台设置。如果您曾经做过 “调试静态”、“调试DLL” 、“发布静态” 和“发布DLL”等构建配置，平台可以真正简化事情。
```
configurations {"Debug","Release"}
platforms {"Static","DLL"}

filter {"platforms:Static"}
	kind "StaticLib"

filter {"platforms:DLL"}
	kind "SharedLib"
	defines {"DLL_EXPORTS"}
```

## 每个项目的配置
现在可以为每个项目指定配置和平台列表，例如，应针对Windows而不是游戏机生成的项目可以删除改平台：
```
workspace "MyWorkSpace"
	configurations {"Debug","Release"}
	platforms {"Windows","PS3"}

project "MyProject"
	removeplatforms {"PS3"}
```

配置图是一项相关功能，可将工作区级配置转化为项目级值，从而允许将具有不同配置和平台列表的项目组合到单个工作区中。例如，可以使用通用调试和发布配置单元测试库。
```
project "UnitTest"
	configurations {"Debug","Release"}
```

若要在包含一组更复杂的配置的工作区中重用该测试项目，请创建从工作区的配置到相应项目配置的映射
```
workspace "MyWorkspace"
	configurations {"Debug","Development","Profile","Release"}
 project "MyProject"
	 configmap{
		["Development"] = "Debug"
		["Profile"] = "Release"	
	}
```

请务必注意，项目无法向工作区添加新配置，他们只能删除对现有工作区配置的支持，或将其映射到其他项目配置。

# 过滤器(filter)

Premake的过滤系统允许您将构建设置定位到您希望它们出现的确切配置。可以按特定生成配置平台、操作系统、目标操作等进行筛选。

下面是一个示例，该符号在工作区的“Debug"生成配置中设置名为"DEBUG"的预处理器符号，并在发布配置中设置名为”NDEBUG"的预处理器符号。
```
workspace "MyWorkspace"  
	configurations { "Debug", "Release" }  
  
	filter "configurations:Debug"  
		defines { "DEBUG" }  
  
	filter "configurations:Release"  
		defines { "NDEBUG" }
```
筛选器始终由两部分组成：指定筛选器所依据的字段的前缀，以及指定应接受该字段的那些值得模式。

过滤器遵循Premake得脚本伪声明样式：调用 filter() 使该过滤器条件 “活动”。随后出现在脚本中得所有设置都将按此条件进行过滤，知道激活新得过滤器或容器(工作区、项目)。

筛选器在生成时（创建工作区或项目文件并将其写入磁盘时）进行评估。当需要输入此工作区得调试生成配置得设置时，Premake会评估筛选器列表以查找与调试条件匹配得筛选器。

使用上面得例子，Premake将首先 考虑过滤器"configurations:Debug"。它将检查当前正在输出得配置得名称，查看它是否匹配，因此包括下一次筛选调用之前得任何设置。

筛选器"configurations:Release" 将被跳过，因为模式“Release”与当前正在生成得配置得名称不匹配("Debug")。
最后一个筛选器“{}”未定义任何筛选条件，因此不会排除任何内容。在此筛选器之后应用得任何设置都将显示在工作区或项目得所在配置中。
过滤器也可以组合。用“or"或”not"进行修改，并使用模式匹配。

```
-- All of these settings will appear in the Debug configuration
filter "configurations:Debug"
  defines { "DEBUG" }
  flags { "Symbols" }

-- All of these settings will appear in the Release configuration
filter "configurations:Release"
  defines { "NDEBUG" }
  optimize "On"

-- This is a sneaky bug (assuming you always want to link against these lib files).
-- Because the last filter set was Release. These libraries will only be linked for release.
-- To fix this place this after the "Deactivate" filter call below. Or before any filter calls.
links { "png", "zlib" }

-- "Deactivate" the current filter; these settings will apply
-- to the entire workspace or project (whichever is active)
filter {}
  files { "**.cpp" }
```

# 构建设置
Premake 提供了越来越多得构建设置列表，您可以调整这些设置；下表列出了一些最常见得配置任务，并附有指向相应函数得链接

| 函数 | 描述|
| --- | --- |
| kind | 指定二进制类型（可执行文件、库)|
| files,removefiles | 指定源代码文件 |
| defines | 定义编译器或预处理器符号|
| includedirs | 找到包含文件 |
| pchheader,pchsource | 设置预编译标头 |
| links,libdirs | 链接库、框架或其他项目 |
| symbols | 启用调试信息 |
| optimize | 针对尺寸或速度进行优化 |
| buildoptions,linkoptions | 添加任意构建标志 |
| targetname,targetdir | 设置已编译目标得名称或位置 |

# 命令行参数
## 操作和选项
预制件识别两种类型得参数:操作和选项。

操作指示预制在任何给定运行种应执行得操作。例如，该操作指示应生成Visual Studio 2013项目文件。该操作会导致删除所有生成得文件。一次只能指定一个操作。 `vs2013 clean`。

选项可修改操作得行为。例如，该选项用于更改生成得文件中使用的.Net编译器集。选项可以接受值(如 或 充当标志)，如。 `dotnet --dotnet=mono --with-opengl`

在脚本中，可以使用 `_ACTION` 全局变量表示当前操作。您可以使用包含键值对列表得 `_OPTIONS` 表检查选项。键是选项标识符 ("dotnet")，它引用命令行值(“mono")或无值选项得空字符串
```
if _ACTION =="clean" then
	--do something
end

targetdir ( _OPTIONS["outdir"] or "out")
```

## 创建新选项
新得命令行选项是使用 `newoption` 函数创建的，传递一个完全描述该选项的表。这最好用一些例子来说明。
这是一个旨在强制在 3D 应用程序中使用 OpenGL 的选项。它用作一个简单的标志，并且不采用任何值。
```
newoption{
	trigger = "with-opengl",
	description = "Force the use of OpenGL for rendering,regardless of platform"
}
```

注意每个键值对后面的逗号；这是lua表所需的语法。添加脚本后，该选项将显示在帮助文本中，您可以将触发器用作配置块中的关键字。
```
filter { "options:with-opengl" }
	links {"opengldrv"}
filter { "not options:with-opengl" }
	links {"direct3ddrv"}
```

下一个示例显示具有一组固定允许值的选项。与上面的示例一样，它旨在允许用户指定 3D API。
```
newoption{
	trigger = "gfxapi",
	value = "API",
	description = "Choose a particular 3D API for rendering",
	allowed = {
		{"opengl","OpenGL"},
		{"direct3d","Direct3D(Windows only)"},
		{"software","Software Renderer"}
	},
	default = "opengl"
}
```

与以前一样，此新选项将集成到帮助文本中，以及每个允许值的说明。预制件将在启动时检查选项值，并对无效值引发错误。值字段显示在帮助文本中，旨在为用户提供有关预期值类型的线索。在这种情况下，帮助文本将如下所示：
```
--gfxapi=API      Choose a particular 3D API for rendering; one of:
    opengl        OpenGL
    direct3d      Direct3D (Windows only)
    software      Software Renderer
```

与上面的示例不同，您现在将该值用作配置块中的关键字。
```
filter {"options:gfxapi=opengl"}
	links{"opengldrv"}
filter {"options:gfxapi=direct3d"}
	links{"direct3ddrv"}
filter { "options:gfxapi=software" }  
	links { "softwaredrv" }
```

作为选项的最后一个示例，您可能希望指定接受不受约束的值的选项。只需省略允许的值列表即可。
```
newoption{
	trigger = "outdir",
	value = "path",
	description = "Output directory for the compiled executable"
}
```

## 创建新做操作
操作的定义方式与选项大致相同，可以像这样简单:
```
newaction{
	trigger = "install",
	description = " Install the software",
	excute = funaction()
		--copy files ,etc.here
	end
}
```

触发操作时要执行的实际代码应放在函数中。 `execute()`
这是简单的版本，非常适合不需要访问特定项目信息的一次性操作。

# 使用模块
预制可以通过使用第三方模块进行扩展，模块可以添加对新工具集、语言和框架以及全新功能的支持。

要使用模块，请将模块的储存库下载或克隆到Premake的搜索路径之一，确保目标文件夹与模块的主脚本同名。
` git clone https://github.com/dcourtois/premake-qt qt`
然后，只需要从项目或系统脚本调用即可包含它。 `require()`
```
require "qt"
```
## 在项目中包括模块

为方便起见，您可能希望在项目的源代码树中保留所需模块的副本。在这种情况下，您可以将它们放置在您希望的任何位置，并在需要时提供相对路径。例如，如果你的主**premake5.lua**位于项目树的根目录下，并且你的模块位于一个名为**build**的文件夹中，你可以像这样加载它：

```
require "build/qt"
```
## 系统模块

您也可以将模块放在 Premake 搜索路径上的任何位置，例如**在 ~/.premake** 中。在这种情况下，不需要路径信息，您只需调用：

```
require "qt"
```

如果您希望使模块始终可用于_所有_项目，则可以在系统脚本中调用 to。在这种情况下，每次运行预制件时都会自动加载模块，并且其所有功能都将可用。`require()`

## 版本要求

为了确保与项目脚本的兼容性，有时要求模块依赖项的最低版本或版本范围会很有帮助。Premake包含一个修改版本Lua的`require()` 函数，该函数接受版本测试作为其第二个参数。

```
require("qt", ">=1.1")
```
