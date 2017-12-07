---
title: java.util.ConcurrentModificationException迭代器出错
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 问题
最近在一次代码保守中，遇到了这样一个问题：java.util.ConcurrentModificationException。调查出的结果是ArrayList在迭代的时候如果同时对其进行修改就会抛出Java.util.ConcurrentModificationException异常。
比如以下情况：

``` java
		Iterator<Integer> iterator = list.iterator();
		while (iterator.hasNext()) {
			Integer integer = iterator.next();
			if (integer == 2)
				list.remove(integer);
		}
		// 或者
		for(int i : list){......}
```
使用迭代器遍历的时候，如果遇到2，就删除这个元素，会立刻抛错ConcurrentModificationException。

## 调查：
查看ArrayList的源码，发现抛错的573行确实是 throw new ConcurrentModificationException();而导致这个错误的原因是ourList.modCount != expectedModCount
代码在遍历的时候，使用的list的iterator方法：

``` java
    @Override public Iterator<E> iterator() {
        return new ArrayListIterator();
    }
```
返回了一个ArrayListIterator。

``` java
private class ArrayListIterator implements Iterator<E> {
        ......
        /** The expected modCount value */
        private int expectedModCount = modCount;
		......
        @SuppressWarnings("unchecked") public E next() {
            ArrayList<E> ourList = ArrayList.this;
            int rem = remaining;
            if (ourList.modCount != expectedModCount) {
                throw new ConcurrentModificationException();// 573行
            }
            if (rem == 0) {
                throw new NoSuchElementException();
            }
            remaining = rem - 1;
            return (E) ourList.array[removalIndex = ourList.size - rem];
        }
		......
}
```
从上述源码可知，ArrayListIterator是ArrayList的内部类，expectedModCount是ArrayListIterator的一个成员变量，modCount是ArrayList的成员变量。expectedModCount初始值和modCount相同，那么为什么会不一样呢。
查看ArrayList的add函数：

``` java
    @Override public boolean add(E object) {
        Object[] a = array;
        int s = size;
        if (s == a.length) {
            Object[] newArray = new Object[s +
                    (s < (MIN_CAPACITY_INCREMENT / 2) ?
                     MIN_CAPACITY_INCREMENT : s >> 1)];
            System.arraycopy(a, 0, newArray, 0, s);
            array = a = newArray;
        }
        a[s] = object;
        size = s + 1;
        modCount++;
        return true;
    }
```
发现ArrayList在进行add和remove操作的时候会对modCount进行++，那么如果此时同时使用迭代器进行遍历的话，ArrayList中的modCount和ArrayListIterator中的expectedModCount，一定不会相等。
在观察一下ArrayListIterator自己的remove方法：

``` java
        public void remove() {
            Object[] a = array;
            int removalIdx = removalIndex;
            if (modCount != expectedModCount) {
                throw new ConcurrentModificationException();
            }
            if (removalIdx < 0) {
                throw new IllegalStateException();
            }
            System.arraycopy(a, removalIdx + 1, a, removalIdx, remaining);
            a[--size] = null;  // Prevent memory leak
            removalIndex = -1;
            expectedModCount = ++modCount;
        }
```
代码中在最后对expectedModCount和modCount进行了同步，所以如果在单线程的状态下，迭代器的使用和remove操作同时进行的时候，可以使用迭代器自己的remove方法解决这个问题。
## 单线程解决方案：
代码如下：

``` java
		Iterator<Integer> iterator = list.iterator();
		while (iterator.hasNext()) {
			Integer integer = iterator.next();
			if (integer == 2)
				iterator.remove();
		}
```
很可惜迭代器没有add方法，所以如果同步解决的是添加的操作，好像无解了？
## 多线程解决方案
那么add操作或者是多进程怎么办呢，根据上面的源码，ArrayList在调用iterator返回迭代器的时候，返回的是一个new ArrayListIterator();那就意味着，两个线程，所获取的ArrayListIterator不一样，他们的expectedModCount也是分开的，一旦其中一个线程进行了操作，修改了modCount，即便我们使用迭代器自己的remove方法，同步了线程1的expectedModCount和modCount，线程2的expectedModCount还是会和modCount不一致的。

所以有一个最直接的方式，不用迭代器，比如我们这次遇到的问题，可以把

``` java
for (int i : lists)
```
改为
``` java
for (int i = 0; i < lists.size();  i++)
```
当然如果一定要使用迭代器，网上给出了两个方案：

 1. 在迭代时使用synchronized或者Lock进行同步
 2. 使用CopyOnWriteArrayList替代ArrayList。
 CopyOnWriteArrayList是一种写时copy的方案，在需要修改的时候复制一个副本，并锁住内容，避免多个副本生成，修改完成之后将引用指向新的副本。所以会带来两个问题，一个是副本生成时的内存消耗，一个是不能及时反映修改，只能保证最终结果的一致性。
 以下是尝试代码： 

``` java
		new Thread(new Runnable() {

			@Override
			public void run() {
				Iterator<Integer> iterator = list.iterator();
				while (iterator.hasNext()) {
					Integer integer = iterator.next();
					if (integer % 2 == 0)
						list.remove(integer);
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
				System.out.println("result:");
				for (int i = 0; i < list.size(); i++) {
					System.out.print(list.get(i));
				}
			}
		}).start();
		new Thread(new Runnable() {

			@Override
			public void run() {
				Iterator<Integer> iterator = list.iterator();
				while (iterator.hasNext()) {
					System.out.println(iterator.next());
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
			}
		}).start();
```
