## duplicateFF -  Duplicate File Finder (under renovation, come back after 10PM 11 July 2026) 
#### A Bash that will search supplied directory(s) and compare files using sha256 checksum and produces CSV reports on duplicate and unique files that can be used with spreadsheets or scripts.      
___________________________________________________________________
#### _Changes in progress_
* Excecutables created using the shc utilitiy being replaced by the original script. 
* Adding a summary reporting option (-r)
* Improved efficiency
* Fixing search level (-l) option bug 
___________________________________________________________________
### Inputs

* It requires a least one directory to search many directories can be compared.  
* Searches are selectable, the **-l** option is manditary:
  * Search all directories and subdirectories **(-l 0)**.
  * Search only the top level of the directories **(-l 1)**.
  * Search top level and subdirectories to a depth level that is required **(-l _n_)**
* Files names and maximum file sizes can be used as filters to narrow searches and save time.
* Linux hidden files are ignored, hidden directories are processed.  

Directories with name '$RECYCLE.BIN' are ignored. Linux sees some MS Windows directories as executable only, a user or app can go into them but can't read them. If the Windows "executable only" directory is user accessible, it be easily corrected by respondiong to "You don't currently have permission to access this folder".  If the directory is not user accessible then its probably a system directory that is not worth checking for dulicate files. 

### Output Reports

The following reports are generated in CSV format for easy processing with spreadsheets and scripts. See **OUTPUTS** section for more information.
* All duplicate files found, two reports.
* All files processed.
* All unique files found.
* A basic log.
In the reports every file checked is accompanied by a SHA256 checksum.

If no duplicates are found or the reporting option **(-r)** is used, only the all files processed report (**all_files_\<date-time\>.csv**) and the basic log (**log_\<date-time\>.txt**) are produced.  

### Notes

To move or remove duplicate files there are some suggestions in [How to delete duplicate files](https://github.com/Jim-JMCD/duplicateFF/blob/main/How%20to%20delete%20duplicate%20files.md) using the **_duplicateFF_KeepCopy_** and **_duplicateFF_RemoveALL_** scripts in this repository.

__Foreign Language Characters:__ If Microsoft Excel is the default application for CVS files, Microsoft Excel will not display foreign language characters correctly.  To fix this problem, change the default app for CSV files to Notepad or Wordpad and manually __import__ into excel, alternatively rename *.csv file to a *.txt and manually import into Microsoft Excel. Do not attempt to use __Open with__ and select Excel, it always has to be an __Import__.   
  
__________________________________________________________________________________________

### Usage 
#### duplicateFF -r -l \<search level\> [-f \<filter\>] [-k|K|m|M|g|G \<size\>] -s \<source directory\> ... -s \<source directory\>  

1. Check all mp4 files that are smaller 300MB in Fred's downloads and ./video, all subdirectories will be processed 
~~~
duplicateFF -f '.mp4' -m 300 -s './video/' -s '/home/fred/downloads' -l 0   
~~~

2. Check all image (jpg) files Fred's home directory, ignoring directory paths longer that three directories i.e. only search three levels deep.
~~~
duplicateFF -l 3 -f '.jpg' -s .      
~~~
Note: **-s .** could be replaced with **-s $PWD** or the full path. The search filter will process both the .jpg and .JPG files

Inputs of 'filter' 'source directory' 'output directory' should have single or double quotes otherwise any names with spaces will not be processed.

### Options 
_Manditory Options_

**-l Search level** 
* -l 0 Process the current directory and all subdirectories.
* -l 1 Process only the current directory
* -l 2 Process the current directory and one level below.
* -l n Process n levels deep in directory structure, includes current directory.

**-s Directories to process** 

One or many directories can be entered each must start with -s.

_Optional Options_

**-r Reporting**

An optional paramater that records the total files processed and the total number of duplicatres to the log (log_<date-time>.txt).  No other reports are produced.

**-f File name filter** 

Optional case insensitive filter, filter the source by part or the whole name of a file. This option can only be used once.

**Maximum file size filter**

Maximum file sixe filter is optional, default is 20 GiB.  Ignoring large files can save time.

**-k or -K** kilobytes (KiB)

**-m or -M** megabytes (MiB)

**-g or -G** gigabytes (GiB)
    
_Additional Notes_
* Default output directory, will be is created in the directory from which the script is run.
* Temporary files are created in the directory from which the script is run - all removed or moved on scipt termination.
* All hidden (dot) files are ignored, hidden directories are processed.
* The 'filter' and 'source directory' require single or double quotes for spaces in the filter and directory input.
* Filtering example : **_-f 'rar'_** that will pick both the word 'LIBRARY' and suffix '.rar'.  They will be ignored
*  Only the last instances of -f filter is used, the rest are ignored.
*  Only the last instances of file size filter is used, the rest are ignored.  
* Files with identical names and have differeing checksums are considered different files. File contents determines if they are copies.
* Files with different contents and the same SHA256 have known to occur.
* Empty files have the SHA256 checksums. Moving and deleting these could cause application problems, they could be 'flag' files. 
* The app is designed to be thorough, not designed for speed.
* Windows file systems occasionally produce some odd stuff that cannot be processed when mounted on Linux.

### OUTPUTS 
Output directory created in current directory with name **duplicate_chk_\<date-time\>**  where date-time = yymmdd-HHMMSS. 

The output directory contains four reports and one log file. If there are no unique files found, the Unique files report will not be produced:
  
|  Report type           |        Report file name            | CSV Format                                   |
|------------------------|------------------------------------|----------------------------------------------|
|  Duplicate files #1    | duplicate_FILES1_\<date-time\>.csv |  One file per row - see table                |
|  Duplicate files #2    | duplicate_FILES2_\<date-time\>.csv |  One SHA256 checksum per row - see table     |
|  All files checked     | all_files_\<date-time\>.csv        |  check_sum,"\<full path\>/\<file name\>"     |
|  Unique files          | unique_files_\<date-time\>.csv     |  check_sum,"\<full path\>/\<file name\> "    |
|  Basic log             | log_\<date-time\>.txt              |                                              |


**Duplicate files #1** One file per row

|  CSV field (column) | Attribute              |
|   :---:  |------------------------|
|   1      | SHA256 checksum        |
|   2      | fully pathed file name |
|   3      | full path of containing directory |
|   4      | file size in KiB |

**Duplicates files #2** One SHA256 checksum per row  

|  CSV field (column)  | Attribute                       | 
|   :---:  |---------------------------------------------|
|   1      | SHA256 checksum                             |
|   2      | file size in KiB                            |
|   3      | file 1 - fully pathed file name             |
|   4      | file 1 - full path of containing directory  |
|   5      | file 2 - fully pathed file name             |
|   6      | file 2 - full path of containing directory  |
|   7      | file 3 - fully pathed file name             |
|   8      | file 3 - full path of containing directory  |
|   9      | file 4 - fully pathed file name             |
|   10     | file 4 - full path of containing directory  |
|   _etc_  |     _more added as required_                |                  

Each CSV line has a minimum of two files, If more files match the checksum they are added as columns in <file> <directory> pairs i.e. repeats of columns 3 and 4.  



   



