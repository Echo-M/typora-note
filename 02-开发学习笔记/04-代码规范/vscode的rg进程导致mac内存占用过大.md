#### vscode有个 rg 进程followSymlinks导致 Mac内存占用过大

最近，Mac运行卡顿，迟缓，耗电量极快，以为是进程开多了，于是乎重启了几次，几次之后呢，依旧如此…

后来打开活动监视器，发现一个 rg 进程，有三个，每个占了足足三四个G的大小，总体16G，算下来几乎被吃没了，这能不卡死才叫强悍…

查了查，是 vscode 的进程，乖乖，改掉他：

![image-20200103095623230](/Users/baola/Library/Application Support/typora-user-images/image-20200103095623230.png)

打开 settings，搜索?：search.followSymlinks，改为 false

重启Vscode，观察进程：

![image-20200103095635638](/Users/baola/Library/Application Support/typora-user-images/image-20200103095635638.png)

解决！

