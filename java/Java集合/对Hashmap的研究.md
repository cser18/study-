## 探究为什么HashMap总是使用2的幂作为哈希表的大小

HashMap 默认的初始化⼤⼩为16。之后 每次扩充，容量变为原来的2倍。②创建时如果给定了容量初始值，那么 Hashtable 会直接使⽤ 你给定的⼤⼩，⽽ HashMap 会将其扩充为2的幂次⽅⼤⼩（HashMap 中的 tableSizeFor() ⽅法保证）

#### 1. 第一步 找到带有初始容量的构造函数

```java
// 第一个
public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }

//第二个
public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }
```

#### 第二步 找到tableSizeFor(int cap) 

```java
static final int tableSizeFor(int cap) {
    
        int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
        return (n < 0) ? 1 : (n >= 1 << 30) ? 1 << 30 : n + 1;
    }
```

#### 第三步 研究Integer.numberOfLeadingZeros(cap - 1);

```java
/*
* 1. << 左移 表示左移移，不分正负数，低位补0；　
* 2. >> 右移动 表示右移，如果该数为正，则高位补0，若为负数，则高位补1；
* 3. >>> 表示无符号右移，也叫逻辑右移，即若该数为正，则高位补0，而若该数为负数，则右移后高位同样补0
*/

public static int numberOfLeadingZeros(int i) {
        // HD, Count leading 0's
        if (i <= 0)
            return i == 0 ? 32 : 0;
        int n = 31;
        if (i >= 1 << 16) {
            n -= 16;
            i >>>= 16;
        }
        if (i >= 1 << 8) {
            n -= 8;
            i >>>= 8;
        }
        if (i >= 1 << 4) {
            n -= 4;
            i >>>= 4;
        }
        if (i >= 1 << 2) {
            n -= 2;
            i >>>= 2;
        }
        return n - (i >>> 1);
    }
```

```txt
1. tableSizeFor(15)

2. 
numberOfLeadingZeros(14) 
14的2进制 0000 1110
会进入第5个if语句 
n=29 的2进制 0001 1101
i 0000 0011 3
return (29 - 1) =  28

3.
tableSizeFor 
-1 32个1
移动28位 0000 0000 0000 0000 0000 0000 0000 1111  十进制(15) 
最后 15 + 1=16 为2的4次方
```

#### 第四步：

```txt
为了能让 HashMap 存取⾼效，尽量减少碰撞，也就是要尽量把数据分配均匀。我们上⾯也讲到了过了，Hash 值的范围值-2147483648到2147483647，前后加起来⼤概40亿的映射空间，只要哈希函数映射得比较均匀松散，⼀般应⽤是很难出现碰撞的。但问题是⼀个40亿⻓度的数组，内存是放不下的。所以这个散列值是不能直接拿来⽤的。⽤之前还要先做对数组的⻓度取模运算，得到的余数才能⽤来要存放的位置也就是对应的数组下标。这个数组下标的计算⽅法是“ (n - 1) & hash ”。（n代表数组⻓度）。这也就解释了 HashMap 的⻓度为什么是2的幂次⽅。
这个算法应该如何设计呢？我们首先可能会想到采⽤%取余的操作来实现。但是，重点来了：“取余(%)操作中如果除数是2的幂次则等价于与其除数减⼀的与(&)操作（也就是说 hash%lengthdehash&(length-1)的前提是 length 是2的n 次⽅；）。” 并且 采⽤⼆进制位操作 &，相对于%能够提⾼运算效率，这就解释了 HashMap 的⻓度为什么是2的幂次方。
```

