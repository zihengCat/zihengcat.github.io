---
title: Java 数据结构与算法 - 链表
subtitle: Java Data Structure and Algorithm - Linked List
catalog: true
date: 2019-05-15
tags: ["Java", "Data Structure", "Algorithm", "LinkedList"]
---

# 链表（Linked List）数据结构概览

链表（Linked List）是线性表的一种（线性表包含顺序表与链表），通过指针将一系列位于不连续的内存空间中的元素连接起来，链表结构可以充分利用计算机内存空间，实现灵活的内存动态管理，但也失去了快速随机存取的优点，同时增大了内存开销（存储指针域）。

链表数据结构由一连串节点（Node）组成，每个节点包含数据域（Data Fields）和一或两个用来指向上一个/或下一个节点位置的指针域（Pointer Fields）。链表可以方便地插入或移除表中任意位置的节点，但是随机存取效率不高。链表有很多种不同的类型：单向链表，双向链表以及循环链表。

# 链表（Linked List）数据结构操作接口

我们将要使用 Java 语言手写一枚链表数据结构。首先，明确链表数据结构所具有的操作接口方法。

| 接口方法                            | 解释说明                         |
| ----------------------------------- | -------------------------------- |
| `void addFirst(E element)`          | 向链表头部添加一个新的元素。     |
| `void addLast(E element)`           | 向链表尾部添加一个新的元素。     |
| `E removeFirst()`                   | 移除链表头部第一个元素。         |
| `E removeLast()`                    | 移除链表尾部最后一个元素。       |
| `E getFirst()`                      | 返回链表头部第一个元素。         |
| `E getLast()`                       | 返回链表尾部最后一个元素。       |
| `boolean contains(E element)`       | 检查链表中是否包含指定元素。     |
| `E insert(int index, E element)`    | 向链表指定索引位置插入新元素。   |
| `E get(int index)`                  | 返回链表中指定索引的元素。       |
| `E set(int index, E element)`       | 为链表中指定索引的元素设新值。   |
| `E remove(int index)`               | 移除链表中指定索引的元素。       |
| `boolean remove(E element)`         | 移除链表中指定的元素。           |
| `int indexOf(E element)`            | 返回指定元素所在链表的索引，元素不存在则返回`-1`，若存在多个相同元素，则返回第一次出现的索引下标。 |
| `int size()`                        | 返回链表存储元素数量。           |
| `boolean isEmpty()`                 | 检查链表是否为空。               |
| `void clear()`                      | 清空链表。                       |
| `String toString()`                 | 返回链表的字符串形式。           |

> 表：链表（Linked List）数据结构接口方法表

# 单向链表（Single Linked List）

我们使用 Java 语言实现一枚简单的**单向链表**。顾名思义，单向链表只能做单向遍历（头节点 -> 尾节点），因为单向链表的节点（Node）只包含数据域和一个指针域（指向下一个节点）。

我们来定义出单链表中节点（Node）的数据结构，使用泛型类提高节点存储数据的灵活性。节点数据结构包含**构造方法**，两个私有变量：**数据域和指针域**，及其**对应的`Getter/Setter`公开方法**。

```java
class Node<E> {
    /**
     * 数据域
     */
    private E elem;
    /**
     * 指针域
     */
    private Node<E> next;
    /**
     * 构造方法
     */
    public Node(E element, Node<E> next) {
        this.elem = element;
        this.next = next;
    }
    /**
     * 无参构造方法
     */
    public Node() {
        this(null, null);
    }
    /* Setter & Getter */
    public void setElem(E element) {
        this.elem = element;
    }
    public E getElem() {
        return this.elem;
    }
    public void setNext(Node<E> next) {
        this.next = next;
    }
    public Node<E> getNext() {
        return this.next;
    }
}
```
> 代码清单：单链表节点（Node）

我们考虑单链表数据结构：**单链表包含一枚头节点（Head），头结点不存储数据，而是指向第一个实际存储数据的节点；尾节点可以被定义为指针域为`null`的最后一枚节点。**

单链表的构造方法即为单链表初始化：**构造一枚数据域和指针域均为空的头节点。**

