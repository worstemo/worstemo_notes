## 实验概览与目标


本实验简要概述了如何启动、管理和监控 Amazon EC2 实例以及调整其大小。

**Amazon Elastic Compute Cloud (Amazon EC2)** 是一项 Web 服务，可在云中提供大小可调的计算容量。该服务旨在让开发人员能更轻松地进行 Web 规模的云计算。

Amazon EC2 的 Web 服务接口非常简单，让您可以轻松获取和配置容量。您可以使用该服务完全控制自己的计算资源，还可以在成熟的 Amazon 计算环境中运行资源。Amazon EC2 将获取并启动新服务器实例所需要的时间缩短至几分钟，这样一来，在您的计算要求发生变化时，您便可以快速扩展或缩减计算容量。

Amazon EC2 按您实际使用的容量收费，改变了计算的经济性。Amazon EC2 还为开发人员提供了相关工具，创建能够迅速从故障中恢复的应用程序，并避免常见故障。

在完成本实验后，您应该能够：

- 启动 Web 服务器，并且启用终止保护
- 监控您的 EC2 实例
- 修改 Web 服务器用来允许 HTTP 访问的安全组
- 调整 Amazon EC2 实例的大小以实现扩展，并启用停止保护
- 探索 EC2 限制
- 测试停止保护
- 停止 EC2 实例

## 时长

完成本实验大约需要 **35 分钟**。

## AWS 服务限制

在本实验环境中，对 AWS 服务和服务操作的访问权限可能仅限于完成实验说明所需的服务和操作。如果您尝试访问其他服务，或执行本实验所述之外的操作，则可能会遇到错误。

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

## 任务 1：启动 Amazon EC2 实例

在本任务中，您将启动一个 Amazon EC2 实例，并且为实例启用_终止保护_和_停止保护_。终止保护可防止意外终止 EC2 实例，而停止保护则可防止意外停止 EC2 实例。您还将指定在启动实例时运行的用户数据脚本，该脚本将部署简单的 Web 服务器。

4. 在 **AWS 管理控制台**中，依次选择 **Services**（服务）、**Compute**（计算）和 **EC2**。
    
    **注意**：确认您的 EC2 控制台当前正在管理 **N. Virginia**（弗吉尼亚北部）(us-east-1) 区域的资源。您可以查看页面顶部用户名左侧的下拉菜单来进行验证。如果尚未显示 **N. Virginia**（弗吉尼亚北部），请在进入下一步之前从 **Region**（区域）菜单中选择 **N. Virginia**（弗吉尼亚北部）区域。
    
5. 选择 Launch instance（启动实例）菜单，然后选择 **Launch instance**（启动实例）。
    

### 第 1 步：名称和标签

6. 将实例命名为 `Web Server`。
    
    提供的实例名称将存储为标签。标签可让您按各种标准（例如用途、拥有者或环境）对 AWS 资源进行分类。这非常适用于同时存在多个同类型资源的情况 — 您可以利用分配给特定资源的标签快速识别资源。每个标签都包含您定义的键和值。如果需要，您可以定义多个与实例关联的标签。
    
    在本例中，对于要创建的标签，它的_键_是 `Name`，而_值_是 `Web Server`
    

### 第 2 步：应用程序和操作系统映像（亚马逊机器映像）

7. 在可用的 _Quick Start_（快速启动）AMI 列表中，保持选中默认的 **Amazon Linux** AMI。
    
8. 同时保持选中默认的 **Amazon Linux 2023 AMI**。
    
    **亚马逊机器映像 (AMI)** 提供启动实例所需的信息，该实例是云中的一个虚拟服务器。AMI 包括：
    
    - 一个用于实例根卷的模板（例如，可能是操作系统或具有应用程序的应用程序服务器）
    - 启动许可，用于控制哪些 AWS 账户可以使用 AMI 来启动实例
    - 块储存设备映射，用于指定在实例启动时关联到该实例的卷
    
    **Quick Start**（快速启动）列表包含最常用的 AMI。您也可以自行创建 AMI 或从 AWS Marketplace 选择 AMI。AWS Marketplace 是专供用户销售或购买在 AWS 上运行的软件的在线商店。
    

### 第 3 步：实例类型

