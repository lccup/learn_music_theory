# note_MusicXML

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

> 音符
```xml
<note>
    <pitch>
        <step>E</step>
        <octave>3</octave>
    </pitch>
    <duration>2</duration>
    <type>eighth</type>
    <voice>1</voice>
</note>
```

> 休止符
```xml
<note>
    <rest/>
    <duration>1</duration>
    <type>eighth</type>
    <voice>1</voice>
</note>
```