```java
public class SingleLinkedList<E> {
    /**
     * 链表头节点
     */
    private Node<E> head;
    /**
     * 构造方法：创建空链表
     * @param void
     */
    public SingleLinkedList() {
        this.head = new Node<E>();
    }
}
```
> 代码清单：单链表（Single Linked List）结构

单链表`addFirst()`方法向链表头部添加一个新的元素，插入的新元素总是位于链表头部（头节点指向的节点），这种插入元素的方式称为**头插法**，通过以下3步，即可完成向链表头部插入元素。

1. 根据新元素构建一枚新节点

2. 将新节点指针域置为头节点指向的节点

3. 头节点指向新节点

```java
...
    /**
     * 向链表头部添加一个新的元素（头插法）。
     * @param element
     * @return void
     */
    public void addFirst(E element) {
        Node<E> node = new Node<E>(element, null);
        node.setNext(head.getNext());
        head.setNext(node);
    }
...
```
> 代码清单：单链表`addFirst()`头插法

单链表`addLast()`方法向链表尾部添加一个新的元素，插入的新元素总是位于链表尾部（指针域为空的尾节点），这种插入元素的方式称为**尾插法**，通过以下4步，即可完成向链表尾部插入元素。

1. 根据新元素构建一枚新节点

2. 将新节点指针域置空

3. 遍历链表找到尾节点（指针域为空的节点）

4. 尾节点指向新节点

```java
...
    /**
     * 向链表尾部添加一个新的元素（尾插法）。
     * @param element
     * @return void
     */
    public void addLast(E element) {
        Node<E> node = new Node<E>(element, null);
        Node<E> tail = head;
        while (tail.getNext() != null) {
            tail = tail.getNext();
        }
        tail.setNext(node);
    }
...
```
> 代码清单：单链表`addLast()`尾插法

理解了`addFirst()`与`addLast()`方法实现后，实现`getFirst()`与`getLast()`方法就非常简单了，返回头/尾节点数据域中存储的数据即可，但是需要考虑到**链表为空**的情况：直接抛出`NoSuchElementException`异常。

```java
...
    /**
     * 取得链表头部第一个元素，链表为空则抛出异常。
     * @param void
     * @return First element of {@code LinkedList}.
     * @throws NoSuchElementException if this {@code LinkedList} is empty.
     */
    public E getFirst() throws NoSuchElementException {
        if (head.getNext() == null) {
            throw new NoSuchElementException();
        }
        return head.getNext().getElem();
    }
    /**
     * 取得链表尾部最后一个元素，链表为空则抛出异常。
     * @param void
     * @return Last element of {@code LinkedList}.
     * @throws NoSuchElementException if this {@code LinkedList} is empty.
     */
    public E getLast() throws NoSuchElementException {
        Node<E> tail = head;
        while (tail.getNext() != null) {
            tail = tail.getNext();
        }
        if (tail == head) {
            throw new NoSuchElementException();
        }
        return tail.getElem();
    }
...
```
> 代码清单：单链表`getFirst()`与`getLast()`方法实现

上述实现代码段其实已经涉及到了`isEmpty()`与`size()`接口方法，现在我们来实现这两个方法。

- `size()`：遍历链表元素并计数，计算链表存储元素数量。

- `isEmpty()`：判断链表是否为空，可以借用`size()`方法（链表存储元素数量为`0`则表示链表为空），也可以直接判断头结点指针域是否为空。

```java
...
    /**
     * 计算链表存储元素数量。
     * @param void
     * @return Size of elements in {@code LinkedList}.
     */
    public int size() {
        int cnt = 0;
        for (Node<E> n = head; n.getNext() != null; n = n.getNext()) {
            ++cnt;
        }
        return cnt;
    }
    /**
     * 检查链表是否为空。
     * @param void
     * @return Boolean {@code true} or {@code false}.
     */
    public boolean isEmpty() {
        return head.getNext() == null ? true : false;
    }
    /**
     * 检查链表是否为空。
     * @param void
     * @return Boolean {@code true} or {@code false}.
     */
    public boolean _isEmpty() {
        return this.size() > 0 ? false : true;
    }
...
```
> 代码清单：单链表`isEmpty()`与`size()`方法实现

