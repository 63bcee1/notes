nuitka doc

机翻，可能不准确！

## 选项

> --version             显示程序的版本号并退出
> -h, --help            显示此帮助消息并退出
> --module              创建扩展模块可执行文件而不是程序。默认为关闭。
> --standalone          为输出启用独立模式。这样，您无需使用现有的Python安装程序即可将创建的二进制文件转移到其他计算机上。这也意味着它将变得很大。它包含以下选项：“-全部递归”。您可能还想使用“ --python- flag = no_site”来避免使用“ site.py”模块，该模块可以节省很多代码依赖性。默认为关闭。
> --onefile             在独立模式下，启用单文件模式。这意味着不是文件夹，而是创建并使用了压缩的可执行文件。目前尚处于实验阶段，并非所有操作系统都支持。默认为关闭。
> --python-arch=PYTHON_ARCH
>                      使用的Python体系结构。 “ x86”或“ x86_64”之一。默认为运行Nuitka的内容（当前为“ x86_64”）。
> --python-debug        是否使用调试版本。默认使用您用于运行Nuitka的内容，很可能是非调试版本。
> --python-flag=PYTHON_FLAGS
>                       要使用的Python标志。默认使用您用于运行Nuitka的内容，这将强制执行特定模式。这些是标准Python可执行文件也存在的选项。当前受支持：“-S”（别名“ nosite”），“ static_hashes”（不使用哈希随机化），“ no_warnings”（不提供Python运行时警告），“-O”（别名“ noasserts”）。默认为空。
> --python-for-scons=PYTHON_SCONS
>                       如果使用Python3.3或Python3.4，请提供用于Scons的Python二进制文件的路径。否则，Nuitka可以使用运行Nuitka的程序或在PATH中找到的“ scons”二进制文件，或从Windows注册表中安装Python。
> --warn-implicit-exceptions
>                       对在编译时检测到的隐式异常启用警告。
> --warn-unusual-code   对在编译时检测到的异常代码启用警告。
> --assume-yes-for-downloads
>                       如有必要，允许Nuitka下载代码，例如Windows上的依赖项遍历器。

## 控制模块和包的包含

> --include-package=PACKAGE
>                    包括整个包装。以Python名称空间的形式给出，例如然后，some_package.sub_package''和Nuitka将找到它并将其以及该磁盘位置下方找到的所有模块包含在它创建的二进制或扩展模块中，并使其可用于代码导入。默认为空。
> --include-module=MODULE
>                     包括一个模块。以Python名称空间的形式给出，例如some_package.some_module``和Nuitka将找到它并将其包含在它创建的二进制或扩展模块中，并使其可用于代码导入。默认为空。
> --include-plugin-directory=MODULE/PACKAGE
>                     包括该目录的内容，无论给定的主程序是否以可见形式使用该目录。覆盖所有其他递归选项。可以多次给予。默认为空。
> --include-plugin-files=PATTERN
>                     将与PATTERN匹配的文件包括在内。覆盖所有递归其他选项。可以多次给予。默认为空。
> --prefer-source-code
>                     对于既有源文件又有扩展模块的已编译扩展模块，通常使用扩展模块，但最好从可用源代码编译该模块以获得最佳性能。如果不需要，可以使用--no- preferred-source-code禁用有关它的警告。默认关闭。

## 控制到导入模块的递归

> --follow-stdlib, --recurse-stdlib
>                     还可以从标准库导入到导入的模块中。这将大大增加编译时间。默认为关闭。
> --nofollow-imports, --recurse-none
>                     使用--recurse-none时，根本不要进入任何导入的模块，而是覆盖所有其他递归选项。默认为关闭。
> --follow-imports, --recurse-all
>                     使用--recurse-all时，请尝试下降到所有导入的模块中。默认为关闭。
> --follow-import-to=MODULE/PACKAGE, --recurse-to=MODULE/PACKAGE
>                     递归到该模块，或者递归到整个包。可以多次给予。默认为空。
> --nofollow-import-to=MODULE/PACKAGE, --recurse-not-to=MODULE/PACKAGE
>                    在任何情况下，都不要递归到该模块名，或者如果递归到整个包，则不要递归到所有其他选项。可以多次给予。默认为空。

