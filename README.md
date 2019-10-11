# Leetcode 题解 - 字符串.md

##1、leetcode13. 罗马数字转整数
解法一：
思路：
    先罗马字符转对应数字放入数组，然后遍历数组，相邻两项相减，大于0则加减数，小于0则减掉两数之差，对这n-1项sum求和。
但是这样没有考虑最后一项，如果最后两项还存在小的在前面的情况则不必操作，不然还要要加上最后一项。
遍历数组还得加一个条件，当只有一个罗马字符时，直接返回这个字符对应的数就行，否则遍历会数组越界。
代码：
<pre>
/**
  * @author lihe
  * @date 2019/10/11 16:34
  * @descriptor 罗马数字转整数
  */
public class RomanToInt_13_1 {
    public static int romanToInt(String s){
        char[] chars = s.toCharArray();
        int[] numbers = new int[chars.length];
        for (int i = 0; i < chars.length; i++) {
            if(chars[i]=='I')
                numbers[i] = 1;
            if(chars[i]=='V')
                numbers[i] = 5;
            if(chars[i]=='X')
                numbers[i] = 10;
            if(chars[i]=='L')
                numbers[i] = 50;
            if(chars[i]=='C')
                numbers[i] = 100;
            if(chars[i]=='D')
                numbers[i] = 500;
            if(chars[i]=='M')
                numbers[i] = 1000;
        }
        int result = 0;
        for (int i = 0; i < numbers.length-1; i++) {
            if (numbers[i] - numbers[i + 1] <= 0){
                result -= numbers[i] - numbers[i + 1];
                i++;
            }
            else
                result += numbers[i];
        }
        if(numbers[numbers.length-2]-numbers[numbers.length-1]>0)
            result += numbers[numbers.length-1];
        return result;
    }

    public static void main(String[] args) {
        String s = "MCMXCIV";
        int i = romanToInt(s);
        System.out.println(i);
    }

}
</pre>
解法二：
思路：
    首先将所有的组合可能性列出并添加到哈希表中
    然后对字符串进行遍历，由于组合只有两种，一种是 1 个字符，一种是 2 个字符，其中 2 个字符优先于 1 个字符
    先判断两个字符的组合在哈希表中是否存在，存在则将值取出加到结果 result 中，并向后移2个字符。不存在则将判断当前 1 个字符是否存在，存在则将值取出加      到结果 result 中，并向后移 1 个字符
    遍历结束返回结果 result
代码：
/**
 * @author lihe
 * @date 2019/10/11 16:34
 * @descriptor 罗马数字转整数
 */
public class RomanToInt_13 {

    private static Map<String,Integer> map = new HashMap<String,Integer>();

    public static int romanToInt(String s){
        map.put("I",1);
        map.put("IV",4);
        map.put("V",5);
        map.put("IX",9);
        map.put("X",10);
        map.put("XL",40);
        map.put("L",50);
        map.put("XC",90);
        map.put("C",100);
        map.put("CD",400);
        map.put("D",500);
        map.put("CM",900);
        map.put("M", 1000);
        int result = 0;
        for (int i = 0; i < s.length(); ) {
            if(i+1<s.length() && map.containsKey(s.substring(i, i + 2))){
                result += map.get(s.substring(i, i + 2));
                i += 2;
            }else{
                result += map.get(s.substring(i, i + 1));
                i++;
            }

        }
        return result;
    }

    public static void main(String[] args) {
        String s = "MCMXCIV";
        int i = romanToInt(s);
        System.out.println(i);
    }

}