单链表`removeFirst()`方法返回并移除链表第一个元素，通过以下步骤完成。

1. 检查链表是否为空

2. 获取链表首元素节点

3. 头节点指向第二元素节点（首元素节点的下一个节点）

4. 返回首元素节点数据域

```java
...
    /**
     * 返回并移除链表头部第一个元素。
     * @param void
     * @return First element of this {@code Linked List}.
     * @throws NoSuchElementException
     */
    public E removeFirst() throws NoSuchElementException {
        if (this.isEmpty()) {
            throw new NoSuchElementException();
        }
        Node<E> first = head.getNext();
        head.setNext(
            first.getNext()
        );
        return first.getElem();
    }
...
```
> 代码清单：单链表`removeFirst()`方法实现

单链表`removeLast()`方法返回并移除链表最后一个元素，通过以下步骤完成。

1. 检查链表是否为空

2. 获取链表倒数第二元素节点（尾元素前一节点）

3. 获取链表尾元素节点

4. 将链表倒数第二元素节点指针域置空

5. 返回尾元素节点数据域

```java
...
    /**
     * 返回并移除链表尾部最后一个元素。
     * @param void
     * @return Last element of this {@code Linked List}.
     * @throws NoSuchElementException
     */
    public E removeLast() throws NoSuchElementException {
        if (this.isEmpty()) {
            throw new NoSuchElementException();
        }
        Node<E> prev = head;
        while (prev.getNext().getNext() != null) {
            prev = prev.getNext();
        }
        Node<E> last = prev.getNext();
        prev.setNext(null);
        return last.getElem();
    }
...
```
> 代码清单：单链表`removeLast()`方法实现

我们来考虑单链表的`contains(E e)`方法，检查链表中是否包含指定元素。我们使用`equals()`比较方法判断两个元素是否相等，因此，存入链表的数据类型必须实现`equals()`比较方法。

`contains(E e)`方法具体实现为：**遍历链表，比较每个元素，找到即返回`true`，找不到则返回`false`。**

```java
...
    /**
     * 检查链表中是否包含目标元素，
     * 元素相等使用 {@code o.equals(obj)} 判断。
     * @param element
     * @return Boolean
     */
    public boolean contains(E element) {
        for (Node<E> current = head.getNext();
            current != null;
            current = current.getNext()) {
            if (element.equals(current.getElem())) {
                return true;
            }
        }
        return false;
    }
...
```
> 代码清单：单链表`contains()`方法实现

链表使用链式存储结构，存储的数据在内存空间中不连续，不能做到像数组一般高效的直接随即访问。我们来实现`indexOf(E e)`方法，返回指定元素所在链表的索引，元素不存在则返回`-1`，若存在多个相同元素，则返回第一次出现的索引。**注意，我们将链表索引下标从`0`计起，与数组保持一致。`indexOf()`方法实现与`contains()`方法相似，加入一枚索引下标计数器即可。**

```java
...
    /**
     * 返回指定元素所在链表的索引。
     * @param element
     * @return The index of element in {@code LinkedList},
     *  return {@code -1} if element does not found.
     */
    public int indexOf(E element) {
        int index = 0;
        for (Node<E> current = head.getNext();
            current != null;
            current = current.getNext()) {
                if (element.equals(current.getElem())) {
                    return index;
                }
                ++index;
        }
        return -1;
    }
...
```
> 代码清单：单链表`indexOf()`方法实现

我们使用`get(int index)`方法获取链表中指定索引的元素，如果索引越界，抛出`IndexOutOfBoundsException`异常。

```java
...
    /**
     * 获取链表指定索引的元素。
     * @param index
     * @return element
     * @throws IndexOutOfBoundsException
     */
    public E get(int index) throws IndexOutOfBoundsException {
        if (index < 0 || index >= size()) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> n = head.getNext();
        while (index > 0) {
            n = n.getNext();
            --index;
        }
        return n.getElem();
    }
...
```
> 代码清单：单链表`get()`方法实现

我们使用`set(int index, E element)`方法为链表中指定索引位置的元素设新值，如果索引越界，抛出`IndexOutOfBoundsException`异常。

