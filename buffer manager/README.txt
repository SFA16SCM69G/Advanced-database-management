README.txt:
---------------------------------------------------------
****cs525_group12******

A20377269	sgao20@hawk.iit.edu    Shen Gao	
A20359022	tmei1@hawk.iit.edu     Tian Mei	
A20373003	yliu308@hawk.iit.edu   Yi Liu	
A20314971	yzeng12@hawk.iit.edu   Yixin Zeng
----------------------------------------------------------

(1) Files and address including in assign1: 
Address In the fourier sever: /home/class/fall-17/cs525/tmei1/assign2
----------------------------------------------------------
	README.txt
                makefile
		buffer_mgr.h
		buffer_mgr_stat.c
		buffer_mgr_stat.h
		dberror.c
		dberror.h
		dt.h
		storage_mgr.h
		test_assign2_1.c
		test_assign2_2.c
		test_helper.h
----------------------------------------------------------

(2) How to use the Makefile:
----------------------------------------------------------
1.log in the fourier sever for IIT, cd to the correct address /home/class/fall-17/cs525/tmei1/assign2, or in your linux computer, open the terminal and cd to the address of the "assign1" where you put.

2.Type in "make clean" and click "enter". (make sure no other files except as the above files in the assign1)
If shows as:
"rm -f test test2 *.o *~ *.log"
Continue step 3.   Else: Check if address is correct.

3.Type in "make" or "make test", Click "enter".(run test case in test_assign2_1)
If shows as :
"gcc  -c test_assign2_1.c
gcc  -c storage_mgr.c
gcc  -c dberror.c
gcc -c  buffer_mgr.c
gcc -c buffer_mgr_stat.c
gcc -o test test_assign2_1.o storage_mgr.o dberror.o buffer_mgr.o buffer_mgr_stat.o"
Then continue step 4. Else: repeat step 2.

4.Type in "make test_run" , Click "enter".(Output the testing result)

5. If you Wanna try again, start from step 2.
----------------------------------------------------------

(3) Data Structure Introductions:
----------------------------------------------------------
We use the double linked list to store the page queue of buffer pool, each page is a node in the list.
Structure pageQueue:
Variable “size” stores the size of queue.
Variable “node” stores the current page in queue.
Structure Page:
Variable  ”ph” stores the page handler, which stores data and page number.
Variable “frameNum” stores frame number.
Variable “dirty” stores when a page is modified, dirty become true.
Variable “fixCount” means when no clients is using the page, fixCount is zero.
Variable “pre” stores the pointer point to the previous page.
Variable “post” stores the pointer point to the next page.

(4) Functions Introductions:
----------------------------------------------------------
*** PAGE REPLACEMENT ALGORITHM FUNCTIONS ***
-----------------
1.void initQueue(BM_BufferPool * const bm):
This function initialize the queue of page

2.RC searchPage(BM_BufferPool * const bm,BM_PageHandle * const page,PageNumber pageNum):
This function search the requested page from the buffer pool, if the page is found, return RC_PAGE_FOUND, if the page is not found, return RC_PAGE_NOT_FOUND

3.RC addPage(BM_BufferPool * const bm, BM_PageHandle * const page,const PageNumber pageNum):
This function read the requested page from disk and add it to the end of queue. It read the requested page first, then change the pointer.

4.RC FIFO(BM_BufferPool * const bm, BM_PageHandle * const page,const PageNumber pageNum)：
This function is the FIFO replacement strategy. Search the page first, if the page in the queue, set the fixCount and return. If the page is not in the queue, check if the buffer pool has spare space. If have, call addPage function to add the page into the tail. If not, find a removable page from head to tail in the buffer pool and replace it.

5.RC LRU(BM_BufferPool * const bm, BM_PageHandle * const page,const PageNumber pageNum)：
This function is the LRU replacement strategy. Call the FIFO first, if the return value is RC_FOUND, change the pointer and set the requested page to the tail.

