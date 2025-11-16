# Manim

教程见：[ManimGL 教程文档](https://docs.manim.org.cn/)

## 环境配置

### 使用 winget 安装 FFmpeg 并修复 PATH 失败问题

在 Windows 上，

```sh
winget install ffmpeg
```

开新的终端执行 `ffmpeg -version` 可能会 `'ffmpeg' 不是内部或外部命令，也不是可运行的程序`。

原因为：winget 安装的 FFmpeg 不会自动写入 PATH。

可以查看 winget 的安装位置：

```sh
where /r "%LOCALAPPDATA%" ffmpeg.exe
```

得到类似如下格式输出：

```sh
C:\Users\24789\AppData\Local\Microsoft\WinGet\Packages\
Gyan.FFmpeg_Microsoft.Winget.Source_8wekyb3d8bbwe\
ffmpeg-8.0-full_build\bin\ffmpeg.exe
```

然后设置一下 PATH：

```sh
setx PATH "C:\Users\24789\AppData\Local\Microsoft\WinGet\Packages\Gyan.FFmpeg_Microsoft.Winget.Source_8wekyb3d8bbwe\ffmpeg-8.0-full_build\bin"
```

开新的 CMD 验证即可：

```sh
where ffmpeg
ffmpeg -version
```