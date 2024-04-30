create folder "C:\Users\<USER>\AppData\Local\Temp\Catlogger\"
	

wait for a new file with file extension "kissa.txt" to be created in that folder.

runs "fileopplay.exe" creates outputfile "C:\Users\<USER>\AppData\Local\Temp\Catlogger\kissa.txt" during start. 

set OpLock on that output file once the file was created.   so the app looks for when that file is created and sets the lock after. !! See readdirectorychanges
PathCchFindExtension(fileName, MAX_PATH, &extension2); // Find the file extension

The process "fileopplay.exe" attempt to remove the output file after it finishs doing other operations.
The process "fileopplay.exe" will be paused due to the OpLock

When OpLock is triggered, user move all files in "C:\Users\<USER>\AppData\Local\Temp\Catlogger\" to c win temp to empty the folder, to create junction
BOOL Move, function to move to a new location via a uuid to c windows temp.

create junction C:\Users\<USER>\AppData\Local\Temp\Catlogger\ to "\RPC Control", empty folder is necesary

User create symbolic link "GLOBAL\GLOBALROOT\RPC Control\<RANDOM>.out" to target file (e.g., C:\Windows\System32\secrets.txt)
User create symbolic link "GLOBAL\GLOBALROOT\RPC Control\kissa.txt" to target file (e.g., C:\Windows\System32\secrets.t

cb1 release OpLock
delete symbolic link
Target file (e.g., C:\Windows\System32\secrets.txt) would be deleted

-------------------------------------------------------------------------------

There are 2 main operations 
	-delete arbitrary file
	-escalte priviliege

-delete arbitrary file

command 
PoC.exe del targetFile

will perform oplock on a kissa.txt file, and set a file as a target to delete.
user will run fileopPlay.exe program and it will trigger vulnerability.

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-escalte priviliege

1. use the command
command

PoC.exe pe RollbackScript.rbs

2.
use app and trigger vulnerability

cmd.rbs will spawn command prompt

public_run_bat.rbs will execute C:\Users\Public\run.bat