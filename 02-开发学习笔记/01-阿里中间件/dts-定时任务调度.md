dts现在更名为scheduleX

参考文档：

DTS原理分析：https://www.atatech.org/articles/90248?spm=ata.13261165.0.0.3ce567d4gm9nqY

官方网站：http://mw.alibaba-inc.com/product-schedulerx2.0.html?spm=a1zco.detail-schedulerx.2851214.14.3b7e2f41T2BNP4

用户文档：https://yuque.antfin-inc.com/schedulerx/igu2vh/hbdgke

## DTS - 分布式任务调度

包括定时触发和非定时（API方式）两种，非定时可以看做是定时触发的一种特例。

### 一、原理分析

#### 1、架构

客户端：DTS的应用服务器

服务端：DTS的调度管理服务器，负责任务的触发、调度等

web控制台：可以用于配置任务、通过API管理任务。

zk服务器：用于服务器注册，DTS客户端不直接依赖zk，而是通过web服务器进行拿到服务端的机器列表，进而进行通信。

Mysql数据库：DTS的数据都存储在Mysql数据库中。

![image-20200109110804950](/Users/baola/Library/Application Support/typora-user-images/image-20200109110804950.png)

### 二、快速上手使用

参考：https://yuque.antfin-inc.com/schedulerx/igu2vh/hbdgke

两步：

1. 继承JobProcess抽象类，重载process方法
2. 在控制台创建任务

```java
package com.alibaba.paimaitest.image.task;

import akka.stream.impl.fusing.Map;
import com.alibaba.common.logging.Logger;
import com.alibaba.common.logging.LoggerFactory;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.paimaitest.image.service.ImageAlgoService;
import com.alibaba.paimaitest.image.service.impl.ImageAlgoServiceImpl;
import com.alibaba.schedulerx.worker.domain.JobContext;
import com.alibaba.schedulerx.worker.processor.JavaProcessor;
import com.alibaba.schedulerx.worker.processor.ProcessResult;
import org.json.JSONObject;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

@Component
public class PageCheckTask extends JavaProcessor {
    protected static Logger logger = LoggerFactory.getLogger(PageCheckTask.class);
//    private List<String[]> checkParams = new ArrayList<>();
    @Autowired
    private ImageAlgoServiceImpl imageAlgoService;

    @Override
    public ProcessResult process(JobContext context) throws Exception {
        ProcessResult processResult = new ProcessResult(true);
//        String[] jobParams = context.getJobParameters().split(";");
//        if(this.checkParams==null) {
//            checkParams = new ArrayList<>(jobParams.length);
//        }
//        try {
//            int i=0;
//            for (String jobParam : jobParams) {
//                checkParams.set(i++,jobParam.split(","));
//            }
//            for(String[] checkParam: checkParams) {
//                String result = imageAlgoService.checkPageByUrl(checkParam[0], checkParam[1]);
//                if(result.startsWith("pass")) {
//                    checkResult = true;
//                } else {
//                    checkResult = false;
//                }
//            }
//        } catch (Exception e) {
//            checkResult = false;
//        }
        try {
            String result = imageAlgoService.checkPageByUrl(context.getJobParameters(),"blankbox");
            if (result.startsWith("pass")) {
                processResult.setStatus(true);
                processResult.setResult("pass");
            } else if (result.startsWith("unpass")){
                processResult.setStatus(false);
                processResult.setResult("unpass");
            } else if (result.startsWith("wrong")){
                processResult.setStatus(false);
                processResult.setResult("wrong");
            } else {
                processResult.setStatus(false);
                processResult.setResult("unknown");
            }
        } catch (Exception e) {
            processResult.setStatus(false);
            processResult.setResult("exception" + e.getMessage());
        }

        return processResult;
    }
}
```

### 其他

#### cron语法

参考https://www.cnblogs.com/linjiqin/p/3178452.html