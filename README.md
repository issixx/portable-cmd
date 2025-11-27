# portable-cmd

## Overview
`potable-cmd.bat` is a batch file for Windows environments that automatically downloads and extracts **portable** versions of various development tools (Git, CMake, Python, CUDA, Vulkan, etc.) and makes them available by setting up the PATH.
It allows you to set up development environments with a double-click across different projects and machines without affecting the system, eliminating the hassle of manually installing various essential tools.

## How to Run

Execute
```bash
potable-cmd.bat
```
or Download & Execute
```bash
powershell -NoProfile -Command "$f='potable-cmd.bat'; (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/issixx/potable-cmd/main/potable-cmd.bat') -replace \"`r?`n\",\"`r`n\" | Set-Content $f -Encoding ASCII; cmd /k $f"
```

After execution, the paths for the installed portable versions will be configured.

## Portable Tools
- **Git**:
- **CMake**:
- **Python**: Embedded version with `pip`, `virtualenv`, and `requirements.txt` installation
- **Python venv**: Virtual environment creation and auto-activation with `venv`/`virtualenv`
- **Chrome**, **ChromeDriver**:
- **FFmpeg**:
- **Node.js**:
- **Go**:
- **Svn**:

## System Installation SDKs
- **CUDA Toolkit**: Downloads installer and installs after user confirmation (interactive)
- **Vulkan SDK**: Downloads installer and installs after user confirmation (interactive)  

## Directory Structure (Auto-created on execution)
```
<root>
│
├─ workspace          ← Default workspace directory
│   └─ lib            ← Portable binaries for each tool are stored here
│       ├─ git
│       ├─ cmake
│       ├─ python
│       ├─ chrome
│       ├─ ffmpeg
│       ├─ nodejs
│       ├─ go
│       └─ svn
└─ requirements.txt  ← (Optional) Python package list
```
- `workspace` is created under the execution directory (`%~dp0`).
- If the current directory path is already too long, it will automatically switch to `%USERPROFILE%\<base directory name>\workspace`.

## Installation Tool Configuration

- By default, only Git and Python are installed.
- To install other tools, either modify the settings directly or create a wrapper batch file like the following.

my-potable-cmd.bat
```bash
:: potable tools
set ENABLE_GIT=1
set ENABLE_CMAKE=1
set ENABLE_PYTHON=1
set ENABLE_CHROME=1
set ENABLE_FFMPEG=1
set ENABLE_NODEJS=1
set ENABLE_GO=1
set ENABLE_SVN=1

:: sdks
set ENABLE_CUDA=1
set ENABLE_VULKAN=1

:: launch potable-cmd
call "%~dp0potable-cmd.bat"
if ERRORLEVEL 1 goto :ERROR

:: Switch to interactive mode if the script is called directly
:: (Check if this batch filename is included in the startup command)
echo %CMDCMDLINE:"=% | find /I "%~f0"
if not ERRORLEVEL 1 (
    cmd /K
)

:SUCCESS
exit /b 0

:ERROR
	echo #############
	echo #  !error!  #
	echo #############
	pause
exit /b 1
```

- You can also change the binaries to be installed.

```bash
set POTABLE_GIT_URL=https://github.com/git-for-windows/git/releases/download/v2.47.0.windows.2/PortableGit-2.47.0.2-64-bit.7z.exe
set POTABLE_CMAKE_URL=https://github.com/Kitware/CMake/releases/download/v3.30.5/cmake-3.30.5-windows-x86_64.zip
set POTABLE_CMAKE_URL=https://github.com/Kitware/CMake/releases/download/v3.28.6/cmake-3.28.6-windows-x86_64.zip
set POTABLE_PYTHON_URL=https://www.python.org/ftp/python/3.13.0/python-3.13.0-embed-amd64.zip
set POTABLE_PYTHON_URL=https://www.python.org/ftp/python/3.12.9/python-3.12.9-embed-amd64.zip
set POTABLE_PYTHON_PIP_URL=https://bootstrap.pypa.io/get-pip.py
set POTABLE_PYTHON_REQUIREMENT_MODULES=blinker==1.7.0 selenium-wire==5.1.0 selenium==4.23.1 requests setuptools packaging
set POTABLE_CHROME_URL=https://storage.googleapis.com/chrome-for-testing-public/138.0.7204.168/win64/chrome-win64.zip
set POTABLE_CHROME_DRIVER_URL=https://storage.googleapis.com/chrome-for-testing-public/138.0.7204.168/win64/chromedriver-win64.zip
set POTABLE_FFMPEG_URL=https://github.com/BtbN/FFmpeg-Builds/releases/download/latest/ffmpeg-master-latest-win64-gpl.zip
set POTABLE_NODEJS_URL=https://nodejs.org/download/release/v22.19.0/node-v22.19.0-win-x64.zip
set POTABLE_GO_URL=https://go.dev/dl/go1.25.1.windows-amd64.zip
set POTABLE_SVN_URL=https://www.visualsvn.com/files/Apache-Subversion-1.14.5-3.zip

set CUDA_URL=https://developer.download.nvidia.com/compute/cuda/12.6.2/local_installers/cuda_12.6.2_560.94_windows.exe
set VULKAN_URL=https://sdk.lunarg.com/sdk/download/1.3.296.0/windows/VulkanSDK-1.3.296.0-Installer.exe
```

## Important Notes
- Tool paths are only valid for processes launched from that batch file.
- For example, if you want to launch VS Code with the auto-installed Python available, launch VS Code from within potable-cmd like this:

```bash
potable-cmd.bat
code .\
```

- To install Python modules:
  - Install using `python -m pip install <module>`.
- To create a venv environment with portable Python:
  - Run `python -m virtualenv <venv-name>`

- To use system tools:
  - Set `set USE_SYSTEM_EXE=1` to use already installed tools that are in the PATH.
- To use system Python with venv:
  - Additionally set `set ENABLE_PYTHON_VENV=1` to use system Python while installing dependency modules with venv.

## License

- `MIT`
- See [LICENSE](LICENSE).
