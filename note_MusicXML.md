# note_MusicXML

[osmd: musicxml渲染](https://github.com/opensheetmusicdisplay/opensheetmusicdisplay)


# 音乐逻辑与视觉逻辑的解耦

在MusicXML中，同一件事物可能存在两个标签

+ 一个标签用于处理音乐逻辑(**计算机**如何演奏)
+ 一个用于处理视觉逻辑(人类实际上看到的)

|音乐逻辑||视觉逻辑|||
|:-|:-|:-|:-|:-|
|duration||type||均为时值|
|tie|延音线|tied|延音线/连线||
||||||
||||||

# 一些元素

|Music/Vision|ele|text|||
|:-|:-|:-|:-|:-|
|Vision|||||
||\<divisions\>|决定duration使用的单位|将**四分音符**分为数份|视为一个单位|
||\<duration\>|控制音持续数个单位|单位由divisions||
||||||
||\<voice\>|分配声部|第num声部||
||\<stem\>|控制符干方向|up;down||
||\<beam\>|符梁，控制音符之间的连接|begin;continue;end||
||||||
||\<notations\>|容器，存储连音线、滑音、颤音、强弱记号等标记|||
||||||
||||||
||||||


|\<type\>|||
|:-|:-|:-|
|1/1|whole||
|1/2|half||
|1/4|quarter||
|1/8|eighth||
|1/16|16th||
|1/32|32th||
|$...$|$...$||


# 结构层次

> 整体结构层次

```xml
<!--注释-->


<!--
    <?xml>
    <!DOCTYPE score-partwise        xml文件头
    score-partwise                  musicxml的根元素
-->

<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE score-partwise PUBLIC "-//Recordare//DTD MusicXML 4.0 Partwise//EN" "http://www.musicxml.org/dtds/partwise.dtd">
<score-partwise version="4.0">
    <!--
        score-partwise 的数个头元素 用于 存放元数据
        以下以缩进表示元素之间的层级关系
    -->


    <work>                  <!--乐章信息-->
        <work-title>            <!--乐章大标题-->
        <work-number>           <!--乐章小标题，通常一章一个文件-->

    <movement-number>       <!--第几章-->
    <movement-title>        <!--章节标题-->
    <identification>        <!--其他元数据-->
        <creator type="XXX">    <!--作者，通过type标定贡献-->
        <rights>                <!--版权信息-->
        <miscellaneous>         <!--杂项，存放musicxml不支持的标识元素-->

    <encoding>              <!--文件构建信息，该文件如何构建的-->
        <encoding-date>
        <encoder>
        <software>
        <encoding-description>
    
    <part-list>             <!--本章节中的章节列表（必须）-->
        <score-part id="P1">
            <part-name>
        <score-part id="P2">
            <part-name>
        <score-part id="P3">
            <part-name>

    <part id="P1">          <!--本章节中的第一章节, id 与score-part 的id对应-->
        <measure number="1">    <!--小节-->
            <!--见下文-->
        <measure number="2">
        <measure number="3">
        <measure number="4">
    <part id="P2">
        <!--...-->
    <part id="P3">
        <!--...-->



</score-partwise>
```

> measure 小节的内部层次结构

```xml
<measure number="1">
    <attributes>            <!--小节属性-->
    <note>                  <!--音符-->
    <note>
    <note>
    <note>
    <note>

```

```xml
<measure number="1">
    <attributes>            <!--小节属性-->
        <divisions>             <!--将四分音符分数份作为一个时间单位，
                                    与duration共同决定音符的时长-->

        <staves>                <!--有几个五线谱 默认为1-->
        <clef number="2">       <!--谱号 number表示staff编号，即是第几个五线谱中的谱号-->
                                <!--可出现在measure中的任意位置，并非只能在attributes下-->
            <sign>                  <!--谱号-->
            <line>                <!--第几线-->

        <key>
            <fifths>            <!--调号-->
            <mode>              <!--模式，默认值为major 大调-->
        
        <time>                  <!--拍号-->
            <beats>                 <!--一小节有几拍-->
            <beat-type>             <!--几分音符为一拍-->

        <transpose>             <!--移调，定义乐器实际发音音高与记谱音高之间的音程差，
                                    服务于移调乐器-->
            ​​<chromatic>
            <diatonic>
            <octave-change>​​

```
> note  音符的内部层次结构

```xml
<note>
    <pitch>              <!--音高-->
        <step>              <!--CDEFGAB-->
        <alter>             <!--变音 1 为升半音 负数为降-->
        <octave>            <!--第几组-->

    <voice>               <!--第几声部，默认为1-->
    <type>                <!--持续时长,视觉逻辑-->
    <duration>            <!--持续时长,音乐逻辑,持续多少个divisions定义的单位-->
                          <!--musicxml将其作为计数器-->
                          <!--故而多声部需要backup 和 forward，和弦需要chrod-->

    <stem>                <!--符杆方向 up down-->

    <beam number="1">     <!--符梁-->
                          <!--number为符梁的层级，配和content控制具体层级的符梁-->
                          <!--  1表示第一根符梁，即八分音符的符梁-->
                          <!--  2表示第一根符梁，即十六分分音符的符梁, 以此类推-->
                          <!--content 可为 begin continue end-->
                          <!--continue优先-->
                          <!--若第一个为begin osmd不会显示符梁-->


    <tie type="start"/>   <!--连音线，音乐逻辑,type start end 标识开始和结束-->

    <lyric>               <!--歌词-->
        <elision>           <!--连字符或省略号-->
        <syllabic>          <!--音节类型，默认为单音节词，多音节词分begin、middle和end-->
        <text>              <!--文本 （必须），多音节文字时syllabic与text成对排列-->


```

> note 标识休止符

```xml
<note>
    <rest/>
    <duration>1</duration>
    <type>eighth</type>
    <voice>1</voice>
</note>
```

> note 和弦

```xml
<note>
    <chord>              <!--和弦标记，计数器不会增加duration个单位-->
    <pitch>
    <duration>           <!--不能比上一个非和弦的duration长-->

```

> 多声部

|ele|||
|:-|:-|:-|
|backup|||
|forward|||
|voice|||


> 反复与结束

```xml
<barline location = "left">         <!--location表示barline在小节的方位-->
                                    <!--若为left ，则其应在所有measure所有note的之前-->
                                    <!--若为right，则其应在所有measure所有note的之后-->
    <bar-style>                         <!--bar 样式，视觉逻辑-->
                                        <!--heavy-light 左重右轻 前反复标记-->
                                        <!--heavy-light 左重右轻 后反复标记-->
                                        <!--轻重顺序应当与音乐逻辑匹配-->

    <repeat direction="forward"/>       <!--重复记号，音乐逻辑-->
                                        <!--forward并非表示向前重复，而是它是重复的起始-->
```

```xml
<!--前反复标记-->
<barline location="left">
    <bar-style>heavy-light</bar-style>
    <repeat direction="forward"/>
</barline>


<!--后反复标记-->
<barline location="right">
    <bar-style>light-heavy</bar-style>
    <repeat direction="backward"/>
</barline>


<!--终止标记-->
<barline location="right">
    <bar-style>light-heavy</bar-style>
    <ending type="stop"/>
</barline>
```

> 延音线


```xml

                                                <!--延音线起始-->
<note>
    <!--...-->
    <tie type="start"/>                             <!--延音线，音乐逻辑-->
    <notations>                                     <!--容器，容纳音符的音乐符号-->
        <tied type="start" orientation="over"/>         <!--延音线，视觉逻辑-->
                                                        <!--orientation over/under-->
                                                        <!--控制延音线的位置 位于符头的上/下-->
    </notations>
    <!--...-->
</note>


                                                <!--延音线结束-->
<note>
    <!--...-->
    <tie type="end"/>
    <notations>
        <tied type="end"/>
    </notations>
    <!--...-->
</note>
```


# 非音符标记

速度记号、强弱记号、踏板标记、文字指令

```xml
<direction>                 <!--容器，容纳非音符标记-->
    <direction-type>            <!--容器，标记类型，容纳标记的具体内容-->
    <offset>                    <!--相对于当前小节的偏移量，以duration为单位-->
    <staff>
    <sound>                     <!--控制播放效果-->

```





