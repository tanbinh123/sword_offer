# 面试题2

## 单例模式

设计一个类，只能生成该类的一个实例

单例模式需要满足如下规则：

- 构造函数私有化（private），使得不能直接通过new的方式创建实例对象；
- 通过new在代码内部创建一个（唯一）的实例对象；
- 定义一个public static的公有静态方法，返回上一步中创建的实例对象；由于在静态方法中，所以上一步的对象也应该是static的。

### 饿汉式单例

```java
public class EagerSIngleton {

    //类加载时先New一个出来
    private static EagerSIngleton eagerSIngleton = new EagerSIngleton();
    private EagerSIngleton() {

    }

    public static EagerSIngleton getInstance() {
        return eagerSIngleton;
    }

    public static void main(String[] args) {
        EagerSIngleton s1 = EagerSIngleton.getInstance();
        EagerSIngleton s2 = EagerSIngleton.getInstance();

        System.out.println(s1 == s2);

    }
}
```

### 懒汉式单例

```java
public class LazySingleton {
    private LazySingleton() {

    }

    private static LazySingleton instance = null;

    public static LazySingleton getInstance() {
        if (instance == null) {
            //锁定代码块
            synchronized (LazySingleton.class) {
                //第二重判断
                if (instance == null) {
                    instance = new LazySingleton();
                }
            }
        }
        return instance;
    }
}
```



# 面试题3

## 数组中的重复数字

在一个长度为n的数组里的所有数字都在0~n-1的范围内，数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次，请找出数组中任意一个重复的数字.

> 例如：如果输入长度为7的数组{2,3,1,0,2,5,3}那么对应的输出是重复的数字2或者3

### 使用数组排序

```java
public Boolean duplicate1(int[] arrays,int length,int[] duplicate) {
        if (arrays == null || arrays.length != length || length == 0  ) {
            return false;
        }
        Arrays.sort(arrays);
        for (int i = 0; i < arrays.length - 1; i++) {
            if (arrays[i] == arrays[i + 1]) {
                duplicate[0] = arrays[i];
                return true;
            }
        }

        return false;
    }
```

### 交换数组元素位置

```java
public Boolean duplicate2(int[] arrays, int length, int[] duplicate) {
        if (arrays == null || arrays.length != length || length == 0  ) {
            return false;
        }
        for (int i = 0; i < length; i++) {
            if (arrays[i] < 0 || arrays[i] > length-1) {
                return false;
            }
        }
        for (int i = 0; i < length; i++) {
            while (i != arrays[i]) {
                if (arrays[arrays[i]] == arrays[i]) {
                    duplicate[0] = arrays[i];
                    return true;
                }
                swap(arrays, i, arrays[i]);
            }

        }



        return false;
    }
```



### 测试用例

* 长度为n的数组里包含一个或多个重复的数字
* 数组中不包含重复的数字
* 无效输入测试用例（输入空指针；长度为n的数组中包含0~n-1之外的数字）

## 不修改数组找出重复的数字

在一个长度为n+1的数组里的所有数字都在1-n 的范围内，所以数组中至少有一个数字是重复的，请找出数组中任意一个重复的数字，但不能修改输入的数组。

>  例如：如果输入长度为8的数组{2,3,5,4,3,2,6,7},那么对应的输出重复数字为2或3

```java
public int duplicate(int[] arrays, int length, int[] duplicate) {
        if (arrays == null || arrays.length <= 0 || length != arrays.length) {
            return 0;
        }
        for (int i = 1; i < length ; i++) {
            if (arrays[i] < 1 || arrays[i] > length - 1) {
                return 0;
            }


        }
        for (int i = 1; i < length; i++) {
            if (arrays[i] == duplicate[arrays[i]]) {
                return arrays[i];
            }
            duplicate[arrays[i]] = arrays[i];
        }

        return 0;
    }

```



# 面试题4:

## 二维数组中的查找

在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序，请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

> 例如下面的二维数组就是每行，每眼列都递增排序。如果在这个数组中查找数字7，则返回truel；旭盯想找数字5，由于数组不含有该数字，则返回false

| 1    | 2    | 8    | 9    |
| ---- | ---- | ---- | ---- |
| 2    | 4    | 9    | 12   |
| 4    | 7    | 10   | 13   |
| 6    | 8    | 11   | 15   |

### 二分法查找

### 利用二维数组特性缩小范围

首先选取数组中右上角的数字，如果该数字等于要查找的数字，则查找过程结束；如果该数字大于要查找的数字，则剔除这个数字所在的列；如果该数字小于要查找的数字，则剔除为个数字所在的行。也就是说，如果要查找的数字不在数组的右上角，则每一次都在数组的查找范围中剔除一行或一列，这样每一步都可以缩小查找的范围，直到查找的数字，或者查找范围为空。

```java
public boolean find2(int[][] arrays, int target) {
        if (arrays == null || arrays.length <= 0) {
            return false;
        }

        int row = 0;
        int rows = arrays.length;
        int cols = arrays[0].length-1;

        while (row < rows && cols >= 0) {
            if (arrays[row][cols] > target) {
                cols--;
            }else if (arrays[row][cols] <target) {
                row++;
            } else{
                return true;
            }
        }
        return false;

    }

```

### 测试用例

* 二维数组中包含查找的数字（查找的数字是数组中的最大值和最小值；查找的数字介于数组中的最大值与最小值之间）
* 二维数组中没有查找的数字（查找的数字大于数组中的最大值；查找的数字小于数组中的最小值；查找的数字在数组的最大值和最小值之间但数组中没有这个数字）
* 特殊输入测试（输入空指针）

# 面试题5

## 替换空格

题目：请实现一个函数。把字符串中的每个空格替换成”％20“。例如，输入”We are happy“,则输出”We%20are%20happy“

### 使用Java内置方法

```java
public String replaceBlank1(StringBuffer s) {
        if (s == null || s.length() == 0) {
            return null;
        }

        return s.toString().replace(" ", "%20");
    }
```

### 从后往前扫描

**从前往后扫描要移动那么多次，不妨反过来从后往前扫描试试。**

- 先遍历一遍原字符串，统计空格字符的个数。
- 由于要将空格（一个字符）变成`%20`（三个字符），所以需要将原字符串增长`2 * 空格数`
- 设置两个指针，一个指针oldP指向原字符串的末尾；另一个指针newP指向增长后的新字符串末尾。不断将oldP处的字符移动到newP处，然后两个指针都要左移；如果oldP处字符是空格，就在newP处设置三个字符：按顺序分别是`0、2、%`，同样的两个指针相应的左移。

```java
public String replaceBlank2(StringBuffer s) {
        if (s == null || s.length() == 0) {
            return null;
        }
        int spaceNum = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i)== ' ') {
                spaceNum++;
            }
        }
        int oldP = s.length() - 1;
        s.setLength(s.length() + 2 * spaceNum);
        int newP = s.length() - 1;
        while (oldP >= 0 && oldP != newP) {
            if (s.charAt(oldP) == ' ') {
                s.setCharAt(newP--, '0');
                s.setCharAt(newP--, '2');
                s.setCharAt(newP--, '%');
            } else {
                s.setCharAt(newP--, s.charAt(oldP));
            }
            oldP--;
        }
        return s.toString();
    }
```

### 测试用例

* 输入的字符串中包含空格（空格位于字符串的最前面；空格位于字符串的最后面，空格位于字符串的中间，字符串中有连续多个空格
* 输入的字符串中没有空格
* 特殊输入测试（字符串是一个nullptr指针；字符串是一个空字符串；字符串只有一个空格字符；字符串中有连续多个空格）

## 相关题目

有两个有序的数组A1和A2，A1末尾有足够空间容纳A2。实现一个函数将A2的所有数字插入到A1中，并且所有数字是有序的。

**因为空闲的空间在A1的末尾，所以从后往前比较两个A1和A2的数字，将更大的那个移动到A1的末尾，然后左移指针，继续比较两个数组中的数。当某个数组中的元素被取完了，就直接从另外一个数组取。**

比如下面的例子

```
A1 = [1, 2 ,4 ,7, 9, , , ...]
A2 = [3, 5, 8, 10, 12]
```

假设A1的长度为10，现暂时只有5个元素，这个长度刚好能装下A2。从后往前比较A1和A2：12比9大，将12移动到A1[9]中，然后9和10继续比较，10移动到A1[8]中，9和8比较9移动到A1[7]中，如此这般直到扫描完两个数组，所有数字也都有序了。

```java
public class MergeTwoSortedArray {

    public static void merge(Integer[] a, Integer[] b) {
        if (a.length < b.length) {
            return;
        }
        int la = 0;

        for (int i = 0; i < a.length; i++) {
            if (a[i] != null) {
                la++;
            }
        }
        la--;
        int k = la + b.length ;
        int lb = b.length-1;
        while (k >= 0 && lb>=0) {
            if (b[lb] > a[la]) {
                a[k--] = b[lb--];
            } else if (b[lb] < a[la]) {
                a[k--] = a[la--];
            } else if (b[lb] == a[la]) {
                a[k--] = a[la--];
                lb--;
            }
        }

    }

    public static void main(String[] args) {
        Integer[] a = new Integer[10];
        for (int i = 0; i < 4; i++) {
            a[i] = 2 * i + 1;
        }
        Integer[] b = {1, 4, 6, 8};
        merge(a, b);
        System.out.println(Arrays.toString(a));
    }
}

```

# 面试题6

题目：输入一个链表的头节点，从尾到头反过来打印出每个节点的值。

链表定义如下

```java
public class ListNode {
    public int val;
    public ListNode next = null;

    public ListNode(int val) {
        this.val = val;
    }
}
```

## 使用栈

```java
public static ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if (listNode == null) {
            return null;
        }
        LinkedList<Integer> stack = new LinkedList<>();
        for (ListNode node = listNode; node != null; node = node.next) {
            stack.push(node.val);
        }
        return new ArrayList<>(stack);
    }
```

## 递归

```java
public  ArrayList<Integer> printListFromTailToHead2(ListNode listNode) {
        if (listNode == null) {
            return null;
        }
        printListFromTailToHead2(listNode.next);

        arrayList.add(listNode.val);
        return arrayList;

    }
```

## 测试用例

* 功能测试（输入的链表有多个节点；输入的链表只有一个节点）
* 特殊输入测试（输入的链表头节点指针为nullptr）

# 面试题7

## 重建二叉树

题目：输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如，输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4，7，2，1，5，3，8，6}，则重建二叉树并输入它的头节点，二叉树和定义如下：

```java

public class TreeNode {
    
    public int val = 0;
    public TreeNode left = null;
    public TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }
}
```

### 递归

```java
 public TreeNode reconstructBinaryTree(int[] preOrder, int[] inOrder) {
        if (preOrder == null || inOrder == null || preOrder.length == 0 || inOrder.length == 0) {
            return null;
        }

        return construct(preOrder,0,preOrder.length-1,inOrder, 0, inOrder.length-1);

    }

    private TreeNode construct(int[] preOrder, int startPre, int endPre, int[] intOrder, int startInOrder, int endInOrder) {

        if (startPre > endPre || startInOrder > endInOrder) {
            return null;
        }

        int rootValue = preOrder[startPre];
        TreeNode root = new TreeNode(rootValue);
        for (int i = 0; i < endInOrder; i++) {
            if (intOrder[i] == rootValue) {
                root.left = construct(preOrder, startPre + 1, startPre+i-startInOrder, intOrder, startInOrder, i - 1);
                root.right = construct(preOrder, startPre + i-startInOrder+1, endPre, intOrder, i+1, endInOrder);
            }
        }
        return root;
```

# 面试题8

题目：给定一棵二叉树和其中一个节点，如何找出中序遍历序列的下一节点？树中的节点除了有两个分别指向左，右节点的指针，还有一个指向父节点的指针。

```java
public class TreeLinkNode {
    public int val;
    public TreeLinkNode left = null;
    public TreeLinkNode right = null;
    public TreeLinkNode next = null;

    public TreeLinkNode(int val) {
        this.val = val;
    }
}
```



要找出中序遍历的下一个结点，要分几种情况探讨。

- 如果当前结点的右子结点不为空，那么下一个结点就是以该右子结点为根的子树的最左子结点；
- 如果当前结点的右子结点为空，看它的父结点。此时分两种情况，如果父结点的右子结点就是当前结点，说明这个结点在中序遍历中已经被访问过了，需要继续往上看其父结点...直到父结点的左子结点是当前结点为止，该父结点就是下一个结点。如果在一直往上的过程中已经到达根结点，而根结点的父结点为null，这种情况说明当前结点已经是中序序列的最后一个结点了，不存在下一个结点，应该返回null.

```java
public static TreeLinkNode getNextNode(TreeLinkNode target) {
        if (target == null) {
            return null;
        }

        if (target.right != null) {
            target = target.right;
            while (target.left != null) {
                target = target.left;
            }
            return target;
        }
        if (target.right == null) {
            if (target == target.next.left) {
                return target.next;
            } else {
                target = target.next;
                while (target.next != null && target != target.next.left ) {
                    target = target.next;
                }
                if (target.next == null) {
                    return null;
                } else {
                    return target.next;
                }
            }
        }
        return null;
    }
```