## 编译后立即执行

> ​    --run               立即执行创建的二进制文件（或导入已编译的模块）。默认为关闭。
> ​    --debugger, --gdb   在“ gdb”内部执行以自动获取堆栈跟踪。默认为关闭。
> ​    --execute-with-pythonpath
> ​                        立即执行创建的二进制文件（--execute）时，请勿重置PYTHONPATH。成功包含所有模块后，您应该不再需要PYTHONPATH。

## 内部树的转储选项

> --xml               将优化的最终结果转储为XML，然后退出。

## 代码生成选择

> --full-compat       加强与CPython的绝对兼容性。甚至不允许与CPython行为有微小偏差，例如没有更好的回溯或异常消息，它们并不是真正不兼容的，只是有所不同。这仅用于测试，不应用于正常使用。
>     --file-reference-choice=FILE_REFERENCE_MODE
>                         选择“ __file__”的值。对于“运行时”（独立二进制模式和模块模式的默认设置），创建的二进制文件和模块将使用其自身的位置来扣除“ __file__”的值。包含的软件包伪装在该位置下的目录中。这使您可以在部署中包括数据文件。如果您仅寻求加速，则最好使用“原始”值，该值将使用源文件的位置。对于“冻结”，使用符号“ <冻结模块名称>”。出于兼容性原因，“ __ file__”值的后缀始终为“ .py”，而与实际情况无关。
>
> 输出选择：
>     -o FILENAME         指定可执行文件的命名方式。对于扩展模块，没有选择，对于独立模式也没有选择，使用它会出错。但是，这可能包括需要存在的路径信息。在此平台上默认为<program_name>。 。可执行程序
>     --output-dir=DIRECTORY
>                         指定中间和最终输出文件的放置位置。目录将用C文件，目标文件等填充。默认为当前目录。
>     --remove-output     生成模块或exe文件后删除构建目录。默认为关闭。
>     --no-pyi-file       不要为扩展模块创建“.pyi”文件由Nuitka创建。用于检测隐式进口。默认为关闭。

## 调试功能

> --debug             执行所有可能的自检以查找Nuitka，不要用于生产。默认为关闭。
> --unstripped        在生成的对象文件中保留调试信息，以便更好地进行调试器交互。默认为关闭。
> --profile           启用基于vmprof的时间分析。不是正在工作。默认为关闭。
> --graph             创建优化过程图。默认为关闭。
> --trace-execution   跟踪执行输出，在执行前输出代码行。默认为关闭。
> --recompile-c-only  这不是增量编译，而是针对Nuitka仅限于开发。获取现有文件并再次将它们编译为C。允许编译已编辑的C用于快速调试对生成的源代码，例如查看代码是否经过，值输出等默认为关闭。取决于编译Python源代码来确定它应该查看哪些文件在。
>
> --generate-c-only   只生成C源代码，不编译为二进制或模块。这是调试和代码不浪费CPU的覆盖率分析。默认为关。别以为你能直接用这个。
> --experimental=EXPERIMENTAL
>                     使用声明为“实验”的功能。可能没有如果没有实验特征出现在代码。根据实验使用秘密标签（检查来源）功能。
> --disable-dll-dependency-cache
>                     禁用依赖项遍历缓存。将导致创建分发文件夹的时间更长，但可能在怀疑缓存会导致错误的情况下使用。
> --force-dll-dependency-cache-update
>                    以更新依赖项walker缓存。将导致创建分发文件夹的时间更长，但可能在怀疑缓存会导致错误或已知需要更新时使用。

## 后端C编译器选择

> --clang             强制使用叮当声。在Windows上，这需要一个工作的visualstudio版本来支持。默认为关闭。
> --mingw64           在Windows上强制使用MinGW64。默认为关闭。
> --msvc=MSVC        强制在Windows上使用特定的MSVC版本。允许的值是例如14.0，为已安装的编译器列表指定非法值，注意只有最新的MSVC才真正受支持。默认为最新版本。
> -j N, --jobs=N      指定允许的并行C编译器作业数。默认为系统CPU计数。
> --lto               如果可用，请使用链接时间优化（MSVC或GCC4.6及更高版本）。默认为关闭。

