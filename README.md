# GridFSTree
Grid FS 文件系统技术研究


<pre>
GridFS是MongoDB数据库之上的一个简单文件系统抽象。

GridFS是基于MongoDB存储引擎实现的分布式文件系统，底层基于MongoDB存储机制，和其他本地文件
系统相比，它具备大数据存储的多个优点。
</pre>

<pre>
     1）GridFS适合存储超过16MB的大型文件
     2) 可以使用GridFS构建大规模的图片服务器，文档服务器，视频，音频服务器
</pre>

<pre>
     GridFS并不是将单个文件直接存储为一个document，而是将文件分成多个parts或者说chunks,
然后将每个chunk作为一个单独的document存储，然后将chunks有序保存，默认情况下，GridFS的
chunk大小为255k。GridFS使用2个Collections来存储这些文件，一个collection存储文件的chunks
（实际文件数据），另一个存储文件的metadata。

     当用户查询GridFS中的文件时，客户端或者driver将会重新按序组装这些chunks。用户可以
range查询文件，也可以获取文件的任意部分的信息，比如跳过视频或者音频的中间部分等。

     对于MongoDB而言，每个document组大尺寸为16M，如果想存储一条数据超过16M，那么只能
使用GridFS;
     GridFS可以支持单个文件尺寸达到数G，读取文件时可以分段读取。此外，GridFS可以从MongoDB
的高性能，高可用特性中获益。
</pre>

<pre>
使用场景
    document的大小超过16M是使用GridFS的条件之一，因为mongodb普通的collection无法支
    持16M以上的document，我们不得不选择其他方案；在一些情况下，将这些大文件存储在
    GridFS中，比直接存储在本地文件系统中更加适合：
       1）如果你的文件系统对每个目录下文件的个数有限制（或者太多，将会影响文件的打开速度等）。

       2）如果你的文件数据，有分数据中心镜像保存（大数据情况，可用性保证）。

       3）如果你希望访问一个超大的文件，而不希望将它全部加入内存，而是有“range access”
       的情况，即分段读取，那么GridFS天生就具备这种能力，你可以随意访问任意片段。
</pre>