## 测试用例

* 普通二叉树（完全二叉树，不完全二叉树）
* 特殊二叉树（所有节点都没有右子节点的二叉树；所有节点都没有左子节点的二叉树；只有一个节点的二叉树；二叉树的根节点指针为null）
* 不同位置的节点的下一个节点（下一个节点为当前节点的右子节点，右子树的最左子节点，父节点，跨层的父节点等；当前节点没有下一个节点）

# 面试题9

题目：用两个栈实现一个队列。队列的声明如下，请实现它的两个函数appendTail和deleteHead,分别完成在队列尾部插入节点和在队列头部删除节点的功能。

```java
 private LinkedList<Integer> stack1 = new LinkedList<>();
    private LinkedList<Integer> stack2 = new LinkedList<>();

    public void enqueue(int node) {
        stack1.push(node);
    }

    public int dequeue() {
        if (stack1.isEmpty() && stack2.isEmpty()) {
            throw new RuntimeException("队列为空");
        }
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }

```

## 测试用例

* 往空的队列里添加，删除元素
* 往非空的队列里添加，删除元素
* 连续删除元素直至队列为空

## 相关题目

题目：两个队列模拟一个栈

刚开始两个队列都为空，进栈时，可以进入任意一个队列。不妨默认进入`Queue a`。后续进栈时操作，哪个队列不为空（将看到，另外一个队列肯定为空）就进入该队列中。出栈操作，**最后进入队列的要先出栈**，而此时要出栈的元素在队列最后，但是队列只支持在队列头删除，因此将除了最后一个元素之外的所有元素都删除并复制一份到另一个队列`Queue another`，然后出列最后一个元素即可。此时`Queue a`成了空队列。之后每次出列操作都像上述以样：将不为空的队列中除最后一个元素的其余元素删除并复制到另一个空队列中，再删除原队列中唯一一个元素并弹出。**每次出栈操作后，总有一个队列是空的。又因为进栈时也是进入不为空的那个队列，因此进出栈操作时总有一个队列是空的。**

这两个队列不像上面的例子中有明确的分工，在两个队列实现栈的例子中，它们交替实现进栈或出栈的功能。

总结一下：

- 进栈，进入不为空的那个队列中
- 出栈，将不为空队列中除倒数最后一个元素外的其余元素移动到另一个空队列中，紧接着弹出原队列的最后一个元素。

```java
public class TwoQueueImpStack {

    private Queue<Integer> q1 = new LinkedList<>();
    private Queue<Integer> q2 = new LinkedList<>();

    public void push(int node) {
        if (q1.isEmpty() && q2.isEmpty()) {
            q1.offer(node);
        } else if (!q1.isEmpty()) {
            q1.offer(node);
        } else {
            q2.offer(node);
        }
    }

    public int pop() {
        if (q1.isEmpty() && q2.isEmpty()) {
            throw new RuntimeException("栈已空");
        }
        if (!q1.isEmpty()) {
            for (int i = 0; i < q1.size()-1; i++) {
                q2.offer(q1.poll());
            }
            return q1.poll();
        } else{
            for (int i = 0; i < q2.size()-1; i++) {
                q1.offer(q2.poll());
            }
            return q2.poll();
        }


    }

    public static void main(String[] args) {
        TwoQueueImpStack a = new TwoQueueImpStack();
        a.push(54);
        a.push(55);
        a.push(56);
        System.out.println(a.pop());
        System.out.println(a.pop());
        a.push(53);
        System.out.println(a.pop());
        a.push(52);
        System.out.println(a.pop());
        System.out.println(a.pop());
    }

}
```

# 面试题10

题目一：求斐波那契数列的第n项

写一个函数，输入n，求斐波那契（Fibonacci）数列的第n项。斐波那契数列的定义如下：
$$
f(n) =
\begin{cases}
0  & \text{n=0} \\
1 & \text{n=1} \\
f(n-1)+f(n-2)  &\text{n>1}
\end{cases}
$$

## 非递归

```java
public static long fibonacci(int n) {
        if (n < 0) {
            throw  new RuntimeException("非法数字");
        }
        int[] result = {0, 1};
        if (n < 2) {
            return result[n];
        }

        long MinOne = 1;
        long MinTwo = 0;
        long fibN = 0;
        for (int i = 2; i <= n; i++) {
            fibN = MinOne + MinTwo;
            MinTwo = MinOne;
            MinOne = fibN;
        }
        return fibN;

    }
```

## 递归

```java
public static long finbonacci2(int n) {
        if (n == 1 || n == 0) {
            return n;
        }

        return finbonacci2(n - 1) + finbonacci2(n - 2);
    }
```

## 青蛙跳台阶问题

题目2: 一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶，求该青蛙跳上一个n级台阶总共有多少种跳法。



到达1级台阶只有1种可能，到达2级台阶有2种可能；可记为f(1) = 1,f(2) = 2。要到达3级台阶，可以选择在1级台阶处起跳，也可以选择在2级台阶处起跳，所以只需求到达1级台阶的可能情况 + 到达2级台阶的可能情况，即f(3) = f(2) +f(1)

同理到达n级台阶，可以在n-1级台阶起跳，也可在n-2级台阶起跳，f(n) = f(n-2)+f(n-1)

可以看做是斐波那契数列。

```java
public static long fabonacci(long n) {
        if (n == 1 || n == 2) {
            return n;
        }
        if (n < 1) {
            throw new RuntimeException("非法值");
        }
        long a = 1;
        long b = 2;
        long f = 0;
        for (int i = 3; i <= n; i++) {
            f = a + b;
            a = b;
            b = f;
        }
        return f;
    }
```

### 测试用例

* 功能测试（输入3，5，10）
* 边界值测试（如输入0，1，2）
* 性能测试（输入较大的数字，如40，50，100等）

# 面试题11

## 快速排序

```java
public static void quickSort(int[] array,int low,int high) {

        int i = low;
        int j = high;

        if (low < high) {
            int piv = array[low];
            while (low < high) {
                while (low<high && array[high] >= piv) {
                    high--;
                }
                if (low < high) {
                    array[low] = array[high];
                    low++;
                }
                while (low < high && array[low] < piv) {
                    low++;
                }
                if (low < high) {
                    array[high] = array[low];
                    high--;
                }
            }
            array[low] = piv;
            quickSort(array, i, low - 1);
            quickSort(array, low + 1, j);
        }

    }
```

## 对年龄排序O(n)

题目：请实现一个排序算法，要求时间效率为O（n）

> 对公司所有员工的年龄排序，公司员工人数几万，可以使用辅助空间

```java
public static void ageSort(int[] ages) {

        if (ages == null) {
            return;
        }
        int[] temp = new int[100];
        for (int i = 0; i < ages.length; i++) {
            temp[ages[i]]++;
        }
        int index = 0;
        for (int i = 0; i < 100; i++) {
            for (int j = 0; j < temp[i]; j++) {
                ages[index++] = i;
            }
        }


    }
```

公司员工的年龄有一个范围，在上面的代码中，允许的范围是0-99岁。数组temp胜来统计每个年龄出现的次数，某个年龄出现了多少次，就在数组ages里设置几次该年龄，这就相当于组数组ages排序了。该方法用长度100的整数数组作为辅助空间换来了O（n）的时间效率。

## 旋转数组的最小数字

题目：把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转。输出旋转数组的最小元素。例如，数组{3,4,5,1,2}为{1，2，3，4，5}的一个旋转，该数组的最小值为1

```java
public class SpanArrayMin {
    public static int spanArrayMin(int[] array) {
        if (array == null || array.length == 0) {
            throw new RuntimeException("非法输入");
        }
        int low = 0;
        int high = array.length - 1;
        if (array[low] < array[high]) {
            return array[low];
        }
        while (low + 1 != high) {
            int mid = (low + high) / 2;
            if (array[mid] == array[low] && array[mid] == array[high]) {
                return inorder(array, low, high);
            }
            if (array[mid] >= array[low]) {
                low = mid;
            }
            if (array[mid] <= array[high]) {
                high = mid;
            }
        }
        return array[high];

    }

    private static int inorder(int[] array, int low, int high) {
        int res = array[low];
        for (int i = low; i < high; i++) {
            if (array[i] < res) {
                res = array[i];
            }
        }
        return res;
    }


    public static void main(String[] args) {
        int[] array1 = {3, 4, 5, 0, 2, 3};
        int[] array2 = {1, 2, 3, 4, 5, 6};
        int[] array3 = {1, 0, 1, 1, 1};
        int[] array4 = {1, 1, 1, 0, 1};
        int[] array5 = {1, 1, 1, 1, 1};

        System.out.println(spanArrayMin(array1));
        System.out.println(spanArrayMin(array2));
        System.out.println(spanArrayMin(array3));
        System.out.println(spanArrayMin(array4));
        System.out.println(spanArrayMin(array5));


    }
}

```

### 测试用例

* 功能测试（输入的数组是升序排序数组的一个旋转，数组中有重复数字或者没有重复数字）
* 边界值测试（输入的数组是一个升序排序的数组，只包含一个数字的数组）
* 特殊输入测试（输入null指针）



## 冒泡排序

```java
public class BubbleSort {

    public static void bubbleSort(int[] array) {
        boolean flag = true;
        while (flag) {
            flag = false;
            for (int i = 1; i < array.length ; i++) {
                if (array[i] < array[i - 1]) {
                    swap(array, i, i - 1);
                    flag = true;
                }
            }
        }
    }

    private static void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }


    public static void main(String[] args) {
        int[] array = {49, 38, 65, 97, 76, 13, 27, 49};
        bubbleSort(array);

        System.out.println(Arrays.toString(array));

    }

}
```

# 面试题12

## 矩阵中的路径

题目：请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左，右，上，下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3x4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用下划线标出）。但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的每一行第二个格子之后，路径不能再次进入这个格子。

| a    | `b`  | t    | g    |
| ---- | ---- | ---- | ---- |
| c    | `f`  | `c`  | s    |
| j    | d    | `e`  | h    |

## 回溯算法

这有点像图的深度优先搜索。除了矩阵边界，每个点都可以在4个方向上选择任意一个前进。当某一条路径失败后，需要回溯到上一次选择处，选择另一个方向再尝试。如果该处的方向都被尝试过了，继续回溯到上次选择处...每一次选择都会来到一个新的格子，在这个格子处又有若干个方向可选择，就这样不断前进、回溯、再前进，直到找到一条满足要求的路径为止；如果所有点都作为起点搜索一遍后还是没有找到满足要求的路径，说明在这个矩阵中不存在该条路径。

上面的描述，使用递归比较好理解。还有一点需要注意，由于路径上访问过的点不能进入第二次，所以需要一个`boolean[] marked`标记那些**当前路径上被访问过的点**。

```java
public class HasPath {
    public static boolean hasPath(char[] matrix, char[] str, int rows, int cols) {
        if (matrix == null || str == null || str.length == 0 || rows < 1 || cols < 1) {
            return false;
        }
        boolean[] marked = new boolean[rows * cols];

        int pathLength = 0;
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                if (hasPathCore(matrix, rows, cols, row, col, str, pathLength,marked)) {
                    return true;
                }
            }
        }
        return false;
    }

    private static boolean hasPathCore(char[] matrix, int rows, int cols, int row, int col, char[] str, int pathLength, boolean[] marked) {
        int index = row * cols + col;
        if (row < 0 || col < 0 || marked[index] || matrix[index] != str[pathLength] || col > cols || row > rows) {
            return false;
        }
        //递归深度到了字符串尾
        if (pathLength == str.length - 1) {
            return true;
        }
        marked[index] = true;

        if (hasPathCore(matrix, rows, cols, row-1, col, str, pathLength+1, marked) ||
                hasPathCore(matrix, rows, cols, row+1, col, str, pathLength+1, marked) ||
                hasPathCore(matrix, rows, cols, row, col-1, str, pathLength+1, marked) ||
                hasPathCore(matrix, rows, cols, row, col+1, str, pathLength+1, marked)) {
            return true;
        }


        // 对于搜索失败需要回溯的路径上的点，则要重新标记为“未访问”，方便另辟蹊径时能访问到
        marked[index] = false;
        return false;
    }

    public static void main(String[] args){
        String m = "abtgcfcsjdeh";
        char[] matrix = m.toCharArray();
        char[] path1 = {'b', 'f','c','f', 'f', 'e'};
        char[] path2 = {'a','b','f','d'};
        boolean b1 = hasPath(matrix, path1, 3, 4);
        boolean b2 = hasPath(matrix, path2, 3, 4);
        System.out.println(b1 + "\n" +  b2);

    }


}
```

