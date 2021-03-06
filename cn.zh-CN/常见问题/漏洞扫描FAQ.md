# 漏洞扫描FAQ {#concept_fbd_xdc_zdb .concept}

-   [漏洞扫描会扫描系统层面和应用层面的漏洞吗？](#)
-   [漏洞实时扫描是如何实现的？](#)
-   [如何处理连接阿里云官方Yum源超时？](#)
-   [修复漏洞时，提示“token校验失败”，应该如何处理？](#)
-   [态势感知无法验证系统漏洞修复时，应该如何处理？](#)
-   [如何获取当前软件版本漏洞信息？](#)
-   [如何将Ubuntu 14.04系统的3.1\*内核升级至4.4内核？](#)
-   [为什么内核漏洞升级修复后，态势感知仍然提示存在漏洞？](#)
-   [为什么态势感知管理控制台中某些漏洞提示无更新？](#)
-   [如何手动检测服务器上的Linux软件漏洞？](#)
-   [为什么漏洞修复后手动验证没有反应？](#)
-   [为什么单击生成修复命令后，生成的修复命令为空？](#)
-   [为什么进行漏洞回滚操作会失败？](#)

## 漏洞扫描会扫描系统层面和应用层面的漏洞吗？ {#section_fxb_32c_zdb .section}

是的，漏洞扫描会扫描系统漏洞（主机上系统层级漏洞）和Web漏洞（应用层漏洞）。

## 漏洞实时扫描是如何实现的？ {#section_tfg_qzm_q2b .section}

漏洞扫描每天会收集用户资产中新增的URL，然后在凌晨对这些新增的URL进行扫描。同时，还会扫描之前被曝出的漏洞，验证这些漏洞有没有被修复。实时主要指实时获取URL，等到凌晨再进行扫描。

## 如何处理连接阿里云官方Yum源超时？ {#section_xvc_3lp_r2b .section}

当连接阿里云官方Yum源超时时，会出现类似如下的报错信息：

```
[Errno 12] Timeout on http://mirrors.aliyun.com/centos/6/os/x86_64/repodata/repomd.xml: (28, 'connect() timed out!')
```

这种情况下，请检查您本机的DNS设置是否正常，并稍作等待。如果一段时间后仍无法解决，请提交工单，通过售后服务进行排查。

## 修复漏洞时，提示“token校验失败”，应该如何处理？ {#section_k1n_kck_s2b .section}

当您在云盾态势感知控制台执行某项操作，却收到“token校验失效”的提示时，您可以刷新当前页面，重新登录云盾态势感知控制台。

**说明：** 您可按Ctrl+F5，强制刷新当前浏览器页面。

## 态势感知无法验证系统漏洞修复时，应该如何处理？ {#section_jcq_bdk_s2b .section}

当态势感知无法验证系统漏洞修复时，请按照以下步骤进行排查：

1.  查看漏洞的版本信息。
2.  确认系统是否使用阿里云的官方源。
3.  确认系统升级后是否执行过验证操作。

    **说明：** 升级内核需重启才能生效。

4.  确认选择修复的版本不低于态势感知建议的版本。

如果以上方案未能解决问题，建议您升级操作系统。

## 如何获取当前软件版本漏洞信息？ {#section_o4g_f25_q2b .section}

态势感知通过匹配您的系统软件版本和存在漏洞（CVE 漏洞）的软件版本，判断您的服务器是否存在软件漏洞。因此，您可以通过以下方式查看当前软件版本的漏洞信息：

-   **在态势感知中查看当前软件漏洞信息**

    您可以登录[云盾态势感知控制台](https://yundun.console.aliyun.com/?p=sas)，并前往**漏洞管理** \> **主机漏洞**页面，查看态势感知在您的服务器上检测到的系统软件漏洞信息。关于态势感知对系统软件漏洞的各项参数说明，请参考[系inux统软件漏洞参数说明](../../../../../intl.zh-CN/用户指南/漏洞管理/Linux软件漏洞参数说明.md#)。

-   **在您的服务器上查看当前软件版本信息**

    您也可以在服务器上直接查看当前软件版本信息：

    -   **CentOS系统**

        执行`rpm -qa | grep xxx`命令查看软件版本信息。其中，`xxx`为软件包名。例如，执行`rpm -qa | grep bind-libs`命令查看服务器上的bind-libs软件版本信息。

    -   **Ubuntu 和 Debian系统**

        执行`dpkg-query -W -f '${Package} -- ${Source}\n' | grep xxx`命令查看软件版本信息。其中，`xxx`为软件包名。例如，执行`dpkg-query -W | grep bind-libs`命令查看服务器上的bind-libs软件版本信息。

        **说明：** 如果显示无法找到该软件包，您可以执行`dpkg-query –W`查看服务器上所安装的软件列表。

    通过以上命令获取您服务器上的软件版本信息后，您可以将得到的软件版本信息与态势感知系统软件漏洞中检测到的相关漏洞的说明信息进行对比。漏洞说明参数中的“软件”和“命中”分别指当前软件版本和漏洞的匹配命中原因。

    **说明：** 如果升级后旧版本软件包还有残留信息，这些旧版本信息可能仍会被态势感知检测收集，并作为漏洞上报。如果确认是由于这种情况触发的漏洞告警，建议您选择忽略该漏洞。您也可以使用`yum remove`或者`apt-get remove`命令删除旧版本的软件包。删除前，请务必确认所有业务和应用都不再使用该旧版本软件。


## 如何将Ubuntu 14.04系统的3.1\*内核升级至4.4内核？ {#section_s4g_f25_q2b .section}

系统内核升级有一定风险，强烈建议您参考[系统软件漏洞修复最佳实践](../../../../../intl.zh-CN/用户指南/漏洞管理/服务器软件漏洞修复.md#)中的方法进行升级。

1.  执行`uname -av`命令，确认当前服务器的系统内核版本是否为3.1\*。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13674/15511067328522_zh-CN.png)

2.  执行以下命令，查看是否已有最新的内核Kernel更新包。

    -   `apt list | grep linux-image-4.4.0-94-generic`
    -   `apt list | grep linux-image-extra-4.4.0-94-generic`
    如果没有相关更新，您可执行`apt-get update`命令获取最新的更新包。

3.  执行以下命令，进行内核升级。
    -   `apt-get update && apt-get install linux-image-4.4.0-94-generic`
    -   `apt-get update && apt-get install linux-image-extra-4.4.0-94-generic`
4.  更新包安装完成后，重启服务器完成内核加载。
5.  服务器重启后，执行以下命令验证内核升级是否成功。
    -   执行`uname -av`命令查看当前调用内核。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13674/15511067328523_zh-CN.png)

    -   执行`dpkg -l | grep linux-image`命令查看当前内核包情况。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13674/15511067328524_zh-CN.png)


## 为什么内核漏洞升级修复后，态势感知仍然提示存在漏洞？ {#section_rpg_f25_q2b .section}

由于内核升级比较特殊，一般都会有旧版本信息残留。确认该漏洞告警是由于旧版本信息残留造成后，您可以选择忽略该漏洞告警，或者删除旧版本的残留信息。

1.  确认内核升级完成后，通过执行`uname –av`命令和`cat /proc/version`命令查看当前内核版本，确保当前使用的内核版本已符合漏洞说明命中条件中的要求。
2.  执行`cat /etc/grub.conf`命令查看配置文件，确认当前已经调用最新的内核版本。
3.  由于系统软件漏洞主要通过软件版本匹配进行检测，如果系统中依然存在旧版本的内核rpm安装包，则态势感知仍会检测到该信息并进行漏洞告警。您需要确认当前系统中已经没有旧版本rpm安装包残留。如果有，您可以卸载旧版本安装包。
4.  卸载旧版本安装包前，请务必确认当前系统已经使用新内核。强烈建议您在卸载旧版本内核安装包前，为您的系统创建快照，以便在发生异常情况时进行恢复。

**说明：** 如果由于某些原因不想卸载旧版本内核，在您确认系统已经调用新内核后，可以登录[云盾态势感知控制台](https://yundun.console.aliyun.com/?p=sas)，在**漏洞管理** \> **主机漏洞**页面忽略该漏洞（在该漏洞的漏洞处理页面，单击操作栏中的**忽略**），暂时忽略该系统漏洞的告警提醒。

## 为什么态势感知管理控制台中某些漏洞提示无更新？ {#section_qqg_5f5_q2b .section}

-   您在对某些漏洞进行更新修复时，可能收到以下提示：

```
Package xxx already installed and latest version
Nothing to do
```

或者

```
No Packages marked for Update
```

    这种情况是由于官方更新源暂时还未提供更新，请您等待官方更新源的更新。

    目前已知未更新的软件包包括：

    -   Gnutls
    -   Libnl
    -   Mariadb
-   您已经更新到了最新的软件包，但仍然无法满足态势感知管理控制台中报告的软件版本条件。

    请检查您的操作系统版本是否在官方的支持范围中。例如，截止到 2017 年 9 月 1 日，官方已经停止对CentOS 6.2-6.6/7.1等版本的支持。这种情况下，建议您在态势感知管理控制台中忽略该漏洞（该漏洞对您服务器的风险可能依然存在），或者升级您的服务器操作系统。


## 如何手动检测服务器上的Linux软件漏洞？ {#section_xpg_f25_q2b .section}

如果您需要手动验证您服务器上的系统软件漏洞，您可以参考[如何手动检测Linux软件漏洞](intl.zh-CN/常见问题/如何手动检测系统软件漏洞？.md#)。

建议您使用态势感知的系统软件漏洞功能定期自动检测服务器上的系统软件漏洞，以便及时发现漏洞。

## 为什么漏洞修复后手动验证没有反应？ {#section_ffg_my4_r2b .section}

您在服务器上手动执行态势感知生成的系统软件漏洞修复命令，将相关的系统软件成功升级到新的版本，并且该版本已符合态势感知控制台漏洞管理页面的描述要求；然而，当您在态势感知管理控制台的漏洞处理页面，选择相应的漏洞，单击**验证**，该漏洞的状态没有正常更新为已修复。

您可以使用以下方法进行排查，解决该问题：

-   检查漏洞扫描等级

    登录[云盾态势感知控制台](https://yundun.console.aliyun.com/?p=sas)，前往**设置** \> **告警设置**页面，查看**漏洞管理**区域中的**我关注的等级**选项。

    如果对应的扫描等级没有勾选，则相应等级的漏洞数据不会自动更新。

-   态势感知Agent版本过低

    如果您服务器上的态势感知Agent版本过低，则可能不支持漏洞扫描功能。如果您的态势感知Agent没有正常自动更新，建议您参考[安装Agent](../../../../../intl.zh-CN/用户指南/接入态势感知/安装Agent.md#)手动安装最新版态势感知Agent。

-   态势感知Agent离线

    如果您服务器上的态势感知Agent显示为离线，您将无法通过漏洞管理的验证功能对您的服务器进行验证。建议您参考[Agent 离线排查](../../../../../intl.zh-CN/用户指南/接入态势感知/Agent离线排查.md#)进行排查，确保您服务器上的态势感知Agent在线。


## 为什么单击生成修复命令后，生成的修复命令为空？ {#section_rmt_m1p_r2b .section}

当您在态势感知漏洞管理中，选择一个Linux软件漏洞，单击**生成修复命令**时，生成的漏洞修复命令为空。您可以使用以下方法进行排查，解决该问题：

-   检查漏洞扫描等级

    登录[云盾态势感知控制台](https://yundun.console.aliyun.com/?p=sas)，前往**设置** \> **告警设置**页面，查看**漏洞管理**区域中的**我关注的等级**选项。

    如果对应的扫描等级没有勾选，则相应等级的漏洞数据不会自动更新。

-   态势感知Agent版本过低

    如果您服务器上的态势感知Agent版本过低，则可能不支持漏洞扫描功能。如果您的态势感知Agent没有正常自动更新，建议您参考[安装Agent](../../../../../intl.zh-CN/用户指南/接入态势感知/安装Agent.md#)手动安装最新版态势感知Agent。

-   态势感知Agent离线

    如果您服务器上的态势感知Agent显示为离线，您将无法通过漏洞管理的验证功能对您的服务器进行验证。建议您参考[Agent 离线排查](../../../../../intl.zh-CN/用户指南/接入态势感知/Agent离线排查.md#)进行排查，确保您服务器上的态势感知Agent在线。


## 为什么进行漏洞回滚操作会失败？ {#section_yh5_l2p_r2b .section}

当通过态势感知漏洞管理功能对某个漏洞进行回滚操作时，提示回滚失败，您可以使用以下方法排查问题：

1.  确认您的服务器的态势感知Agent处于在线状态。如果您的服务器显示离线，请参考[Agent 离线排查](../../../../../intl.zh-CN/用户指南/接入态势感知/Agent离线排查.md#)进行排查。
2.  确认您服务器上该漏洞的相关文件是否已被手工修改或者删除。

    **说明：** 如果在漏洞修复后相关文件已被手动修改或者删除，态势感知为了防止误改动您的文件，不会对该漏洞的相关文件进行回滚。


