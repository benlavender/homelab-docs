# Solid State Drives

Solid Sate Drives, which are cell-based non-volatile storage devices using NAND memory, perform differently to Hard Disk Drives (HDDs). **Cells** are used to store data, data is written across **Pages** which are the smallest unit of readable and writeable data (4K usually) and consist of a number of cells. These pages are then grouped into **Blocks**. 

When an SSD reads data, it reads data at the page level, and given pages are the smallest unit of storage in an SSD the whole page, regardless of how small the actual data is must be read in whole. The same applies to writes, except the whole page must be free for the data, it cannot overwrite the page. Therefore writes are written to the next free page sequentially and the previous page is marked as unused (INVALID). 

Erasing is a different matter, SSDs cannot erase data at the page level, it can only do this at the block level which means any valid data within the pages inside the block must be re-allocated and written elsewhere. When this is needed, the controller will need to move any valid data to new pages and write the data. Because of this, in order to simply erase data it has to write valid data to new pages just to erase the old ones, resultantly running more writes than what might be assumed. This is called Write Amplification. Furthermore NAND flash has limited writes due to Cell Trapping. 

So Write Amplification which by-design effectively reduces the write lifespan of the SSD. There is builtin features of modern SSDs to reduce this problem which can be cumulatively named Wear Levelling. These are used to adjust writes across pages more equally so they do not wear out.

## Garbage Collection:

One of these is a scheduled procedure named Garbage Collection. Garbage collection runs, usually at idle, to free up blocks containing unused data and erasing them so that new writes are performed more effectively as they do not have to wait for blocks to be erased.

## Trim:

Trim is an ATA command that informs the disk controller of unused blocks so that the controller can erase them so they can be re-used at a later time. SSDs benefit from Trim because it provides more accurate information on what data is actually deleted. 

When Trim is used on SSDs the operating system, which in Linux is `fstrim(8)`, informs the SSD controller of the file deletions so that the controller can mark the pages as unused (INVALID) for it's own garbage collection schedule. If this Trim was disabled, garbage collection would have no idea what pages contain valid data because the controller has no knowledge of the filesystem; it only knows about invalid and valid pages. Therefore when running, usually at idle, garbage collection may relocate both valid and deleted data to new blocks before erasing the previous block.

[This guide](trim.md) goes through configuring Trim in Linux.