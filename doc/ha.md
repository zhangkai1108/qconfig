# 可用

对于可用方面，考虑如下情况

1. 单台Server下线

   在单台Server下线的情况下，不会产生额外影响。Client会自动重连到其他Server上。

2. 整个Server集群下线

   整个Server不可用时，会出现Client无法请求配置文件，同时无法接受到配置更新。Admin无法查看Consumer，无法进行推送操作。

   - 降级

     已经读取过配置的机器会使用已有的缓存。新扩容的机器可以在项目资源目录下新建qconfig_test目录来使用本地文件

3. 单台admin下线

   无影响

4. admin集群全体下线

   无法进行更新、发布等配置文件操作。对Server和Client无影响。

5. 数据库宕机

   admin无法更新配置。Server无法获取新的配置文件，但是已经完成读取且缓存的配置不会收到影响。

   - 原因

     在Server中，为了获得更高的性能，所有配置文件从数据库中读取以后，会被缓存到磁盘中。同时，还会将热点数据缓存到内存中。所以，如果配置文件已经从数据库中读取，将会使用磁盘中的文件，而不会再去数据库中获取。