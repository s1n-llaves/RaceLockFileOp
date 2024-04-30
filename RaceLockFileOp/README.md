# 0-Day FileOpPaly.exe EoP

On Windows system, i have an app perform privileged operation, such as creating, executing and deleting files, within a folder owned by an unprivileged user. 
The below PoC will perform arbitrary file delete.
It was observed that when a Windows unprivileged runs the process "FileOpsPlay.exe" will do the following with SYSTEM privilege:

It defines a base directory C:\Users\katvandamme\AppData\Local\Temp\Catlogger\ and three subdirectories: deletepost, FirstCAT, and 2ndCAT.
The program ensures these directories are created if they don't exist using the function createDirectoryIfNeeded.
It writes the content "Meow" to files within these directories (kissa.txt in the base directory, charming.txt in deletepost, HereFirst.txt in FirstCAT) using the createAndWriteFile function.

The file charming.txt is created and then deleted after a 2-second delay in the deletepost directory.
Files kissa.txt, kissaroundtheworld.txt, and MovedOrPastedHere.txt are also deleted from their respective directories.

The file HereFirst.txt from the FirstCAT directory is copied and renamed to MovedOrPastedHere.txt in the 2ndCAT directory using the copyAndRenameFile function.
Original files are deleted after their content is moved or copied.
After the operations, the program deletes all the created files and directories, 
including deletepost, FirstCAT, 2ndCAT, and the base directory Catlogger.


To perform arbitrary file delete from a unprivileged user, the user could perform follow steps:

1. User create folder "C:\Users\\\<USER\>\AppData\Local\Temp\fstmpsc_\<USER\>\\"
2. User wait for a new file with file extension ".out" to be created in that folder. 
3. User recheck compliance status. The process "SecureConnector.exe" creates output file "C:\Users\\<USER\>\AppData\Local\Temp\fstmpsc_\<USER\>\\\<RANDOM\>.out" during compliance check.
4. User set OpLock on that output file once the file was created
5. The process "SecureConnector.exe" attempt to remove the output file after it finishs the compliance check
6. The process "SecureConnector.exe" will be paused due to the OpLock
7. When OpLock is triggered, user move all files in "C:\Users\\katvandamme\AppData\Local\Temp\fstmpsc_\<USER\>\" to somewhere else to empty the folder
8. User create junction "C:\Users\\<USER\>\AppData\Local\Temp\fstmpsc_\<USER\>\" to "\RPC Control"
9. User create symbolic link "GLOBAL\GLOBALROOT\RPC Control\\<RANDOM\>.out" to target file (e.g., C:\Windows\System32\secrets.txt)
10. User release OpLock
11. User delete symbolic link
12. Target file (e.g., C:\Windows\System32\secrets.txt) would be deleted



To perform local privilege escalation from arbitrary file delete, we could leverage Windows Installer as described in this article https://www.zerodayinitiative.com/blog/2022/3/16/abusing-arbitrary-file-deletes-to-escalate-privilege-and-other-great-tricks. Noted that the PoC for local PE is unstable because it require race condition. It is recommended to run on a system with a minimum of 4 processor cores.



Delete arbitrary file:

```
PoC.exe del targetFile
```

click "recheck compliance status" to trigger vulnerability



Escalate privilege:

```
PoC.exe pe RollbackScript.rbs
```

click "recheck compliance status" to trigger vulnerability

cmd.rbs will spawn command prompt

public_run_bat.rbs will execute C:\Users\Public\run.bat

