## How to delete duplicate files.

### Before deleting or moving any file make sure you have a guaranteed backup.

Run **_duplicateFF_** first to generate the **_duplicate_FILES1_yymmdd-hhmm.csv_** and **_duplicate_FILES2_yymmdd-hhmm.csv_** files

### *THE QUICK METHOD* ###
DO NOT USE THIS METHOD if you rely upon directory and files names to provide identification and context to the files. Useful for image files and other files with generic names where the be file can be easily identified by contents. 

The scripts for this method have minimal input checking, wise to check everything before moving or deleting anything.  

This method moves one file of a duplicate set to a user defined directory then that is followed by deleting all the duplicate copies that were not moved. 

Step 1.  This will move files to designated directory. Run bash script 
~~~
duplicateFF_KeepCopy <input_list> <destination_directory>
~~~
* **_input_list_** = a duplicate_FILES2_yymmdd-hhmm.csv file.  NOTE: It must be a **FILES2** file.
* If the destination_directory does not exist it will be created in present working directory ($PWD)

Step 2. This will remove all files than have not been moved.  Run bash script
~~~
duplicateFF_RemoveALL <input_list>
~~~  
* **_input_list_** = a duplicate_FILES1_yymmdd-hhmm.csv file  NOTE: It must be a **FILES1** file.
* This script attempts to removes ALL duplicate files. If any file does not have a copy that was moved in **_Step 1_** all copies will be removed.

### *THE SLOW TARGETED METHOD* ###
To remove unwanted duplicates create a list of files to be removed. Run the command

~~~
awk -F "," '{ printf "rm -f %s\n",$2}' duplicate_FILES1_yymmdd-hhmm.csv > files_to_remove
~~~

From the the file **_files_to_remove_** delete any lines that has a reference to files that you want to keep.  When finished removing lines of files that you want to keep run the command 

~~~
bash files_to_remove
~~~

.....that will delete all unwanted files. 

OR

Move unwanted duplicates by create a list of files to be moved. Run the command
~~~
awk -F "," '{ printf "mv %s /some/destination/directory/ \n",$2}' duplicate_FILES1_yymmdd-hhmm.csv > files_to_move
~~~
From the the file **_files_to_move_** delete any lines that has a reference to files that you want to keep.  When finished removing lines of files that you want to keep run the command 
~~~
bash files_to_move
~~~  

.....that will move all unwanted files to /some/destination/directory    