```java
...
    /**
     * 为链表指定索引位置的元素设新值。
     * @param index
     * @param element
     * @return Previous element in the index.
     * @throws IndexOutOfBoundsException
     */
    public E set(int index, E element) throws IndexOutOfBoundsException {
        if (index < 0 || index >= size()) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> n = head.getNext();
        while (index > 0) {
            n = n.getNext();
            --index;
        }
        E oldValue = n.getElem();
        n.setElem(element);
        return oldValue;
    }
...
```
> 代码清单：单链表`set()`方法实现

我们使用`remove(int index)`方法移除链表中指定索引下标位置的元素，具体步骤如下。如果索引下标越界，则抛出`IndexOutOfBoundsException`异常。

1. 找到链表中指定索引下标的待移除节点及其前驱、后继节点

2. 将指定索引下标节点的前后节点使用指针连接起来

3. 返回移除节点数据域

```java
...
    /**
     * 移除链表指定索引下标元素。
     * @param index
     * @return Removed element
     * @throws IndexOutOfBoundsException
     */
    public E remove(int index) throws IndexOutOfBoundsException {
        if (index < 0 || index >= size()) {
            throw new IndexOutOfBoundsException();
        }
        index -= 1;
        Node<E> prev = head;
        while (index >= 0) {
            prev = prev.getNext();
            --index;
        }
        Node<E> current = prev.getNext();
        Node<E> next = current.getNext();
        prev.setNext(next);
        return current.getElem();
    }
...
```
> 代码清单：单链表`remove(int index)`方法实现

移除元素`remove()`方法还有另一种形式：`boolean remove(E element)`，移除链表中的指定元素。我们可以使用`indexOf(E element)`配合`remove(int index)`实现，先获取指定元素在链表中的索引下标，再移除掉，操作成功返回`true`，如果不存在目标元素，则返回`false`。

```java
...
    /**
     * 移除链表指定元素，
     * 操作成功返回{@code true}，不存在目标元素则返回{@code false}。
     * @param element
     * @return Boolean
     */
    public boolean remove(E element) {
        int index = indexOf(element);
        return index == -1 ?
        false : element.equals(remove(index));
    }
...
```
> 代码清单：单链表`remove(E element)`方法实现

链表数据结构的优势在于其插入元素的开销比起数组要小很多，我们来实现链表插入元素`insert()`方法，具体步骤如下所示。如果索引下标越界，则抛出`IndexOutOfBoundsException`异常。

1. 找到链表中指定索引下标节点（当前节点）及其前驱节点

2. 创建一枚新节点

3. 新节点指向当前节点

4. 前驱节点指向新节点

5. 返回当前节点数据域

```
...
    /**
     * 向列表指定位置插入一个新的元素。
     * @param index
     * @param element
     * @return Previous element
     * @throws IndexOutOfBoundsException
     */
    public E insert(int index, E element)
    throws IndexOutOfBoundsException {
        if (index < 0 || index > size()) {
            throw new IndexOutOfBoundsException();
        }
        index -= 1;
        Node<E> prev = head;
        while (index >= 0) {
            prev = prev.getNext();
            --index;
        }
        Node<E> current = prev.getNext();
        Node<E> node = new Node<E>(element, null);
        node.setNext(current);
        prev.setNext(node);
        return current == null ? null : current.getElem();
    }
...
```
> 代码清单：单链表`insert()`方法实现

我们为链表提供一枚`clear()`方法，用于清空链表元素。由于 Java 语言的自动垃圾回收机制，直接将头节点（Head）置空即可表示清空链表，不用担心内存泄露问题，但是为了帮助垃圾收集器更好地做内存回收工作，这里我们选择**显式清空每一个节点**。

```java
...
    /**
     * 清空链表。
     * @param void
     * @return void
     */
    public void clear() {
        while (head != null) {
            Node<E> n = head;
            head = head.getNext();
            n.setElem(null);
            n.setNext(null);
        }
        head = new Node<E>();
    }
...
```
> 代码清单：单链表`clear()`方法实现

最后，我们来覆写链表`toString()`方法，更加方便地查看链表元素。

