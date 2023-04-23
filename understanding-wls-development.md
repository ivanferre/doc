# Understanding Development with WLS

We want to use a Windows computer to develop software mainly under a Linux environment, to benefit from the huge amount of tools available (e.g. `grep`, `sed`, `awk`, etc.). And we want to do this while also using the capabilities of advanced Integrated Development Environments (IDE) like VS Code, Eclipse, NetBeans, etc.

The way to do it is to use the Windows Linux Subsystem (WSL), where we may create one or more instances of different Linux distributions.

An additional requirement is to be able to synchronize the development projects with some cloud services, to allow the possibility to work from different computers.

In this test we will use Windows11, Ubuntu and VS Code as development platform.

# ZZZ Contents

## Installing Linux distros in WSL

The installation of [Windows Subsystem for Linux](https://learn.microsoft.com/windows/wsl/install) along with the preferred Linux distributions is straightforward from the Microsoft Store. However, the version 1 of the WSL has some limitations, and it is advised to upgrade to WSL2. This is to be done from the Linux shell as explained in the same document.

## Accessing Files

It is possible to both to execute commands from the _other environment_ on _local_ files, and to operate with local binaries on files from the alternative operating system.

### Windows from Linux

To access Windows files from Linux, have in mind that Linux _mounts_ Windows' `C:` drive as with any other external device. Thus, `/mnt/c/userjane/janesproject` points to the Windows `C:\Users\userjane\janesproject` folder.

We may execute Windows binaries from the Linux distribution as well. The easiest example is executing the File Explorer from bash:

```
explorer.exe .
```

Note the dot at the end of the command to make Explorer to focus on the current directory.

### Linux from Windows

We access Linux binaries from Windows by preceding the command with `wsl`. As an example, we may `grep` text files looking for string ocurrences:

```
wsl grep -i html index.html README.md
```

To access the files in the Linux distribution from Windows, we must remember that Windows represents Linux distros as network units, so we would be accessing other devices in the local network, like this:

```
type \\wsl$\Ubuntu\home\userjohn\README.md
```

This would be roughly equivalent to the execution of `cat README.md` in the `/home/userjohn` Linux folder.

Note that you may use the autocompletion feature by pressing the `tab` key. However, keep in mind that the command line interpreter will be case sensitive regarding Linux files, even if it is not when operating in Windows.

### Mixing Commands

It is possible to mix Linux and Windows commands in the the command line interface. For instance, when using Powershell, pipes and redirections _a la_ Unix are available:

```
dir | wsl grep string
wsl ls -la | findstr "string"
```

Commands are passed to `wsl` without any modification. This has the following consequences:

- The commands use the same current path as `cmd` or the Powershell who invokes the commands.
- They are executed by the WSL default user.
- They have the same administrative rights as the invoking process and terminal.
- File paths must be specified in the WSL format.

In the following example we list the files in `C:\Program Files` directory using the Linux flags:

```
wsl ls -la "/mnt/c/Program Files/"
```

The same way, the Windows applications we in the WSL environment

- Have the same working directory as the WSL command prompt.
- Have the same permission rights as the WSL process.
- Run as the active Windows user.

The following is a suitable use from bash:

```
ipconfig.exe | grep IPv4 | cut -d: -f2
```

Windows tools must include the `.exe` extension, be executable, and match the file case. CMD native commands may be run by using `/C` as in

```
cmd.exe /C dir
```

## Workflow

Microsoft advices to operate on the filesystem where the files are created. That is, if the files belong to the Windows environment, it is better to use them in Windows, unless there is some reason to do otherwise.

There is a performance gap when using the files from _the other_ file system. Therefore, Microsoft discourages accessing files across the different file systems.

If instead of being led by the storage platform, we are led by the development environment, the statement works this way: it is better to place the files in the same filesystem where they are to be used.

### VS Code

If [Visual Studio Code]() is the IDE, just remember to check the **Add to PATH** option when installing the software, under **Select Additional Tasks**.

Once installed, it's better to directly install the Microsoft WSL Extension (check the ID `ms-vscode-remote.remote-wsl`), that allows working directly in the Linux environment with optimal performance.

From this point on, **if the intention is to use Linux as development environment, all operations on project files should be done in the Linux subsystem**, either by opening a shell (run `ubuntu.exe`, if this is your distro), or by using the terminal within VS Code.

A typical source of problems is to have in the settings paths specified in Windows format that VS Code cannot handle appropriately when launched from the WSL; or vice versa.

A particular example of this is `terminal.integrated.cwd`, the current working directory (cwd) for the shell process. If this is set to `C:\Users\<username>`, the terminal will crash when invoked from a WSL-launched VS Code. The [Troubleshoot Terminal launch failures](https://code.visualstudio.com/docs/supporting/troubleshoot-terminal-launch) summarizes some settings to have a look to in these scenarios.

### Prettier

Prettier extension finds the configuration and script files in different paths when it's installed in Windows or in WSL.

I have found that the best is to look for the three path settings in the extension and set them to blank. This way, the extension assumes the default values and, if it was a typical installation, it works.

### Live Server

ZZZ
===

### Python

## Conclussions

## References

- [Working across Windows and Linux file systems](https://learn.microsoft.com/en-us/windows/wsl/filesystems)

- [Developing in WSL](https://code.visualstudio.com/docs/remote/wsl)

- [Get started using Visual Studio Code with Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)

- [Install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

- [Troubleshoot Terminal launch failures](https://code.visualstudio.com/docs/supporting/troubleshoot-terminal-launch)

- [Frequently Asked Questions about Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/faq)

### To read

- [Working with Visual Studio Code on WSL2](https://ubuntu.com/tutorials/working-with-visual-studio-code-on-ubuntu-on-wsl2)

- [Adjust case sensitivity](https://learn.microsoft.com/en-us/windows/wsl/case-sensitivity)

- [What is the difference between Windows Subsystem for Linux and bash on Ubuntu on Windows?](https://superuser.com/questions/1261617/what-is-the-difference-between-windows-subsystem-for-linux-and-bash-on-ubuntu-on)

- [How to install multiple instances of Ubuntu in WSL2](https://cloudbytes.dev/snippets/how-to-install-multiple-instances-of-ubuntu-in-wsl2)

- [Install Multiple Linux Distros on WSL on Windows 11](https://compile.blog/windows/multiple-linux-distros-on-wsl/)

- [Supporting Remote Development and GitHub Codespaces](https://code.visualstudio.com/api/advanced-topics/remote-extensions#architecture-and-extension-types)

- [Manual installation steps for older versions of WSL](https://learn.microsoft.com/en-gb/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)

### Shorter secondary sources

- [Accessing WSL files in Windows, and vice versa](https://ling123labs.com/posts/WSL-files-in-Windows-and-vice-versa/)

- [How to Access Files on Windows Subsystem for Linux](https://virtualizationreview.com/articles/2022/05/26/accessing-files-wsl.aspx)

# Pending Topics

1. `ubuntu.exe` vs. `wsl -d Ubuntu`
1. Extensions installed on Windows / wls
    1. Prettier vs Beautify
    1. GitLens
    1. EsLint
    1. ...
1. Sync with Cloud `\\wsl.localhost\Ubuntu\home\ivan`
1. Debug
    1. CSS
    1. JavaScript
    1. Python
