# lab5文件系统
与文件相关操作在user/fd.c文件


# 用户态驱动
调用自己写的exercise1系统调用函数（其实就是将设备物理地址处的数据复制拷贝出来），可以模仿内核态调用read_sector。

# 磁盘文件结构
文件相关定义在fs/fs.h,include/fs.h  

bitmap结构tools/fsformat  


BY2SECT = 512  每个磁盘分区大小  
sector to a block = BY2BLK/BY2SECT  
BY2BLK = BY2PG， BIT2BLK = BY2BLK*8  
文件控制块File结构体  
BY2FILE = 256  
MAXFILESIZE = NINDIRECT*BY2BLK  
NINDIRECT = BY2BLK/4间接指针
NDIRECT = 10直接指针个数  
　　我们使用的操作系统内核中，文件名的最大长度为 MAXNAMELEN(128)，每个文
件控制块设有 10 个直接指针，用来记录文件的数据块在磁盘上的位置。每个磁盘块的
大小为 4KB，也就是说，这十个直接指针能够表示最大 40KB 的文件


## 函数方法
file_block_walk(struct File *f, uint32_t filebno, uint32_t **ppdiskbno, bool alloc)
这个函数是查找文件第filebno块的数据块的地址，查到的地址存储在 ppdiskbno 中。注意这里要检查间接块，如果alloc为1且寻址的块号>=NDIRECT，而间接块没有分配的话需要分配一个间接块。 
   
file_get_block(struct File *f, uint32_t filebno, char **blk)
查找文件第filebno块的块地址，并将块地址在虚拟内存中映射的地址存储在 blk 中(即将diskaddr(blockno)存到blk中)。
  
  
dir_lookup(struct File *dir, const char *name, struct File **file)
在目录dir中查找名为name的文件，如果找到了设置*file为找到的文件。因为目录的数据块存储的是struct File列表，可以据此来查找文件。  
  

file_open(const char *path, struct File **pf)
打开文件，设置*pf为查找到的文件指针。  
  
file_create(const char *path, struct File *pf)
创建路径/文件，在pf存储创建好的文件指针。  
  
file_read(struct File *f, void *buf, size_t count, off_t offset)
从文件的offset处开始读取count个字节到buf中，返回实际读取的字节数。  


file_write(struct File *f, const void *buf, size_t count, off_t offset)
从文件offset处开始写入buf中的count字节，返回实际写入的字节数。  

## 文件系统用户接口
文件描述符在user/fd.h定义
fsipc.c在user/fsipc.c里  

## 文件系统服务  
serve函数在fs/serv.c，open结构体定义，opentab也在里面  

remove操作接口，fsipc_remove，fsreq结构体在  
file.c里remove函数调用fsipc_remove函数

serv主函数开始执行
