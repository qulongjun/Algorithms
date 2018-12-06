# 设计一个有 `getMin` 功能的栈

## 【题目】
实现一个特殊的栈，在实现栈的基本功能的基础上，再实现返回栈中最小元素的操作。

## 【要求】
* pop、push、getMin操作的时间复杂度都是 `O(1)`。
* 设计的栈类型可以使用现成的栈结构。

## 【解答】
我们使用两个栈来存储元素：
* 用来存储所有数据元素的通用数据栈，记做 `dataStack` 。
* 用来存储最小数据元素的最小数据栈，记做 `minStack`。

### 方案一
#### 入栈操作
若待入栈数据为 `data`，当调用 `push`  进行入栈操作时，首先会将该数据存入通用数据栈，然后判断待入栈数据与最小数据栈栈顶元素的大小，会有如下情形：
* 若最小数据栈为空，则将待入栈数据插入最小数据栈。
* 若最小数据栈不为空，且栈顶元素值大于等于待入栈数据，则将待入栈数据压入最小数据栈。
* 否则不进行任何操作。

#### 出栈操作
根据入栈操作顺序可知，最小数据栈的排列顺序为由小到大，即越靠近栈顶的数据越小，因此，最小数据栈栈顶元素的值即是最小数据栈中的最小值，也是通用数据栈中的最小值。

取通用数据栈栈顶元素`value` 和最小数据栈栈顶元素 `minValue` 进行比较，会有如下情形：
* 若 `value`值不存在，则数据栈为空，抛出异常。
* 若`value` 值与 `minValue`值相同，则将最小数据栈栈顶元素弹出，弹出并返回通用数据栈栈顶元素的值。
* 若`value` 值与 `minValue`值不相同，则弹出并返回通用数据栈栈顶元素的值。

#### 查询最小值操作
根据入栈操作顺序可知，最小数据栈的排列顺序为由小到大，即越靠近栈顶的数据越小，因此，最小数据栈栈顶元素的值即是最小数据栈中的最小值，也是通用数据栈中的最小值。
因此，仅需获取最小数据栈中栈顶元素的值，即为最小值。

#### 代码示例
```java
public class GetMinStack01 {

    private Stack<Integer> minStack = new Stack<Integer>(); // 最小数据栈，用于存放栈中最小值

    private Stack<Integer> dataStack = new Stack<Integer>(); // 通用数据栈，用于存放所有数据


    /**
     * 判断当前数据栈是否为空
     */
    public void isEmpty() {
        if (this.dataStack.isEmpty()) throw new RuntimeException("当前数据栈为空！"); // 若通用数据栈为空，则表示当前没有数据或数据已全部清空
    }


    /**
     * 获取当前栈中最小值
     *
     * @return 栈中最小的值
     */
    public int getMin() {

        this.isEmpty();

        return this.minStack.peek(); // 获取最小数据栈中顶部数据
    }

    /**
     * 推出当前栈中顶部数据
     *
     * @return 栈中最后一位
     */
    public int pop() {

        this.isEmpty();

        if (this.dataStack.peek() == this.getMin()) this.minStack.pop(); // 若通用数据栈的最顶部数据和最小数据栈的顶部数据相同，则一起推出栈

        return this.dataStack.pop(); // 推出通用数据栈中顶部数据
    }

    /**
     * 将数据添加至数据栈中
     *
     * @param data 新数据
     */
    public void push(int data) {

        if (this.minStack.isEmpty() || data <= this.getMin()) {

            this.minStack.push(data); // 若当前最小数据栈为空，则将数据推入最小数据栈

        }

        this.dataStack.push(data); // 将数据推入通用数据栈

    }
}
```

### 方案二
与方案一类似，方案二在每次入栈操作时，均为插入一个当前栈中最小数据。

#### 入栈操作
若待入栈数据为 `data`，当调用 `push`  进行入栈操作时，首先会将该数据存入通用数据栈，然后判断待入栈数据与最小数据栈栈顶元素的大小，会有如下情形：
* 若最小数据栈为空，则将待入栈数据插入最小数据栈。
* 若最小数据栈不为空，且栈顶元素值大于等于待入栈数据，则将待入栈数据压入最小数据栈。
* 若最小数据栈不为空，且栈顶元素值小于待入栈数据，则将最小数据栈当前栈定元素压入最小数据栈。

#### 出栈操作
根据入栈操作顺序可知，最小数据栈的排列顺序为由小到大，即越靠近栈顶的数据越小，因此，最小数据栈栈顶元素的值即是最小数据栈中的最小值，也是通用数据栈中的最小值。

取通用数据栈栈顶元素`value` 和最小数据栈栈顶元素 `minValue` 进行比较，会有如下情形：
* 若 `value`值不存在，则数据栈为空，抛出异常。
* 将最小数据栈栈顶元素出栈，并将通用数据栈栈顶元素出栈后返回。

#### 查询最小值操作
根据入栈操作顺序可知，最小数据栈的排列顺序为由小到大，即越靠近栈顶的数据越小，因此，最小数据栈栈顶元素的值即是最小数据栈中的最小值，也是通用数据栈中的最小值。
因此，仅需获取最小数据栈中栈顶元素的值，即为最小值。

#### 代码示例
```java
public class GetMinStack02 {

    private Stack<Integer> minStack = new Stack<Integer>(); // 最小数据栈，用于存放栈中最小值

    private Stack<Integer> dataStack = new Stack<Integer>(); // 通用数据栈，用于存放所有数据


    /**
     * 判断当前数据栈是否为空
     */
    public void isEmpty() {
        if (this.dataStack.isEmpty()) throw new RuntimeException("当前数据栈为空！"); // 若通用数据栈为空，则表示当前没有数据或数据已全部清空
    }

    /**
     * 获取当前栈中最小值
     *
     * @return 栈中最小的值
     */
    public int getMin() {

        this.isEmpty();

        return this.minStack.peek(); // 获取最小数据栈中顶部数据
    }

    /**
     * 推出当前栈中顶部数据
     *
     * @return 栈中最后一位
     */
    public int pop() {

        this.isEmpty();

        this.minStack.pop(); // 推出最小数据栈顶部数据

        return this.dataStack.pop(); // 推出通用数据栈中顶部数据
    }

    /**
     * 将数据添加至数据栈中
     *
     * @param data 新数据
     */
    public void push(int data) {

        if (this.minStack.isEmpty() || data <= this.getMin()) {

            this.minStack.push(data); // 若当前最小数据栈为空，则将数据推入最小数据栈

        } else {

            this.minStack.push(this.getMin()); // 将当前最小数据栈中顶部元素追加到最小数据栈中

        }

        this.dataStack.push(data); // 将数据推入通用数据栈

    }
}
```

## 对比
方案一和方案二共同点都是使用两个栈对数据进行保存，其中通用数据栈保存全部数据，最小数据栈道的栈顶元素保存当前插入时点最小值，时间复杂度均为`O(1)`，空间复杂度均为`O(n)` 。

方案一与方案二区别是方案一在出栈时进行最小值判断，方案二在入栈时进行最小值判断。
