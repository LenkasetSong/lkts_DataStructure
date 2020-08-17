# vscode + mingw + C/C++

<https://github.com/lenkasetGitHub/lkts_DataStructure.git/>

1. 安装MinGW-x64,设置环境变量Path+\mingw\bin  
2. cmake-x64,设置环境变量Path+\cmake\bin,vscode扩展cmake添加camke.exe路径
3. 配置launch.json  

   ``` json
   {
    // 使用 IntelliSense 了解相关属性。
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [

        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            // "program": "${fileDirname}/${fileBasenameNoExtension}.out",
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,

            "MIMode": "gdb", "miDebuggerPath": "D:\\Program Files\\mingw-w64\\mingw64\\bin\\gdb.exe",


            "linux": { // Linux 系统下的配置
             "MIMode": "gdb" },
            "osx": { // OS X系统下的配置
            "MIMode": "lldb" },
            "windows": { // Windows 系统下的配置
            "MIMode": "gdb", "miDebuggerPath": "D:\\Program Files\\mingw-w64\\mingw64\\bin\\gdb.exe" },
            "preLaunchTask": "build"

        },

        {
            "name": "(gdb) debug multi",
            "type": "cppdbg",
            "request": "launch",
            // "program": "${fileDirname}/${fileBasenameNoExtension}.out",
            "program": "${fileDirname}/build/TEST.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,

            "MIMode": "gdb", "miDebuggerPath": "D:\\Program Files\\mingw-w64\\mingw64\\bin\\gdb.exe",


            "linux": { // Linux 系统下的配置
             "MIMode": "gdb" },
            "osx": { // OS X系统下的配置
            "MIMode": "lldb" },
            "windows": { // Windows 系统下的配置
            "MIMode": "gdb", "miDebuggerPath": "D:\\Program Files\\mingw-w64\\mingw64\\bin\\gdb.exe" },
            // "preLaunchTask": "debug"
        }
      ]
   }
   ```

4. 配置task.json

   ``` json
   {
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "g++",

            // "args": ["-g","${file}","-o","${fileDirname}/${fileBasenameNoExtension}.out","-std=c++11"],
            // "args":[],
            "args": ["-g","${file}","-o","${fileDirname}/build/TEST.exe","-std=c++11"],
            "presentation": {
                "reveal": "always",
                "panel": "shared"
            },


            "problemMatcher":{
                "owner": "cpp",
                "fileLocation": ["relative", "${workspaceRoot}"],
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }

            }
        }
      ]
    }
     ```

5. 文件-首选项-设置

    ``` json
    {
      "cmake.debugConfig": {
          "miDebuggerPath": "D:\\Program Files\\mingw-w64\\mingw64\\bin\\gdb.exe",    // Windows 下指定gdb路径（已添加到PATH）
          "externalConsole": true,    // 使用外部控制台
          "stopAtEntry": false    // 在起点处停顿（噢！在这停顿！）
      },
      "git.ignoreLimitWarning": true
    }
    ```

6. 创建CMakeLists.txt  
  根目录下的

    ``` text
    # 使用CMake Tools插件（可选，如果这个项目去到一个没有这个插件的机器也同样可以生成项目）
    include(CMakeToolsHelpers OPTIONAL)

    # CMake 最低版本号要求
    cmake_minimum_required(VERSION 2.8)

    # 项目名称
    project(TEST)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
    # 查找当前目录下的所有源文件
    # 并将名称保存到 DIR_ROOT_SRCS变量 a
    aux_source_directory(. DIR_ROOT_SRCS)

    # 添加 MyLib子目录
    add_subdirectory(MyLib)

    # 指定生成目标 TEST
    add_executable(TEST ${DIR_ROOT_SRCS})

    # 添加链接库
    target_link_libraries(TEST PrinterLib)
    ```

7. 创建CMakeLists.txt  
  子文件夹MyLib下的

    ``` text
    include(CMakeToolsHelpers OPTIONAL)
    cmake_minimum_required(VERSION 2.8)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
    aux_source_directory(. DIR_LIB_SRCS)
    # 生成链接库
    add_library(PrinterLib ${DIR_LIB_SRCS})
    ```

``` text
${workspaceRoot} 当前打开的文件夹的绝对路径+文件夹的名字
${workspaceRootFolderName}   当前打开的文件夹的名字
${file} 当前打开正在编辑的文件名，包括绝对路径，文件名，文件后缀名
${relativeFile} 从当前打开的文件夹到当前打开的文件的路径
如 当前打开的是test文件夹，当前的打开的是main.c，并有test / first / second / main.c
那么此变量代表的是  first / second / main.c
${fileBasename}  当前打开的文件名+后缀名，不包括路径
${fileBasenameNoExtension} 当前打开的文件的文件名，不包括路径和后缀名
${fileDirname} 当前打开的文件所在的绝对路径，不包括文件名
${fileExtname} 当前打开的文件的后缀名
${cwd} the task runner's current working directory on startup
不知道怎么描述，这是原文解释，
跟 cmd 里面的 cwd 是一样的
${lineNumber}  当前打开的文件，光标所在的行数
```
