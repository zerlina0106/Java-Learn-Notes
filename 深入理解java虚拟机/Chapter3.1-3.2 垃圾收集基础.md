# Chapter3.1-3.2   垃圾收集基础

#### ------- Java与C++之间有一堵由内存动态分配和垃圾收集技术所围城的高墙，墙外面的人想进去，墙里面的人却想出来

----

## 3.1 垃圾收集基础  

	1. Garbage Collection需要完成的三件事：  
		 哪些内存需要回收、什么时候回收、如何回收
	2. 根据chapter2 ，我们知道栈中的栈帧随着方法进入退出入栈出栈，一个栈帧的内存数基本是类结构确定后已知，pc,stack,native method stack也是随着线程产生消亡的；  
	   因此，java堆和方法区是运行期间才可能知道会创建哪些对象，这部分的内存分配和回收都是动态的，是主要的GC区  
	   
## 3.2 如何判断对象是否可回收  

### 3.2.1 引用计数算法 Reference Counting  

	1. 给对象加一个引用计数器，每有一个地方引用它，计数器值加一；引用失效时，计数器值减一；任何时刻计数器为0的对象不会再被使用  
	2. 实现简单、判定效率高  COM,FlashPlayer,Python,Squirrel都是用的这个做内存管理    
	3. 但是JAVA虚拟机没有用，因为很难解决对象之间相互循环引用的问题  
	4. 类似死锁，两个对象分别引用了对方，除此之外无其他被引，导致这两个都不能再访问，引用计数不会为0  
	
----

### 3.2.2 可达性分析算法 Reachability Analysis

	1. java, C#, 都是通过这个判定对象是否存活  
	2. 基本思路：  
		 通过一系列被称为"GC Roots" 的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链（Reference Chain）    
		 当一个对象到GC Roots没有任何引用链相连（不可达）---> 这个对象不可用  
	3. 可作为 GC Roots的对象   
		 （这里书上真的是看不懂了，看了官方文档才看明白）  
		 System Class: Class loaded by bootstrap/system class loader.      
		 		ex. everything from the rt.jar like java.util.*. 
				
		 JNI Local: Local variable in native code  
		 
		 JNI Global: Global variable in native code  
		 
		 Thread Block: Object referred to from a currently active thread block  
		 
		 Thread: a started, but not stopped, thread.  
		 
		 Busy Monitor: Everything that has called wait() or notify() or that is synchronized.  
		 
		 Java Local: Local variable. For example, input parameters or locally created objects  
		 of methods that are still in the stack of a thread.  
		 
		 Native Stack, Finalizable, Unfinalized, Unreachable, Java Stack Frame   
----

### 3.2.3 引用的分类

	1. Strong Reference: 程序中普遍存在，类似 Object obj = new Object()只要强引用还在，垃圾收集器不会回收被引用的对象  
	2. Soft Reference: 还有用但非必须的对象，系统发生内存溢出异常前会被纳入回收范围  
	3. Weak Reference: 对象只能生存到下一次GC发生之前，无论内存够不够都要回收  
	4. Phantom Reference: 让对象被回收时有个系统通知  
	
	
#### 对象可以自救但是运行代价高，不确定性大，就不展开了，书上有展开讲  

----

### 3.2.5 回收方法区  
	1. 回收内容：  
		废弃常量（没有被引用就被回收）  
	        回收类 ： 该类所有实例已被回收+加载类的ClassLoader已被回收+对应的java.lang.Class对象未被引用（无法通过反射访问类方法）  
	2. 大量使用反射、动态代理、CGLib等ByteCode框架、动态生成JSP以及OSGi这类频繁自定义ClassLoader的场景都需要JVM具备类卸载功能  



		 

	

