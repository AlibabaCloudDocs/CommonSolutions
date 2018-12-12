# JSON数据从MongoDB迁移到MaxCompute最佳实践 {#concept_z2b_fl2_xfb .concept}

本文为您介绍如何利用DataWorks数据集成直接从MongoDB提取JSON字段到MaxCompute。

## 数据及账号准备 {#section_qv1_nm2_xfb .section}

首先您需要将数据上传至您的MongoDB数据库。本例中使用阿里云的[云数据库 MongoDB 版](../../../../cn.zh-CN/单节点快速入门/开始使用MongoDB.md#)，网络类型为VPC（需申请公网地址，否则无法与DataWorks默认资源组互通），测试数据如下。

```

{
    "store": {
        "book": [
             {
                "category": "reference",
                "author": "Nigel Rees",
                "title": "Sayings of the Century",
                "price": 8.95
             },
             {
                "category": "fiction",
                "author": "Evelyn Waugh",
                "title": "Sword of Honour",
                "price": 12.99
             },
             {
                 "category": "fiction",
                 "author": "J. R. R. Tolkien",
                 "title": "The Lord of the Rings",
                 "isbn": "0-395-19395-8",
                 "price": 22.99
             }
          ],
          "bicycle": {
              "color": "red",
              "price": 19.95
          }
    },
    "expensive": 10
}
```

登录MongoDB的DMS控制台，本例中使用的数据库为admin，集合为userlog，您可以在查询窗口使用db.userlog.find\(\).limit\(10\)命令查看已上传好的数据，如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64919/154458079032875_zh-CN.png)

此外，需提前在数据库内新建用户，用于DataWorks添加数据源。本例中使用命令`db.createUser({user:"bookuser",pwd:"123456",roles:["root"]})`，新建用户名为bookuser，密码为123456，权限为root。

## 使用DataWorks提取数据到MaxCompute {#section_sk4_zq2_xfb .section}

1.  新增MongoDB数据源

    进入DataWorks[数据集成](../../../../cn.zh-CN/使用指南/数据集成/数据集成简介/数据集成概述.md#)控制台，新增[MongoDB类型](../../../../cn.zh-CN/使用指南/数据集成/数据源配置/配置MongoDB数据源.md#)数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64919/154458079032876_zh-CN.png)

    具体参数如下所示，测试数据源连通性通过即可点击**完成**。由于本文中MongoDB处于VPC环境下，因此**数据源类型**需选择**有公网IP**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64919/154458079032877_zh-CN.png)

    访问地址及端口号可通过在[MongoDB管理控制台](https://mongodb.console.aliyun.com)点击实例名称获取，如下图所示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64919/154458079032878_zh-CN.png)

2.  新建数据同步任务

    在DataWorks上新建[数据同步类型节点。](../../../../cn.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置OSS Reader.md#)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64919/154458079032879_zh-CN.png)

    新建的同时，在DataWorks新建一个[建表任务](../../../../cn.zh-CN/使用指南/数据开发/表管理.md#)，用于存放JSON数据，本例中新建表名为mqdata。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62284/154458079131544_zh-CN.png)

    表参数可以通过图形化界面完成。本例中mqdata表仅有一列，类型为string，列名为MQ data。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62284/154458079131545_zh-CN.png)

    完成上述新建后，您可以在图形化界面进行数据同步任务参数的初步配置，如下图所示。选择目标数据源名称为odps\_first，选择目标表为刚建立的mqdata。数据来源类型为MongoDB，选择我们刚创建的数据源mongodb\_userlog。完成上述配置后，**点击转换为脚本**，跳转到脚本模式。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64919/154458079132880_zh-CN.png)

    脚本模式代码示例如下。

    ```
    
    {
        "type": "job",
        "steps": [
            {
                "stepType": "mongodb",
                "parameter": {
                    "datasource": "mongodb_userlog",
     //数据源名称
                    "column": [
                        {
                            "name": "store.bicycle.color", //JSON字段路径，本例中提取color值
                            "type": "document.document.string" //本栏目的字段数需和name一致。假如您选取的JSON字段为一级字段，如本例中的expensive，则直接填写string即可。
                        }
                    ],
                    "collectionName //集合名称": "userlog"
                },
                "name": "Reader",
                "category": "reader"
            },
            {
                "stepType": "odps",
                "parameter": {
                    "partition": "",
                    "isCompress": false,
                    "truncate": true,
                    "datasource": "odps_first",
                    "column": [
                              "mqdata"  //MaxCompute表列名
                    ],
                    "emptyAsNull": false,
                    "table": "mqdata"
                },
                "name": "Writer",
                "category": "writer"
            }
        ],
        "version": "2.0",
        "order": {
            "hops": [
                {
                    "from": "Reader",
                    "to": "Writer"
                }
            ]
        },
        "setting": {
            "errorLimit": {
                "record": ""
            },
            "speed": {
                "concurrent": 2,
                "throttle": false,
                "dmu": 1
            }
        }
    }
    ```

    完成上述配置后，点击运行接即可。运行成功日志示例如下所示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62284/154458079131550_zh-CN.png)


## 结果验证 {#section_cwq_bcf_xfb .section}

在您的[业务流程](../../../../cn.zh-CN/使用指南/数据开发/业务流程/业务流程介绍.md#)中新建一个ODPS SQL节点。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62284/154458079131551_zh-CN.png)

您可以输入`SELECT * from mqdata;`语句，查看当前mqdata表中数据。当然这一步您也可以直接在[MaxCompute客户端](../../../../cn.zh-CN/工具及下载/客户端.md#)中输入命令运行。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64919/154458079132881_zh-CN.png)