## 跟踪功能

> --quiet            禁用所有信息输出，但显示警告。默认为关闭。
> --show-scons        在非安静模式下操作scon，显示执行的命令。默认为关闭。
> --show-progress     提供进度信息和统计数据。默认为关闭。
> --show-memory       提供内存信息和统计信息。默认为关闭。
> --show-modules      提供包含模块的信息，DLL默认为关闭。
> --show-modules-output=SHOW_INCLUSION_OUTPUT
>                    输出位置—显示模块，应为文件名。默认为标准输出。
> --verbose           输出所采取行动的详细信息，特别是在优化中。可以变成很多。默认为关闭。
> --verbose-output=VERBOSE_OUTPUT
>                     输出位置——详细，应为文件名。默认为标准输出。

## Windows特定控件

> --windows-dependency-tool=DEPENDENCY_TOOL
>                     为Windows编译时，请使用此依赖项工具。默认为依赖.exe，其他允许的值是“pefile”。
> --windows-disable-console
>                     为Windows编译时，请禁用控制台窗口。默认为关闭。
> --windows-icon-from-ico=ICON_PATH
>                     添加可执行图标（此时仅限Windows）。可以给多次。
> --windows-icon-from-exe=ICON_EXE_PATH
>                     从此现有可执行文件复制可执行图标（仅限Windows）。
> --windows-uac-admin
>                     请求Windows用户控件，以授予执行时的管理权限。（仅限Windows）。默认为关闭。
> --windows-uac-uiaccess
>                    请求Windows用户控制，强制仅从几个文件夹运行，远程桌面访问。（仅限Windows）。默认为关闭。
> --windows-company-name=WINDOWS_COMPANY_NAME
>                     要在Windows版本信息中使用的公司名称。当需要添加版本资源时，需要文件或产品版本之一，例如指定产品名称或公司名称。默认为未使用。
> --windows-product-name=WINDOWS_PRODUCT_NAME
>                     要在Windows版本信息中使用的产品的名称。默认为二进制文件的基本文件名。
> --windows-file-version=WINDOWS_FILE_VERSION
>                     要在Windows版本信息中使用的文件版本。必须是最多4个数字的序列，不允许有其他数字。当需要添加版本资源时，需要文件或产品版本之一，例如指定产品名称或公司名称。默认为未使用。
> --windows-product-version=WINDOWS_PRODUCT_VERSION
>                     要在Windows版本信息中使用的产品版本。必须是最多4个数字的序列，不允许有其他数字。当需要添加版本资源时，需要文件或产品版本之一，例如指定产品名称或公司名称。默认为未使用。
> --windows-file-description=WINDOWS_FILE_DESCRIPTION
>                     在Windows版本信息中使用的文件的说明。当需要添加版本资源时，需要文件或产品版本之一，例如指定产品名称或公司名称。默认为胡说八道。

## 插件控件

> --plugin-enable=PLUGINS_ENABLED, --enable-plugin=PLUGINS_ENABLED
>                    启用的插件。必须是插件名称。使用--plugin-list查询完整列表并退出。默认为空。
> --plugin-disable=PLUGINS_DISABLED, --disable-plugin=PLUGINS_DISABLED
>                    禁用的插件。必须是插件名称。使用--plugin-list查询完整列表并退出。默认为空。
>
> --plugin-no-detection
>                    插件可以检测它们是否可能被使用，您可以通过--plugin disable=plugin-禁用警告，或者您可以使用此选项完全禁用该机制，这当然也会略微加快编译速度，因为一旦确定要使用哪些插件，此检测代码将徒劳地运行。默认为关闭。
> --plugin-list      显示所有可用插件的列表并退出。默认为关闭。
> --user-plugin=USER_PLUGINS
>                    用户插件的文件名。可以给多次。默认为空。

## 常用命令