递归方法` hasPathTo`中有个参数len，它表示当前递归的深度。第一次调用传入0，之后在其基础上的每一次递归调用len都会加1，递归的深度也反映了当前路径上有几个字符匹配相等了。能**绕过**下面的if判断，说明当前字符匹配相等了。接下来判断递归深度是否到达字符串末尾，比如字符串`abc`，第一次调用hasPath（传入len为0，递归深度为0）绕过了第一个if，说明字符a已经匹配相等，再判断`len == str.length - 1`不通过；再次递归，此时深度为1，绕过了第一个if，说明字符b已经匹配相等，再判断`len == str.length - 1`不通过；再次递归，此时深度为2，绕过了第一个if，说明字符c已经匹配相等，所有字符匹配相等，因此再次判断`len == str.length - 1`通过，应该返回true.

还有注意：四个方向搜索不是同时发生的，当某一个方向搜索失败后，会退回来进行下一个方向的搜索，回溯法就体现在此。

当搜索失败时，别忘了`marked[index] = false;`，将搜索失败路径上点重新标记为“未访问”，以便回溯后选择其他方向继续前进时能再次访问到这些点。

### 测试用例

* 功能测试（在多行多列的矩阵中存在或者不存在路径）
* 边界值测试（矩阵只有一行或者只有一列：矩阵和路径中的所有字母都是相同的）
* 特殊输入测试（输入null）

# 面试题13

## 机器人的运动范围

题目：地上有一个m行n列的方格。一个机器人从坐标（0，0）的格子开始移动，它每次可以向左 右、上、下、移动一格，但不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格（35,37）,因为3+5+3+7=18。但它不能进入方格（35，38），因为3+5+3+8=19.请问庐机器人能够到达多少个格子？

## 回溯算法

此题和*面试题12——矩阵中的路径*有相似之处，依然是回溯法。每来到一个新的且满足条件的格子时，计数加1。除矩形的边界外，任意一个方格都有四个方向可以选择，选择任一方向后来到新的格子又可以选择四个方向，但是一个到达过的格子不能进入两次，因为这将导致对同一个格子的重复计数。也就是说，**一个格子一旦满足条件进入后，就被永久标记为“访问过”，一个满足条件的格子只能使计数值加1。**这是和面试题12有区别的地方（那个例子中是搜索路径，失败路径上的点要重新标记为“未访问”，因为另开辟的新路径需要探索这些点）。

这道题用通俗的话来讲就是：m行n列的所有方格中，有多少个满足**行坐标和列坐标的数位之和小于等于门限值k**的格子？

代码和面试题12长得有点像，但是两个问题是有明显区别的！

```java
public class MovingCount {
    public static int movingCount(int threshold, int rows, int cols) {
        if (threshold < 0 || rows <= 0 || cols <= 0) {
            return 0;
        }
        boolean marked[] = new boolean[rows * cols];
        return movingCountCore(threshold, rows, cols, 0, 0, marked);
    }

    private static int movingCountCore(int threshold, int rows, int cols, int row, int col, boolean[] marked) {
        int count = 0;
        if (check(threshold, rows, cols, row, col, marked)) {
            marked[row * cols + col] = true;
            count = 1 + movingCountCore(threshold, rows, cols, row - 1, col, marked)
                    + movingCountCore(threshold, rows, cols, row + 1, col, marked)
                    + movingCountCore(threshold, rows, cols, row, col - 1, marked)
                    + movingCountCore(threshold, rows, cols, row, col + 1, marked);
        }


        return count;
    }

    private static boolean check(int threshold, int rows, int cols, int row, int col, boolean[] marked) {
        if (row >= 0 && row< rows && col>=0 && col<cols && getDigitSum(row) + getDigitSum(col) <= threshold
        && !marked[row*cols+col]) {
            return true;
        }
        return false;
    }

    private static int getDigitSum(int num) {
        int sum = 0;
        while (num > 0) {
            sum += num % 10;
            num = num / 10;
        }
        return sum;
    }


    public static void main(String[] args) {
        int count = movingCount(2, 3, 4);
        System.out.println(count);
    }

}

```

## 测试用例

* 功能测试（方格为多行多列；k为正数）
* 边界值测试（方格只有一行或者一列;k等于0）
* 特殊输入测试（k 为负数）

# 面试题14

## 剪绳子

题目：给你一根长度为n的绳子，请把绳子剪成m段（m，n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1]....,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2，3，3的三段，此时得到的最在乘积是18.

### 动态规划算法

首先定义函数f（n）为把长度为n的绳子剪成若干段后各段长度乘积的最大值。在剪第一刀的时候，我们有n-1种可能的选择，也就是剪出来的第一段绳子的可能长度分别不1，2，...，n-1。因此，f(n) = max(f(i)xf(n-i))，其中0<i<n

```java
public static int maxProductAfterCutting_solution(int length) {
        if (length < 2) {
            return 0;
        }
        if (length == 2) {
            return 1;
        }
        if (length == 3) {
            return 2;
        }
        int[] products = new int[length + 1];

        //先存绳子的长度
        products[0] = 0;
        products[1] = 1;
        products[2] = 2;
        products[3] = 3;
        int max;
        for (int i = 4; i <= length; i++) {
            max = 0;
            for (int j = 1; j <= i/2; j++) {
                int product = products[j] * products[i - j];
                if (max < product) {
                    max = product;
                }
                products[i] = max;
            }
        }
        max = products[length];
        return max;

    }
```

### 贪婪算法

如果我们按照如下的策略来剪绳子，则得到的各段绳子的长度的乘积将最大：当n>=5时，我们尽可能多地剪长度为3的绳子；当剩下的绳子长度为4时，把绳子剪成两段长度为2的绳子。为种思路对应的参考代码如下：

```java
public static int maxProductAfterCutting_solution2(int length) {
        if (length < 2) {
            return 0;
        }
        if (length == 2) {
            return 1;
        }
        if (length == 3) {
            return 2;
        }

        //尽可能多地剪去长度为3的绳子段
        int timesOf3 = length / 3;
        //当绳子最后剩下的长度为4的时候，不能再剪长度为3的绳子段
        //此时更好的方法是殷绳子剪成长度为2的两段，因为2x2>1x3
        if (length - timesOf3 * 3 == 1) {
            timesOf3--;
        }
        int timesOf2 = (length - timesOf3 * 3) / 2;
        return (int)(Math.pow(3, timesOf3) * Math.pow(2, timesOf2));


    }
```

### 测试用例

* 功能测试（绳子的初始长度大于5）
* 边界值测试（绳子的初始长度分别为0，1，2，3，4）

# 面试题15

## 二进制中1的个数

题目:请实现一个函数，输入一个整数，输出该数二进制表示中1的个数，例如，把9表示成二进制是1001，有2位是1.因此，如果输入9，则该函数输出2.

> 容易想到的思路：该数的各位不断和1相与，然后将该数右移1位，直到所有位都比较过。

### 右移版本

**错误代码：**

```java
public static int numberOf1(int num) {
        int count = 0;
        while (num != 0) {
            if ((num & 1) == 1) {
                count++;
            }
            num = num >> 1;
        }
        return count;
    }
```

**注意！上面的代码是错误的！对于非负数来说没有问题，但是当传入负数的时候，由于`>>`是带符号的右移，对于负数来说高位会以1补位，n永远也不会等于9，因此会出现无限循环。**

在Java中右移分两种，一种是上面那样带符号的右移，用`>>`表示，如果数是正数高位以0补位，如果是负数高位以1补位；还有就是无符号的右移，用`>>>`表示，不论正负数，统统高位以0补位。因此只需改动将上述程序的`>>`改成`>>>`即可通过。

**正解代码**

```java
public static int numberOf1(int num) {
        int count = 0;
        while (num != 0) {
            if ((num & 1) == 1) {
                count++;
            }
            num = num >>> 1;
        }
        return count;
    }
```

### 左移版本

上面的第一个程序之所以会出现无限循环，是因为我们改变了被输入的数本身。换个角度，我们不改变输入的数，而是通过改变一个变量，那么不管是输入正负数都能得到正确答案。

所以用一个1和输入数的每一位相与，然后将这个1不断**左移**。

```java
public static int numberOf1Left(int num) {
        int count = 0;
        int flag = 1;
        while (flag != 0) {
            if ((num & flag) != 0) {
                count++;
            }
            flag = flag << 1;
        }
        return count;

    }
```

在这个解法中，循环的次数等于整数二进制的位数，32位的整数需要循环32次。下面介绍一种算法，整数中有几个1就只需要循环几次

### 给人 带来惊喜的解法

把一个整数减去1，再和原整数做与运算，会把该整数最右边的1变成0.那么一个整数的二进制表示中有多少个1，就可以进行多少次这样的操作。

```java
public static int numberOf1Best(int num) {
        int count = 0;
        while (num != 0) {
            num = (num - 1) & num;
            count++;
        }

        return count;
    }

```

## 测试用例

* 正数（包括边界值1，0x7FFFFFFF）
* 负数（包括边界值0x80000000，0xFFFFFFFF）
* 0

## 一个整数是否为2的整数次方

题目：用一条语句判断一个整数是不是2的整数次方。

> 一个整数如果是2的整数次方，那么它的二进制表示中有且只有一位是1，而其他所有位都是0.根据前面的分析，把这个整数减去1之后再和它自己做与运算，这个整数中唯一的1就会变成0

```java
 public static boolean powOf2(int n) {
        return ((n - 1) & n) == 0;
    }
```

## 改变m二进制位得到n

题目：输入两个整数m和n，计算需要改变m的二朝向表示中的多少位才能得到n。

> 比如10的二进制表示为1010，13的二进制表示为1101。需要改变1010中的3位才能得到1101。
>

我们可以分为两步解决这个问题：

​	第一步求这两个数的异或；

​	第二步统计异或结果中1的位数

```java
public static int changeM2N(int m, int n) {
        int a = m ^ n;
        int count = 0;
        while (a != 0) {
            count++;
            a = (a - 1) & a;
        }
        return count;
    }
```

# 面试题16

## 数值的整数次方

