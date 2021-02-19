---
title: StringUtils.join内部处理逻辑分析
date: 2021-02-19 15:14:16
tags: join,list,java,StringUtils
---

今天看了下源码才发现自己根本就没想到里面对于null empty的处理。 也没有想到当只有一个元素的时候是join是如何处理的。

源码见：
```java
    public static String join(final Iterator<?> iterator, final String separator) {

        // handle null, zero and one elements before building a buffer
        if (iterator == null) {
            return null;
        }
        if (!iterator.hasNext()) {
            return EMPTY;
        }
        final Object first = iterator.next();
        if (!iterator.hasNext()) {
            return Objects.toString(first, "");
        }

        // two or more elements
        final StringBuilder buf = new StringBuilder(STRING_BUILDER_SIZE); // Java default is 16, probably too small
        if (first != null) {
            buf.append(first);
        }

        while (iterator.hasNext()) {
            if (separator != null) {
                buf.append(separator);
            }
            final Object obj = iterator.next();
            if (obj != null) {
                buf.append(obj);
            }
        }
        return buf.toString();
    }

```


可以看到 对于null empty，和单元素的Iterator迭代器都是有特殊处理的，直接返回null，返回空字符串，直接toString单元素。 考虑的场景其实是极为周全的。但是有个场景没有过滤处理 那就是空字符 也会进行拼接。
```java
    public static void main(String[] args) {
        ArrayList<String> strings = Lists.newArrayList(" ");
        strings.add(" ");
        strings.add(" ");
        strings.add(" ");
        System.out.println(org.apache.commons.lang3.StringUtils.join(strings, ","));
    }

输出：

 , , , 
```
