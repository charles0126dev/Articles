
ASCII GB2312 Unicode UTF-8 等常见字符编码方式
---

在开发过程中，经常要碰到各种字符集/编码方式，总结一下由来、联系、区别。

1.  > ASCII，全称American Standard Code for Information Interchange。很久前一群人决定用8个可以开合的晶体管来组合不同的状态，并称这8个晶体管的开关状态为**_字节_**。后来有了一些可以处理这些字节的机器，并用字节来组合出很多状态，这种机器被称为**_计算机_**。8个字节可以表示256种不同的状态，并约定机器在某些状态下做出某些反应，如当终端遇到0x10就换行，遇到0x1b打印机就打印反白的字等等，于是把0x20以下的字节状态称为_**控制码**。_并把空格、标点符号、数字、大写英文字母、小写英文字母分别用连续的字节状态表示，一直编到了127号，这样就可以用不同字节来存储英文文字了，这个编码方案被称为ANSI（美国国家标准协会）的_**ASCII码**_。此时，世界上的所有的计算机都用ASCII方案来保存英文文字。

2.  > 可是并不是全世界都用英文的，所以随着世界上更多的国家开始使用计算机，越来越多人发现ASCII无法满足他们要存储其他文字的需求。于是他们把127号之后的没有被ASCII使用的位用来表示这些新的字母、符号、横线、竖线等，一直把序号编到了255。从128到255的这些字符集被称为**_扩展字符集。_**

3.  > 后来中国人想存汉字，可是已经没有可用的字节状态了，而且常用汉字6000多个，就算8位的字节全都用来表示汉字也不够。国人直接把127号以后的扩展字符集全部去取消，然后规定一个小雨127的字符的意义与ASCII中一样，但两个大于127的字符连在一起就表示一个汉字，前面一个字节为_**高字节**_，从0xA1到0xF7；后面一个字节为_**低字节**_，从0xA1到0xFE。这样就可以组合大约7000多个简体汉字了。且在这些编码里把数学符号、希腊字母、日文的假名都编进去了。并对ASCII中本来就有的数字、标点、字母都重新编了两个字节长的编码，这就是我们总可以看到的**_全角字符_**，而原来127以下的那些就是_**半角字符**_。这种编码方式被规定为GB2312，可以看出GB2312是对ASCII的中文扩展，其中GB就是“国标”的拼音的缩写。

4.  > 显然，汉字太多了，尤其有些人的名字里还有生僻字，打不出自己名字怎么行呢，于是，我们把GB2312的限制再放开一些，刚才说GB2312后面一个字节（低字节）为127之后才认为是汉字，现在把这个规定取消，只要高字节是127之后，那就认为是一个汉字的开始了。这个扩展之后的结果就是GBK标准。GBK包括了GB2312的所有内容，同时还增加了2万个新的汉字（含繁体字）和符号。

5.  > 中国56个民族的语言当然不是汉字可以囊括的，为增加少数名族的文字，又把GBK扩展成了GB18030。

6.  > 这一系列的中文编码标准被统称为DBCS(Double Byte Character Set 双字节字符集)，其特点就是双字节长的中文字符和一字节长的英文字符并存于同一套编码方案，这也是为什么一个汉字算两个英文字符的原因。于是为了支持中文处理，程序员们需要必须注意字串里的每一个字节的值。

7.  > 当然，除了中文外还有很多其他非英文的语言，于是世界各地的不同语言的国家纷纷自己搞一套标准，谁也不兼容谁。甚至隔海相望的台湾也采用了不同的DBCS编码方案（如繁体中文Big5），当时为了可以处理汉字，必须在计算机上装一个“汉卡”才行。联想柳传志找来中科院的倪光南做了“联想式汉卡”、史玉柱后来也开发了一个汉卡系统，并且都因此而起家。

8.  > ISO（国际标准化组织）决定着手解决这个问题，办法简单而粗暴，抛开所有地方性编码，重新制定包括所有文字、所有字母、符号的编码，叫做Unicode（Universal Multiple-Octet Coded Character Set,简称UCS，俗称Unicode）。这时候的计算机的存储容量极大地发展了，空间再也不成问题，于是ISO直接规定必须用两个字节来进行字符编码，也就是统一用16位进行编码。对于ASCII中的字符，Unicode保持其原编码值不变，只是将其长度由原来8位扩展为16位（高8位补0）。

9.  > 正如上面所说的，Unicode在制定的时候没有想过要跟任何一种现有的编码方案保持兼容，所以GBK与Unicode在汉字内码的编排上完全是不一样的，没有一种简单的算法可以把文本内容从Unicode编码和另一种编码进行转换，这种转换只能查表。

10.  > UCS（Unicode的简称）还有4字节版，可以组合出21亿个不同的字符，叫UCS-4。

11.  > Unicode解决了很多公司的难题，终于可以不用为了进入某个地区的市场而额外处理文字的编码问题了。Windows NT开始，微软把操作系统中的核心代码都改成了Unicode方式工作的版本，从而无需加装本土语言系统就可以显示各国字符了。

12.  > 随着计算机网络的发展，Unicode如何在网络上传输成了必须考虑的问题。于是有了面向传输的众多UTF(UCS Transfer Format)标准的出现,而UTF8（8-bit Unicode Transformation Format）就是以每次8个位传输数据，是一种针对Unicode的可变长度的字符编码，也是一种前缀码，可以用来表示Unicode中的任何字符，且其编码中的第一个字节仍与ASCII兼容，Unicode形式的ASCII字符经过UTF8编码后得到的长度为一个字节，且意义与ASCII中一致。其余Unicode字符经过UTF8编码后至少2个字节。UTF8编码有几个显著的优势，其一是兼容ASCII码；其二是乱码不会扩散，而GB2312在丢失一字节等情况下会造成后续所有文字全部变成乱码；其三是UTF8可以是一个更大的字符集。