```java
...
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append('[');
        for (Node<E> current = head.getNext();
            current != null;
            current = current.getNext()) {
                sb.append(current.getElem().toString());
                sb.append(", ");
        }
        sb.append(']');
        return sb.toString();
    }
...
```
> 代码清单：单链表`toString()`方法实现

# 双向链表（Double Linked List）

双向链表（Double Linked List）可以做到双向遍历（头节点 <--> 尾节点），因为双向链表的节点（DLNode）包含数据域和两个指针域（前驱与后继）。

我们来定义出双向链表中节点（DLNode）数据结构，使用泛型类提高节点存储数据的灵活性。节点数据结构包含**构造方法**，三个私有变量：**数据域和两枚指针域**，及其**对应的`Getter/Setter`方法**。

```java
public class DLNode<E> {
    private E elem;
    private DLNode<E> prev;
    private DLNode<E> next;
    public DLNode(E elem, DLNode<E> prev, DLNode<E> next) {
        this.elem = elem;
        this.prev = prev;
        this.next = next;
    }
    public DLNode() {
        this(null, null, null);
    }
    public void setElem(E elem) {
        this.elem = elem;
    }
    public void setPrev(DLNode<E> prev) {
        this.prev = prev;
    }
    public void setNext(DLNode<E> next) {
        this.next = next;
    }
    public E getElem() {
        return this.elem;
    }
    public DLNode<E> getPrev() {
        return this.prev;
    }
    public DLNode<E> getNext() {
        return this.next;
    }
}
```
> 代码清单：双向链表节点（DLNode）

我们考虑双向链表数据结构：**双向链表包含一枚头节点`head`与一枚尾节点`tail`，头尾结点均不存储数据，而是指向实际存储数据的节点；头节点前驱节点为`null`，尾节点后继节点为`null`。**

双向链表的初始化过程为：**构造头尾节点，并将头尾节点相连。**

双向链表的优势在于：**支持双向遍历，「头插法」和「尾插法」的算法复杂度均为$O(1)$，而单链表「尾插法」的算法复杂度为$O(n)$。**

以下 Java 代码给出双向链表`DoubleLinkedList`完整实现。

