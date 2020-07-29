------------------
CFSpace
------------------

	FAT32 partition contiguous file space manager.
	Creating, checking or copying contiguous, not-fragmented files.
	Command line utility for Linux and Windows platforms.
	
	Features:
	
		* Checks if file can be created contiguous before 
		  begining to copy the data, by default. 
		  Usefull for limiting the wear of the flash memory on the USB 
		  connected devices.

		* Can be used to check if the files on the disk are already 
		  contiguous or not.
		  Providing contiguous, non-fragmented files can be necessary
		  for a hardware that has limited capabilities of reading the 
		  data from the FAT32 partitions.

		* Can be used to open system disks or disk image files.
		  (Administrator or root level privileges are usually required,
		  depending on the disk type, operating system type or version.)
		  
		* Can be configured to run only from the actual shell, with 
		  no need of entering the internal micro shell otherwise.
		  See the '--run_cmd' switch or the 'scr' internal command.


	This project makes use of the FatFs library that
	enables reading and writing files on the FAT32 disks
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

	--crc_ul TEXT
		
		Utility. 
		Generates checksum that goes into the "ul.cfg" file and the ties game entry
		with the game split-files. USB Advance and OpenPS2Loader (OPL) format.
		This is so that you can manually create and copy files to the disk,
		For example, for game title "New Playstation Game", final file
		name may endup being: ul.88BC6456.SLUS_123.45.00
		

	--ul_cfg_add TEXT
		
		Utility. Opens "ul.cfg" file in current system direntory, 
		adds entry froom text eneterd, and exits the program.
		The TEXT sould contain game name followed by game id.
		Game id can be with or without the punctuation. Either 
		SLUS12345 or SLUS_123.45 is expected.
		Eg. if the TEXT is set to "New Playstation Game SLUS_123.45",
		added will be game with name set to "New Playstation Game"
		and game-id set to "SLUS_123.45".
		Note 1: This is only a command line switch that makes the 
				program exit after it's done - does not starts the main program.
		Note 2: No game files are opened or copied when using this switch.

	--no_disk_reinit_w
		
		Do not reinit the disk at the end of all write operations, like 
		injecting a file using the "inj" command.
	
	--write_sync
		
		For debug purposes.
		Performs a sync operation on each buffer write.
		

Micro shell interface
------------------------------

	This is just simple command line interpreter that parses user 
	input from the STDIN in the console/terminal.
	Once started, enter "help" or "list" to get more info.
	Enter "help all" to show list and info for all commands.

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

	>>> ul_inject <<<
	Alias: uli
	Injects ISO adding entry to the 'ul.cfg' file.
	Asks to press Enter key once files allocated, before file copy.
	WARNING: does not checks if the same game already exists.
	For PS2 games on USB disks.
	This is USB Advance format that is also used by OPL.
	Use '-b' to do not update the 'ul.cfg' file.
	Use '-i' to do set game-id manually.

	>>> ul_list <<<
	Alias: ull
	Lists contents of 'ul.cfg' file in current directory on the disk.
	Use '-l' switch to show more info (game-id).

	>>> ul_dele <<<
	Alias: uld
	Delete entry from 'ul.cfg' file in current directory on the disk.
	Specify one or more entries as asterissk with number, 1-based.
	Eg. 'ul_dele *2' or 'ul_dele *1 *5 *10'
	Use '-t' switch for test mode - don't delete or modify any files.


Build Instructions
----------------------------

	Unix:

		* Download and unpack source code package.
		* Enter directory: "projects/01_cli"
		* Run the command: "make release"
		* On success, the binary file is created under this path:
		  "projects/01_cli/bin/release/cfspace"

	Windows:
		
		Use Microsoft Visual C++ (MSVC) version 2017 and Windows 
		SDK that comes with it. Possibly can be compiled with 
		other MSVC versions.


Changelog
-----------------
	
	v1.1.1
		* Initial release.
		* Builds for Linux x86_64 platforms.
	
	v1.1.2
		* Adds Windows support.
		
	v1.3.1
		* Added handling of USB Advance (also OpenPS2Loader) format games.
		* Added ul.cfg commands: ul_inject, ul_list and ul_dele.
		* Minor filesystem handling improvements.
		* Previous official release was version 1.1.2.
		* Readme file documentation updates.


Links
------------------

	Github Project Page
	https://github.com/ikk00/CFSpace
	
	FatFs Library - Generic FAT Filesystem Module
	http://elm-chan.org/fsw/ff/00index_e.html

	FatFs User Forum
	http://elm-chan.org/fsw/ff/bd/
	
	Forum Topic on PS2-HOME Website
	https://www.ps2-home.com/forum/viewtopic.php?p=43159
	


Special Thanks
--------------------

	Authors of the FatFs project.

	Authors and contributors of the Linux and Ubuntu operating system.