9. 在 _Instance type_（实例类型）面板中，将默认的 **t2.micro** 保持选中状态。
    
    Amazon EC2 提供了多种经过优化的_实例类型_，适合不同的使用案例。实例类型包括由 CPU、内存、存储和网络容量组成的不同组合，您可以灵活地为应用程序选择适当的资源组合。每种实例类型都包括一种或多种_实例大小_，从而使您能够扩展资源以满足目标工作负载的要求。
    
    t2.micro 实例类型具有 1 个虚拟 CPU 和 1GiB 内存。
    
    **注意**：本实验可能会限制您使用其他实例类型。
    

### 第 4 步：密钥对（登录）

10. 对于 **Key pair name - _required_**（密钥对名称 – 必需），选择 **vockey**。
    
    Amazon EC2 使用公有密钥加密法来加密和解密登录信息。为确保您能够登录到所创建实例的来宾操作系统，需要在启动实例时标识现有的密钥对或创建新的密钥对。然后，Amazon EC2 会在实例启动时将密钥安装到来宾操作系统中。这样，当您尝试登录实例并提供私有密钥时，就会获得授权以连接到实例。
    
    **注意**：在本实验中，您不会实际使用指定的密钥对来登录实例。
    

### 第 5 步：网络设置

11. 在 **Network settings**（网络设置）旁边，选择 **Edit**（编辑）。
    
12. 对于 **VPC**，选择 **Lab VPC**。
    
    Lab VPC 在实验的设置过程中使用 AWS CloudFormation 模板创建。此 VPC 在两个不同的可用区中包含两个公有子网。
    
    **注意**：保留默认子网 **PublicSubnet1**。这是将运行实例的子网。还请注意，默认为实例分配公有 IP 地址。
    
13. 在 **Firewall (security groups)**（防火墙（安全组））下，选择 **Create security group**（创建安全组）并配置：
    
    - **Security group name**（安全组名称）：`Web Server security group`
        
    - **Description**（描述）：`Security group for my web server`
        
        _安全组_充当虚拟防火墙，用于控制一个或多个实例的流量。在启动实例时，您需要将一个或多个安全组与该实例相关联。您可以在每个安全组中添加_规则_，从而允许流入或流出关联实例的流量。您可以随时修改安全组的规则；新规则将自动应用于与该安全组关联的所有实例。
        
    - 在 **Inbound security group rules**（入站安全组规则）下，注意已存在一个规则。选择 **Remove**（删除），删除此规则。
        

### 第 6 步：配置存储

14. 在 _Configure storage_（配置存储）部分中，保留默认设置。
    
    Amazon EC2 将数据存储在名为 _Elastic Block Store_ 的网络关联虚拟磁盘上。
    
    您将使用默认的 8GiB 磁盘卷启动 Amazon EC2 实例。该卷将是您的根卷（也称为“启动”卷）。
    

### 第 7 步：高级详细信息

15. 展开 **Advanced details**（高级详细信息）。
    
16. 对于 **Termination protection**（终止保护），请选择 **Enable**（启用）。
    
    当不再需要某个 Amazon EC2 实例时，可以将其_终止_，这意味着将删除该实例并释放其资源。无法再次访问已终止的实例，也无法恢复其中的数据。如果要防止实例意外终止，您可以为实例启用 _Termination protection_（终止保护），只要该设置处于启用状态，实例就无法终止。
    
17. 滚动到页面底部，然后复制下面显示的代码，并将其粘贴到 **User data**（用户数据）框中：
    
    #!/bin/bash
    
    dnf install -y httpd
    
    systemctl enable httpd
    
    systemctl start httpd
    
    echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
    
    当启动实例时，可以将_用户数据_传递给实例，这些数据可用于执行自动安装，以及在实例启动后执行配置任务。
    
    您的实例运行的是 Amazon Linux 2023。在实例启动时，您指定的 _shell 脚本_将以 _root_ 来宾操作系统用户身份运行。该脚本将执行以下操作：
    
    - 安装 Apache Web 服务器 (httpd)
    - 将 Web 服务器配置为在启动时自动启动
    - 安装完成后运行 Web 服务器
    - 创建简单的网页

### 第 8 步：启动实例

18. 在 **Summary**（摘要）面板底部，选择 Launch instance（启动实例）
    
    此时将显示一条成功消息。
    

19. 选择 View all instances（查看所有实例）。
    
    - 在 **Instances**（实例）列表中，选择 **Web Server**。
        
    - 查看 **Details**（详细信息）选项卡中显示的信息。其中包括有关实例类型、安全设置和网络设置的信息。
        
        该实例分配有_公有 IPv4 DNS_，可供您通过互联网与实例通信。
        
        要查看更多信息，请向上拖动窗口分隔线。
        
        最初，该实例的状态将显示为 _Pending_（待处理），表示它正在启动。随后将更改为 _Initializing_（正在初始化），最后更改为 _Running_（正在运行）。
        
    