```java
import java.lang.IndexOutOfBoundsException;
import java.util.NoSuchElementException;
import DLNode;
public class DoubleLinkedList<E> {
    /**
     * 链表头节点
     */
    private DLNode<E> head;
    /**
     * 链表尾节点
     */
    private DLNode<E> tail;
    /**
     * 构造方法：创建空链表
     * @param void
     */
    public DoubleLinkedList() {
        this.head = new DLNode<E>();
        this.tail = new DLNode<E>();
        head.setNext(tail);
        tail.setPrev(head);
    }
    public static void main(String[] args) {
        // ...
    }
    /**
     * 向双向链表指定索引位置插入一个新元素。
     * @param index
     * @param element
     * @return Previous element
     * @throws IndexOutOfBoundsException
     */
    public E insert(int index, E element)
    throws IndexOutOfBoundsException {
        if (index < 0 || index > size()) {
            throw new IndexOutOfBoundsException();
        }
        DLNode<E> current = head;
        while (index >= 0) {
            current = current.getNext();
            --index;
        }
        DLNode<E> node = new DLNode<E>(element, null, null);
        node.setNext(current);
        node.setPrev(current.getPrev());
        current.getPrev().setNext(node);
        current.setPrev(node);;
        return current.getNext() == null ? null : current.getElem();
    }
    /**
     * 移除双向链表指定元素，
     * 操作成功返回{@code true}，不存在目标元素则返回{@code false}。
     * @param element
     * @return Boolean
     */
    public boolean remove(E element) {
        int index = indexOf(element);
        return index == -1 ?
        false : element.equals(remove(index));
    }
    /**
     * 移除双向链表指定索引下标元素。
     * @param index
     * @return Removed element
     * @throws IndexOutOfBoundsException
     */
    public E remove(int index) throws IndexOutOfBoundsException {
        if (index < 0 || index >= size()) {
            throw new IndexOutOfBoundsException();
        }
        /* Input: 1 */
        /* head <--> elem(1) <--> elem(2) <--> elem(3) <--> tail */
        DLNode<E> node = head.getNext();
        while (index > 0) {
            node = node.getNext();
            --index;
        }
        node.getPrev().setNext(node.getNext());
        node.getNext().setPrev(node.getPrev());
        node.setPrev(null);
        node.setNext(null);
        return node.getElem();
    }
    /**
     * 为双向链表指定索引位置的元素设新值。
     * @param index
     * @param element
     * @return Previous element in the index.
     * @throws IndexOutOfBoundsException
     */
    public E set(int index, E element) throws IndexOutOfBoundsException {
        if (index < 0 || index >= size()) {
            throw new IndexOutOfBoundsException();
        }
        DLNode<E> node = head.getNext();
        while (index > 0) {
            node = node.getNext();
            --index;
        }
        E oldElem = node.getElem();
        node.setElem(element);
        return oldElem;
    }
    /**
     * 获取双向链表指定索引位置的元素。
     * @param index
     * @return element
     * @throws IndexOutOfBoundsException
     */
    public E get(int index) throws IndexOutOfBoundsException {
        if (index < 0 || index >= size()) {
            throw new IndexOutOfBoundsException();
        }
        DLNode<E> node = head.getNext();
        while (index > 0) {
            node = node.getNext();
            --index;
        }
        return node.getElem();
    }
    /**
     * 返回指定元素所在双向链表的索引位置。
     * @param element
     * @return The index of element in {@code DoubleLinkedList},
     * return {@code -1} if element does not found.
     */
    public int indexOf(E element) {
        int index = 0;
        for (DLNode<E> current = head.getNext();
            current.getNext() != null;
            current = current.getNext()) {
                if (element.equals(current.getElem())) {
                    return index;
                }
                ++index;
        }
        return -1;
    }
    /**
     * 检查双向链表中是否包含目标元素，
     * 元素相等使用 {@code o.equals(obj)} 判断。
     * @param element
     * @return Boolean
     */
    public boolean contains(E element) {
        for (DLNode<E> current = head.getNext();
            current.getNext() != null;
            current = current.getNext()) {
            if (element.equals(current.getElem())) {
                return true;
            }
        }
        return false;
    }
    /**
     * 移除并返回双向链表尾部最后一个元素。
     * @param void
     * @return Last element of this {@code DoubleLinkedList}.
     * @throws NoSuchElementException
     */
    public E removeLast() throws NoSuchElementException {
        if (isEmpty()) {
            throw new NoSuchElementException();
        }
        DLNode<E> node = tail.getPrev();
        node.getPrev().setNext(tail);
        tail.setPrev(node.getPrev());
        node.setPrev(null);
        node.setNext(null);
        return node.getElem();
    }
    /**
     * 移除并返回双向链表头部第一个元素。
     * @param void
     * @return First element of this {@code DoubleLinkedList}.
     * @throws NoSuchElementException
     */
    public E removeFirst() throws NoSuchElementException {
        if (isEmpty()) {
            throw new NoSuchElementException();
        }
        DLNode<E> node = head.getNext();
        node.getNext().setPrev(head);
        head.setNext(node.getNext());
        node.setPrev(null);
        node.setNext(null);
        return node.getElem();
    }
    /**
     * 向双向链表头部添加一个新元素。
     * @param element
     * @return void
     */
    public void addFirst(E element) {
        DLNode<E> node = new DLNode<E>(element, null, null);
        node.setPrev(head);
        node.setNext(
            head.getNext()
        );
        head.setNext(node);
        head.getNext().setPrev(node);
    }
    /**
     * 向双端链表尾部添加一个新元素。
     * @param element
     * @return void
     */
    public void addLast(E element) {
        DLNode<E> node = new DLNode<E>(element, null, null);
        node.setPrev(tail.getPrev());
        node.setNext(tail);
        tail.getPrev().setNext(node);
        tail.setPrev(node);
    }
    /**
     * 取得双向链表头部第一个元素，链表为空则抛出异常。
     * @param void
     * @return First element of {@code DoubleLinkedList}.
     * @throws NoSuchElementException
     */
    public E getFirst() throws NoSuchElementException {
        if (isEmpty()) {
            throw new NoSuchElementException();
        }
        return head.getNext().getElem();
    }
    /**
     * 取得双向链表尾部最后一个元素，链表为空则抛出异常。
     * @param void
     * @return Last element of {@code DoubleLinkedList}.
     * @throws NoSuchElementException
     */
    public E getLast() throws NoSuchElementException {
        if (isEmpty()) {
            throw new NoSuchElementException();
        }
        return tail.getPrev().getElem();
    }
    /**
     * 计算双向链表存储元素数量。
     * @param void
     * @return Size of {@code DoubleLinkedList}.
     */
    public int size() {
        int cnt = 0;
        for (DLNode<E> n = head.getNext();
            n.getNext() != null;
            n = n.getNext()) {
            ++cnt;
        }
        return cnt;
    }
    /**
     * 检查双向链表是否为空。
     * @param void
     * @return Boolean {@code true} or {@code false}.
     */
    public boolean isEmpty() {
        return this.size() > 0 ? false : true;
    }
    /**
     * 清空双向链表。
     * @param void
     * @return void
     */
    public void clear() {
        while (head.getNext() != null) {
            DLNode<E> current = head;
            head = head.getNext();
            current.setElem(null);
            current.setPrev(null);
            current.setNext(null);
        }
        head = new DLNode<E>();
        tail = new DLNode<E>();
        head.setNext(tail);
        tail.setPrev(head);
    }
    /**
     * 返回双向链表字符串形式。
     * @param void
     * @return void
     */
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append('[');
        for (DLNode<E> current = head.getNext();
            current.getNext() != null;
            current = current.getNext()) {
                sb.append(current.getElem().toString());
                sb.append(", ");
        }
        sb.append(']');
        return sb.toString();
    }
}
```
> 代码清单：双单链表`DoubleLinkedList`

