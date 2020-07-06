CFSpace
------------------

	FAT32 partition contiguous file space manager.
	Command line utility with its own micro shell.
	Linux and Windows.
	
	This project makes use of the FatFs library
	that enables reading and writing files to the FAT32 disks
	(see the Links section for more info).


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

