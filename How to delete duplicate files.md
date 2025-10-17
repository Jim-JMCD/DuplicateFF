## How to delete duplicate files.     

### Before deleting or moving any file make sure you have a guaranteed backup.   

### *THE QUICK METHOD* ###
DO NOT USE THIS METHOD if you rely upon directory and files names to provide identification and provide context. Useful for image files and other files with generic names with file easily identified by contents. 

This method moves one file of a duplicate set to a user defined directory then that is followed by deleting all the duplicate copies that were not moved. 

Step 1.  This will move files to designated directory.  

Run bash script **_duplicateFF_KeepCopy \<input_list\> \<destination_directory\>_**
* input_list = duplicate_files2_*   NOTE: It must be a file2 file.
* If the destination_directory does not exist it will be created.

Step 2. This will remove all files than have not been moved.  

Run bash script **_duplicateFF_RemoveALL \<input_list\>_**  
* input_list = duplicate_files2_*   NOTE: It must be a file1 file.
* This script attempts to removes all files with a duplicate. If any file does not saved copy in step one all copies will be removed.

### *THE SLOW TARGETED METHOD* ###
Create a list of 
