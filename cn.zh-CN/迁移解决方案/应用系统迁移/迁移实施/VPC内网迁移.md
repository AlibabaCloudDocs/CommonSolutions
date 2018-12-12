# VPC内网迁移 {#concept_dxp_5kk_yfb .concept}

如果您能直接从自建机房（Integrated Data Center，IDC）、虚拟机环境或者云主机访问某一阿里云地域下的专有网络VPC，建议您使用源服务器与VPC内网互连的迁云方案。VPC内网迁云能获得比通过公网更快速更稳定的数据传输效果，提高迁云工作效率。

您可以使用[高速通道](https://help.aliyun.com/product/27782.html)和[VPN](https://help.aliyun.com/document_detail/65310.html)打通VPC内网的方案，然后使用迁云工具来进行VPC内网迁移。

## 背景信息 {#section_a2f_3lp_yfb .section}

迁云工具1.2.8以上版本支持VPC内网迁移。要完成VPC内网迁移，需要将client\_data的net\_mode字段配置为1或2。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65304/154457884533339_zh-CN.png)

net\_mode的参数说明如下：

-   0：默认为0，表示公网迁移，需要待迁移系统支持公网，数据传输阶段会走公网。
-   1：表示待迁移支持访问指定VPC内网；迁移过程分阶段1、2、3，阶段2数据传输在当前系统中进行，同时阶段1和3需要在其他公网环境系统中进行。
-   2：表示待迁移系统同时支持公网和支持访问特定VPC内网；跟一般操作过程相同，但数据传输阶段会走内网。

不同的参数设置有不同的迁移方式。

## 方式一 {#section_l1v_55p_yfb .section}

当net\_mode设置为1时，参考以下步骤。

1.  在外网环境中创建中转实例。

    1.  登录某个有外网的系统A，并下载迁云工具。
    2.  配置user\_config.json文件。
    3.  配置client\_data文件的指定目标vpc\_id、vswitch\_id、zone\_id等信息。详情请参见[配置client\_data文件到指定的VPC环境](#)。
    4.  运行迁云工具，直到提示`Stage 1 Is Done!`。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65304/154457884533354_zh-CN.png)

2.  在VPC内网环境中传输系统数据。

    1.  登录需要迁移的VPC内网环境系统B。
    2.  将系统A的迁云工具复制到系统B。

        **说明：** 系统B中的user\_config.json和client\_data文件必须要与系统A迁云工具中的一致。

    3.  运行迁云工具，直到提示`Stage 2 Is Done!`。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65304/154457884533355_zh-CN.png)

3.  在外网环境中创建镜像。

    1.  回到系统A，将系统B的迁云工具复制到系统A。

        **说明：** user\_config.json和client\_data文件必须与系统A迁云工具中的一致。

    2.  运行迁云工具，直到提示`Stage 3 Is Done!`以及迁云完成。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65304/154457884533356_zh-CN.png)


## 方式二 {#section_jtg_jwp_yfb .section}

当net\_mode设置为2时，参考以下步骤。

1.  登录待迁移的系统中，并下载迁云工具。
2.  配置user\_config.json文件。
3.  配置client\_data文件的指定目标vpc\_id、vswitch\_id、zone\_id等信息。详情请参见[配置client\_data文件到指定的VPC环境](#)。
4.  运行迁云工具直至迁云完成。

**说明：** 迁移过程中，迁移数据阶段通过VPC内网传输，其他阶段通过公网传输。

## 配置client\_data文件 {#clientData .section}

按照以下步骤配置client\_data文件到指定的VPC环境。

1.  配置vpc\_id为指定VPC的ID。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65304/154457884533357_zh-CN.png)

2.  配置vswitch\_id为指定交换机的ID。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65304/154457884533358_zh-CN.png)

3.  配置zone\_id为指定可用区相关的ID。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65304/154457884633359_zh-CN.png)

4.  （可选）配置security\_group\_id为指定安全组的ID。如果您没有手动配置，则会自动创建。

    **说明：** 指定安全组需要开放入方向的8080、8703端口。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65304/154457884633360_zh-CN.png)


