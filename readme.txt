CFSpace
------------------

	FAT32 partition contiguous file space manager.
	Command line utility with its own micro shell.
	Linux and Windows.
	
	This project makes use of the FatFs library
	that enables reading and writing files to the FAT32 disks
	(see the Links section for more info).


WARNING!
---------------------

	USE AT YOUR OWN RISK!
	NO WARRANTY PROVIDED!
	BACKUP YOUR DATA FIRST!


Command line interface
-----------------------------

	--open_disk DISKNAME
		
		Open disk on the program startup.
		DISKNAME can be:
		* File name of the FAT32 partition image.
		* System name of the partition on the physical HDD/SDD.
		  Eg. on Linux this may be "/dev/sdd2".
		  
		Note that this is an optional switch and the disk can be always 
		opened with the "open" command from the program's micro shell.
		
	--read_only
		
		Open the disk in read only mode.
		This affect both, the "--open_disk" switch 
		and the "open" command.
		
	--current_dir PATH
		
		Directory on the opened disk to srart in.
		Valid only when opening disk on startup with the "--open_disk" switch.

	--error_exit
	
		Exit the program when command error occuts.
		Usefull when the program is running text script via the "scr" command.

	--buff_rw_size NUM

		Buffer size for read and write operations. Default is 1MiB.
		For debug purposes.
		
	--run_cmd CMD
		
		Run command on the program startup.
		Can be used for automation from the shell.
		See "scr" command on how to execute commands from text file.
		
	--no_disk_reinit_w
		
		Do not reinit the disk at the end of the write operations, like 
		injecting a file using the "inj" command.
		By default the disk is reinited.
	
	--write_sync
		
		For debug purposes.
		Performs a sync operation on each buffer write.
		

Micro shell interface
------------------------------

	This is just simple command line interpreter that parses user 
	input from the STDIN in the console/terminal.
	Once started, enter "help" or "list" to get more info.
	Enter "help all" to show list and info of all the commands.

Commands
--------------------

	>>> open <<<
	Opens FAT32 disk, partition or disk image.
	Use -r switch to open in the read-only mode.

	>>> close <<<
	Closes current, previously opened disk.

	>>> ls <<<
	Alias: dir
	Shows contents of current directory.
	Use '-l' switch to get long listing.
	Use '-f' switch to show fragmentation for each file.

	>>> cd <<<
	Changes current directory.
	Use 'cd ..' to switch to the parent directory.
	Use 'cd /' to switch to the root directory.

	>>> pwd <<<
	Shows current directory path.

	>>> diskinfo <<<
	Alias: dinf
	Shows basic disk information.
	Size, empty space, label, etc.

	>>> exit <<<
	Aliases: quit, q
	Exits the program and returns to the shell.

	>>> dele <<<
	Deletes single file or empty-directory.
	Will not delete files with read-only flag
	or non-empty directories.
	Use '-f' switch to force delete of read-only file.

	>>> mkfile <<<
	Alias: mkf
	Creates new file. Empty or with prealocated size.
	File is created contiguous, with contents undefined.
	Use '-s SIZE' switch to specify its size.
	Fails if there is not enough contiguous space available.

	>>> extr <<<
	Copies file from curently mounted disk to the system.
	Syntax: extr <source> <dest>.

	>>> inj <<<
	Copies file from the system to the currently opened disk.
	By default, new file is being created contiguous.
	Fails if there is not enouh contiguous space on the disk.
	Use '-o' switch to allow file to be created non-contiguous.
	Syntax: inj [-o] <source> [<dest>]
	If <dest> is omited, new file name is automatically taken from the path in <source>.
	If the file name in <dest> is without the path part, creates file in the current directory.

	>>> showfrag <<<
	Alias: sfr
	Shows the file fragmentation.
	Syntax: showfrag <file-name>
	This command is limited in that it can only tell if the file is fragmented or not.
	If retrieved value of is 1 then the file is not fragmented, contiguous.
	If the file is fragmented, the value shown is '2+'.
	Note: you can also use the 'ls -f' command.

	>>> scr <<<
	Alias: s
	Runs commands from the text script - file on the system.
	Syntax: scr [-c] <file-name>
	File name is expected to be an ANSI or UTF-8 text file.
	Use '-c' switch to continue executing on errors.
	This command can be used to automate tasks from the actual shell.
	For shell automation, consider command line switches: '--run_cmd'
	and '--error_exit'.
	If file name is without the extension, auto checks for the TXT file.

	>>> show_disks <<<
	Alias: sd
	Shows system disks or volumes.
	Use the 'open' command to open diisk from the list.
	For example, to open disk D, use 'open D:' command.
	On unix platforms use device names from the /dev directory.
	Example: 'open /dev/sdc1'.


Build Instructions
----------------------------

	* Download and unpack source code package.
	* Enter directory: "projects/01_cli"
	* Run the command: "make release"
	* On success, the binary file is create under this path:
	  "projects/01_cli/bin/release/cfspace"


Links
------------------

	Github Project Page
	https://github.com/ikk00/CFSpace
	
	FatFs Library - Generic FAT Filesystem Module
	http://elm-chan.org/fsw/ff/00index_e.html

	FatFs User Forum
	http://elm-chan.org/fsw/ff/bd/


Special Thanks
--------------------

	Authors of the FatFs project.

	Authors and contributors of the Linux and Ubuntu operating system.

