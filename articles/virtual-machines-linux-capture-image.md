<properties
	pageTitle="捕获运行 Linux 的虚拟机的映像 | Windows Azure"
	description="了解如何使用经典部署模型和 Azure CLI 捕获运行 Linux 的 Azure 虚拟机 (VM) 的映像。"
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines"
	ms.date="07/16/2015"
	wacn.date="11/12/2015"/>


# 如何捕获一台 Linux 虚拟机以用作模板

本文将展示如何捕获运行 Linux 的 Azure 虚拟机，以便你可以将其像模板一样用来创建其他虚拟机。此模板包括 OS 磁盘和附加到虚拟机的数据磁盘。它不包括网络配置，因此你在使用此模板创建其他虚拟机时需要配置网络配置。

Azure 将此模板视为一个映像并将其存储在“映像”下。这也是你上载和存储任何映像的地方。有关映像的详细信息，请参阅[关于 Azure 中的虚拟机映像][]。

## 开始之前

这些步骤假定你已使用经典部署模式创建了 Azure 虚拟机并配置了操作系统，包括附加任何数据磁盘。如果你尚未执行此操作，请参阅以下说明：


## 捕获虚拟机

1. 使用所选 SSH 客户端连接到虚拟机。有关详细信息，请参阅[如何登录到运行 Linux 的虚拟机][]。

2. 在 SSH 窗口中，键入以下命令。请注意，`waagent` 的输出结果可能会因此实用程序的版本而略有差异：

	`sudo waagent -deprovision`

	此命令将尝试清除系统并使其适用于重新预配。此操作执行以下任务：

	- 删除 SSH 主机密钥（如果在配置文件中 Provisioning.RegenerateSshHostKeyPair 为“y”）
	- 清除 /etc/resolv.conf 中的 nameserver 配置
	- 从 /etc/shadow 中删除 `root` 用户的密码（如果在配置文件中 Provisioning.DeleteRootPassword 为“y”）
	- 删除缓存的 DHCP 客户端租赁
	- 将主机名重置为 localhost.localdomain
	- 删除上次预配的用户帐户（从 /var/lib/waagent 获得）**和关联数据**。

	>[AZURE.NOTE]取消预配会删除文件和数据，目的是使映像“一般化”。仅在需要用作新映像模板的虚拟机上运行此命令。无法确保映像中的所有敏感信息均已清除，或者说无法确保该映像适合再分发给第三方。


3. 键入 **y** 继续。添加 `-force` 参数即可免除此确认步骤。

4. 键入 **Exit** 关闭 SSH 客户端。


	>[AZURE.NOTE]后续步骤假定你已在客户端计算机上[安装 Azure CLI](/documentation/articles/xplat-cli-install)。以下所有步骤也可以在[管理门户][]中执行。

5. 从客户端计算机中打开 Azure CLI 并登录到你的 Azure 订阅。有关详细信息，请阅读[从 Azure CLI 连接到 Azure 订阅](/documentation/articles/xplat-cli-connect)。

6. 请确保你是在服务管理模式下：

	`azure config mode asm`

7. 使用以下命令关闭已在上述步骤中预配的虚拟机：

	`azure vm shutdown <your-virtual-machine-name>`

	>[AZURE.NOTE]你可以使用 `azure vm list` 找出在订阅中创建的所有虚拟机

8. 在虚拟机停止后，使用以下命令捕获映像：

	`azure vm capture -t <your-virtual-machine-name> <new-image-name>`

	键入你需要的映像名称以替换 _new-image-name_。此命令创建通用 OS 映像。`-t` 子命令将删除原始虚拟机。

9.	新映像现在会出现在映像列表中，可以用于配置任何新的虚拟机。你可以使用以下命令来查看它：

	`azure vm image list`

	在“管理门户”中，它会显示在“映像”列表中。[][]

	![成功捕获映像](./media/virtual-machines-linux-capture-image/VMCapturedImageAvailable.png)


## 后续步骤
该映像已就绪，可用作创建虚拟机的模板了。你可以使用 Azure CLI 命令 `azure vm create` 并提供刚创建的映像名称。有关此命令的详细信息，请参阅[将 Azure CLI 用于服务管理 API](/documentation/articles/virtual-machines-command-line-tools)。此外，你也可以使用[管理门户][]来创建自定义虚拟机，只需使用**“从库中”**方法并选择你刚创建的映像即可。如需更多详细信息，请参阅[如何创建自定义虚拟机][]。

**另请参阅：**[Azure Linux 代理用户指南](/documentation/articles/virtual-machines-linux-agent-user-guide)


[管理门户]: http://manage.windowsazure.cn
[如何登录到运行 Linux 的虚拟机]: /documentation/articles/virtual-machines-linux-how-to-log-on
[关于 Azure 中的虚拟机映像]: http://msdn.microsoft.com/zh-cn/library/azure/dn790290.aspx
[如何创建自定义虚拟机]: /documentation/articles/virtual-machines-create-custom
[How to Attach a Data Disk to a Virtual Machine]: /documentation/articles/storage-windows-attach-disk
[如何创建运行 Linux 的虚拟机]: /documentation/articles/virtual-machines-linux-tutorial

<!---HONumber=79-->