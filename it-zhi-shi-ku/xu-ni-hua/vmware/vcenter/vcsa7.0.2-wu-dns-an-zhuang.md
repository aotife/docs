# VCSA7.0.2无DNS安装

7.0.2既 7.0U2

### 第一阶段

* FQDN，IP地址，DNS服务器填写同一个IP地址
* 待第一阶段提示部署成功后，在vsphere web client里，按F2进入VCSA的配置界面，选择Troubleshooting Mode Options，回车进入，选择Enable SSH，然后回车。确保SSH状态开启。
*   ssh登录VCSA的管理地址，输入shell，

    ```
      Command> shell
      Shell access is granted to root
    ```
*   编辑/etc/hosts文件，

    ```
    vi /etc/hosts
    ```
*   添加一行如下内容：

    ```
    192.169.1.22 localhost  
    ```

    格式说明：VCSA管理地址 主机名

VCSA7.0默认的主机名是localhost，VCSA6.7默认的主机名是photon-machine。

### 第二阶段

* **如果你进入5480安装，并且更改了默认的主机名localhost，则/etc/hosts文件中的主机名和修改后的主机名一致**











参考链接：

{% embed url="https://docs.vmware.com/cn/VMware-vSphere/index.html" %}
VMware 官网文档
{% endembed %}
