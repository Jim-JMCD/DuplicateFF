## How to delete duplicate files.   -- under construction  

### Before deleting or moving any file make sure you have a guaranteed backup.

Run duplicateFF first to generate the duplicate_files1_yymmdd-hhmm.csv and duplicate_files2_yymmdd-hhmm.csv files

### *THE QUICK METHOD* ###
DO NOT USE THIS METHOD if you rely upon directory and files names to provide identification and provide context. Useful for image files and other files with generic names with file easily identified by contents. 

This method moves one file of a duplicate set to a user defined directory then that is followed by deleting all the duplicate copies that were not moved. 

Step 1.  This will move files to designated directory.  

Run bash script **_duplicateFF_KeepCopy \<input_list\> \<destination_directory\>_**
* **_input_list_** = a duplicate_files2_yymmdd-hhmm.csv file.  NOTE: It must be a **file2** file.
* If the destination_directory does not exist it will be created.

Step 2. This will remove all files than have not been moved.  

Run bash script **_duplicateFF_RemoveALL \<input_list\>_**  
* **_input_list_** = a duplicate_files1_yymmdd-hhmm.csv file  NOTE: It must be a **file1** file.
* This script attempts to removes all files with a duplicate. If any file does not saved copy in step one all copies will be removed.

### *THE SLOW TARGETED METHOD* ###
To remove unwanted duplicates create a list of files to be removed. Run the command

**_awk -F "," '{ printf "rm -f %s\n",$2}' duplicate_files1_yymmdd-hhmm.csv > files_to_remove_**

From the the file **_files_to_remove_** delete any lines that has a reference to files that you want to keep.  When finished removing lines of files that you want to keep run the command 

**_bash files_to_remove_**  that will delete all unwanteed files. 

OR

Move unwanted duplicates by create a list of files to be moved. Run the command

**_awk -F "," '{ printf "mv %s /some/destination/directory \n",$2}' duplicate_files1_yymmdd-hhmm.csv > files_to_move_**

From the the file **_files_to_move_** delete any lines that has a reference to files that you want to keep.  When finished removing lines of files that you want to keep run the command 

**_bash files_to_move_**  that will move all unwanteed files to /some/destination/directory    

