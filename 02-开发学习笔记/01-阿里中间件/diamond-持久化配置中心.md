[官方文档](http://mw.alibaba-inc.com/products/diamondserver/_book/programer-magazine-configserver.html?spm=a1zco.8292286.0.0.27ce2588nFbSMV)

### 一、overview

diamond是配置中心，提供持久化管理和动态配置推送服务。 配置的持久化管理，是Diamond与淘宝内部另外一个软负载配置产品ConfigServer的主要区别。应用方发布的配置会通过持久化存储保存，与发布者的生命周期无关。 <font color=dark>实时的动态配置推送，则是Diamond的核心功能</font>，在淘宝内部有很多应用场景，如数据库动态切换和扩容，业务系统开关配置运行时变更等。

#### 应用场景

比较适合低频变更的动态配置管理

- 业务系统：预案、降级开关
- TDDL：动态切换数据库



### 和configServer的对比

- configServer非持久化，diamond持久化
- configServer主动推，diamond推拉结合



