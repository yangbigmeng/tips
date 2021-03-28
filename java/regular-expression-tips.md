# 正则表达式使用

## 使用示例

1. 使用正则表达式构建Pattern(java.util.regex.Pattern)
2. 使用pattern.matcher(String)方法进行匹配
3. 根据matcher.find()的true/false来判断是否还有匹配结果
4. matcher.group()为本次的匹配结果

```java
public class RegularExpression {

    /**
     * 正则表达式示例
     * @param regular 正则表达式
     * @param content 内容
     * @return 匹配内容
     */
    public String demo(String regular, String content) {
        Pattern pattern = Pattern.compile(regular);
        Matcher matcher = pattern.matcher(content);
        StringBuilder builder = new StringBuilder();
        while (matcher.find()) {
            builder.append(matcher.group()).append(", ");
        }
        return builder.toString();
    }

    public static void main(String[] args) {
        String regular = "\\d";
        String content = "今天是2021年3月20日";
        RegularExpression regularExpression = new RegularExpression();
        String res = regularExpression.demo(regular, content);
        System.out.println("res: " + res);
    }
}
```

## 正则表达式

正则表达式的设置有一套固定的逻辑，需要专门来学习。如一些字符类的典型方式

|表达式| 含义|
|----|----|
|. | 任意字符|
|[abc] | 包含a,b,c的任何字符，和a\|b\|c相同|
|[^abc] | 否定，不包含a,b,c的任何字符|
|[a-zA-Z] |-范围，从a-z，A-Z的任何字符|
|\s| 空白字符，空格/tab/换行/回车/换页|
|\S| 否定，非空白字符|
|\d| 数字|
|\D| 非数字|
|\w| 词字符[a-zA-Z0-9]|
|\W| 非词字符 [^\w]|


## POSIX 字符类

与一些创建字符类的典型方式不同，POSIX比较特殊不怎么常用，不过使用起来确实比较方便。

|表达式| 含义|
|----|----|
|\p{Lower}| 小写字母字符：[a-z]|
| \p{Lower} |小写字母字符：[a-z] |
| \p{Upper} |大写字母字符：[A-Z] |
| \p{ASCII} |所有 ASCII：[\x00-\x7F] |
| \p{Alpha} |字母字符：[\p{Lower}\p{Upper}] |
| \p{Digit} |十进制数字：[0-9] |
| \p{Alnum} |字母数字字符：[\p{Alpha}\p{Digit}] |
| \p{Punct} |英文标点符号：!"#$%&'()*+,-./:;<=>?@[\]^_`{\|}~ |
| \p{Graph} |可见字符：[\p{Alnum}\p{Punct}] |
| \p{Print} |可打印字符：[\p{Graph}\x20] |
| \p{Blank} |空格或制表符：[ \t] |
| \p{Cntrl} |控制字符：[\x00-\x1F\x7F] |
| \p{XDigit} |十六进制数字：[0-9a-fA-F] |
| \p{Space} |空白字符：[ \t\n\x0B\f\r]|

- 示例
```java
public static void main(String[] args) {
	String content = "今天是2021年3月20日，在忙中守住欢笑，斩不断情意yi如潮!";
	RegularExpression regularExpression = new RegularExpression();
	String regular = "\\d";
	System.out.println("数字: " + regularExpression.demo(regular, content));

	regular = "\\p{Lower}";
	System.out.println("小写字母: " + regularExpression.demo(regular, content));

	regular = "\\p{ASCII}";
	System.out.println("ascii: " + regularExpression.demo(regular, content));

	regular = "\\p{Punct}";
	System.out.println("英文标点符号: " + regularExpression.demo(regular, content));
}

```
