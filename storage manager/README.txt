README.txt:
---------------------------------------------------------
****cs525_group12******

A20377269	sgao20@hawk.iit.edu    Shen Gao	
A20359022	tmei1@hawk.iit.edu     Tian Mei	
A20373003	yliu308@hawk.iit.edu   Yi Liu	
A20314971	yzeng12@hawk.iit.edu   Yixin Zeng
----------------------------------------------------------

(1) Files and address including in assign1: 
Address In the fourier sever: /home/class/fall-17/cs525/tmei1/assign1
----------------------------------------------------------
	README.txt
        dberror.c
        dberror.h
        storage_mgr.c
        storage_mgr.h
        test_assign1_1.c
	  test_assign_extra.c
        test_helper.h
	Makefile
----------------------------------------------------------

(2) How to use the Makefile:
----------------------------------------------------------
1.log in the fourier sever for IIT, cd to the correct address /home/class/fall-17/cs525/tmei1/assign1, or in your linux computer, open the terminal and cd to the address of the "assign1" where you put.

2.Type in "make clean" and click "enter". (make sure no other files except as the above files in the assign1)
If shows as:
"rm -f test *.o *~ *.log"
Continue step 3.   Else: Check if address is correct.

3.Type in "make" or "make test", Click "enter".
If shows as :
"gcc  -c test_assign1_1.c
gcc  -c storage_mgr.c
gcc  -c dberror.c
gcc -o test test_assign1_1.o storage_mgr.o dberror.o"
Then continue step 4. Else: repeat step 2.

4.Type in "make test_run" , Click "enter".(Output the testing result)

5. If you Wanna try again, start from step 2.
----------------------------------------------------------

(3) Functions Introductions:
----------------------------------------------------------
*** manipulating page files ***
-----------------
1.initStorageManager (void)
/*initialize the storage manager*/

2.createPageFile (char *fileName)
/*create a file with an empty page*/
First,create an empty file for both reading and writing,and check whether the file point is null or not.Next,create an empty page and request a block of memory for the empty page. Then, write the empty page to the file. Finally,free the memory and close the file.

3.openPageFile (char *fileName, SM_FileHandle *fHandle)
/*open an existing page file*/
First,open a file to update both reading and writing and the file must exist. To get the total page of the file, must find the end of the file first. So use fseek to go to the end of the file and use ftell to get the pointer of end position of the file. Now, we know the end position and we can get the total number of pages of the file. Then,initialize the file handler.

4.closePageFile (SM_FileHandle *fHandle)
/*close an open page file*/

5.destroyPageFile (char *fileName)
/*destory a page file*/


***reading blocks from disc ***
-----------
6. readBlock (int pageNum, SM_FileHandle *fHandle, SM_PageHandle memPage);
 int getBlockPos (SM_FileHandle *fHandle)
/*Read the pageNumth block from a file and stores its content in the memory pointed to by the memPage page handle*/ 
If the handler is null, then return. The file pointer is null if the file handled by handler are not initialized and return. The if the page number is illegal if the value is more than total page number or less than 0 and return. We use fseek to go to the pageNumth page of the file and return if it fails to go to he correct position. Then int variable element to count the total number of elements successfully read and return if fail. Finally, set the currPage to PageNum.

7.readFirstBlock (SM_FileHandle *fHandle, SM_PageHandle memPage)
/*This function read the first page in the file*/
Call the readblock function and set the pageNum=0.

8.readPreviousBlock (SM_FileHandle *fHandle, SM_PageHandle memPage)
/*This function read the previous block in the file*/
If the current position is at first page, then read fails, else call the readblock() function and set pageNum to current position -1 which is the previous page.

9.readCurrentBlock (SM_FileHandle *fHandle, SM_PageHandle memPage)
/*This function read the current block in the file*/
 Call the readblock() and set pageNum to current position.

10.readNextBlock (SM_FileHandle *fHandle, SM_PageHandle memPage)
/*Read the next block in the file*/
If the current position is at the last page, the read fail, else call the readblock() and set pageNum to current position +1 which is the next page.

11.readLastBlock (SM_FileHandle *fHandle, SM_PageHandle memPage)
/*read the last block in the file*/
Call the readblock and set pageNum to last page which is the total number of page.

/* writing blocks to a page file */
------------
12.writeBlock (int pageNum, SM_FileHandle *fHandle, SM_PageHandle memPage)
/*write a page to disk using an absolute position*/
First, check the page number, which is not legal when it is more than the total page or less than 0. To make the method "appendEmptyBlock" easy to be accomplished, we let totalNumPages+1 when pageNum equals to totalNumPages. Next, calculate the offset and use fseek to go to the pageNumth page of the file. Then use fwrite to write a page. Finally, set the currPage to the PageNum.

13.writeCurrentBlock (SM_FileHandle *fHandle, SM_PageHandle memPage)
/*write to disk using the current position*/
Call the writeblock and set pageNum to current position.

14.appendEmptyBlock (SM_FileHandle *fHandle)
/*increase the number of pages in the file by one*/
First,create an empty page and request a block of memory for the empty page.Then,write the empty page at the last page.

15.ensureCapacity (int numberOfPages, SM_FileHandle *fHandle)
/*if the file has less than numberOfPages pages then increase the size to numberOfPages*/
First, check whether the file has more pages than numberOfPages or not. If not, calculate the number of pages to be added and do appendEmptyBlock for difference of numberOfPages and totalNumPages times.

----------------------------------------------------------

(4) additional error type we found and added in the error.h
----------------------------------------------------------
1.#define RC_NOT_REMOVED 18
For the function destroyPageFile(char *fileName), if the file name is null, function will return RC_NO_FILENAME, but if remove page failed, there should be another error type returned, we defined it as â€œRC_NOT_REMOVEDâ€? Error number is 18.

2.#define RC_WRITE_NON_EXISTING_PAGE 19
For the function writeBlock(), we need to check if the page number is correct, when it more than the total page or less than 0, the function should return an error to clarify that the page number is illegal. We defined it as RC_WRITE_NON_EXISTING_PAGE, error number is 19. 

3.test_assign_extra.c
We added the additional part. This part accomplished test case for the following methods:
writeBlock, readBlock, readPreviousBlock, readNextBlock, readCurrentBlock, readLastBlock.


