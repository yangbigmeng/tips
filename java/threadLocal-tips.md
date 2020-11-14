# 关于TheadLocal的几个问题
1. ThreadLocal的目的是实现线程间的数据隔离，如何做到呢？

    通过Thread内部变量ThreadLocalMap实现

```java
/* ThreadLocal values pertaining to this thread. This map is maintained
 * by the ThreadLocal class. */
ThreadLocal.ThreadLocalMap threadLocals = null;
```

2. ThreadLocalMap定义
  
    类似HashMap定义，key-value类型，set方法通过计算hash值存储value，通过线性寻址解决Hash冲突

```java
/**
 * The table, resized as necessary.
 * table.length MUST always be a power of two.
 */
private Entry[] table;

/**
 * 弱引用
 * ThreadLocal的弱引用， key=ThreadLocal
 */
static class Entry extends WeakReference<ThreadLocal<?>> {
    /** The value associated with this ThreadLocal. */
    Object value;

    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}

 /**
  * set方法
  */
 private void set(ThreadLocal<?> key, Object value) {
	Entry[] tab = table;
	int len = tab.length;
	// 计算hash值
	int i = key.threadLocalHashCode & (len-1);

	for (Entry e = tab[i];
		// hash冲突，nextIndex通过线性寻址解决冲突
		 e != null;
		 e = tab[i = nextIndex(i, len)]) {
		ThreadLocal<?> k = e.get();

		// key相同，更新value
		if (k == key) {
			e.value = value;
			return;
		}
  
		// 设置value
		if (k == null) {
			replaceStaleEntry(key, value, i);
			return;
		}
	}

	tab[i] = new Entry(key, value);
	int sz = ++size;
	if (!cleanSomeSlots(i, sz) && sz >= threshold)
		rehash();
}


```

3. ThreadLocalMap中的Entry为什么做成数组？

    一个thread被多个ThreadLocal使用，如ThreadLocal<Integer>, ThreadLocal<String>, 多种类型方便查找
    
4. 线程之间的ThreadLocal值如何共享？

    子线程共享父线程的ThreadLocal, Thread中定义的ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
    
5. 内存泄漏问题

    ThreadLocalMap中的Entry定义是ThreadLocal的弱引用
    场景：当使用线程池时，单次请求结束后定义的ThreadLocal被回收掉，但是线程还在，导致内存泄漏。
    通过ThreadLocal的remove方法，get方法之后使用
    
6. 应用场景

    上下文，日志，时间传递等
  
