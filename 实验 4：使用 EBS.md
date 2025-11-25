## 实验概览

![架构图](https://labs.vocareum.com/web/4516851/4615947.0/ASNLIB/public/docs/lang/zh-cn/images/lab-scenario.jpeg)

本实验着重介绍 Amazon Elastic Block Store (Amazon EBS)，这是一种适用于 Amazon EC2 实例的重要底层存储机制。在本实验中，您将了解如何创建 Amazon EBS 卷，将其关联到实例，将文件系统应用到卷，然后创建快照备份。

## 涵盖的主题

本实验结束时，您将能够：

- 创建 Amazon EBS 卷
- 将卷关联并挂载到 EC2 实例
- 创建卷的快照
- 通过快照创建新的卷
- 将新卷关联并挂载到 EC2 实例

## 时长

完成本实验大约需要 **30 分钟**。

## AWS 服务限制

在本实验环境中，对 AWS 服务和服务操作的访问权限可能仅限于完成实验说明所需的服务和操作。如果您尝试访问其他服务，或执行本实验所述之外的操作，则可能会遇到错误。

### 什么是 Amazon Elastic Block Store？

**Amazon Elastic Block Store (Amazon EBS)** 为 Amazon EC2 实例提供持久性存储。Amazon EBS 卷连接到网络上，并独立于实例生命周期持续存在。Amazon EBS 卷是可用性和可靠性都非常高的存储卷，可用作 Amazon EC2 实例的启动分区，或作为标准块储存设备关联到正在运行的 Amazon EC2 实例。

当用作启动分区时，Amazon EC2 实例可在停止后重新启动，因此您可以仅为维护实例状态时使用的存储资源付费。与本地 Amazon EC2 实例存储相比，Amazon EBS 卷的持久性大大提高，因为 Amazon EBS 卷是在后端自动复制的（在单个可用区中）。

希望进一步提高持久性的客户可以使用 Amazon EBS 为卷创建一致性时间点快照，这些快照随后将存储在 Amazon Simple Storage Service (Amazon S3) 中，并在多个可用区中自动复制。这些快照可用作新 Amazon EBS 卷的起点，并能确保数据的长期持久性。您也可以与同事和其他 AWS 开发人员轻松共享这些快照。

本实验指南将逐步介绍 Amazon EBS 的基本概念，但只会进行简单的概述。有关详细信息，请参阅 [Amazon EBS 文档](http://aws.amazon.com/ebs/)。

### Amazon EBS 卷特性

Amazon EBS 卷具有以下特性：

- **持久性存储**：卷生命周期独立于任何特定的 Amazon EC2 实例。
    
- **通用**：Amazon EBS 卷是未格式化的原始块储存设备，可以在任何操作系统中使用。
    
- **高性能**：Amazon EBS 卷相当于或优于本地 Amazon EC2 驱动器。
    
- **高可靠性**：Amazon EBS 卷在可用区中具有内置冗余性。
    
- **专为弹性而设计**：Amazon EBS 的 AFR（年故障率）在 0.1% 到 1% 之间。
    
- **可变大小**：卷大小范围从 1 GB 到 16 TB。
    
- **易于使用**：可以轻松创建、关联、备份、还原和删除 Amazon EBS 卷。
    

## 访问 AWS 管理控制台

1. 在本说明的顶部，选择 **Start Lab**（开始实验）。
    
    - 实验会话开始。
        
    - 页面顶部会显示一个计时器，指示会话的剩余时间。
        
        **提示**：只要计时器未达到 0:00，您就可以随时再次选择 **Start Lab**（开始实验）来恢复会话时长。
        
    - 请等待左上角的 AWS 链接右侧的圆形图标变为绿色后再继续。
        

2. 要连接到 AWS 管理控制台，请选择左上角的 **AWS** 链接。
    
    - 新的浏览器标签页将打开，并转到控制台。
        
        **提示**：如果未打开新的浏览器标签页，浏览器顶部通常会显示一个横幅或图标，并附带一条消息，提示浏览器阻止该网站打开弹出窗口。选择该横幅或图标，然后选择 **Allow pop-ups**（允许弹出窗口）。
        
3. 排列 AWS 管理控制台标签页，使其与本说明并排显示。理想情况下，这两个浏览器标签页会同时显示，这样您可以更轻松地执行实验步骤。
    

## 获得作业学分

在本实验结束时，您需要根据指导提交实验，以根据您的进度获得分数。

**提示**：只有按照说明为资源命名并设置配置，作业检查脚本才会给您打分。特别要注意的是，本说明中以 `This Format` 格式显示的值，应完全按照文档中的记录输入（区分大小写）。

## 任务 1：创建新的 EBS 卷

在本任务中，您将创建 Amazon EBS 卷并将其关联到新的 Amazon EC2 实例。

4. 在 AWS 管理控制台中 Services（服务）旁边的搜索框中，搜索并选择 **EC2**。
    
5. 在左侧导航窗格中，选择 **Instances**（实例）。
    
    实验已启动了名为 **Lab** 的 Amazon EC2 实例。
    

6. 记下该实例的**可用区**。该值类似于 _us-east-1a_。
    
7. 在左侧导航窗格中，选择 **Volumes**（卷）。
    
    此时将显示此 Amazon EC2 实例正在使用的现有卷。此卷的大小为 8 GiB，这就让您很容易与接下来创建的卷（大小为 1 GiB）区分开来。
    
8. 选择 Create volume（创建卷），然后进行以下配置：
    
    - **Volume Type**（卷类型）：_General Purpose SSD (gp2)_（通用型 SSD (gp2)）
        
    - **Size (GiB)**（大小 (GiB)）：`1`。**注意**：可能会限制您创建较大的卷。
        
    - **Availability Zone**（可用区）：选择与您的 EC2 实例相同的可用区。
        
    - 选择 Add tag（添加标签）
        
    - 在 **Tag Editor**（标签编辑器）中，输入：
        
        - **Key**（键）：`Name`
            
        - **Value**（值）：`My Volume`
            
9. 选择 Create volume（创建卷）。
    
    新的卷将显示在列表中，并且状态从 _Creating_（正在创建）变为 _Available_（可用）。您可能需要选择**刷新**图标 才能看到新的卷。
    

## 任务 2：将卷关联到实例

在本任务中，您要将新的 EBS 卷关联到 Amazon EC2 实例。

10. 选择 **My Volume**（我的卷）。
    
11. 在 **Actions**（操作）菜单中，选择 **Attach volume**（挂载卷）。
    
12. 选择 **Instance**（实例）字段，然后选择 **Lab** 实例。
    
    请注意，**Device**（设备）名称设置为 _/dev/sdf_。还请注意显示的消息“Newer Linux kernels may rename your devices to _/dev/xvdf_ through _/dev/xvdp_ internally, even when the device name entered here (and shown in the details) is _/dev/sdf_ through _/dev/sdp_.”（较新的 Linux 内核可能会在内部将您的设备重命名为 /dev/xvdf 到 /dev/xvdp 之间的某个名称，即使此处输入的（并且在详细信息中显示的）设备名为 /dev/sdf 到 /dev/sdp 之间的某个名称。）
    
13. 选择 Attach volume（挂载卷）。
    
    卷状态现在为 _In-use_（正在使用）。
    

## 任务 3：连接到 Amazon EC2 实例

在本任务中，您将使用 EC2 Instance Connect 连接到 EC2 实例，该工具提供了对浏览器中的终端的访问权限。

14. 在 AWS 管理控制台中 Services（服务）旁边的搜索框中，搜索并选择 **EC2**。
    
15. 选择 **Instances**（实例）。
    
16. 选择 **Lab** 实例，然后选择 **Connect**（连接）。
    
17. 在 **EC2 Instance Connect** 选项卡上，选择 **Connect**（连接）
    
    将打开 EC2 Instance Connect 终端会话，并显示 `$` 提示符。
    

## 任务 4：创建并配置文件系统

在本任务中，您将在 /mnt/data-store 挂载点下将新卷作为 ext3 文件系统添加到 Linux 实例中。

18. 查看实例上可用的存储：
    
    运行以下命令：
    
    df -h
    
    此时应显示类似于以下内容的输出：
    
    Filesystem      Size  Used Avail Use% Mounted on
    
    devtmpfs        4.0M     0  4.0M   0% /dev
    
    tmpfs           475M     0  475M   0% /dev/shm
    
    tmpfs           190M  2.8M  188M   2% /run
    
    /dev/xvda1      8.0G  1.6G  6.5G  20% /
    
    tmpfs           475M     0  475M   0% /tmp
    
    tmpfs            95M     0   95M   0% /run/user/1000
    
    该输出显示，原始 8 GB **/dev/xvda1** 磁盘卷挂载在 `/`，这表明它是根卷。它托管着 EC2 实例的 Linux 操作系统。
    
    关联到 Lab 实例的另一个 1 GB 卷未列出，因为您尚未在该卷上创建文件系统，或挂载磁盘。必须执行这些操作，Linux 操作系统才可以使用新的存储空间。接下来，您将执行这些操作。
    
19. 在新卷上创建 ext3 文件系统：
    
    sudo mkfs -t ext3 /dev/sdf
    
    输出应表明已在关联的卷上创建新文件系统。
    
20. 创建目录以挂载新存储卷：
    
    sudo mkdir /mnt/data-store
    
21. 挂载新卷：
    
    sudo mount /dev/sdf /mnt/data-store
    
    要将 Linux 实例配置为在启动时挂载此卷，您需要在 _/etc/fstab_ 中添加一行。要完成此目标，请运行以下命令：
    
    echo "/dev/sdf   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
    
22. 查看配置文件，了解最后一行的设置：
    
    cat /etc/fstab
    
23. 再次查看可用存储：
    
    df -h
    
    输出与以下所示内容类似。
    
    Filesystem      Size  Used Avail Use% Mounted on
    
    devtmpfs        484M     0  484M   0% /dev
    
    tmpfs           492M     0  492M   0% /dev/shm
    
    tmpfs           492M  460K  491M   1% /run
    
    tmpfs           492M     0  492M   0% /sys/fs/cgroup
    
    /dev/xvda1      8.0G  1.5G  6.6G  19% /
    
    tmpfs            99M     0   99M   0% /run/user/0
    
    tmpfs            99M     0   99M   0% /run/user/1000
    
    /dev/xvdf       976M  1.3M  924M   1% /mnt/data-store
    
    请注意最后一行。输出现在列出了 _/dev/xvdf_，这是新挂载的卷。
    
24. 在挂载的卷上，创建文件并在其中添加一些文本。
    
    sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
    
25. 验证文本是否已写入到卷中。
    
    cat /mnt/data-store/file.txt
    

让 EC2 Instance Connect 会话保持运行。稍后您将在本实验中返回到该网页。

## 任务 5：创建 Amazon EBS 快照

在本任务中，您将创建 EBS 卷的快照。

您可以通过 Amazon EBS 卷随时创建任意数量的一致性时间点快照。Amazon EBS 快照存储在 Amazon S3 中，具有高持久性。您可以通过快照创建新的 Amazon EBS 卷，用于克隆或还原备份。也可以在 AWS 用户之间轻松共享 Amazon EBS 快照，或者在 AWS 区域之间复制快照。

26. 在 **EC2 管理控制台**中，选择 **Volumes**（卷），然后选择 **My Volume**（我的卷）。
    
27. 在 **Actions**（操作）菜单中，选择 **Create snapshot**（创建快照）。
    
28. 选择 Add tag（添加标签），然后进行以下配置：
    
    - **Key**（键）：`Name`
    - **Value**（值）：`My Snapshot`
    - 选择 Create snapshot（创建快照）。

29. 在左侧导航窗格中，选择 **Snapshots**（快照）。
    
    此时将显示您的快照。状态最初是 _Pending_（待处理），这意味着正在创建快照。随后，状态将变为 _Completed_（已完成）。
    
    注意：只有已使用的存储块才会复制到快照，因此，空块不会占用任何快照存储空间。
    
30. 在 EC2 Instance Connect 会话中，删除在卷上创建的文件。
    
    sudo rm /mnt/data-store/file.txt
    
31. 验证是否已删除此文件。
    
    ls /mnt/data-store/
    
    文件已删除。
    

## 任务 6：还原 Amazon EBS 快照

如果希望检索快照中存储的数据，可以将快照**还原**到新的 EBS 卷。

### 使用快照创建卷

32. 在 **EC2 控制台**中，选择 **My Snapshot**（我的快照）。
    
33. 在 **Actions**（操作）菜单中，选择 **Create volume from snapshot**（从快照创建卷）。
    
34. 对于 **Availability Zone**（可用区），请选择您之前使用的相同可用区。
    
35. 选择 Add tag（添加标签），然后进行以下配置：
    
    - **Key**（键）：`Name`
    - **Value**（值）：`Restored Volume`
    - 选择 Create volume（创建卷）。
    
    **注意**：在将快照还原到新卷时，您还可以修改配置，例如，更改卷类型、大小或可用区。
    

### 将还原的卷关联到 EC2 实例

36. 在左侧导航窗格中，选择 **Volumes**（卷）。
    
37. 选择 **Restored Volume**（还原的卷）。
    
38. 在 **Actions**（操作）菜单中，选择 **Attach volume**（挂载卷）。
    
39. 选择 **Instance**（实例）字段，然后选择显示的 **Lab** 实例。
    
    请注意，**Device**（设备）字段设置为 _/dev/sdg_。您将在稍后的任务中用到此设备标识符。
    
40. 选择 Attach volume（挂载卷）。
    
    卷状态现在为 _In-use_（正在使用）。
    

### 挂载还原的卷

41. 创建目录以挂载新存储卷：
    
    sudo mkdir /mnt/data-store2
    
42. 挂载新卷：
    
    sudo mount /dev/sdg /mnt/data-store2
    
43. 验证挂载的卷是否包含之前创建的文件。
    
    ls /mnt/data-store2/
    
    此时应显示 file.txt。
    

## 提交作业

44. 要记录您的进度，请选择本说明顶部的 **Submit**（提交）。
    
45. 在系统提示时，选择 **Yes**（是）。
    
    几分钟后，成绩面板会显示每个任务所获得的分数。如果在几分钟后仍未显示结果，请选择本说明顶部的 **Grades**（成绩）。
    
    **提示**：您可以多次提交作业。更改作业后，再次选择 **Submit**（提交）。最后一次提交的作业记为本实验的作业。
    
46. 要查找有关作业的详细反馈，请选择 **Submission Report**（提交报告）。
    
    **提示**：对于未得满分的任何检查，有时可在提交报告中找到有用的详细信息。
    

## 总结

恭喜！现在，您已成功完成以下任务：

- 创建 Amazon EBS 卷
    
- 将卷关联到 EC2 实例
    
- 在卷上创建文件系统
    
- 将文件添加到卷
    
- 创建卷的快照
    
- 通过快照创建新卷
    
- 将新卷关联并挂载到 EC2 实例
    
- 验证之前创建的文件是否位于新创建的卷上
    

## 实验完成

恭喜！您已完成本实验。

47. 选择此页面顶部的 End Lab（结束实验），然后选择 Yes（是），以确认结束本实验。 
    
    此时将显示一个面板，提示 **DELETE has been initiated... You may close this message box now.**（删除操作已启动…您现在可以关闭此消息框。）
    
48. 选择右上角的 **X**，关闭面板。