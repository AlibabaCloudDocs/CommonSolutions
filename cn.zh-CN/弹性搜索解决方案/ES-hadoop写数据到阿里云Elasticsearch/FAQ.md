# FAQ {#concept_ykx_kpn_h2b .concept}

## ClassNotFoundException异常 {#section_r5g_dv4_f2b .section}

遇到找不到EsOutputFormat所在类，导致的ClassNotFoundException异常：

```
java.lang.NoClassDefFoundError: org/elasticsearch/hadoop/mr/EsOutputFormat
```

解决办法，使用maven-assembly-plugin插件，把第三方库打到jar包中。

## 连接不到Elasticsearch集群 {#section_f4j_zpn_h2b .section}

-   第一个原因：没有配置X-PACK 的用户名和密码，加上以下两行配置：

    ```
    conf.set("es.net.http.auth.user", "你的X-PACK用户名");
    conf.set("es.net.http.auth.pass", "你的X-PACK密码");
    ```

-   第二个原因：EMR集群和Elasticsearch集群网络不通，在创建集群的时尽量选择同一区域，比如EMR集群在华东1区，Elasticsearch集群也在华东1区，事先用Ping命令测试。
-   第三个原因：一般TCP端口\(比如使用Java客户端\)是9300，ES-Hadoop中使用的仍然是9200端口，不要设置错了。

## Reduce过程中格式错误 {#section_jwn_bqn_h2b .section}

注意测试文件中每一行都是一个JSON，在设置中加上：

```
conf.set("es.input.json", "yes");
```

否则会出现解析文件格式异常。

