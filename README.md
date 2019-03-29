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

## 递归

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

题目：给你一根长度为n的绳子，请把绳子剪成m段（m，n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1]....,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我信把它剪成长度分别为2，3，3的三段，此时得到的最在乘积是18.



# 面试题15

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