# 链表测试（Linked List Test）

我们来写个链表单元测试程序，检测链表能否正常工作。

```java
import SingleLinkedList;
import DoubleLinkedList;
public class LinkedListTest {
    public static void main(String[] args) {
        System.out.println("[INFO]: Test DoubleLinkedList() ...");
        DoubleLinkedList<String> aList = new DoubleLinkedList<String>();
        for (int i = 0; i < 10; ++i) {
            aList.addLast(String.valueOf(i));
        }
        System.out.println(aList);
        System.out.println(
            "aList.size(): " +
            aList.size()
        );
        System.out.println(
            "aList.contains(\"2\"): " +
            aList.contains("2")
        );
        System.out.println(
            "aList.contains(\"10\"): " +
            aList.contains("10")
        );
        System.out.println(
            "aList.addFirst(\"Java\")"
        );
        System.out.println(
            "aList.addLast(\"Python\")"
        );
        aList.addFirst("Java");
        aList.addLast("Python");
        System.out.println(aList);
        System.out.println(
            "aList.remove(1): " +
            aList.remove(1)
        );
        System.out.println(aList);
        System.out.println(
            "aList.remove(\"8\"): " +
            aList.remove("8")
        );
        System.out.println(aList);
        System.out.println(
            "aList.get(0): " +
            aList.get(0)
        );
        System.out.println(
            "aList.set(1, \"JavaScript\"): " +
            aList.set(1, "JavaScript")
        );
        System.out.println(aList);
        System.out.println(
            "aList.indexOf(\"Python\"): " +
            aList.indexOf("Python")
        );
        System.out.println(
            "aList.indexOf(\"C++\"): " +
            aList.indexOf("C++")
        );
        System.out.println(
            "aList.insert(1, \"Golang\"): " +
            aList.insert(1, "Golang")
        );
        System.out.println(aList);
        System.out.println(
            "aList.clear()"
        );
        aList.clear();
        System.out.println(aList);
    }
}
```
> 代码清单：链表单元测试（Unit Test）

# 参考资料

- Wikipedia: https://en.wikipedia.org/wiki/Linked_list

- Introduction to Java Programming: Fundamental Data Structures and Algorithms: https://courses.edx.org/courses/course-v1:UC3Mx+IT.1.3x+1T2019/course/

- Fundamental Data Structures and Algorithms: https://www.bilibili.com/video/av52559737