*** BUFFER POOL FUNCTIONS ***
-----------
The buffer pool related functions are used to create a buffer pool for an existing page file on disk. The buffer pool is created in memory while the page file is present on disk. We make the use of Storage Manager (Assignment 1) to perform operations on page file on disk.

6. RC initBufferPool(BM_BufferPool *const bm, const char *const pageFileName, const int numPages, ReplacementStrategy strategy, void *stratData):
This function creates a new buffer pool in memory. numPages anumber of page frames defines the size of the buffer and number of page frames that can be stored in the buffer. pageFileName stores the name of the page file whose pages are being cached in memory. strategy represents buffer pool page replace strategy, stratData is parameters for the page replacement strategy. 

7. RC shutdownBufferPool(BM_BufferPool *const bm):
Destory pool and free up all resources associated with buffer pool,Check if all dirty pages have been written to disk before destroy. 

8. forceFlushPool(BM_BufferPool *const bm)
write all dirty pages into disk.
checks if pageframe's dirtyBit = 1(means have been modified)and fixCount = 0 (means no user use), then write it into disk.


*** PAGE MANAGEMENT FUNCTIONS ***
------------
9. RC pinPage() function. This function used to pin page with the required pageNum in the Buffer Pool. If the page is not in the Buffer pool, load it from the file to the Buffer Pool.

10. RC unPin() function. This function used to unpin a page when client is done reading or writing the page, the buffer manager will search the page list to find the required page and the unpin it and decrement the fix count by one.

11. RC markDirty() function. This function used to mark a page as dirty if it has been modified by client, find the page in the page list and compare the data in the buffer pool and disk, if not match, then set dirtyFlag true.

12. RC forcePage() function. This function used to write the current content of the page back to the page file on disk. Buffer manager found the page in the page list and then call writeBlock() function to write back to disk, and check if its success.

*** STATISTICS FUNCTIONS ***
------------
13. These functions return statistics about a buffer pool and its contents. these content include get Frame content, get Dirty Flage, get fix counts and get number read or write.

14. The getFrameContents function returns an array of PageNumbers (of size numPages) where the ith element is the number of the page stored in the ith page frame. An empty page frame is represented using the constant NO_PAGE.

15. The getDirtyFlags function returns an array of bools (of size numPages) where the ith element is TRUE if the page stored in the ith page frame is dirty. Empty page frames are considered as clean.

16. The getFixCounts function returns an array of ints (of size numPages) where the ith element is the fix count of the page stored in the ith page frame. Return 0 for empty page frames.

17. The getNumReadIO function returns the number of pages that have been read from disk since a buffer pool has been initialized. You code is responsible to initializing this statistic at pool creating time and update whenever a page is read from the page file into a page frame.

18. The getNumWriteIO returns the number of pages written to the page file since the buffer pool has been initialized.





----------------------------------------------------------

(5) additional error type we found and added in the error.h
----------------------------------------------------------
1.#define RC_NOT_REMOVED 18
For the function destroyPageFile(char *fileName), if the file name is null, function will return RC_NO_FILENAME, but if remove page failed, there should be another error type returned, we defined it as √¢‚Ç¨≈ìRC_NOT_REMOVED√¢‚Ç¨? Error number is 18.

2.#define RC_WRITE_NON_EXISTING_PAGE 19
For the function writeBlock(), we need to check if the page number is correct, when it more than the total page or less than 0, the function should return an error to clarify that the page number is illegal. We defined it as RC_WRITE_NON_EXISTING_PAGE, error number is 19. 

3.#define RC_PAGE_NOT_FOUND 206

4.#define RC_LIST_EMPTY 207

5.#define RC_PAGE_FOUND 208

6.#define RC_PAGE_NOT_EXIST 209

7.#define RC_NO_REPLACEMENT_STRATEGY 210


(6) extra test case by our group. 
---------------------------------------------------------


