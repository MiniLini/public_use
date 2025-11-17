I have here free to use tools. 

**MENU:**

**ctm - C to mount**

Converts windows file paths to WSL related file-paths. Sometimes I need to take a Windows path, like 

D:\Projects\ExampleProject\file.txt

and use it directly in WSL or Linux scripts. That’s where the ctm command comes in handy. I simply provide the Windows path to ctm, and it converts it into a Linux-style path by lowercasing the drive letter, replacing all backslashes with forward slashes, and removing any surrounding quotes. So in this example, running 

ctm "D:\Projects\ExampleProject\file.txt" 

outputs:

/mnt/d/Projects/ExampleProject/file.txt

Now the path is ready to use seamlessly inside WSL, without manually changing slashes or worrying about syntax errors.

Conversely, when I’m working in WSL or Linux and need a path that Windows can access, I use the mtc command. For instance, if I have a Linux path like /mnt/d/Projects/ExampleProject/file.txt, I can pass it to mtc, which converts the forward slashes to backslashes and prepends a UNC prefix so Windows can recognize it. The result looks like 

\\wsl.localhost\kali-linux\mnt\d\Projects\ExampleProject\file.txt

making it easy to open in Windows Explorer or reference in Windows scripts. Using ctm and mtc together allows me to switch paths between Windows and Linux effortlessly, keeping workflows smooth and reducing the chance of errors.


**mtc - mount to C **

Converts linux based file-paths to windows based, essentially the opposite to ctm 

Additionally:

Suppose you have a Windows path 

D:\Projects\ExampleProject\file.txt. Normally, in WSL you’d need to manyually convert /mnt/d/Projects/ExampleProject/file.txt to a useable format elsewhere. With ctm, you can do it in a one-liner:

cat $(ctm "D:\Projects\ExampleProject\file.txt")

Here’s what happens: 

the command ctm "D:\Projects\ExampleProject\file.txt" converts the Windows path into /mnt/d/Projects/ExampleProject/file.txt. The $(...) syntax substitutes the converted path directly into the cat command, and cat then reads and outputs the contents of the file from WSL/Linux, all without manually rewriting the path.

This approach works with any command that expects a Linux path. For example, you can edit a Windows file with nano in WSL:

nano $(ctm "D:\Projects\ExampleProject\file.txt")

Or copy a Windows file using cp in WSL:

cp $(ctm "D:\Projects\ExampleProject\file.txt") /mnt/d/Backup/

Using ctm in a one-liner like this is super practical for scripts, pipelines, and ad-hoc terminal commands, because it bridges Windows and WSL paths seamlessly.
