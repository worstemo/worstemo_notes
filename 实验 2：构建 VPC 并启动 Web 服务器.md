## 实验概览与目标

在本实验中，您将使用 Amazon Virtual Private Cloud (VPC) 创建自己的 VPC，并添加其他组件生成自定义的网络。您还将创建一个安全组。然后，您将配置并自定义 EC2 实例来运行 Web 服务器，然后启动 EC2 实例在 VPC 中的子网上运行。

通过使用 **Amazon Virtual Private Cloud (Amazon VPC)**，您可以在定义的虚拟网络中启动 Amazon Web Services (AWS) 资源。这个虚拟网络与您在自己的数据中心中运行的传统网络极其相似，同时让您可以获得使用 AWS 可扩展基础设施的优势。您可以创建跨多个可用区的 VPC。

在完成本实验后，您应该能够：

- 创建 VPC。
    
- 创建子网。
    
- 配置安全组。
    
- 在 VPC 中启动 EC2 实例。
    

## **时长**

完成本实验大约需要 **30 分钟**。

## AWS 服务限制

在本实验环境中，对 AWS 服务和服务操作的访问权限可能仅限于完成实验说明所需的服务和操作。如果您尝试访问其他服务，或执行本实验所述之外的操作，则可能会遇到错误。

## **场景**

在本实验中，您将构建以下基础设施：

