title: "一种区分语言环境的 String 比较方法"
date: 2015-04-23 22:08:11
tags: string 排序
---
例如：

    //ResolveInfo的按名字排序
    Collections.sort(resolveInfos,new ResolveInfo.DisplayNameComparator(pm));

    //类似的packageItemInfo的按名字排序实现

    public static class DisplayNameComparator
            implements Comparator<PackageItemInfo> {
        public DisplayNameComparator(PackageManager pm) {
            mPM = pm;
        }

        public final int compare(PackageItemInfo aa, PackageItemInfo ab) {
            CharSequence  sa = aa.loadLabel(mPM);
            if (sa == null) sa = aa.name;
            CharSequence  sb = ab.loadLabel(mPM);
            if (sb == null) sb = ab.name;
            return sCollator.compare(sa.toString(), sb.toString());
        }

        private final Collator   sCollator = Collator.getInstance();
        private PackageManager   mPM;
    }



[类 Collator](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/text/Collator.html)

>Collator 类执行区分语言环境的 String 比较。使用此类可为自然语言文本构建搜索和排
序例程。
>
>Collator 是一个抽象基类。其子类实现具体的整理策略。Java 平台目前提供了 RuleBasedCollator 子类，它适用于很多种语言。还可以创建其他子类，以处理更多的专门需要。
>
>与其他区分语言环境的类一样，可以使用静态工厂方法 getInstance 来为给定的语言环境
获得适当的 Collator 对象。如果需要理解特定整理策略的细节或者需要修改策略，只需查
看 Collator 的子类即可。
>
>下面的示例显示了如何使用针对默认语言环境的 Collator 比较两个字符串。
>
    // Compare two strings in the default locale
    Collator myCollator = Collator.getInstance();
    if( myCollator.compare("abc", "ABC") < 0 )
      System.out.println("abc is less than ABC");
    else
      System.out.println("abc is greater than or equal to ABC");

>可以设置 Collator 的 strength 属性来确定比较中认为显著的差异级别。提供了四种 strength：PRIMARY、SECONDARY、TERTIARY 和 IDENTICAL。对语言特征的确切 strength 赋>值和语言环境相关。例如在捷克语中，"e" 和 "f" 被认为是 PRIMARY 差异，而 "e" 和 "ě" 则是 SECONDARY >差异，"e" 和 "E" 是 TERTIARY 差异，"e" 和 "e" 是 IDENTICAL。下
面的示例显示了如何针对美国英语忽略大小写和重音。
>
    //Get the Collator for US English and set its strength to PRIMARY
    Collator usCollator = Collator.getInstance(Locale.US);
    usCollator.setStrength(Collator.PRIMARY);
    if( usCollator.compare("abc", "ABC") == 0 ) {
      System.out.println("Strings are equivalent");
    }
>如果正好比较 String 一次，则 compare 方法可提供最佳性能。但在对 String 列表排序
时，通常需要对每个 String 进行多次比较。在这种情况下，CollationKey 可提供更好的>性能。CollationKey 类将一个 String >转换成一系列可与其他 CollationKey 进行按位比
较的位。CollationKey 是由 Collator 对象为给定的 String 所创建的。
>注：不能比较由不同 Collator 创建的 CollationKey。有关使用 CollationKey 的示例，
请参阅 CollationKey 的类描述。

[类 CollationKey](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/text/CollationKey.html)

>CollationKey 表示遵守特定 Collator 对象规则的 String。比较两个 CollationKey 将>返回它们所表示的 String 的相对顺序。使用 CollationKey 来比较 String 通常比使用 Collator.compare 更快。因此，当必须多次比较 String 时（例如，对一个 String 列表进
行排序），使用 CollationKey 会更高效。

>不能直接创建 CollationKey。而是通过调用 Collator.getCollationKey 来生成。只能比
较同一个 Collator 对象生成的 CollationKey。

>为一个 String 生成 CollationKey 涉及到检查整个 String，并将它转换成可以按位比较
的一系列位。一旦生成了键，就允许进行快速比较。当 String 需要多次比较时，以更快速
的比较方式生成键的成本可以忽略不计。另一方面，比较的结果通常由每个 String 的第一
对字符确定。Collator.compare 只检查实际需要比较的字符，当进行单次比较时，此比较>方式更快。

>以下例子显示如何使用 CollationKey 对一个 String 列表进行排序。

    // Create an array of CollationKeys for the Strings to be sorted.
    Collator myCollator = Collator.getInstance();
    CollationKey[] keys = new CollationKey[3];
    keys[0] = myCollator.getCollationKey("Tom");
    keys[1] = myCollator.getCollationKey("Dick");
    keys[2] = myCollator.getCollationKey("Harry");
    sort( keys );
    
    //...
    
    // Inside body of sort routine, compare keys this way
    if( keys[i].compareTo( keys[j] ) > 0 )
    // swap keys[i] and keys[j]
    
    //...
    
    // Finally, when we've returned from sort.
    System.out.println( keys[0].getSourceString() );
    System.out.println( keys[1].getSourceString() );
    System.out.println( keys[2].getSourceString() );