20. 等待您的实例显示以下内容：
    
    - **Instance state**（实例状态）： _Running_（正在运行）
    - **Status Checks**（状态检查）： _2/2 checks passed_（2/2 项检查已通过）

**恭喜**！您已成功启动第一个 Amazon EC2 实例。

## 任务 2：监控实例

监控是保持 Amazon Elastic Compute Cloud (Amazon EC2) 实例和 AWS 解决方案的可靠性、可用性和性能的重要一环。

21. 选择 **Status checks**（状态检查）选项卡。
    
    通过实例状态监控，您可以快速确定 Amazon EC2 是否检测到任何可能阻止实例运行应用程序的问题。Amazon EC2 对每个正在运行的 EC2 实例执行自动检查，从而识别硬件和软件问题。
    
    请注意，该实例已通过 **System reachability**（系统可到达性）和 **Instance reachability**（实例可到达性）检查。
    
22. 选择 **Monitoring**（监控）选项卡。
    
    此选项卡显示您的实例的 Amazon CloudWatch 指标。目前，由于实例是最近启动的，因此没有多少指标可显示。
    
    您可以选择任意图表中的三点图标，然后选择 **Enlarge**（放大），查看所选指标的放大视图。
    
    Amazon EC2 将 EC2 实例的指标发送到 Amazon CloudWatch。默认情况下启用基本（5 分钟）监控。您也可以启用详细（1 分钟）监控。
    
23. 在控制台顶部的 Actions （操作）菜单中，选择 **Monitor and troubleshoot**（监控和故障排除） **Get system log**（获取系统日志）。
    
    系统日志将显示实例的控制台输出，它是一种很重要的问题诊断工具。内核问题和服务配置问题可能会导致实例在其 SSH daemon 启动之前终止或无法访问，而系统日志对于解决这些问题尤其有帮助。如果您没有看到系统日志，请等待几分钟，然后重试。
    
