# 1.开发环境搭建

## 1.1 安装Mysql数据库

* 安装步骤（重点部分）
  * 1.执行`MySQL.exe`安装文件
  * 2.选择只安装数据库![img](https://img.mukewang.com/wiki/6041f8d408552ef807840561.jpg)

MySQL帐户密码加密方式选择传统（强烈建议），否则新的加密方式，导致很多运维工具和老的项目无法连接到MySQL数据库，切记

![img](https://img.mukewang.com/wiki/6041f9170834507f07840561.jpg)

* ### 导入EMOS系统数据库

  * 1.从本课程的GIT项目中下载到`emos.sql`文件

  * 2.在`Navicat`(其他工具也可以,我用的DBeaver)上面新建`emos`数据库

  * 3.然后在`emos`数据库上右键选择执行`SQL`文件

  * 4.刷新`emos`数据库

```mysql
mysql> show tables;
+--------------------------+
| Tables_in_emos           |
+--------------------------+
| qrtz_blob_triggers       |
| qrtz_calendars           |
| qrtz_cron_triggers       |
| qrtz_fired_triggers      |
| qrtz_job_details         |
| qrtz_locks               |
| qrtz_paused_trigger_grps |
| qrtz_scheduler_state     |
| qrtz_simple_triggers     |
| qrtz_simprop_triggers    |
| qrtz_triggers            |
| sys_config               |
| tb_action                |
| tb_checkin               |
| tb_city                  |
| tb_dept                  |
| tb_face_model            |
| tb_holidays              |
| tb_meeting               |
| tb_module                |
| tb_permission            |
| tb_role                  |
| tb_user                  |
| tb_workday               |
+--------------------------+
24 rows in set (0.00 sec)
```





## 1.2 安装MongoDB数据库