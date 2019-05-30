---
title: 使用小根堆查找海量数据前k大数据
tags:
  - Java
abbrlink: 62588
date: 2019-05-28 14:41:13
---

## 题目介绍

&emsp;&emsp;一位朋友的面试题问到我，刚好自己之前的面试也有碰到过，挺经典的题目，所以结合代码分享一下思路。

&emsp;&emsp;题目描述：对已有的海量数据进行筛选（数量级是十亿）。每个文件包含若干个条目，每个条目就是一条URL地址。这个情景就相当于模仿后台记录的用户访问记录，然后对数据进行统计。要求找出出现数量最多的100个URL。

<!-- more -->

---

## 思路

&emsp;&emsp;对于海量数据的处理思路还是很简单的：分而治之以及并行化处理。并行化很简单，调用接口传递数据即可，这里的重点是算法本身。每个文件我们都维护一个**小根堆**，因为我们要找前100大，所以要用小根堆维护（通过每次去除掉最小的堆根，保证堆中存放是当前最大的前100个数）。最后将若干个小根堆的数据再用一个大小为100的小根堆筛选就可以了。

&emsp;&emsp;上面说的可能有点抽象，数据量小一点我们举个例。假设我们有10000个数据，我们要找前10大的数。我们将10000个数据分为100个文件，每个文件有100条数据。然后对于每个文件，使用大小为10的小根堆找出前10大的数。完成后，现在我们一共有1000个数据（100个文件，每个文件10个元素）。我们对这1000个数据再用一个大小为10的小根堆筛选就得到前10大的数了。

---

## 实现

&emsp;&emsp;接下来我们就用一个demo来实现。项目代码在：<https://github.com/leungyukshing/java/tree/master/Heap_v2>

&emsp;&emsp;因为没有真实的数据，这里我先自己随机生成了8个txt文件模拟URL记录，记录的形式为`URLxx`。

小根堆`Heap.java`：

```java
import java.util.Arrays;


public class Heap {

    public class Node {
        public int count;
        public String url;

        public void setNumber(int number) {
            this.count = number;
        }

        public void setUrl(String url) {
            this.url = url;
        }



    }
    private void print() {
      for (int i = 0; i < array.length; i++) {
          System.out.print(array[i].count + " ");
      }
      System.out.println();
    }

    Node[] array;
    int size;

    public void buildHeapWithArray(Node[] array, int size) {
        if (size > array.length)
            throw new RuntimeException("元素个数超过数组大小");

        this.array = array;

        // look array
        // print()

        this.size = size;
        for (int i = size / 2 - 1; i >= 0; i--)
            percolateDown(i);
    }

    public int deleteMin() {
        if (size == 0)
            throw new RuntimeException("No element in heap, CANNOT delete");

        // System.out.println("Before delete: " + size);
        // look array
        // print();

        int minItem = array[0].count;
        // System.out.println("size1: " + size);
        array[0].count = array[--size].count;
        array[0].url = array[size].url;
        // print();

        // System.out.println("size2: " + size);
        array[size].count = -1;

        // System.out.println("Before percolate: ");
        // look array
        // print();

        percolateDown(0);


        // System.out.println("After percolate: ");
        // look array
        // print();

        return minItem;
    }

    public void insert(int x, String xx) {
        if (size == array.length)
            throw new RuntimeException("Array is full, CANNOT insert");
        int hole = size++;
        // System.out.println("Insert " + x + ", URL = " + xx);
        for (; hole != 0; hole = (hole - 1) / 2) {
            if (x < array[(hole - 1) / 2].count && xx != array[(hole - 1) / 2].url) {
              //array[hole] = array[(hole - 1) / 2];
                // deep copy!!!
              array[hole].count = array[(hole - 1) / 2].count;
              array[hole].url = array[(hole - 1) / 2].url;
            }
            else
                break;
        }
        array[hole].count = x;
        array[hole].url = xx;

        // look array
        // System.out.println("After insert: ");
        // print();
    }

    public int getMin() {
        return array[0].count;
    }

    private void percolateDown(int hole) {
        int tmp = array[hole].count;
        String emp = array[hole].url;
        int childIndex;

        for (; hole * 2 <= size - 2; hole = childIndex) {
            childIndex = hole * 2 + 1;
            if (childIndex != size - 1 && array[childIndex + 1].count < array[childIndex].count && array[childIndex + 1].url != array[childIndex].url)
                childIndex++;

            if (tmp > array[childIndex].count && emp != array[childIndex].url) {
                array[hole].count = array[childIndex].count;
                array[hole].url = array[childIndex].url;
            }
            else
                break;
        }
        array[hole].count = tmp;
        array[hole].url = emp;
    }
}
```

主程序`Main.java`：

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Set;
import java.io.*;


public class Main {
    public static void main(String[] args) throws Exception {
        System.out.println(System.getProperty("user.dir"));
        int n = 1;
        HashMap<String,Integer> HashURL = new HashMap<String, Integer>();
        while (n <= 8) {
            BufferedReader readTxt = new BufferedReader(new FileReader(new File(System.getProperty("user.dir") + "/" + n + ".txt")));

            String textLine = "";

            String str = "";

            while ((textLine = readTxt.readLine()) != null) {
                str += " " + textLine;
            }

            String[] numbersArray = str.split(" ");

            Integer i = 1;

            while (i < numbersArray.length) {
                String URL = numbersArray[i];
                if (HashURL.containsKey(URL) == false) {
                    HashURL.put(URL, 1);
                } else {
                    Integer count = HashURL.get(URL);
                    //System.out.println(count + URL + " " + i);
                    count = count + 1;
                    HashURL.put(URL, count);
                    //System.out.println(count);
                }
                i = i + 1;
            }
            n = n + 1;
            readTxt.close();
        }
        
        Set<String> keys = HashURL.keySet();
        /*
        for (String key : keys) {
            System.out.println(key + " " + HashURL.get(key));
        }
        */

        
        int size = 4; // top size
        Heap heap = new Heap();

        Heap Node = new Heap();
        Heap.Node[] data = new Heap.Node[size];

        // Initialization
        for (int i = 0; i < size; i++) {
            data[i] = Node.new Node();
        }

        for (int i = 0; i < size; i++) {
            data[i].setNumber(0);
            data[i].setUrl("");
        }
        // Construct the Heap
        heap.buildHeapWithArray(data, size);

        for (String key : keys) {
            int num;
            num = HashURL.get(key);
            // System.out.println(num);
            int minEle = heap.getMin();
            // System.out.println(minEle);
            if (num > minEle) {
                heap.deleteMin();
                heap.insert(num, key);
                // System.out.println("After insert, min = " + heap.getMin());
            }
        }
        // System.out.println(Arrays.toString(data));
        for(int i = 0; i < size; i++) {
            System.out.println("count: " + data[i].count + " " + "url: " + data[i].url);
        }
    }
}

```

运行结果：

![HeapResult](/images/heap_result.png)

---

## 小结

&emsp;&emsp;这道题的思路是比较经典的，也是涉及到了堆的应用，是一道比较有价值的题目。自己动手实现堆也能学到不少东西。coding中特别要注意交换节点的时候要deep copy，不然会导致内存地址出错！最后，希望这篇博客能够帮助到你，谢谢您的支持！