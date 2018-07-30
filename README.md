# bubble-sort
algorithm

我的冒泡排序分析，如何确定两次for循环的次数，还是以例子说明，比较靠谱：
1 4 2 5 7  数组length为5，length-1为4
外层循环用i来表示，内层循环用j来表示。
第一次外层循环，i=0，                     j
    第1次内层循环：1 4 2 5 7  1、4比较    0
    第2次内层循环：1 2 4 5 7  4、2比较    1
    第3次内层循环：1 2 4 5 7  4、5比较    2
    第4次内层循环：1 2 4 5 7  5、7比较    3
    共计4次即length-1次循环，内层循环比较了4次，j最大小于4即length-1-i。
    此时最大的已经搬到了最右边，那么数组的最后两个元素一定是有序的。
第二次外层循环，i=1，                    j
    第1次内层循环：1 4 2 5 7  1、4比较   0
    第2次内层循环：1 2 4 5 7  4、2比较   1
    第3次内层循环：1 2 4 5 7  4、5比较   2
    第4次内层循环：1 2 4 5 7  5、7比较   此时最后两个元素已经有序，无需比较。
    共计4次即length-1-i次循环，内层循环比较了3次，j最大小于3即length-1-i。
    
自此初步确定外层的循环次数是 < length - 1
           内层的循环次数是 < length - 1 - i

第一版冒泡排序
/**
 * 冒泡排序，将最大的数字赶到最右边
 * Created by Ablert
 * on 2018/7/18.
 * @author Ablert
 */

public class BubbleSortVersion1 {

    /**
     * 冒泡排序算法，双循环进行排序
     * 外层循环控制整理次数，内层循环进行排序。
     * @param array
     */
    private static void bubbleSort(int[] array) {
        int temp = 0;
        for (int i = 0; i < array.length - 1; i++) {
            for (int j = 0; j < array.length - 1 - i; j++) {
                if (array[j] > array[j + 1]) {
                    //前一个比后一个大需要调换位置
                    temp = array[j+1];
                    array[j+1] = array[j];
                    array[j] = temp;
                }
            }
        }
    }

第二版冒泡排序
/**
 * 冒泡排序的第二版，主要是加入了是否排序的标识isSorted
 * 判断数列是否已经有序，如果能判断出数组已经有序，并作出标记，剩下的几轮排序就可以不必执行，提早结束工作。
 * 有元素交换，说明不是有序
 * 如果在本轮排序中，元素有交换，则说明数列无序；如果没有元素交换，说明数列已然有序，直接跳出大循环。
 * Created by Ablert
 * on 2018/7/18.
 * @author Ablert
 */

public class BubbleSortVersion2 {

    /**
     * 冒泡排序第二版
     * @param array
     */
    private static void bubbleSort(int[] array) {
        int temp = 0;
        for (int i = 0; i < array.length-1; i++) {
            //有序标记，每一轮的初始值都是true
            boolean isSorted = true;
            for (int j = 0; j < array.length - 1 - i; j++) {
                if (array[j] > array[j + 1]) {
                    temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                    //有元素交换，说明不是有序，变为false
                    isSorted = false;
                }
            }
            //提早几轮结束排序
            if (isSorted) {
                break;
            }
        }
    }

第三版冒泡排序
/**
 * 冒泡排序的第三版
 * 数列前半部分无序，后半部分有序
 * 比如例子中仅仅第二轮，后面5个元素实际都已经属于有序区。因此后面的许多次元素比较是没有意义的。
 * 如何避免这种情况呢？我们可以在每一轮排序的最后，记录下最后一次元素交换的位置，那个位置也就是无序数列的边界，再往后就是有序区了。
 * Created by Ablert
 * on 2018/7/18.
 * @author Ablert
 */

public class BubbleSortVersion3 {

    /**
     * 冒泡排序第三版
     * @param array
     */
    private static void bubbleSort(int[] array) {
        int temp = 0;
        //记录最后一次交换的位置
        int lastExchangeIndex = 0;
        //无界数列的边界，每次比较只需比到这里为止
        int sortBorder = array.length - 1;
        for (int i = 0; i < array.length; i++) {
            boolean isSorted = true;
            for (int j = 0; j < sortBorder; j++) {
                if (array[j] > array[j + 1]) {
                    temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                    isSorted = false;
                    //把无序数列的边界更新为最后一次交换元素的位置
                    lastExchangeIndex = j;
                }
            }
            sortBorder = lastExchangeIndex;
            if (isSorted) {
                break;
            }
        }
    }

测试

    public static void main(String[] args) {
        int[] array = {12, 32, 32, 32, 56, 987, 98, 34};
        bubbleSort(array);
        System.out.println(Arrays.toString(array));
    }
