# File_System_Implementation

Furkan Demir

A simple file system implementation to study and investigate the usual file systems in operating systems.
Sfs, the simple file system, is implemented as libsimplefs.a, a library, and stored in a virtual disk. The virtual disk is a simple regular Linux file of certain size that depends on the program input.

Specs: 
Block size is 4 KB. The first block, block 0, has the superblock information about the file system. Blocks 1, 2, 3, 4 forms the bitmap for the entire file system. Since block size is 4 KB, 4096 x 4 = 16384 x 8 = 131072. This calculation means that with 4 blocks allocated for bitmap, and each bit represents a block, There can be 131072 blocks in total in the file system. Each bit represents the corresponding blocks status (0 = EMPTY, 1 = OCCUPIED). 

Blocks 5, 6, 7, 8 forms the root directory with directory entries. Each directory entry has information (Name, inode (index in the FCB table), occupied) about the directory and it is limited to 128 bytes. Maximum directory name is 110 bits. Since we have 4 blocks for the root directory, 4 x 4096 /128 = 128, meaning that there can be 128 directory files at most.

Blocks 9, 10, 11, 12 forms the FCB(File Control Block) table. Each file has a FCB entry in the FCB table that has the size of 128 bytes. FCB contains the information about the status of the file, the address of the index table(The table that keeps track of the blocks that the file occuppies in an order. Table occupies a block in the system) on the virtual disk. The system can have 128 FCB entries at most.

Index tables are used for the indexed allocation (one level index). FCB of a file don't contain any pointers to the data blocks or information about them for simplicity sake. The index numbers of the data blocks are used as pointers and stored in the index table of the file. Resulting in one data block that is allocated for index table for each file. Thus, a file can have at most 1024(index pointers or data block numbers are 4 bytes long) index pointers, with each of them pointing to a block with 4KB space, making the maximum size of a file 4MB.
