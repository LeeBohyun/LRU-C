# LRU-C
LRU-C: Parallelizing Database I/Os for Flash SSDs

## Abstract
The conventional database buffer managers have two inherent sources of I/O serialization: read stall and mutex conflict. The serialized I/O makes storage and CPU under-utilized, limiting transaction throughput and latency. Such harm stands out on flash SSDs with asymmetric read-write speed and abundant I/O parallelism.

To make database I/Os parallel and thus leverage the parallelism in flash SSDs, we propose a novel approach to database buffering, the LRU-C method. It introduces the LRU-C pointer that points to the least-recently-used-clean page in the LRU list. Upon a page miss, LRU-C selects the current LRU-clean page as a victim and adjusts the pointer to the next LRU-clean one in the LRU list. This way, LRU-C can avoid the I/O serialization of read stalls. The LRU-C pointer enables two further optimizations for higher I/O throughput: dynamic-batch-write and parallel LRU-list manipulation. The former allows to flush more dirty pages at a time, while the latter does to mitigate two mutex-induced I/O serializations. Experiment result from running OLTP workloads using MySQL-based LRU-C prototype on flash SSDs shows that it improves transaction throughput over the vanilla MySQL and the state-of-the-art solution by 3x and 2x, respectively, and also cut the tail latency drastically. Though LRU-C might compromise the hit ratio slightly, its increased I/O throughput far offsets the reduced hit ratio.

## Link to slides
The pdf file summarizes the main concept of LRU-C. I hope you can better understand our paper with this slide. 
- Slide Link: [Link](https://drive.google.com/file/d/1FgjDNfjSeSQgjwSQBYEz5nEardbZRJ1B/view?usp=sharing)

## Modified files
Modified parts in files are marked with ```For LRU-C``` with comments in the source code. Modifications are made only in the following files in ```LRU-C/storage/innobase/buf``` and ```LRU-C/storage/innobase/include```.
- ```buf0lru.cc```
- ```buf0flu.cc```
- ```buf0buf.cc```
- ```buf0lru.h```
- ```buf0flu.h```

## How to run TPC-C@MySQL with LRU-C

1. Download the LRU-C source code and unzip the file.
```bash
$ wget https://github.com/LeeBohyun/LRU-C/archive/refs/heads/main.zip
$ unzip main.zip
```

2. Build and initialize MySQL. Conider ```LRU-C-main``` as ```mysql-5.6.26``` from the installation guide link.
- Installation guide link: [Insgallation Guide](https://github.com/LeeBohyun/mysql-tpcc/blob/master/installation_guide/how-to-install-and-run-mysql-tpcc.md)

3. Run TPC-C and check out the result!
