# git 基础

## git log

`git log` ：查看日志简介。

可用选项：
+ `-p` ：查看具体commit的修改内容。
+  `-{n}` ：显示最近的n条日志。
+  `--stat` ：简略显示日志，更默认log不同的是，会显示那些文件发生变化。
+  `--since="year-month-day"` ：显示指定时间之后的提交。
+  `--before="<time>"` ：指定时间之前的提交。
+  `--after="<time>"` ：指定时间之后的提交。
+  `--author=<name>` ：指定提交人名称的提交。
+  `--pretty=<mode>` ：按不同的方式显示日志。
    +  `oneline` ：单行显示日志。
    +  `short` ：简短显示。
    +  `full` ：完全显示。
    +  `format:"<str>"` ：按指定格式输出日志内容。
        
        |选项|作用|
        |---|---|
        |%H/%h|显示提交对象的hash值|
        |%T/%t|显示树对象的hash值|
        |%P/%p|显示父对象的hash值|
        |%an/%ae|作者的名称和email|
        |%ad/%ar|修改的时间|
        |%cn/%ce|提交者的名称跟email|
        |%cd/%cr|提交的时间|
        |%s|提交的说明|