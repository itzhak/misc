This manual explain how to install BBFS from scratch.
The BBFS program is application for backuping files and folders. 
There is two application for BBFS system: backup_server is server run on machine where files is backuped,
                                          content_server is application that run on machine from where files is copied.
So you need to prepare two computers(it can be only one computer if you want that data will be copied from and backuped to the same computer).								  

The BBFS system done on ruby language so ruby should be installed on two computers.

1. Installing ruby, devkit (required by 'algorithms' gem and installed only on windows) and rubygems (if not installed)
	a. check if ruby and rubygems installed on you computer "ruby -v", "gem -v"
	b. installing ruby and gemspec 
           On Windows: go to http://rubyinstaller.org/ and download and install ruby. 
           On Linux: sudo apt-get install ruby1.9.3
	c. installing devkit 
           On Windows: go to http://rubyinstaller.org/ and download devkit, follow instructions from https://github.com/oneclick/rubyinstaller/wiki/Development-Kit
           On Linux: no need todo it.
	
2. On machine intendend to be content_server.
   a. Install content server by 'gem install content_server'
   b. Create configuration file for content server which should be located
       in ~/.bbfs/etc/file_monitoring.yml
the content of the file is:
------- begin of file_monitoring.yml --------
paths:
   - path: ~/.bbfs/test_files  # <=== replace with your local dir.
     scan_period: 1
     stable_state: 5
------- end of file_monitoring.yml --------
  
  c. run the content_server by "content_server --remote_server='backup_address'"
	   
3. On machine itended to be backup_server.
  a. Install backup_server by 'gem install content_data'
  b. Create configuration file for backup server which should be located
       in ~/.bbfs/etc/backup_file_monitoring.yml
       File content:
------- begin of backup_file_monitoring.yml --------
paths:
   - path:  ~/.bbfs/backup_data  # <=== replace with your local dir.
     scan_period: 1
     stable_state: 5
------- end of backup_file_monitoring.yml --------

c. run the backup_server by "backup_server --remote_server='content_address' --backup_destination_folder=~/.bbfs/backup_data
"

4. Explanation about file_monitoring.yml and backup_file_monitoring.yml
       configuration files:

    	"path:" - say to program which folder to scan recursively in order to find
    	          new/changed files and folders.
    	"scan_period:" - how much time in seconds passed before two consecutive scans
    	                 for files and directories.
		"stable_state:" - how many scan_period passed until the file is considered
		                  stable.

Uninstalling:

1. unistall all gems by: 'gem list | cut -d" " -f1 | xargs gem uninstall -aIx' 
2. uninstall ruby (from ruby193/unins000.exe) and delete ruby193 folder.