题目：实现了函数double Power(double base,int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。



### 连乘

```java
 public static double power(double base, int exponent) {
        if (base == 0) {
            return 0;
        }
        double result = 1.0;
        int positiveExponent = Math.abs(exponent);
        for (int i = 0; i < positiveExponent; i++) {
            result = result * base;
        }
        return exponent < 0 ? 1 / result : result;
    }
```

### 快速幂

我们要求
$$
a^n
$$
，分n为奇数和偶数两种情况，如下


$$
a^n = a ^{n /2}  \times  a ^{n /2} 
$$
 ，n为偶数


$$
a^n = a ^{(n-1) /2}  \times  a ^{(n-1) /2} \times a
$$
，n为奇数

假如要求
$$
2^{32}
$$
，按照上面连乘的思路，需要进行31次乘法；采用上面的公式，只需要5次：先求平方，然后求四次方、八次方、十六次方，最后三十二次方。将时间复杂度降到了O(lg n)。

```java
public static double power2(double base, int exponent) {
        if (base == 0) {
            return 0;
        }
        double result = 1.0;
        int positiveExponent = Math.abs(exponent);

        while (positiveExponent != 1) {

            //判断是否为奇数
            if ((positiveExponent & 1) == 1) {
                result = result * base;
            }
            base = base * base;

            //positiveExponent 除以2
            positiveExponent = positiveExponent >> 1;
        }

        return exponent < 0 ? 1 / (result * base) : result * base;

    }
```

# 面试题17

## 打印从1到最大的n位数

题目：输入数字n，按顺序打印出从1到最大的n位十进制数。比如输入3，则打印出1，2，3一直到最大的3位数999

> 这道题有陷阱，可能容易想到输入4就打印1~9999，输入5就打印1~99999...那我要是输入100呢？int型不能表示出来吧，甚至更大的值，即便是long型也不能表示出来。

这是一道**大数问题**，牵涉到大数问题我们可以将其转化为字符串表示。因为字符串任意长度都行。

### 在字符串上模拟数字的加法

本题要求按照递增顺序打印出1~最大的n位十进制数，所以字符串的长度定也应该是n。首先将长度为n的字符串中每个字符初始化为0，比如n = 3时，字符串一开始为`000`。我们要做的只有两步：

- 模拟数字那样在字符串上做加法；
- 将字符串表达的数字打印出来，为了可读性需要忽略不必要的0；

从第一步得知，需要经常改变字符串，我们知道**Java中字符串是不可变对象，意味着每次对String的改变都会产生一个新的字符串对象**，这将浪费大量空间，所以在下面的程序中将使用`StringBuilder`。根据这些描述，可写出如下代码。

```java
package edu.xjtu.soto.chap17;

/**
 * 从1至m位 所有的数
 */

public class PrintToMaxOfNDigits {

    /**
     * 使用字符串模拟数字加法
     * @param n
     */
    public static void printToMaxOfNDigits1(int n) {
        if (n <= 0) {
            return;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            sb.append(0);
        }

        while (stillIncrease(sb)) {
            print(sb);
        }
    }

    /**
     * 判断sb是否还能增加1
     * @param sb
     * @return
     */
    private static boolean stillIncrease(StringBuilder sb) {
        int len = sb.length();
        int token = 0;
        for (int i = len - 1; i >= 0; i--) {
            int sum = sb.charAt(i) - '0' + token;
            if (i == len - 1) {
                sum++;
            }
            if (sum == 10) {

                if (i == 0) {
                    return false;
                } else {
                    //进位
                    sb.setCharAt(i, '0');
                    token = 1;

                }
            } else {
                //没有进位
                sb.setCharAt(i, (char) (sum + '0'));

                //自增完退出
                return true;
            }

        }
        return false;
    }

    private static void print(StringBuilder sb) {
        int start = sb.length();
        for (int i = 0; i < sb.length(); i++) {
            if (sb.charAt(i) != '0') {
                start = i;
                break;

            }
        }
        if (start < sb.length()) {
            System.out.println(sb.substring(start));
        }

    }

    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("010");
//        print(sb);

        printToMaxOfNDigits1(1);

    }
}

```

`stillIncrease`方法会对当前数进行加1操作，该方法返回一个布尔值，用来表明当前数还能不能继续增长。比如n = 3时，当前数为999已经是最大了，不能再增长，此时如果调用该方法就应该返回false，因此跳出循环，不会打印1000（虽然不打印，但实际在该方法中字符串已经从"999"被更新到了“1000”）。当当前数可以继续增长时，会先对个位上的数进行自增操作，如果此时得到的sum < 10，说明不需向前进位，直接退出并返回；如果sum == 10，说明需要向前进位，需要将当前位设置为0，然后进位设置为1，在下一循环中，需要加到在前一位上，继续判断这一位上需不需要进位......直到某位上sum  < 10循环才终止。

`print`方法，为了符合人的阅读习惯，比如"002"其实就是数字2，应保证**从左到右第一个不为0的数前面的0不会被打印出来。**

### 递归求解

```java
public class printFrom1ToMaxOfNDigitRecur {
    public static void printFrom1ToMaxOfNDigitRecur(int n) {
        if (n <= 0) {
            return;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            sb.append("0");
        }

        printRecur(sb, n, -1);


    }

    private static void printRecur(StringBuilder sb, int len, int index) {
        if (index == len - 1) {
            print(sb);
            return;
        }
        for (int i = 0; i < 10; i++) {
            sb.setCharAt(index+1, (char) (i + '0'));
            printRecur(sb,len,index+1);
        }


    }

    private static void print(StringBuilder sb) {
        int start = sb.length();
        for (int i = 0; i < sb.length(); i++) {
            if (sb.charAt(i) != '0') {
                start = i;
                break;

            }
        }
        if (start < sb.length()) {
            System.out.println(sb.substring(start));
        }

    }

    public static void main(String[] args) {
        printFrom1ToMaxOfNDigitRecur(2);
    }

}

```

# 面试题18

## 删除指定链表节点

题目：在O（1）时间内删除链表节点

给定单向链表的头指针和一个节点指针，定义一个函数在O（1）时间内删除该节点，链表节点与函数的定义如下：

```java
public class ListNode {
    public int val;
    public ListNode next = null;

    public ListNode(int val) {
        this.val = val;
    }
}
```

```java
public class DeleteNode {
    public static void deleteNode(ListNode head, ListNode p) {
        if (head == null || p == null) {
            return;
        }

        //要删除的节点不是尾节点
        if (p.next != null) {
            ListNode pNext = p.next;
            p.val = pNext.val;
            p.next = pNext.next;
            pNext = null;
            //是尾节点也是头节点
        } else if (head == p) {
            head = head.next;
            //仅是尾节点
        } else {
            ListNode first = head;
            while (first.next != p) {
                first = first.next;
            }
            first.next = null;
        }
    }

}
```

## 删除链表中的重复结点

题目：在一个排序的链表中，如何删除重复的节点？

> 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

注意重复的结点不保留：并不是将重复结点删除到只剩一个，而是重复结点的全部会被删除。所以链表1->2->3->3->4->4->5 处理后不是1->2->3->4->5。



```java
public class DeleteDuplication {
    /**
     * 重新添加头结点
     * @param head
     */
    public static void deleteDuplicate(ListNode head) {
        if (head == null || head.next == null) {
            return;
        }
        ListNode first = new ListNode(head.val - 1);
        first.next = head;
        ListNode pre = first;
        ListNode p = pre.next;
        while (p != null && p.next != null) {
            if (p.val != p.next.val) {
                p = p.next;
                pre = pre.next;
            } else {
                p = p.next;
                while (p.val == p.next.val) {
                    p = p.next;
                }
                p = p.next;
                pre.next = p;
            }

        }

        head = first.next;
        first = null;
    }

    public static void main(String[] args) {
        ListNode node11 = new ListNode(1);
        ListNode node21 = new ListNode(2);
        ListNode node31 = new ListNode(3);
        ListNode node32 = new ListNode(3);
        ListNode node33 = new ListNode(3);
        ListNode node41 = new ListNode(4);
        ListNode node42 = new ListNode(4);
        ListNode node51 = new ListNode(5);
        node11.next = node21;
        node21.next = node31;
        node31.next = node32;
        node32.next = node33;
        node33.next = node41;
        node41.next = node42;
        node42.next = node51;

        deleteDuplicate(node11);

        while (node11 != null) {
            System.out.println(node11.val);
            node11 = node11.next;
        }
    }


}
```

# 面试题19

## 正则表达式匹配

题目：请实现一个函数用来匹配包含`.`和`*`的正则表达式。模式中的字符`.`表示任意一个字符，而`*`表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`aaa`与模式`a.a`和`ab*ac*a`匹配，但与`aa.a`和`ab*a`不匹配。

注意`.`就是一个字符，而`*`前面必须有一个字符（可以是`.`）才有意义，所以可以将`x*`看成一个整体，其中x表示任意字符。`*`在匹配时有两种情况，第二个字符是`*`或者不是。

- 第二个字符不是`*`号。这种情况很简单，第二个字符要么是`.`要么是一个具体的字符。此时如果第一个字符匹配成功了，只需将模式和文本指针都前进一位。比如`ab`和`ac`以及`ab`和`.b`，分别对应着字符一样、模式字符为`.`的情况。**第一个字符匹配失败了，直接就可以得出结论——匹配失败。**

- 第二个字符是`*`。有几种情况：

  1、`*`匹配0次，比如`a*ab`和`ab`匹配，此时需要将模式指针前移2位，文本指针保持不动；

  2、`*`匹配了1次，比如`a*b`和`ab`匹配，此时需要将模式指针前移2位，文本指针前移1位；

  3、`*`匹配了多次，比如`a*b`和`aaab`匹配，此时需要将模式保持不动，文本指针前移1位；

  **同样的比较第二个字符的前提是第一个字符已经匹配成功。**



我们将长度任意的模式和文本分解成一个或者两个字符，可以用递归地方法写出如下程序：

```java
public class Match {
    public static boolean match(char[] str, char[] pattern) {
        if (str == null || pattern == null) {
            return false;
        }
        return matchCore(str, pattern, 0, 0);
    }

    private static boolean matchCore(char[] str, char[] pattern, int s, int p) {
        if (s == str.length && p == pattern.length) {
            return true;
        }

        //模式串到头 ，文本串未到头 匹配失败
        if (p == pattern.length && s < str.length) {
            return false;
        }

        // 两种情况，1、模式和文本都没有到结尾
        // 2、或者文本到了结尾而模式还没有到结尾，此时肯定会调用else分支
        // 第二个字符是*
        if (p < pattern.length - 1 && pattern[p + 1] == '*') {
            if (s < str.length && str[s] == pattern[p] || (pattern[p] == '.' && s < str.length)) {
                return matchCore(str, pattern, s, p + 2) ||
                        matchCore(str, pattern, s + 1, p + 2) ||
                        matchCore(str, pattern, s + 1, p);
            }else {
                return matchCore(str, pattern, s, p + 2);
            }
        }

        //第二个字符不是*
        if ((s < str.length && str[s] == pattern[p]) || (pattern[p] == '.' && s < str.length)) {
            return matchCore(str, pattern, s + 1, p + 1);
        }
        return false;
    }

    public static void main(String[] args) {
        boolean b = match("aaa".toCharArray(), "a.a".toCharArray());
        System.out.println(b);
    }
}

```

如果匹配的最后两个指针都到达末尾，说明完全匹配上了，返回true。如果模式指针先于文本到达末尾，一定是匹配失败了，举个例子，`a*bcd`和`abcdefg`。

接下来的两个if分别是第二个字符为`*`和不为`*`的情况。

- 第二个字符为`*`。在保证第一个字符已经匹配上的情况下，`*`可以有三种匹配方式，有任一种方式匹配成功即可，所以return语句中用的是`||`。
- 第二个字符不为`*`。当第一个字符已经匹配上的情况下，直接将模式和文本的指针前移一位，即比较下一个字符。

如果不是上面的情况（比如第一个字符就匹配失败了），返回false。

# 面试题20

## 表示数值的字符串

题目：请实现一个函数用来判断字符串是否表示数值（包括整数和小数），例如，字符串“+100”，“5e2”，“-123”，“3.1416”及“-1E-16”都表示数值，但"12e"，"1a3.14"，“1.2.3”，“+-5”及“12e+5.4”都不是。

### 正则表达式

```java
/**
     * 正则表达式匹配
     * @param str
     * @return
     */
    public static boolean isNumeric(String str) {
        if (str == null) {
            return false;
        }
        return str.matches("[\\+\\-]?[0-9]*(\\.[0-9]+)?([Ee][\\+\\-]?[0-9]+)?");
    }
```

# 面试题21

## 调整数组顺序使奇数位于偶数前面

题目：输入一全整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

### 使用两个指针

```java
public static void recorderOddEven(int[] array) {
        if (array == null || array.length == 0) {
            return;
        }
        int start = 0;
        int end = array.length - 1;

        while (start < end) {
            while ((array[start] & 1) == 1) {
                start++;
            }
            while ((array[end] & 1) == 0) {
                end--;
            }
            if (start < end) {
                int temp = array[start];
                array[start] = array[end];
                array[end] = temp;
            }
        }
    }
```

# 面试题22

## 链表中倒数第k个节点

题目：输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

>  例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。链表节点定义如下：

```java

public class ListNode {
    public int val;
    public ListNode next = null;

    public ListNode(int val) {
        this.val = val;
    }
}

```

```java
public class FindKthToTail {
    public static ListNode findKthToTail(ListNode head, int k) {
        if (head == null || k == 0) {
            return null;
        }
        ListNode p = head;
        ListNode q = head;
        for (int i = 0; i < k-1; i++) {
            if (p.next != null) {
                p = p.next;
            } else {
                return null;
            }
        }
        while (p.next != null) {
            q = q.next;
            p = p.next;
        }
        return q;
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1);
        ListNode head2 = new ListNode(2);
        ListNode head3 = new ListNode(3);
        ListNode head4 = new ListNode(4);
        ListNode head5 = new ListNode(5);
        head.next = head2;
        head2.next = head3;
        head3.next = head4;
        head4.next = head5;
        System.out.println(findKthToTail(head, 6).val);
    }
}

```

## 测试用例

* 功能测试（第k个节点在链表的中间；第k个节点是链表的头节点；第k个节曙中链表的尾节点）
* 特殊输入测试（链表头节点为null指针；链表的节点总数少于k；k等于0）

# 面试题23

## 链表中环入口节点

题目：如果一个链表中包含环，如何找出环的入口节点？

### 双指针法

```java
public static ListNode meetingNode1(ListNode head) {
        if (head == null || head.next == null || head.next.next == null) {
            return null;
        }

        ListNode p = head.next;
        ListNode q = head.next.next;
        while (p != q && q != null) {
            p = p.next;
            q = q.next.next;
        }
        if (q == null) {
            return null;
        } else {
            q = head;
        }
        while (p != q) {
            p = p.next;
            q = q.next;
        }
        return p;
    }
```

1. p走一步，q走2步，直至二者相遇
2. 将q重置为头结点
3. p q 各走一步，直至相遇，该点即为环

## 利用Set中元素的唯一性

我们知道Set中不能存放相同的元素。当在遍历链表的过程中，不断将当前结点放入Set中，当**第一次add失败**时，说明这个结点之前已经访问过了，这个结点刚好就是环的入口结点。

```java
 /**
     * 利用set的无序性
     * @param head
     * @return
     */
    public static ListNode meetingNode2(ListNode head) {
        if (head == null || head.next == null || head.next.next == null) {
            return null;
        }
        ListNode p = head;
        Set<ListNode> set = new HashSet<>();
        while (set.add(p)) {
            p = p.next;
        }
        return p;

    }

public static void main(String[] args) {
        ListNode l1 = new ListNode(1);
        ListNode l2 = new ListNode(2);
        ListNode l3 = new ListNode(3);
        ListNode l4 = new ListNode(4);
        ListNode l5 = new ListNode(5);
        ListNode l6 = new ListNode(6);
        l1.next = l2;
        l2.next = l3;
        l3.next = l4;
        l4.next = l5;
        l5.next = l6;
        l6.next = l3;

        ListNode result = meetingNode1(l2);
        System.out.println(result == null ? -1 : result.val);


        ListNode res = meetingNode2(l1);
        System.out.println(res == null ? -1 : res.val);

    }
```



## 测试用例

* 功能测试（链表中包含或者不包含环；链表中有多个或者只有一个节点）
* 特殊输入测试（链表头节点为null）

# 面试题24

## 反转链表

题目:定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。链表节点定义如下：

```java
public class ListNode {
    public int val;
    public ListNode next = null;

    public ListNode(int val) {
        this.val = val;
    }
}
```

### 使用头插法

```java
public static ListNode reverseList1(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode first = new ListNode(-1);
        first.next = head;
        ListNode p = head.next;
        head.next = null;
        ListNode q = p;
        while (p != null) {
            q = p.next;
            p.next = first.next;
            first.next = p;
            p = q;
        }
        head = first.next;
        first = null;
        return head;

    }
```

### 使用递归

```java
 /**
     * 使用递归
     * @param head
     * @return
     */
    public static ListNode reverseList2(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }

        //递归后的头节点
        ListNode headNode  = reverseList2(head.next);
        head.next.next = head;
        head.next = null;
        return headNode;

    }
```

只可意会，不可言传

## 测试用例

* 功能测试（输入的链表含有多个节点：链表中只有一个节点）。
* 特殊输入测试（链表头节点为null）

# 面试题25

## 合并两个排序的链表

题目：输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍是递增排序的。

> 例如，输入链表1：1--3--5--7      链表2:2--4--6--8       
>
> 合并之：  链表3 ：   1--2--3--4--5--6--7--8

链表节点定义如下

```java
public class ListNode {
    public int val;
    public ListNode next = null;

    public ListNode(int val) {
        this.val = val;
    }
}
```

### 非递归版本

```java
public static ListNode merge(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        ListNode head= null;
        ListNode p = l1;
        ListNode q = l2;

        if (l1.val <= l2.val) {
            head = l1;
            p = l1.next;
        } else {
            head = l2;
            q = l2.next;
        }

        ListNode cur = head;
        while (p != null || q != null) {
            if (p == null) {
                cur.next = q;
                break;
            } else if (q == null) {
                cur.next = p;
                break;
            } else if (p.val < q.val) {
                cur.next = p;
                cur = cur.next;
                p = p.next;
            } else {
                cur.next = q;
                cur = cur.next;
                q = q.next;
            }
        }
        return head;

    }
```

## 测试用例

* 功能测试（输入的两个链表有多个节点；节点的值互不相同或者存在值相等的多个节点）。
* 特殊输入测试（两个链表的一个或者两个头节点为null；两个链表中只有一个节点）。



# 面试题26

## 树的子结构

题目：输入两棵二叉树A和B，判断B是不是A的子结构。二叉树节点的定义如下：

```java
public class TreeNode {

    public int val = 0;
    public TreeNode left = null;
    public TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }
}
```

二叉树这种递归的数据结构。想到用递归的方法解决是很自然的。

首先我们要在二树A中找到和树B根结点值一样的结点R，结点R可能有多个，因为树A中的结点值可能不止一个与树B的根结点值相同。对于每个结点R，其子树都有可能是树A的子结构，因此只要还未在树A中找到一个子结构，就要继续遍历树A判断其他R结点。

对于某一个R结点，我们要求根结点R的左子结点、右子结点和树B根结点的左子结点、右子结点分别相等（值相等），而且R的子结点的左右子结点也要和B的子结点值相等......直到和树B的叶子结点也相等才说树A包含树B。这是个递归的过程。

对于这两个步骤，可写出如下


```java
public class HasSubtree {
    public static boolean hasSubtree(TreeNode root, TreeNode subRoor) {
        boolean result = false;
        if (root != null && subRoor != null) {
            if (root.val == subRoor.val) {
                result = doesTree1HasTree2(root, subRoor);
            }
            if (!result) {
                result = hasSubtree(root.left, subRoor);
            }
            if (!result) {
                result = hasSubtree(root.right, subRoor);
            }
        }
        return result;

    }

    private static boolean doesTree1HasTree2(TreeNode t1, TreeNode t2) {
        if (t2 == null) {
            return true;
        }
        if (t1 == null) {
            return false;
        }
        if (t1.val != t2.val) {
            return false;
        }
        return doesTree1HasTree2(t1.left, t2.left)
                && doesTree1HasTree2(t1.right, t2.right);

    }

    public static void main(String[] args) {
        TreeNode root1 = new TreeNode(1);
        TreeNode root2 = new TreeNode(2);
        TreeNode root3 = new TreeNode(3);
        TreeNode root4 = new TreeNode(4);
        TreeNode root5 = new TreeNode(5);

        root1.left = root2;
        root1.right = root3;
        root2.left = root4;
        root2.right = root5;

        boolean b = hasSubtree(root2, root5);

        System.out.println(b);
    }

}

```

## 测试用例

* 功能测试（树A和树B都是普通的二叉树；树B是或者不是树A的子结构）
* 特殊输入测试（两棵二叉树的一个或者两全根节点为null；二叉树的所有节点都没有左子树或者右子树）



# 面试题27

## 二叉树的镜像

题目：请完成一个函数，输入一棵二叉树，该函数输出它的镜像。

二叉树节点的定义如下：

```java
public class TreeNode {

    public int val = 0;
    public TreeNode left = null;
    public TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }
}
```



### 递归(前序遍历)

```java
/**
     * 递归求镜像（先序遍历）
     * @param root
     * @return
     */
    public static TreeNode treeMirror1(TreeNode root) {
        if (root == null || (root.left == null && root.right == null)) {
            return root;
        }
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        if (root.left != null) {
            treeMirror1(root.left);
        }
        if (root.right != null) {
            treeMirror1(root.right);
        }
        return root;
    }
```

### 层次遍历

```java
    /**
     * 非递归，层次遍历
     * @param root
     */
    public static void treeMirror2(TreeNode root) {
        if (root == null) {
            return;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node.left != null || node.right != null) {
                TreeNode temp = node.left;
                node.left = node.right;
                node.right = temp;
            }
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }


    }
```

### 非递归（前序遍历）

```java
/**
     * 非递归前序遍历
     * @param root
     */
    public static void treeMirror3(TreeNode root) {
        LinkedList<TreeNode> stack = new LinkedList<>();

        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                if (root.left != null || root.right != null) {
                    TreeNode temp = root.left;
                    root.left = root.right;
                    root.right = temp;
                }
                root = root.left;
            }

            if (!stack.isEmpty()) {
                root = stack.pop();
                root = root.right;
            }
        }

    }

```

**前序遍历是深度优先搜索，所以用到了栈；而层序遍历是广度优先搜索，用到队列。**

前序遍历是只要某结点还有左子结点，就不断压入栈中，直到没有左子结点时弹栈，接着将根结点指向右子树重复上述过程。

层序遍历很简单，先将根结点入列，然后弹出根结点将根结点子结点入列，不断重复上述过程直到队列为空。

## 测试用例

* 功能测试（普通二叉树；对叉树的所有节点都没有左子树或者右子树；只有一个节点的二叉树）。
* 特殊国俾测试（二叉树的根节点为null）

# 面试题28

## 对称的二叉树

题目：请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

### 递归版本

```java
/**
     * 使用递归
     * @param root
     * @return
     */
    public static boolean isSymmetrical(TreeNode root) {


        return isSymmetrical(root, root);
    }

    private static boolean isSymmetrical(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) {
            return true;
        }
        if (root1 == null || root2 == null) {
            return false;
        }
        if (root1.val != root2.val) {
            return false;
        }
        return isSymmetrical(root1.left, root2.right)
                && isSymmetrical(root1.right, root2.left);

    }
```

如果遇到两棵子树都为空则返回true，这句代码隐含了**“空树也是对称的”**这样的信息。否则，只有一子树为空另一不为空，显然不是对称的；如果两个子树都不为空，比较它俩根结点的值，不相同肯定不是对称的。之后递归地对树的子树进行上述判断，直到检查到叶子结点，如果都满足就返回true。

### 非递归版本

思路和上面一样，非递归实现需要用到两个队列。**队列A专门存入左子树，队列B专门存入右子树。**

入列时，**将左子树的左子结点和右子树的右子结点分别存入队列A和队列B，紧接着还要将左子树的右子结点和右子树的左子结点分别存入队列A和队列B。**

出列时，两个队列同时出列一个元素，根据存入的顺序，这两个出列元素就是左子树的左子结点和右子树的右子结点，或者左子树的右子结点和右子树的左子结点。之后对这两个元素进行比较，比较操作和上面递归版本的一样。



```java
/**
     * 使用队列
     * @param root
     * @return
     */
    public static boolean isSymmetrical2(TreeNode root) {
        if (root == null) {
            return true;
        }
        Queue<TreeNode> queue1 = new LinkedList<>();
        Queue<TreeNode> queue2 = new LinkedList<>();

        queue1.offer(root.left);
        queue2.offer(root.right);

        TreeNode p = null;
        TreeNode q = null;

        while (!queue1.isEmpty() && !queue2.isEmpty()) {
            p = queue1.poll();
            q = queue2.poll();

            if (p == null && q == null) {
                continue;
            } else if (p == null || q == null) {
                return false;
            }
            if (p.val != q.val) {
                return false;
            }

            queue1.offer(p.left);
            queue2.offer(q.right);

            queue1.offer(p.right);
            queue2.offer(q.left);

        }
        return true;


    }
```

## 测试用例：

* 功能测试（对称的二叉树：因结构而不对称的二 叉树；经霜你对称但节点的值不对称的二叉树）。
* 特殊输入测试（二叉树和根节点为null；只有一个节点的二叉树；所有节点的值都相同的二叉树），



# 面试题29

## 顺时针打印矩阵

题目：输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

> 例如，输入如下矩阵：



| 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- |
| 5    | 6    | 7    | 8    |
| 9    | 10   | 11   | 12   |
| 13   | 14   | 15   | 16   |

> 依次打印出数字：1，2，3，4，8，12，16，15，14，13，9，5，6，7，11，10

假设这个矩阵的行数是rows，列数是columns。打印第一圈的左上角的坐标是（0，0），第二圈的左上角的坐标是（1，1）。以此类推，我们注意到，左上角的坐标中行标和列标总是相同的，于是可以在矩阵中选取左上角为（start,start）的一圈作为我们分析的目标。

对于一个5x5的矩阵而言，最后一圈只有一个数字，对应的坐标为（2，2）。我们发现5>2x2。对于一个6x6的矩阵而言，最后一圈有4个数字，其左上角的坐标仍然为（2，2）.我们发现6>2x2依然顾立，于是可以得出，让循环继续的条件是columns> startX x2 并且rows>startYx3。所以我们可以用如下的循环来打针矩阵：

```java
public void printMatrixClockwisely(int[] numbers, int columns, int rows) {
        if (numbers == null || columns <= 0 || rows <= 0) {
            return;
        }
        int start = 0;
        while (columns > start * 2 && rows > start * 2) {
            PrintMatrixInCircle(numbers, columns, rows, start);
            ++start;
        }
    }
```

我们把打印一圈分4步：

1. 从左到右打印一行
2. 从上到下打印一列
3. 从右到左打印一行
4. 从下到上打印一列

每一步我们根据起始坐标和终止坐标用一个循环就能打印出一行或者一列。

不过需要注意的是，最后一圈有可能退化成只有一行。只有一列，甚至只有一个数字，因此打针这样的一圈就不再需要4步。

因此，我们需要仔细分析打印每一步的前提条件，第一步总是需要的，因为打针一圈至少有一步。如果只有一行，那就不用第二步了。也就是需要第二步的前提条件是终止行号大于起始行号，需要第三步打印的前提条件是圈内至少有两行两列，也就是说除了要求终止行号大于起始行号，还要求终止列号大于起始列号。同理，需要打针第四步的前提条件是至少有三行两列，因此要求终止行号比起始行号至少大2，同时终止列号大于起始列号。



```java
public class PrintMatrix {

    private ArrayList<Integer> list = new ArrayList<>();
    public void printMatrixClockwisely(int[][] numbers, int columns, int rows) {
        if (numbers == null || columns <= 0 || rows <= 0) {
            return;
        }
        int start = 0;

        while (columns > start * 2 && rows > start * 2) {
            PrintMatrixInCircle(numbers, columns, rows, start);
            ++start;
        }
    }

    private void PrintMatrixInCircle(int[][] numbers, int columns, int rows, int start) {
        int endX = columns - 1 - start;
        int endY = rows - 1 - start;

        //从左到右打印一行
        for (int i = start; i <= endX; i++) {
            int number = numbers[start][i];
            list.add(number);
        }

        //从上到下打印一列
        if (start < endY) {
            for (int i = start+1; i <= endY ; i++) {
                int number = numbers[i][endX];
                list.add(number);
            }
        }

        //从历到左打印一行
        if (start < endX && start < endY) {
            for (int i = endX - 1; i >= start; i--) {
                int number = numbers[endY][i];
                list.add(number);
            }
        }

        //从下到上打印一列
        if (start < endX && start < endY - 1) {
            for (int i = endY - 1; i >= start + 1; i--) {
                int number = numbers[i][start];
                list.add(number);
            }
        }


    }

    public static void main(String[] args) {
        PrintMatrix pm = new PrintMatrix();
        int[][] numbers1 = {{1, 2, 3, 4}, {5, 6, 7, 8}, {9, 10, 11, 12}, {13, 14, 15, 16}};
        int[][] numbers2 = {{1, 2, 3, 4, 5}, {6, 7, 8, 9, 10}, {11, 12, 13, 14, 15}, {16, 17, 18, 19, 20}, {21, 22, 23, 24, 25}};
        pm.printMatrixClockwisely(numbers1, 4, 4);
        System.out.println(pm.list);
        pm.list.clear();
        pm.printMatrixClockwisely(numbers2, 5, 5);
        System.out.println(pm.list);
    }

}

```

## 测试用例

数组中有多行多列；数组中只有一行；数组中只有一列；数组中只有一行一列

# 面试题30

## 包含min函数的栈

> ```
> 题目：定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的min函数，在该栈中，调用min，push,及pop的时间复杂度都是O(1)。
> ```

有些很容易想到的方法：比如每次想获得栈中的最小元素，将栈中所有元素复制到另一个数据结构中（比如List），然后对这个列表排序可以很简单地得到最小值。但时间复杂度肯定就不是O(1)了。

或者设置一个全局变量min，每次push都和当前最小值比较，如果更小就更新min，否则min不变。但是这种方法有个问题：要是pop出栈的元素正好就是这个min呢，那新的min是多少？我们很难得知，所以另辟蹊径。考虑到要求我们用O(1)的时间复杂度。可以**考虑用空间换时间**，试试使用辅助空间。

**定义一个栈stackMin，专门用于存放当前最小值。**

- 存放数据的stack存入当前元素，如果即将要存入的元素比当前最小元素还小，stackMin存入这个新的最小元素；否则，stackMin将当前最小元素再次存入。
- stack出栈时，stackMin也出栈。

反正就是入栈时，两个栈都有元素入栈；出栈时，两个栈都弹出一个元素。这两个栈总是同步进出栈的

```java
public class StackIncludeFuncMin {
    private LinkedList<Integer> stack = new LinkedList<>();

    //辅助栈，用于存储当前最小值
    private LinkedList<Integer> stackMin = new LinkedList<>();

    public void push(int node) {
        stack.push(node);
        if (stackMin.isEmpty() || node < stackMin.peek()) {
            stackMin.push(node);
        } else {
            stackMin.push(stackMin.peek());
        }
    }

    public void pop() {
        if (stack.isEmpty()) {
            throw new RuntimeException("stake is empty!");
        }
        stack.pop();
        stackMin.pop();
    }


    public int top() {
        if (stack.isEmpty()) {
            throw new RuntimeException("stack is empty!");
        }
        return stack.peek();
    }

    public int min() {
        if (stackMin.isEmpty()) {
            throw new RuntimeException("stack is empty!");
        }
        return stackMin.peek();
    }

    public static void main(String[] args) {
        StackIncludeFuncMin stackIncludeFuncMin = new StackIncludeFuncMin();

        stackIncludeFuncMin.push(5);
        System.out.println(stackIncludeFuncMin.min());
        stackIncludeFuncMin.push(4);
        System.out.println(stackIncludeFuncMin.min());
        stackIncludeFuncMin.push(6);
        System.out.println(stackIncludeFuncMin.min());
        stackIncludeFuncMin.push(3);
        System.out.println(stackIncludeFuncMin.min());

        stackIncludeFuncMin.pop();
        System.out.println(stackIncludeFuncMin.min());
        stackIncludeFuncMin.pop();
        System.out.println(stackIncludeFuncMin.min());
        stackIncludeFuncMin.pop();
        System.out.println(stackIncludeFuncMin.min());
    }
}

```

我们来模拟一下：

| 出入栈 | stack   | stackMin   | min  |
| ------ | ------- | ---------- | ---- |
| 压入5  | 5       | 5          | 5    |
| 压入4  | 5, 4    | 5, 4       | 4    |
| 压入6  | 5, 4, 6 | 5, 4, 4    | 4    |
| 压入3  | 5,4,6,3 | 5, 4, 4, 3 | 3    |
| 弹出   | 5,4,6   | 5, 4, 4    | 4    |
| 弹出   | 5, 4    | 5, 4       | 4    |
| 弹出   | 5       | 5          | 5    |

举点例子模拟能很好地帮助我们理解。

## 测试用例

* 新压入栈的数字比之前的最小值大
* 新压入栈的数字比之前最小值小
* 弹出栈的数字不是最小元素
* 弹出栈的数字是最小元素

# 面试题31

## 栈的压入，弹出序列

> ```
> 题目：输入两个整数序列。第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。
> 例如，序列{1,2,3,4,5,}是某栈的压栈序列，序列{4,5,3,2,1}是该压栈序列对应的一个弹出序列，但{4，3，5，1，2}就不可能是该压栈序列的弹出序列。
> ```

题目只是给出了两个序列，并没有栈，所有我们需要自己定义**一个辅助栈**帮助我们模拟入栈出栈过程。

需要用到一个指针表示在出栈序列中的当前出栈元素。每次入栈一个元素，之后立刻和出栈序列中当前出栈元素对比，若相同就弹出刚压入的元素、同时当前弹出元素指针前移。之后还要继续比较，如果栈顶还和当前弹出元素相同则需要接着弹出。否则压入入栈序列中的下一个元素。

正常情况下，如果出栈顺序正确，当入栈序列中的元素都被压入后，辅助栈能按照出栈序列全部弹出。**如果当元素都被压入后，辅助栈没能弹出所有元素（不为空），说明出栈顺序是错误的。**

```java
/**
     *
     * @param push 压入序列
     * @param pop  弹出序列
     * @return
     */
    public boolean isPopOrder(int[] push, int[] pop) {
        if (push == null || pop == null || push.length == 0 || pop.length == 0) {
            return false;
        }

        int position = 0;
        LinkedList<Integer> stackAur = new LinkedList<>();
        for (int i = 0; i < push.length; i++) {
            stackAur.push(push[i]);
            while (!stackAur.isEmpty() && stackAur.peek() == pop[position]) {
                stackAur.pop();
                position++;
            }
        }
        return stackAur.isEmpty();

    }

    public  static void main(String[] args) {
        int[] push = {1, 2, 3, 4, 5};
        int[] pop1 = {4, 5, 3, 2, 1};
        int[] pop2 = {4, 3, 5, 1, 2};
        StackPopOrder stackPopOrder = new StackPopOrder();
        boolean b1 = stackPopOrder.isPopOrder(push, pop1);
        boolean b2 = stackPopOrder.isPopOrder(push, pop2);


        System.out.println(b1);
        System.out.println(b2);
    }
```

假如入栈顺序是`[1, 2, 3, 4, 5]`举个出栈顺序正确的例子`[4, 5, 3, 2, 1]`：

| 操作  | 辅助栈     | 出栈 |
| ----- | ---------- | ---- |
| 压入1 | 1          |      |
| 压入2 | 1, 2       |      |
| 压入3 | 1, 2, 3    |      |
| 压入4 | 1, 2 ,3 ,4 |      |
| 弹出4 | 1,2 ,3     | 4    |
| 压入5 | 1,2, 3 ,5  |      |
| 弹出5 | 1, 2, 3    | 5    |
| 弹出3 | 1,2        | 3    |
| 弹出2 | 1          | 2    |
| 弹出1 |            | 1    |

再举个错误的出栈顺序的例子`[4,3,5,1,2]`

| 操作  | 辅助栈     | 出栈 |
| ----- | ---------- | ---- |
| 压入1 | 1          |      |
| 压入2 | 1, 2       |      |
| 压入3 | 1, 2, 3    |      |
| 压入4 | 1, 2 ,3 ,4 |      |
| 弹出4 | 1,2 ,3     | 4    |
| 弹出3 | 1, 2       | 3    |
| 压入5 | 1, 2, 5    |      |
| 弹出5 | 1, 2       | 5    |
| 弹出2 |            | 2    |
| 弹出1 |            | 1    |

最后只剩两个元素1, 2时，由于2在栈顶1在栈底，不能先弹出1在弹出2，所以这个出栈顺序是错误的。

## 测试用例

* 功能测试(输入的两个数组含有多个数字或者只有一个数字；第二个数组是或者不是第一个数组表示的压入序列对应的栈和弹出序列)。
* 特殊输入测试（输入两个null）



# 面试题32

## 从上到下打印二叉树

题目一：不分行从上到下打印二叉树

> ```
> 从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。
> ```
>

```java
private ArrayList<Integer> printFromTopToBottom(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        TreeNode node = null;
        while (!queue.isEmpty()) {
            node = queue.poll();
            result.add(node.val);
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
        return result;
    }
```

先将根结点入列，出列并打印然后按照从左到右的顺序依次将该结点的子结点入列....不断重复这个过程直到队列为空。

## 测试用例

* 功能测试（完全二叉树；所有节点只有左子树的二叉树；所有节点只有右子树的二叉树）。
* 特殊输入测试（二叉树根节点为null；只有一个节点的二叉树）。

## 分行从上到下打印二叉树

> ```
> 从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
> ```

和上面类似，现在要求每打印完树的一层需要换行。核心代码其实和上面一样。只是为了确定在何时需要换行操作，**需要用两个变量记录当前层还没有被打印的结点数、下层总结点数。每打印完一行后需要换行，接下来要打印下一层了，所以用下层总结点数更新当前层未被打印的结点数，同时下层总结点数重置为0，准备进行下一层的计数。**

```java
private void printTreeEveryLayer(TreeNode root) {
        if (root == null) {
            return;
        }
        //下一层节点数
        int nextLevel = 0;

        //本层未打印节点数，第一层还未打印。故初始化为1
        int toBePrint = 1;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        TreeNode node = null;
        while (!queue.isEmpty()) {
            node = queue.poll();
            System.out.print(node.val + " ");
            toBePrint--;

            if (node.left != null) {
                queue.offer(node.left);
                nextLevel++;
            }
            if (node.right != null) {
                queue.offer(node.right);
                nextLevel++;
            }

            if (toBePrint == 0) {
                System.out.println();
                toBePrint = nextLevel;
                nextLevel = 0;
            }
        }
    }
```

## 之字形打印二叉树

> ```
> 请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打针，第三行再按照从左到右的顺序打印，其他行以此类推，
> ```

举个例子，下面的二叉树，打印顺序是1 3 2 4 5 6 7

```
   	   1
  	 /   \
    2      3
   / \    / \
  4   5  6   7
```

先搞清楚要求：根结点先被打印，然后从右往左打印第二行，接着从左往右打印第三行...以此类推，**总之偶数层就从右往左打印，奇数行就从左到右打印**。依然需要某种数据结构来存放结点，栈可以满足我们的打印顺序：当前层为奇数层时，按照**从左到右**的顺序将下层结点（偶数层）压入栈中，出栈的时候就是从右往左打印偶数层了；当前层是偶数层时，按照**从右到左**的顺序将下层结点（奇数层）压入栈中，由于此时先出栈的是偶数层最右边的结点，所以可以保证下层最右边的结点被压到了栈底，而最左边的结点位于栈顶，出栈的时候就是从左往右打印奇数层了...如此反复交替。

为了达到上述的交替效果，需要用到**两个栈，一个栈stackOdd存奇数层的结点，另一个栈stackEven存偶数层的结点。**

- 奇数层，其下层的结点按左到右的顺序入栈
- 偶数层，其下层的结点按右到左的顺序入栈

奇偶层顺序是固定的，即根结点是奇数层，则奇偶顺序是“奇偶奇偶....“

stackOdd存放的是某一奇数层的全部结点，**stackOdd不为空说明当前层是奇数层，全部弹出后为空，该处理下一层了;因此当stackOdd为空时当前层必然是偶数层**，stackOdd就这样不断为空，不为空...交替，正好反映了当前层是奇数层还是偶数层，进而采取不同的结点存入顺序即可。

```java
public static void printTreeZ(TreeNode root) {
        if (root == null) {
            return;
        }

        LinkedList<TreeNode> stackOdd = new LinkedList<>();
        LinkedList<TreeNode> stackEven = new LinkedList<>();

        TreeNode node = null;

        stackOdd.push(root);

        while (!stackOdd.isEmpty() || !stackEven.isEmpty()) {
            if (!stackOdd.isEmpty()) {
                while (!stackOdd.isEmpty()) {
                    node = stackOdd.pop();
                    System.out.print(node.val + " ");
                    if (node.left != null) {
                        stackEven.push(node.left);
                    }
                    if (node.right != null) {
                        stackEven.push(node.right);
                    }

                }
            }else {

                while (!stackEven.isEmpty()) {
                    node = stackEven.pop();
                    System.out.print(node.val + " ");
                    if (node.right != null) {
                        stackOdd.push(node.right);
                    }
                    if (node.left != null) {
                        stackOdd.push(node.left);
                    }
                }
            }
            System.out.println();
        }

    }
```

# 面试题33

## 二叉搜索树的后序遍历序列

>```
>题目：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回true，否则返回false，假设输入的数组的任意两个数字都互不相同。例如。输入数组（5，7，6，9，11，10，8}，则返回true，因为这个整数良列是图4.9二叉搜索树的后序遍历结果。如果输入的数组是{7，4，6，5}，由于没有哪棵二叉搜索树的后序遍历结果是这个序列，因此返回false。
>```

```
        8
   6         10
 5    7    9     11
```

注意是二叉搜索（查找）树，特点是父结点的左子树都比父结点小，父结点的右子树都比父结点大。因此其后序遍历的序列有一定的规律：

- 序列最后一位必然是树的根结点；
- 序列前部分是根结点的左子树，后部分是根结点的右子树；具体来说：将序列各个元素和和序列最后一位的根结点比较，序列前部分都小于根结点的值，这部分子序列是左子树；序列后部分的值都大于根结点，这部分子序列是右子树；



```java
public class VerifySquenceOfBST {
    public boolean verify(int[] post) {
        if (post == null || post.length <= 0) {
            return false;
        }
        return isSearchBST(post, 0, post.length - 1);

    }

    private boolean isSearchBST(int[] post, int begin, int end) {
        if (begin >= end) {
            return true;
        }
        int root = post[end];

        //在二叉搜索树中左子树节点的值小于根节点的值
        int i = 0;
        for (; i < end; i++) {
            if (post[i] > root) {
                break;
            }
        }
        //在二叉搜索树中右子树节点的值大于根节点的值
        int j = i;
        for (; j < end; j++) {
            if (post[j] < root) {
                return false;
            }
        }

        //判断左子树是不是二叉搜索树
        boolean left = true;
        if (i > 0) {
            left = isSearchBST(post, begin,i-1);
        }

        //判断左子树是不是二叉搜索树
        boolean right = true;
        if (j < end - 1) {
            right = isSearchBST(post, i, j);
        }

        return left && right;

    }

    public static void main(String[] args) {
        int[] post1 = {5, 7, 6, 9, 11, 10, 8};
        int[] post2 = {7, 4, 6, 5};
        int[] post3 = {7};

        VerifySquenceOfBST vb = new VerifySquenceOfBST();
        System.out.println(vb.verify(post1));
        System.out.println(vb.verify(post2));
        System.out.println(vb.verify(post3));
    }
}

```

# 面试题34

## 二叉树路径与指定值相等

题目：输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径，从树的根节点开始往下一直到叶节点所经过的节点形成一条路径，

二叉树节点的定义如下：

```java
public class TreeNode {

    public int val = 0;
    public TreeNode left = null;
    public TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
```

当用前序遍历的方式访问到某一节点时，我们把该节点添加到路径上，并累加该节点的值。如果该节点为叶节点，并且路径中节点值的和刚好等于输入的整数，则当前路径符合要求，我们把粌打印出来。如果当前节点不是叶节点，则继续访问它的子节点。当前节点访问结束后，递归函数将自动回到它的父节点。因此，我们在函数退出之前要在路径上删除当前节点并减去当前节点的值，以确保返回父节点时路径刚好是从根节点到父节点。我们不难看出保存路径的数据结构实际上是一个栈，因为路径要屯递归调用状态一致，而递归调用的本质就是一个压栈和出栈的过程。

```java
public class FindPath {
    public ArrayList<ArrayList<Integer>> findPath(TreeNode root, int target) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        ArrayList<Integer> path = new ArrayList<>();
        preOrder(root, target, path, result);
        return result;

    }

    private void preOrder(TreeNode root, int curValue, ArrayList<Integer> path, ArrayList<ArrayList<Integer>> result) {
        if (root == null) {
            return;
        }

        //模拟节点进栈
        path.add(root.val);
        curValue -= root.val;

        // 只有在叶子结点处才判断是否和目标值相同，若相同加入列表中
        if (root.left == null && root.right == null) {
            if (curValue == 0) {
                result.add(new ArrayList<>(path));
            }
        }
        preOrder(root.left, curValue, path, result);
        preOrder(root.right, curValue, path, result);

        path.remove(path.size() - 1);
        curValue += root.val;

    }

    public static void main(String[] args) {
        FindPath fp = new FindPath();
        TreeNode root = new TreeNode(10);
        TreeNode root1 = new TreeNode(5);
        TreeNode root2 = new TreeNode(12);
        TreeNode root3 = new TreeNode(4);
        TreeNode root4 = new TreeNode(7);


        root.left = root1;
        root.right = root2;
        root1.left = root3;
        root1.right = root4;


        ArrayList<ArrayList<Integer>> res = fp.findPath(root, 22);

        System.out.println(res);
    }
}

```

## 测试用例

* 功能测试（二叉树中有一条，或多条符合要求的路径；二叉树中没有符合要求的路径）。
* 特殊输入测试（指向二叉树根节点的指针为null）。

# 面试题35

## 复杂链表的复制

> ```
> 题目：请实现函数ComplexListNode Clone(ComplexListNode head),复制一个复杂链表，在复杂链表中，每个节点除了有一个next指针指向下一全邛是，还有一个sibling指针指向链表中的任意节点或者null
> ```

节点定义如下：

```java
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;
    RandomListNode(int label) {
        this.label = label;
    }
}
```

**注意不可将链表头结点引用返回，所以需要自己new一个结点出来。**要完成复杂链表的复制，第一步要完成普通链表的复制，普通链表的复制可以用一个栈复制每一个结点，之后逐个弹出并连接起来。对于本题有两种策略：

第一种是，在复制普通链表时，将原链表结点和复制链表的结点成对存入`HashMap<N, N'>`中，建立一对一的映射。当进行随机结点复制时，遍历原链表，如果某结点的随机结点不为空，那么根据HashMap能以O(1)的时间找到对应的复制链表结点中，若原始链表的结点M指向随机结点S，那么复制链表的M'也要指向结点S'，这种方法时间复杂度为O(N)，但空间复杂度为O(N)。

更好的方法是，插入、连接、拆分三步法。

- 插入：在原始链表的每个结点后插入一个值和它一样的新结点；则有`oriNode.next == cloneNode`这样的关系；
- 连接随机结点：遍历插入新结点后的链表，在访问原始链表中的那些结点时，判断其是否有随机结点，有的话`cloneNode.random = oriNode.random.next`这里`oriNode.random.next`表示原始链表随机结点的下一个结点，其实就是复制链表的随机结点。
- 拆分原始链表和复制链表：将奇数结点相连就是原始链表，将偶数结点相连就是我们想要的复制链表。返回复制链表的头结点即可。

```java
public class CloneNodes {
    public RandomListNode cloneNodes(RandomListNode head) {

        if (head == null) {
            return null;
        }
        RandomListNode pNode = head;

        //复制结点
        while (pNode != null) {
            RandomListNode pClone = new RandomListNode(pNode.label+"'");
            pClone.next = pNode.next;
            pNode.next = pClone;
            pNode = pClone.next;
        }

        //设置每个复制结点的random
        pNode = head;
        while (pNode != null) {
            if (pNode.random != null) {
                pNode.next.random = pNode.random.next;
            }
            pNode = pNode.next.next;
        }

        //拆分链表
        pNode = head;
        RandomListNode cloneHead = pNode.next;
        RandomListNode pcloneNode = cloneHead;

        while (pNode != null) {
            pNode.next = pNode.next.next;

            if (pcloneNode.next != null) {
                pcloneNode.next = pcloneNode.next.next;
            }
            pNode = pNode.next;
            pcloneNode = pcloneNode.next;
        }

        return cloneHead;


    }

    public static void main(String[] args) {
        CloneNodes cn = new CloneNodes();
        RandomListNode r1 = new RandomListNode("A");
        RandomListNode r2 = new RandomListNode("B");
        RandomListNode r3 = new RandomListNode("C");
        RandomListNode r4 = new RandomListNode("D");
        RandomListNode r5 = new RandomListNode("E");

        r1.next = r2;
        r2.next = r3;
        r3.next = r4;
        r4.next = r5;

        RandomListNode head = cn.cloneNodes(r1);

        while (head != null) {
            System.out.println(head.label);
            head = head.next;
        }

    }
}

```

## 测试用例

* 功能测试（节点中的sibling指向节点自身：两个节点的sibling形成环状结构；链表中只有一个节点）
* 特殊输入测试（指向链表头节点的指针为null指针）

# 面试题36

## 二叉搜索树与双向链表转换

> ```
> 题目：输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。
> 比如，输入图中左边的二叉搜索树，则输出转换之后的排序双向链表。
> ```

```
        10
  6             14          ===>   4 <==> 6 <==> 8 <==> 10 <==>12 <==> 14 <==> 16
4    8       12      16
```



二叉树节点定义如下：

```java

public class TreeNode {
    public int val = 0;
    public TreeNode left = null;
    public TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
```

看到这道题我第一反映就是，二叉树的线索化，不过还是有些区别的，下面会讨论。按照二叉搜索树的特点，最左边的结点是值最小的，而题目要求得到排序的双向链表，所以基本确定下来**中序遍历**的方法。

二叉树的线索化：是针对每个叶子结点，为了将空指针利用起来，可以将叶子结点的左子结点指向其遍历顺序的前驱，右子结点指向遍历序列的后继。根据遍历顺序的不同，线索化也分为前序、中序、后序。二叉树的结点定义中需要加入布尔变量，用来标示每个结点的左右指针是否被线索化了（这些标志只可能在叶子结点为true）

```java
public class Convert {
    private TreeNode preNode;

    public TreeNode convert(TreeNode root) {

        if (root == null) {
            return null;
        }

        TreeNode pRoot = root;

        //得到双向链表
        convertNode(root);

        //向左找到双向链表的头结点
        while (root.left != null) {
            root = root.left;
        }
        return root;
    }

    //中序遍历改变指针
    private void convertNode(TreeNode root) {
        if (root == null) {
            return;
        }
        convertNode(root.left);

        root.left = preNode;

        if (preNode != null) {
            preNode.right = root;

        }
        preNode = root;

        convert(root.right);


    }

    public static void main(String[] args) {
        Convert convert = new Convert();

        TreeNode r1 = new TreeNode(10);
        TreeNode r2 = new TreeNode(6);
        TreeNode r3 = new TreeNode(14);
        TreeNode r4 = new TreeNode(4);
        TreeNode r5 = new TreeNode(8);
        TreeNode r6 = new TreeNode(12);
        TreeNode r7 = new TreeNode(16);

        r1.left = r2;
        r1.right = r3;
        r2.left = r4;
        r2.right = r5;
        r3.left = r6;
        r3.right = r7;

        TreeNode res = convert.convert(r1);

        while (res != null) {
            System.out.println(res.val);
            res = res.right;
        }
    }

}
```

最后因为需要返回双向链表的头结点，只需从二叉树的根结点开始，向左遍历，即可找到双向链表的头结点。

## 测试用例

* 功能测试（输入的二叉树是完全二叉树；所有节点都没有左/右子树的二叉树；只有一个节点的二叉树）。
* 特殊输入测试（批向二叉树根节点的指针为null）

---

# 面试题37

## 序列化二叉树

> ```
> 题目：请实现五肉个函数，分别用来序列化和反序列化二叉树。
> ```

刚开始想法太死板了，只记得中序和前序或者中序和后续两个序列才能决定一棵唯一的二叉树，于是分别进行了前序、中序遍历，前序和中序的序列用"|"分隔，之后再根据这个分隔符分成前序和中序序列，最后采用面试题7——重建二叉树的思路进行反序列化。思路是正确的但是太麻烦。

其实遇到空指针可以也用一个特殊的字符表示，比如“#”，**这样前序遍历序列就可以表示唯一的一棵二叉树了。**对于空指针也用一个字符表示，可称这样的序列为**扩展序列**。而二叉树的建立，必须先要建立根结点再建立左右子树（root为空怎么调用root.left是吧），所以必须前序建立二叉树，那么序列化时也应该用前序遍历，保证了根结点在序列前面。

不能使用中序遍历，因为**中序扩展序列**是一个无效的序列，比如

```
   A      B
  / \      \
 B   C  和  A  中序扩展序列都是 #B#A#C#
             \
              C
```

先来看序列化的代码，其实就是在前序遍历的基础上，如果遇到空指针就用“#”表示。

```java
public String serialize(TreeNode root) {
        if (root == null) {
            return null;
        }

        StringBuilder sb = new StringBuilder();

        preOrxder(root, sb);

        return sb.toString();

    }

    private void preOrxder(TreeNode root, StringBuilder sb) {

        if (root == null) {
            sb.append("# ");
            return;
        }
        sb.append(root.val).append(" ");

        preOrxder(root.left, sb);
        preOrxder(root.right, sb);
    }

```

再来看反序列化，通过前序遍历得到的字符串，重建二叉树。

```java
private TreeNode deSerialize(String sb) {
        if (sb == null || sb.length() == 0) {
            return null;
        }

        String[] seq = sb.split("\\s");

        return reconstructBST(seq);
        


    }

    private int index = -1;
    private TreeNode reconstructBST(String[] seq) {
        index++;
        if (seq.length == 0) {
            return null;
        }

        //注意这里的写法
        if (seq[index].equals("#")) {
            return null;
        }

        TreeNode root = new TreeNode(Integer.parseInt(seq[index]));

        root.left = reconstructBST(seq);
        root.right = reconstructBST(seq);


        return root;
    }
```

由于前序遍历时每存入一个结点值或者存入“#”后面都紧跟着一个空格。所以最后得到的序列时这样的格式`A B # # C # #`，可以根据空格将其分割成`[A, B, #, #, C, #, #]`这样就还原了各个结点的值，根据这些值重建二叉树。由于得到的是二叉树的前序序列，因此也要以前序重建二叉树，当遇到结点值是“#”时说明这是一个空指针，那么返回null给上层的父结点。如果不为“#”就递归地重建该结点的左右子树。注意这里使用了一个int型的index，用于表示当前结点在String[] seq中的索引，无需担心index在seq中会造成数组下标越界，因为最后一个结点的左右子树肯定是null，必然会终止递归。

```java
public static void main(String[] args) {
        TreeNode r1 = new TreeNode(1);
        TreeNode r2 = new TreeNode(2);
        TreeNode r3 = new TreeNode(3);
        TreeNode r4 = new TreeNode(4);
        TreeNode r5 = new TreeNode(5);
        TreeNode r6 = new TreeNode(6);
        r1.left = r2;
        r1.right = r3;

        r2.left = r4;
        r3.left = r5;
        r3.right = r6;

        SerializeBT sb = new SerializeBT();
        String str = sb.serialize(r1);
        System.out.println(str);


        TreeNode r = sb.deSerialize(str);
        System.out.println(r);

    }
```

## 测试用例

* 功能测试（输入的二叉树是完全二叉树；所有节点都没能左/右子树的二叉树；只有一个节点的二叉树；所有节点的值都相同的二叉树）。
* 特殊输入测试（批向二叉树根节点的指针为null）













---

# 面试题38

## 字符串的排列

题目：输入一个字符串，打印出该字符串中字符的所有排列。

例如：输入字符串abc，则打印出由字符a、b、c所能排列出来的甩有字符串abc、acb、bac、bca、cab和cba

```java
public class Permutation {


    public static ArrayList<String> permutation(String str) {
        ArrayList<String> result = new ArrayList<>();
        if (str == null || str.length() == 0) {
            return result;
        }
        permutationCore(str.toCharArray(), 0, result);
        Collections.sort(result);
        return result;

    }

    private static void permutationCore(char[] chars,int begin, ArrayList<String> result) {
        if (begin == chars.length - 1) {
            String s = String.valueOf(chars);
            if (!result.contains(s)) {
                result.add(s);
                return;
            }
        }
        for (int i = begin; i < chars.length; i++) {
            swap(chars, begin, i);
            permutationCore(chars, begin + 1, result);
            //交换后再交换回来
            swap(chars, i, begin);
        }

    }

    private static void swap(char[] chars, int i, int j) {
        char temp = chars[i];
        chars[i] = chars[j];
        chars[j] = temp;
    }

    public static void main(String[] args) {
        ArrayList<String> result = permutation("abc");

        for (String s : result
        ) {
            System.out.println(s);

        }
    }
}

```

### 测试用例

* 功能测试（输入的字符串中有一个或者多个字符）。	
* 特殊输入测试（输入的字符串的内容为空或者null）

## 字符串的组合

如果要求字符的所有组合呢？比如abc，所有组合情况是`[a, b, c, ab, ac, bc, abc]`，包含选择1个、2个、3个字符进行组合的情况，即
$$
\sum{C_3^1 + C_3^2 + C_3^ 3}
$$
。这可以用一个for循环完成。所以关键是如何求得在n个字符里选取m个字符总共的情况数，即如何求C(n, m)

n个字符里选m个字符，有两种情况：

- 第一个字符在组合中，则需要从剩下的n-1个字符中再选m-1个字符；
- 第一个字符不在组合中，则需要从剩下的n-1个字符中选择m个字符。

上面表达的意思用数学公式表示就是
$$
C_n^m = C_{n-1}^{m-1} + C_{n-1}^m
$$


全排列的过程：

- 选择第一个字符
- 获得第一个字符固定下来之后的所有的全排列
  - 选择第二个字符
  - 获得第一+ 二个字符固定下来之后的所有的全排列

从这个过程可见，这是一个递归的过程。

```java
public class Permutation2 {
    public static ArrayList<String> permutation2(String str) {
        ArrayList<String> result = new ArrayList<>();
        if (str == null || str.length() == 0) {
            return result;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 1; i <= str.length(); i++) {
            permutationCore(str, i,sb, result);
        }

        return result;
    }

    private static void permutationCore(String str, int num, StringBuilder sb ,ArrayList<String> list) {
        if (num == 0) {
            if (!list.contains(sb.toString())) {
                list.add(sb.toString());
                return;
            }
        }
        if (str.length() == 0) {
            return;
        }
        // 公式C(n, m) = C(n-1, m-1)+ C(n-1, m)
        // 第一个字符是组合中的第一个字符，在剩下的n-1个字符中选m-1个字符
        sb.append(str.charAt(0));  //选中第一个字符
        permutationCore(str.substring(1), num - 1, sb, list);


        // 第一个字符不是组合中的第一个字符，在剩下的n-1个字符中选m个字符
        sb.deleteCharAt(sb.length() - 1);
        permutationCore(str.substring(1), num, sb, list);


    }

    public static void main(String[] args) {
        ArrayList list1 = permutation2("abc");
        System.out.println(list1);
        ArrayList list2 = permutation2("abcca");
        System.out.println(list2);
    }
}

```

## 正方体的八个顶点

题目：输入一个含有8个数字的数组，判断有没有可能把这8个数字分别放到正方体的8个顶点上，使得正方体上三组相对的面上的4个顶点的和都相等。

> 这相当于先得到a1、a2、a3、a4、a5、a6、a7和a8这8数字的所有排列，然后判断有没有某一个排列符合题目给定的条件，即a1+a2+a3+a4 = a5+a6+a7+a8,a1+a3+a5+a7 = a2+a4+a6+a8.并且 a1+ a2+a5+a6=a3+a4+a7+a8

```java
public class Permutation3 {
    public static ArrayList<int[]> possibilitiesOfCube(int[] array) {
        ArrayList<int[]> list = new ArrayList<>();
        if (array == null || array.length != 8) {
            return list;
        }
        ArrayList<int[]> result = new ArrayList<>();

        permutation(array, 0, list);
        if (list.size() > 0) {
            for (int[] a : list) {
                if (check(a)) {
                    result.add(a);
                }
            }

        }
        return result;
    }

    private static void permutation(int[] array, int begin, ArrayList<int[]> result) {
        if (begin == array.length - 1) {
            if (!has(result, array)) {
                result.add(Arrays.copyOf(array, array.length));
                return;
            }
        }

        for (int i = begin; i < array.length; i++) {
            swap(array, i, begin);
            permutation(array, begin + 1, result);
            swap(array, i, begin);
        }
    }

    private static void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }

    /**
     * list中的数组是包含array
     *
     * @param list
     * @param array
     * @return
     */
    private static boolean has(ArrayList<int[]> list, int[] array) {
        for (int i = 0; i < list.size(); i++) {
            if (equal(list.get(i), array)) {
                return true;
            }
        }
        return false;
    }

    /**
     * array1是否与array2值相等
     *
     * @param array1
     * @param array2
     * @return
     */
    private static boolean equal(int[] array1, int[] array2) {
        for (int i = 0; i < array1.length; i++) {
            if (array1[i] != array2[i]) {
                return false;
            }
        }
        return true;
    }

    /**
     * 判断各边和是满足要求
     *
     * @param array
     * @return
     */
    private static boolean check(int[] array) {

        if (array[0] + array[1] + array[2] + array[3] == array[4] + array[5] + array[6] + array[7]
                && array[0] + array[2] + array[4] + array[6] == array[1] + array[3] + array[5] + array[7]
                && array[0] + array[1] + array[4] + array[5] == array[2] + array[3] + array[6] + array[7]) {
            return true;
        }

        return false;
    }

    public static void main(String[] args) {
        int[] a = {1, 1, 2, 2, 3, 3, 4, 4};
        int[] A = {1,2,3,1,2,3,2,2};
        int[] B = {1,2,3,1,8,3,2,2};
        List<int[]> list = possibilitiesOfCube(B);
        System.out.println("有" + list.size() + "种可能");
        for (int[] arr : list) {
            System.out.println(Arrays.toString(arr));
        }
    }
}
```

---

# 面试题39

## 数组中出现次数超过一半的数字

> 题目：数组中有一个数字出现的次数超过数组长度的一半，请找出为个数字。
>
> 例如，输入一个长度为9的数组{1，2，3，2，2，2，5，4，2}。由于数字2在数组中出现了8次，超过数组长度的一半，因此输出2。

可以先将数组排序，然后统计数字出现的次数，先将第一个数字出现次数初始化为1，如果遇到同样的数字，就加1，遇到不一样的就重新初始化为1重新开始计数，直到某个数字计数值大于n / 2（n是数组的长度），终止循环，返回当前数字就是我们要的答案。

但是排序算法的时间复杂度为O(nlgn)，有没有更快的方法呢。

### 切分法，时间复杂度O(n)

**注意到排序之后，如果数组中存在某个数字超过数组长度的一半，那么数组中间的数字必然就是那个出现次数超过一半的数字。**这就将问题转化成了求数组的中位数，快速排序使用的切分算法可以方便地找出中位数，且时间复杂度为O(n)，找出中位数后还需要再遍历一边数组，检查该中位数是否出现次数超过数组长度的一半。总结一下，基于切分法有如下两个步骤：

- 切分法找出中位数
- 检查中位数



### 基于数组特点，时间复杂度O(n)

**数组中有一个数字出现次数超过了一半，说明这个数字的出现次数比其他所有数字的出现次数之和加起来还要多**。于是可以考虑下面的方法：使用两个变量，result表示当前数字，count表示当前数字的计数值。在遍历到一个新的数字时候，计数为1，将该数字赋值给result，之后遇到的数字如果和result相同，那么count加1,；遇到不一样的count减1，直到count等于0，此时需要重新初始化，即将新的数字赋值给result并令count为1......这样遍历完所有数字时，最后result表示的数值就**可能**是我们要找的值，注意是可能，因为输入数组可能本身就不满足里面有某个数字出现次数超过一半。因此和上面的找中位数方法一样，得到的result还需要进一步检验。

```java
public class MoreThanHalfNum {
    public int moreThanHalfNum(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        //计数器
        int count = 0;

        //表示当前数字
        int result = nums[0];

        for (int i = 0; i < nums.length; i++) {
            if (result == nums[i]) {
                count++;
            }else {
                count--;
                if (count == 0) {
                    count++;
                    result = nums[i];
                }
            }
        }

        count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (result == nums[i]) {
                count++;
            }
        }

        return count > nums.length / 2 ? result : 0;

    }

    public static void main(String[] args) {
        MoreThanHalfNum mthn = new MoreThanHalfNum();

        int[] nums = {1, 2, 3, 2, 2, 2, 5, 4, 2};


        System.out.println(mthn.moreThanHalfNum(nums));
    }
}

```

### 测试用例

* 功能测试（输入的数组中存在一个出现次数超过数组长度一半的数字；输入的数组中不契在一个出现次数超过数组长度一半的数字）。
* 特殊输入测试（输入的数组中只有一个数字‘；输入null）。











---

# --发糖果

有N个孩子站成一排，每个孩子有一个分值。给这些孩子派发糖果，需要满足如下需求：

1、每个孩子至少分到一个糖果

2、分值更高的孩子比他相邻位的孩子获得更多的糖果

求至少需要分发多少糖果？

## 贪心算法

思路：

1. 从左到右处理递增序列，分数更高的孩子获得更多的糖果
2. 从右到左处理递增序列（即从左到右的递减序列），分数更高的孩子获得更多的糖果，两个递增序列重复的部分，取最大值（因左右两侧都需满足）

```java
public class Candy {
    public static int candy(int[] scores, int[] candys) {
        if (scores == null || scores.length < 1 || candys.length < scores.length) {
            return 0;
        }
        if (scores.length == 1) {
            candys[0] = 1;
        }
        Arrays.fill(candys, 1);
        for (int i = 0; i < scores.length-1; i++) {
            if (scores[i] < scores[i + 1]) {
                candys[i+1]++;
            }
        }
        for (int i = scores.length - 1; i > 0; i--) {
            if (scores[i] < scores[i - 1] && candys[i] >= candys[i - 1]) {
                candys[i - 1] = candys[i] + 1;
            }
        }
        int sum = 0;
        for (int i = 0; i < candys.length; i++) {
            sum += candys[i];
        }
        return sum;
    }


    public static void main(String[] args) {
        int[] scores1 = {1,2,2};
        int[] scores2 = {1,0,2};
        int[] candys = new int[scores1.length];

        int sum1 = candy(scores1, candys);
//        int sum2 = candy(scores2, candys);

        System.out.println(Arrays.toString(candys));
        System.out.println(sum1);
    }
}

```