24. 滚动浏览输出，请注意，HTTP 包基于创建实例时您添加的**用户数据**安装。
    
    ![控制台输出](https://labs.vocareum.com/web/4516851/4615941.0/ASNLIB/public/docs/lang/zh-cn/images/Console-output.png)
    
25. 选择 **Cancel**（取消）。
    
26. 确保 **Web Server** 仍处于选中状态。然后在 Actions （操作）菜单中，选择 **Monitor and troubleshoot**（监控和故障排除） **Get instance screenshot**（获取实例屏幕截图）。
    
    随即会向您展示当屏幕与 Amazon EC2 实例控制台关联时，该控制台的外观。
    
    ![屏幕截图](https://labs.vocareum.com/web/4516851/4615941.0/ASNLIB/public/docs/lang/zh-cn/images/Screen-shot.png)
    
    如果您无法通过 SSH 或 RDP 访问您的实例，您可以捕获实例的屏幕截图，并以图像方式查看实例的状态。这可以让您了解实例的状态，更快地进行故障排除。
    
27. 选择 **Cancel**（取消）。
    
    **恭喜**！您已经探索了多种监控实例的方法。
    

## 任务 3：更新安全组并访问 Web 服务器

在启动 EC2 实例时，您提供了一个脚本，用于安装 Web 服务器并创建简单的网页。在本任务中，您将从该 Web 服务器访问内容。

28. 确保 **Web Server** 仍处于选中状态。选择 **Details**（详细信息）选项卡。
    
29. 将实例的 **Public IPv4 address**（公有 IPv4 地址）复制到剪贴板。
    
30. 在 Web 浏览器中打开新的标签页，粘贴您刚刚复制的 IP 地址，然后按 **Enter** 键。
    
    **问题**：您能否访问您的 Web 服务器？为什么不能访问？
    
    您当前**无法**访问 Web 服务器，因为_安全组_不允许端口 80（用于 HTTP Web 请求）上的入站流量通过。在本演示中，我们将安全组用作防火墙来限制允许进出实例的网络流量。
    
    要解决此问题，您现在应更新安全组以允许端口 80 上的 Web 流量通过。
    
31. 让浏览器标签页保持打开状态，同时返回 **EC2 控制台**标签页。
    
32. 在左侧导航窗格中，选择 **Security Groups**（安全组）。
    
33. 选择 **Web Server security group**。
    
34. 选择 **Inbound rules**（入站规则）选项卡。
    
    此安全组当前没有入站规则。
    
35. 选择 Edit inbound rules（编辑入站规则），然后选择 Add rule（添加规则），并进行以下配置：
    
    - **Type**（类型）：_HTTP_
        
    - **Source**（源）：_Anywhere-Ipv4_
        
    - 选择 Save rules（保存规则）
        
36. 返回您之前打开的 Web 服务器标签页，然后刷新 页面。
    
    此时应显示消息 _Hello From Your Web Server!_（欢迎访问 Web 服务器！）
    
    **恭喜**！您已成功修改安全组，以允许 HTTP 流量流入 Amazon EC2 实例。
    

## 任务 4：调整实例大小：实例类型和 EBS 卷

随着需求的变化，您可能会发现存在实例过度利用（太小）或利用不足（太大）的情况。如果是这样，您可以更改_实例类型_。例如，如果 _t2.micro_ 实例对于其工作负载来说过小，您可将其更改为 _m5.medium_ 实例。同样，您也可以更改磁盘的大小。

### 停止实例

要调整实例大小，必须先_停止_实例。

停止实例后，实例将关闭。已停止的 EC2 实例不会产生任何运行时费用，但连接的 Amazon EBS 卷仍会产生存储费用。

37. 在 **EC2 管理控制台**中，选择左侧导航窗格中的 **Instances**（实例），然后选择 **Web Server** 实例。
    
38. 在 Instance State （实例状态）菜单中，选择 **Stop instance**（停止实例）。
    
39. 选择 Stop（停止）
    
    您的实例将执行正常的关闭操作，然后停止运行。
    
40. 等待 **Instance State**（实例状态）显示为 _Stopped_（已停止）。
    

### 更改实例类型并启用停止保护

41. 选择 Web Server 实例，然后在 Actions （操作）菜单中，依次选择 **Instance settings**（实例设置） **Change instance type**（更改实例类型），并进行以下配置：
    
    - **Instance Type**（实例类型）：_t2.small_
        
    - 选择 Apply（应用）
        
        当再次启动该实例时，它将作为 _t2.small_ 运行，其内存是 _t2.micro_ 实例的两倍。**注意**：本实验可能会限制您使用其他实例类型。
        
    
42. 选择 Web Server 实例，然后在 Actions （操作）菜单中，依次选择 **Instance settings**（实例设置） **Change stop protection**（更改停止保护）。选择 **Enable**（启用），然后选择 Save（保存），保存更改。
    
    停止实例后，实例将关闭。之后再启动实例时，该实例通常会迁移到新的底层托管计算机，并且系统将会为它分配新的_公有_ IPv4 地址。实例会保留分配给它的_私有_ IPv4 地址。停止实例后，实例不会被删除。任何 EBS 卷以及这些卷上的数据都将保留。
    

### 调整 EBS 卷的大小

43. 在保持选中 Web Server 实例的情况下，选择 **Storage**（存储）选项卡，再选择 **Volume ID**（卷 ID）的名称，然后选中显示的卷旁边的复选框。
    
44. 在 Actions （操作）菜单中，选择 **Modify volume**（修改卷）。
    
    当前磁盘卷的大小为 8GiB。现在，您将增加该磁盘的大小。
    
45. 将大小更改为 `10` **注意**：本实验可能会限制您创建大于 10 GB 的 Amazon EBS 卷。
    
46. 选择 Modify（修改）
    
47. 再次选择 Modify（修改），确认并增加卷的大小。
    

### 启动大小经过调整的实例

现在，您将再次启动实例，该实例现在将具有更多的内存和更多的磁盘空间。

48. 在左侧导航窗格中，选择 **Instances**（实例）。
    
49. 选择 **Web Server** 实例。
    
50. 在 Instance State （实例状态）菜单中，选择 **Start instance**（启动实例）。
    
    **恭喜**！您已成功调整 Amazon EC2 实例的大小。在本任务中，您将实例类型从 _t2.micro_ 更改为 _t2.small_。您还将根磁盘卷的大小从 8GiB 更改为 10GiB。
    

## 任务 5：探索 EC2 限制

Amazon EC2 提供了不同的资源供您使用。这些资源包括映像、实例、卷和快照。在您创建 AWS 账户时，应遵循这些资源在各区域的默认限制。

51. 在 AWS 管理控制台中 **Services**（服务）旁边的搜索框中，搜索并选择 `Service Quotas`
    
52. 从导航菜单中选择 **AWS services**（AWS 服务），然后在 AWS 服务的 _Find services_（查找服务）搜索栏中，搜索 `ec2` 并选择 **Amazon Elastic Compute Cloud (Amazon EC2)**。
    
53. 在 _Find quotas_（查找配额）搜索栏中，搜索 `running on-demand`，但不进行选择。相反，应观察筛选出的符合条件的服务配额列表。
    
    请注意，一个区域内可以运行的实例数量和类型存在限制。例如，您可以在此区域内启动的 _Running On-Demand Standard..._（按需运行标准...）实例的数量存在限制。在启动实例时，该请求不得导致您的用量超出该区域中当前定义的实例限制。
    
    如果您是 AWS 账户的拥有者，则可以请求提高其中的多项限制。
    

## 任务 6：测试停止保护

在您不需要访问实例，但仍希望将其保留时，可以停止实例。在本任务中，您将了解如何使用_停止保护_。

54. 在 AWS 管理控制台中 **Services**（服务）旁边的搜索框中，搜索并选择 `EC2`，返回 EC2 控制台。
    
55. 在左侧导航窗格中，选择 **Instances**（实例）。
    
56. 选择 **Web Server** 实例，在 Instance state （实例状态）菜单中，选择 **Stop instance**（停止实例）。
    
57. 然后选择 Stop（停止）
    
    请注意，会显示以下消息：_Failed to stop the instance i-1234567xxx. The instance 'i-1234567xxx' may not be stopped. Modify its 'disableApiStop' instance attribute and try again._（停止实例 i-1234567xxx 失败。实例“i-1234567xxx”可能未停止。请修改其“disableApiStop”实例属性并重试。）
    
    这表示您之前在本实验中启用的停止保护，正在防止实例意外停止。如果确实要停止实例，则您需要禁用停止保护。
    
58. 选择 Actions （操作）菜单，然后选择 **Instance settings**（实例设置） **Change stop protection**（更改停止保护）。
    
59. 清除 **Enable**（启用）旁边的勾选标记。
    
60. 选择 Save（保存）
    
    您现在可以停止实例。
    
61. 再次选择 **Web Server** 实例，在 Instance state （实例状态）菜单中，选择 **Stop instance**（停止实例）。
    
62. 选择 Stop（停止）
    
    **恭喜**！您已成功测试了停止保护，并成功停止了实例。
    

## 提交作业

63. 要记录您的进度，请选择本说明顶部的 **Submit**（提交）。
    

64. 在系统提示时，选择 **Yes**（是）。
    
    几分钟后，成绩面板会显示每个任务所获得的分数。如果在几分钟后仍未显示结果，请选择本说明顶部的 **Grades**（成绩）。
    
    **重要说明**：本实验提交流程中执行的某些检查只有在完成提交操作至少 5 分钟后才会发放学分。如果您首次提交后未收到学分，则可能需要等待几分钟后再次提交，才能收到作业的学分。
    
    **提示**：您可以多次提交作业。更改作业后，再次选择 **Submit**（提交）。最后一次提交的作业记为本实验的作业。
    
65. 要查找有关作业的详细反馈，请选择 **Submission Report**（提交报告）。
    
    **提示**：对于未得满分的任何检查，有时可在提交报告中找到有用的详细信息。
    

## 实验完成

恭喜！您已完成本实验。

66. 选择此页面顶部的 End Lab（结束实验），然后选择 Yes（是），以确认结束本实验。 
    
    随即将显示 **End Lab**（结束实验）面板，提示 **You may close this message box now.**（您现在可以关闭此消息框。）
    
67. 选择右上角的 **X**，关闭面板。
    

## 其他资源

- [启动实例](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html)
- [Amazon EC2 实例类型](https://aws.amazon.com/ec2/instance-types)
- [亚马逊机器映像 (AMI)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)
- [Amazon EC2 – 用户数据和 Shell 脚本](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)
- [Amazon EC2 根设备卷](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html)
- [标记 Amazon EC2 资源](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)
- [安全组](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)
- [Amazon EC2 密钥对](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
- [实例的状态检查](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html?icmpid=docs_ec2_console)
- [获取控制台输出和重启实例](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-console.html)
- [Amazon EC2 指标和维度](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ec2-metricscollected.html)
- [调整实例大小](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html)
- [停止和启动实例](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html)
- [Amazon EC2 服务限制](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html)
- [终止实例](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)
- [实例终止保护](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)