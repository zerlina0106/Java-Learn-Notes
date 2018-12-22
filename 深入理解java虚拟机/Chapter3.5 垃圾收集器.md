# Chapter3.5 垃圾收集器  


------


        HotSpot虚拟机包含7种作用于不同分代的收集器，如果两个收集器之间存在连线，说明可以搭配使用。目前没有完美的收集器
	
### 3.5.1 Serial收集器

	1. 单线程的收集器：  
		 只会使用一个CPU或者一条收集线程去完成垃圾收集工作  
		 垃圾收集过程中，会暂停其他所有工作的线程，知道收集结束。"Stop The World"  
	2. 虚拟机运行在client模式下的默认新生代收集器：  
		 简单、高效  
		 单个CPU环境中，没有线程交互的开销，效率高  

### 3.5.2 ParNew收集器  
	
	1. Serail的多线程版本：使用多条线程进行垃圾收集  
	2. 运行在Server模式下的虚拟机首选新生代收集器  
		 只有这个能与CMS收集器配合工作，CMS收集器（Concurrent Mark Sweep），CMS第一次实现了让垃圾收集线程与  
   	  用户线程基本是同时工作。但是CMS是老年代的收集器，新生代只能选ParNew/Serial  
		 -XX:+UseConcMarkSweepGC选项之后默认的新生代就是ParNew   
	3. 单CPU环境下没有serial效果好，默认线程数与CPU数量相同，可以用 -XX:ParellelGCThread限制GC线程数  
	---------
	
	
	
	