![架构](https://labs.vocareum.com/web/4516851/4615939.0/ASNLIB/public/docs/lang/zh-cn/images/architecture.png)

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

## 任务 1：创建 VPC

在本任务中，您将使用 VPC 控制台中的 _VPC and more_（VPC 等）选项创建多个资源，包括 _VPC_、_互联网网关_、单个可用区中的_公有子网_和_私有子网_、两个_路由表_和 _NAT 网关_。

4. 在 **Services**（服务）右侧的搜索框中，搜索并选择 **VPC** 以打开 VPC 控制台。
    
5. 开始创建 VPC。
    
    - 在屏幕右上角，确认区域是 **N. Virginia (us-east-1)**（弗吉尼亚北部 (us-east-1)）。
        
    - 选择位于控制台左上角的 **VPC dashboard**（VPC 控制面板）链接。
        
    - 接下来，选择 Create VPC（创建 VPC）。
        
        **注意**：如果您没有看到具有该名称的按钮，请改为选择 **Launch VPC Wizard**（启动 VPC 向导）按钮。
        
    
6. 在左侧的 _VPC settings_（VPC 设置）面板中配置 VPC 详细信息：
    
    - 选择 **VPC and more**（VPC 等）。
        
    - 在 **Name tag auto-generation**（名称标签自动生成）下，将 _Auto-generate_（自动生成）保持选中状态，但将值从 project 更改为 `lab`。
        
    - 将 **IPv4 CIDR block**（IPv4 CIDR 块）设置保留为 10.0.0.0/16
        
    - 对于 **Number of Availability Zones**（可用区数量），请选择 **1**。
        
    - 对于 **Number of _public_ subnets**（公有子网的数量），请将设置保留为 **1**。
        
    - 对于 **Number of _private_ subnets**（私有子网的数量），请将设置保留为 **1**。
        
    - 展开 **Customize subnets CIDR blocks**（自定义子网 CIDR 块）部分
        
        - 将 **Public subnet CIDR block in us-east-1a**（us-east-1a 中的公有子网 CIDR 块）更改为 `10.0.0.0/24`
        - 将 **Private subnet CIDR block in us-east-1a**（us-east-1a 中的私有子网 CIDR 块）更改为 `10.0.1.0/24`
    - 将 **NAT gateways**（NAT 网关）设置为 **In 1 AZ**（在一个可用区中）。
        
    - 将 **VPC endpoints**（VPC 终端节点）设置为 **None**（无）。
        
    - 将 **DNS hostnames**（DNS 主机名）和 **DNS resolution**（DNS 解析）都保留为 _enabled_（已启用）状态。
        
7. 在右侧的 _Preview_（预览）面板中，确认您配置的设置。
    
    - **VPC**：`lab-vpc`
        
    - **Subnets**（子网）：
        
        - us-east-1a
            
            - **_Public_ subnet name**（公有子网名称）：`lab-subnet-public1-us-east-1a`
            - **_Private_ subnet name**（私有子网名称）：`lab-subnet-private1-us-east-1a`
    - **Route tables**（路由表）
        
        - `lab-rtb-public`
        - `lab-rtb-private1-us-east-1a`
    - **Network connections**（网络连接）
        
        - `lab-igw`
        - `lab-nat-public1-us-east-1a`
        
8. 在屏幕底部，选择 Create VPC（创建 VPC）
    
    VPC 资源随即创建完成。NAT 网关需要几分钟才能激活。
    
    请等待_所有_资源创建完毕，然后再执行下一步。
    
9. 在此操作完成后，选择 View VPC（查看 VPC）
    
    此向导预置了 VPC，这个 VPC 在一个可用区中拥有公有子网和私有子网，每个子网都关联路由表。向导还创建了互联网网关和 NAT 网关。
    
    要查看这些资源的设置，请浏览显示资源详细信息的 VPC 控制台链接。例如，选择 **Subnets**（子网）可以查看子网详细信息，选择 **Route tables**（路由表）可以查看路由表详细信息。下图简要说明了您刚刚创建的 VPC 资源及其配置方式。
    
    ![任务 1](https://labs.vocareum.com/web/4516851/4615939.0/ASNLIB/public/docs/lang/zh-cn/images/task1.png)
    
    _互联网网关_是一种 VPC 资源，它允许 VPC 中的 EC2 实例与互联网进行通信。
    
    `lab-subnet-public1-us-east-1a` 公有子网的 CIDR 为 **10.0.0.0/24**，这意味着它包含所有以 **10.0.0.x** 开头的 IP 地址。与这个公有子网关联的路由表会将 0.0.0.0/0 网络流量路由到互联网网关，所以这个子网就是公有子网。
    
    _NAT 网关_是一种 VPC 资源，用于向 VPC 中的_私有_子网上运行的任何 EC2 实例提供互联网连接，这些 EC2 实例无需直接连接到互联网网关。
    
    `lab-subnet-private1-us-east-1a` 私有子网的 CIDR 为 **10.0.1.0/24**，这意味着它包含所有以 **10.0.1.x** 开头的 IP 地址。
    

## 任务 2：创建额外子网

在本任务中，您将在第二个可用区中为 VPC 创建两个额外的子网。在多个可用区中为 VPC 设置子网有助于部署_高可用性_解决方案。

在您按照上述操作创建了 VPC 后，您仍可以实施进一步配置，例如，添加更多**子网**。您创建的每个子网各自位于一个可用区中。

10. 在左侧导航窗格中，选择 **Subnets**（子网）。
    
    首先，您将创建第二个_公有_子网。
    
11. 选择 Create subnet（创建子网），然后进行以下配置：
    
    - **VPC ID**：**lab-vpc**（从菜单中选择）。
    - **Subnet name**（子网名称）：`lab-subnet-public2`
    - **Availability Zone**（可用区）：选择_第二个_可用区（例如 us-east-1b）
    - **IPv4 CIDR block**（IPv4 CIDR 块）：`10.0.2.0/24`
    
    此子网将包含所有以 **10.0.2.x** 开头的 IP 地址。
    
12. 选择 Create subnet（创建子网）
    
    第二个_公有_子网随即创建完成。现在，您将创建第二个_私有_子网。
    
13. 选择 Create subnet（创建子网），然后进行以下配置：
    
    - **VPC ID**：`lab-vpc`
    - **Subnet name**（子网名称）：`lab-subnet-private2`
    - **Availability Zone**（可用区）：选择_第二个_可用区（例如 us-east-1b）
    - **IPv4 CIDR block**（IPv4 CIDR 块）：`10.0.3.0/24`
    
    此子网将包含所有以 **10.0.3.x** 开头的 IP 地址。
    
14. 选择 Create subnet（创建子网）
    
    第二个_私有_子网随即创建完成。
    
    现在，您将配置这个新的_私有_子网，将流向互联网的流量路由到 NAT 网关，以便第二个私有子网中的资源能够连接到互联网，同时这些资源仍然保持私有。这是通过配置_路由表_完成的。
    
    _路由表_包含一组称为_路由_的规则，用于确定网络流量的流向。VPC 中的每个子网必须与一个路由表相关联；而路由表控制子网的路由。
    
15. 在左侧导航窗格中，选择 **Route tables**（路由表）。
    
16. 选择 **lab-rtb-private1-us-east-1a** 路由表。
    
17. 在下面的窗格中，选择 **Routes**（路由）选项卡。
    
    请注意，**Destination 0.0.0.0/0**（目的地 0.0.0.0/0）设置为 **Target nat-xxxxxxxx**（目标 nat-xxxxxxxx）。这意味着，目的地是互联网的流量 (0.0.0.0/0) 将发送到 NAT 网关。NAT 网关随后将流量转发到互联网。
    
    因此，此路由表用于路由来自私有子网的流量。
    
18. 选择 **Subnet associations**（子网关联）选项卡。
    
    您在任务 1 中选择创建 VPC 以及 VPC 中的多个资源时，就已经创建了这个路由表。该操作还创建了 _lab-subnet-private-1_ 子网，并将该子网与这个路由表相关联。
    
    现在，您又创建了一个私有子网 (lab-subnet-private-2)，并且要将这个路由表与第二个子网相关联。
    
19. 在 **Explicit subnet associations**（显式子网关联）面板中，选择 Edit subnet associations（编辑子网关联）
    

20. 将 **lab-subnet-private1-us-east-1a** 保持选中状态，同时还要选择 **lab-subnet-private2**。
    
21. 选择 Save associations（保存关联）
    
    现在，您将配置公有子网使用的路由表。
    
22. 选择 **lab-rtb-public** 路由表（并取消选择其他任何子网）。
    
23. 在下面的窗格中，选择 **Routes**（路由）选项卡。
    
    请注意，**Destination 0.0.0.0/0**（目的地 0.0.0.0/0）设置为 **Target igw-xxxxxxxx**（目标 igw-xxxxxxxx），这是互联网网关。这意味着，流向互联网的流量将通过此互联网网关直接发送到互联网。
    
    现在，您将此路由表与创建的第二个公有子网相关联。
    
24. 选择 **Subnet associations**（子网关联）选项卡。
    
25. 在 **Explicit subnet associations**（显式子网关联）区域中，选择 Edit subnet associations（编辑子网关联）
    
26. 将 **lab-subnet-public1-us-east-1a** 保持选中状态，同时还要选择 **lab-subnet-public2**。
    
27. 选择 Save associations（保存关联）
    
    现在，您的 VPC 在两个可用区中配置了公有子网和私有子网。您在任务 1 中创建的路由表也得以更新，可以路由两个新子网的网络流量。
    
    ![任务 2](https://labs.vocareum.com/web/4516851/4615939.0/ASNLIB/public/docs/lang/zh-cn/images/task2.png)
    

## 任务 3：创建 VPC 安全组

在本任务中，您将创建一个 VPC 安全组作为虚拟防火墙。在启动实例时，您需要将一个或多个安全组与该实例相关联。您可以为每个安全组添加规则，以便允许流经其关联实例的流量。

28. 在左侧导航窗格中，选择 **Security Groups**（安全组）。
    
29. 选择 Create security group（创建安全组），然后进行以下配置：
    
    - **Security group name**（安全组名称）：`Web Security Group`
        
    - **Description**（描述）：`Enable HTTP access`
        
    - **VPC**：选择 **X** 删除当前选择的 VPC，然后从下拉列表中选择 **lab-vpc**
        
30. 在 **Inbound rules**（入站规则）窗格中，选择 Add rule（添加规则）
    
31. 配置以下设置：
    
    - **Type**（类型）：_HTTP_
        
    - **Source**（源）：_Anywhere-Ipv4_
        
    - **Description**（描述）：`Permit web requests`
        
32. 滚动到页面底部，然后选择 Create security group（创建安全组）
    
    您将在下一个任务中启动 Amazon EC2 实例时使用此安全组。
    

## 任务 4：启动 Web 服务器实例

在本任务中，您将在新的 VPC 中启动 Amazon EC2 实例。您可以将该实例配置为充当 Web 服务器。

33. 在 **Services**（服务）右侧的搜索框中，搜索并选择 **EC2** 以打开 EC2 控制台。
    
34. 从 Launch instance（启动实例）菜单中，选择 **Launch Instance**（启动实例）。
    
35. 命名实例：
    
    - 为其指定名称 `Web Server 1`
        
        在您命名实例时，AWS 会创建一个标签并将其与实例相关联。标签是一个键值对。此键值对的键是 _***Name***_，值是您为 EC2 实例输入的名称。
        
36. 选择用于创建实例的 AMI：
    
    - 在可用的 _Quick Start_（快速启动）AMI 列表中，保持选中默认的 **Amazon Linux**。
        
    - 同时保持选中默认的 **Amazon Linux 2023 AMI**。
        
        您选择的_亚马逊机器映像 (AMI)_ 类型决定了在启动的 EC2 实例上运行的操作系统。
        
37. 选择一种实例类型：
    
    - 在 _Instance type_（实例类型）面板中，将默认的 **t2.micro** 保持选中状态。
        
        _实例类型_决定了分配给实例的硬件资源。
        
38. 选择要与实例关联的密钥对：
    
    - 从 **Key pair name**（密钥对名称）菜单中，选择 **vockey**。
        
        所选择的 vockey 密钥对可支持在该实例启动后通过 SSH 连接到该实例。本实验中不需要这样做，但在启动实例时仍然需要指定现有密钥对或新建密钥对，或选择在没有密钥对的情况下继续。
        
39. 配置网络设置：
    
    - 在 **Network settings**（网络设置）旁边，选择 **Edit**（编辑），然后进行以下配置：
        
        - **Network**（网络）：_lab-vpc_
        - **Subnet**（子网）：_lab-subnet-public2_（_非_私有！）
        - **Auto-assign public IP**（自动分配公有 IP）：_Enable_（启用）
    - 接下来，您将实例配置为使用之前创建的 _Web Security Group_。
        
        - 在 **Firewall (security groups)**（防火墙（安全组））下，选择 **Select existing security group**（选择现有安全组）。
            
        - 对于 **Common security groups**（常见安全组），选择 **Web Security Group**。
            
            此安全组将允许对实例进行 HTTP 访问。
            
40. 在 _Configure storage_（配置存储）部分中，保留默认设置。
    
    **注意**：默认设置指定实例的_根卷_（托管您之前指定的 Amazon Linux 来宾操作系统）在大小为 8 GiB 的通用型 SSD (_gp3_) 硬盘驱动器上运行。您也可以添加更多存储卷，但本实验不需要这样做。
    
41. 将脚本配置为在实例启动时在实例上运行：
    
    - 展开 **Advanced details**（高级详细信息）面板。
        
    - 滚动到页面底部，然后复制下面显示的代码，并将其粘贴到 **User data**（用户数据）框中：
        
        #!/bin/bash
        
        # Install Apache Web Server and PHP
        
        dnf install -y httpd wget php mariadb105-server
        
        # Download Lab files
        
        wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip
        
        unzip lab-app.zip -d /var/www/html/
        
        # Turn on web server
        
        chkconfig httpd on
        
        service httpd start
        
        此脚本将在实例的来宾操作系统上以根用户权限运行，并且会在实例首次启动时自动运行。此脚本将安装 Web 服务器、数据库和 PHP 库，然后在 Web 服务器上下载并安装 PHP Web 应用程序。
        
42. 在屏幕右侧的 **Summary**（摘要）面板底部，选择 Launch instance（启动实例）
    
    此时将显示一条成功消息。
    
43. 选择 View all instances（查看所有实例）。
    
44. 等到 **Web Server 1** 的 **Status check**（状态检查）列中显示 _2/2 checks passed_（2/2 项检查已通过）。
    
    该过程可能需要几分钟的时间。每隔 30 秒左右选择页面顶部的刷新 图标，以便更快地了解实例的最新状态。
    
    现在，您将连接到在 EC2 实例上运行的 Web 服务器。
    
45. 选择 **Web Server 1**。
    
46. 复制页面底部的 **Details**（详细信息）选项卡中显示的 **Public IPv4 DNS**（公有 IPv4 DNS）值。
    
47. 打开新的 Web 浏览器标签页，粘贴 **Public DNS**（公有 DNS）值，然后按 Enter 键。
    
    此时应显示包含 AWS 徽标和实例元数据值的网页。
    
    您部署的完整架构是：
    
    ![架构](https://labs.vocareum.com/web/4516851/4615939.0/ASNLIB/public/docs/lang/zh-cn/images/architecture.png)
    

## 提交作业

48. 要记录您的进度，请选择本说明顶部的 **Submit**（提交）。
    
49. 在系统提示时，选择 **Yes**（是）。
    
    几分钟后，成绩面板会显示每个任务所获得的分数。如果在几分钟后仍未显示结果，请选择本说明顶部的 **Grades**（成绩）。
    
    **提示**：您可以多次提交作业。更改作业后，再次选择 **Submit**（提交）。最后一次提交的作业记为本实验的作业。
    
50. 要查找有关作业的详细反馈，请选择 **Submission Report**（提交报告）。
    
    **提示**：对于未得满分的任何检查，有时可在提交报告中找到有用的详细信息。
    

## 实验完成

恭喜！您已完成本实验。

51. 选择此页面顶部的 **End Lab**（结束实验），然后选择 Yes（是）确认结束实验。
    
    面板提示 _You may close this message box now..._（您现在可以关闭此消息框...）。
    

52. 选择右上角的 **X** 关闭面板。