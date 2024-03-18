# 使用命令升级ESXI系统(补丁)

### 1 下载

　在 VMWare 官网下载 ESXi 的软件包。注意，升级时，应该下载 \*\*Offline Bundle 格式（也就是 zip 格式）\*\*的软件包。

### 2 上传文件

点击存储 => datastore1 => 数据存储浏览器 =>上载

<figure><img src="https://pic.chjina.com/2024/03/18/85996713073b5a3ea569ef4b56c0e650.png" alt=""><figcaption></figcaption></figure>

### 3 开启SSH

点击管理 => 服务 => TSM-SSH =>启动

<figure><img src="https://pic.chjina.com/2024/03/18/438e6d345c1aed90c440385ee716f31e.png" alt=""><figcaption></figcaption></figure>

### 4 SSH登录ESXI

账号root

密码：安装时设置的密码

### 5 查看当前版本

当前版本是 6.7U3

```
[root@localhost:~] vmware -vl
VMware ESXi 6.7.0 build-19195723
VMware ESXi 6.7.0 Update 3
```

查看上传的文件

```
[root@localhost:~] cd /vmfs/volumes/datastore1
[root@localhost:/vmfs/volumes/65f7a9a9-c736b201-4867-000c2929362c] ls
VMware-ESXi-7.0.3-22348816-HPE-703.0.0.11.5.0.6-Oct2023-depot.zip
```

### 6 查看配置文件

**VMware-ESXi-7.0.3-22348816-HPE-703.0.0.11.5.0.6-Oct2023-depot.zip** 是下载的升级文件名称

```
esxcli software sources profile list -d /vmfs/volumes/datastore1/VMware-ESXi-7.0.3-22348816-HPE-703.0.0.11.5.0.6-Oct2023-depot.zip
```

HPE定制的升级包

```
[root@localhost:~] esxcli software sources profile list -d /vmfs/volumes/datastore1/VMware-ESXi-7.0.3-22348816-HPE-703.0.0.11.5.0.6-Oct2023-depot.zip
Name                               Vendor                      Acceptance Level  Creation Time        Modification Time
---------------------------------  --------------------------  ----------------  -------------------  -------------------
HPE-Custom-AddOn_703.0.0.11.5.0-6  Hewlett Packard Enterprise  PartnerSupported  2023-10-03T14:15:28  2023-10-03T14:15:28
[root@localhost:~] 

```

一般的升级包会有多个配置文件，一般选择**standard 结尾的那个profile且不带s的**， no-tools结尾的一般用于pxe ，比如 下面的 `ESXi-7.0U3n-21930508-standard`

```
[root@localhost:~] esxcli software sources profile list -d /vmfs/volumes/datastore1/VMware-ESXi-7.0U3n-21930508-depot.zip
Name                           Vendor        Acceptance Level  Creation Time        Modification Time
-----------------------------  ------------  ----------------  -------------------  -----------------
ESXi-7.0U3n-21930508-standard  VMware, Inc.  PartnerSupported  2023-07-06T00:00:00  2023-07-06T00:00:00
ESXi-7.0U3n-21930508-no-tools  VMware, Inc.  PartnerSupported  2023-07-06T00:00:00  2023-06-15T12:39:40

```

### 7 升级命令

```
esxcli software profile update -p HPE-Custom-AddOn_703.0.0.11.5.0-6 -d /vmfs/volumes/datastore1/VMware-ESXi-7.0.3-22348816-HPE-703.0.0.11.5.0.6-Oct2023-depot.zip
```

代码

```
[root@localhost:~] esxcli software profile update -p HPE-Custom-AddOn_703.0.0.11.5.0-6 -d /vmfs/volumes/datastore1/VMware-ESXi-7.0.3-22348816-HPE-703.0.0.11.5.0.6-Oct2023-depot.zip
Update Result
   Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
   Reboot Required: true
   VIBs Installed: BCM_bootbank_bnxtnet_226.0.121.0-1OEM.700.1.0.15843807, BCM_bootbank_bnxtroce_226.0.121.0-1OEM.700.1.0.15843807, BCM_bootbank_lsi-mr3_7.724.03.00-1OEM.700.1.0.15843807, EMU_bootbank_lpfc_14.2.567.0-1OEM.700.1.0.15843807, HPE_bootbank_amsd_701.11.9.5.16-1OEM.701.0.0.16850804, HPE_bootbank_amsdv_701.11.5.0.2-1OEM.701.0.0.16850804, HPE_bootbank_fc-enablement_700.3.9.0.4-1OEM.700.1.0.15843807, HPE_bootbank_hpe-upgrade_901.1.4.9-1OEM.700.1.0.15843807, HPE_bootbank_ilo_700.10.8.1.6-1OEM.700.1.0.15843807, HPE_bootbank_ilorest_700.4.5.0.0.3-15843807, HPE_bootbank_sut_701.4.5.0.9-1OEM.701.0.0.16850804, HPE_bootbank_vnic-enablement_700.2.10.10-1OEM.700.1.0.15843807, INT_bootbank_i40en_2.4.2.0-1OEM.700.1.0.15843807, INT_bootbank_iavmd_3.2.0.1008-1OEM.700.1.0.15843807, INT_bootbank_icen_1.11.2.0-1OEM.700.1.0.15843807, INT_bootbank_igbn_1.11.2.0-1OEM.700.1.0.15843807, INT_bootbank_ixgben_1.14.2.0-1OEM.700.1.0.15843807, MEL_bootbank_mft-oem_4.25.0.703-0, MEL_bootbank_mft_4.25.0.703-0, MEL_bootbank_nmlx4-core_3.19.70.2001-1OEM.700.1.0.15843807, MEL_bootbank_nmlx4-en_3.19.70.2001-1OEM.700.1.0.15843807, MEL_bootbank_nmlx4-rdma_3.19.70.2001-1OEM.700.1.0.15843807, MEL_bootbank_nmlx5-core_4.22.73.1004-1OEM.703.0.0.18644231, MEL_bootbank_nmlx5-rdma_4.22.73.1004-1OEM.703.0.0.18644231, MEL_bootbank_nmst_4.25.0.703-1OEM.703.0.0.18644231, MIS_bootbank_smartpqi_70.4532.0.108-1OEM.700.1.0.15843807, MIS_bootbank_ssacli2_6.25.9.0-7.0.0.15525992.oem, MVL_bootbank_qlnativefc_5.3.81.2-1OEM.703.0.0.18644231, PEN_bootbank_ionic-en_18.0.12-1OEM.700.1.0.15843807, QLC_bootbank_qcnic_2.0.66.0-1OEM.700.1.0.15843807, QLC_bootbank_qedentv_3.70.35.0-1OEM.700.1.0.15843807, QLC_bootbank_qedf_2.72.2.0-1OEM.700.1.0.15843807, QLC_bootbank_qedi_2.72.2.0-1OEM.700.1.0.15843807, QLC_bootbank_qedrntv_3.70.33.0-1OEM.700.1.0.15843807, QLC_bootbank_qfle3_1.4.46.0-1OEM.700.1.0.15843807, QLC_bootbank_qfle3f_2.1.32.0-1OEM.700.1.0.15843807, QLC_bootbank_qfle3i_2.1.14.0-1OEM.700.1.0.15843807, VMW_bootbank_atlantic_1.0.3.0-8vmw.703.0.20.19193900, VMW_bootbank_brcmfcoe_12.0.1500.2-3vmw.703.0.20.19193900, VMW_bootbank_elxiscsi_12.0.1200.0-9vmw.703.0.20.19193900, VMW_bootbank_elxnet_12.0.1250.0-5vmw.703.0.20.19193900, VMW_bootbank_irdman_1.3.1.22-1vmw.703.0.50.20036589, VMW_bootbank_iser_1.1.0.1-1vmw.703.0.50.20036589, VMW_bootbank_lpnic_11.4.62.0-1vmw.703.0.20.19193900, VMW_bootbank_lsi-msgpt2_20.00.06.00-4vmw.703.0.20.19193900, VMW_bootbank_lsi-msgpt35_19.00.02.00-1vmw.703.0.20.19193900, VMW_bootbank_lsi-msgpt3_17.00.12.00-2vmw.703.0.105.22348816, VMW_bootbank_mtip32xx-native_3.9.8-1vmw.703.0.20.19193900, VMW_bootbank_ne1000_0.9.0-1vmw.703.0.50.20036589, VMW_bootbank_nenic_1.0.33.0-1vmw.703.0.20.19193900, VMW_bootbank_nfnic_4.0.0.70-1vmw.703.0.20.19193900, VMW_bootbank_nhpsa_70.0051.0.100-4vmw.703.0.20.19193900, VMW_bootbank_ntg3_4.1.9.0-5vmw.703.0.90.21686933, VMW_bootbank_nvme-pcie_1.2.3.16-3vmw.703.0.105.22348816, VMW_bootbank_nvmerdma_1.0.3.5-1vmw.703.0.20.19193900, VMW_bootbank_nvmetcp_1.0.0.1-1vmw.703.0.35.19482537, VMW_bootbank_nvmxnet3-ens_2.0.0.22-1vmw.703.0.20.19193900, VMW_bootbank_nvmxnet3_2.0.0.30-1vmw.703.0.20.19193900, VMW_bootbank_pvscsi_0.1-4vmw.703.0.20.19193900, VMW_bootbank_qflge_1.1.0.11-1vmw.703.0.20.19193900, VMW_bootbank_rste_2.0.2.0088-7vmw.703.0.20.19193900, VMW_bootbank_sfvmk_2.4.0.2010-6vmw.703.0.20.19193900, VMW_bootbank_vmkata_0.1-1vmw.703.0.20.19193900, VMW_bootbank_vmkfcoe_1.0.0.2-1vmw.703.0.20.19193900, VMW_bootbank_vmkusb_0.1-8vmw.703.0.85.21424296, VMW_bootbank_vmw-ahci_2.0.11-2vmw.703.0.105.22348816, VMware_bootbank_bmcal_7.0.3-0.105.22348816, VMware_bootbank_cpu-microcode_7.0.3-0.105.22348816, VMware_bootbank_crx_7.0.3-0.105.22348816, VMware_bootbank_elx-esx-libelxima.so_12.0.1200.0-4vmw.703.0.20.19193900, VMware_bootbank_esx-base_7.0.3-0.105.22348816, VMware_bootbank_esx-dvfilter-generic-fastpath_7.0.3-0.105.22348816, VMware_bootbank_esx-ui_2.11.2-21988676, VMware_bootbank_esx-update_7.0.3-0.105.22348816, VMware_bootbank_esx-xserver_7.0.3-0.105.22348816, VMware_bootbank_esxio-combiner_7.0.3-0.105.22348816, VMware_bootbank_gc_7.0.3-0.105.22348816, VMware_bootbank_loadesx_7.0.3-0.105.22348816, VMware_bootbank_lsuv2-hpv2-hpsa-plugin_1.0.0-3vmw.703.0.20.19193900, VMware_bootbank_lsuv2-intelv2-nvme-vmd-plugin_2.7.2173-1vmw.703.0.20.19193900, VMware_bootbank_lsuv2-lsiv2-drivers-plugin_1.0.0-12vmw.703.0.50.20036589, VMware_bootbank_lsuv2-nvme-pcie-plugin_1.0.0-1vmw.703.0.20.19193900, VMware_bootbank_lsuv2-oem-dell-plugin_1.0.0-1vmw.703.0.20.19193900, VMware_bootbank_lsuv2-oem-hp-plugin_1.0.0-1vmw.703.0.20.19193900, VMware_bootbank_lsuv2-oem-lenovo-plugin_1.0.0-1vmw.703.0.20.19193900, VMware_bootbank_lsuv2-smartpqiv2-plugin_1.0.0-9vmw.703.0.105.22348816, VMware_bootbank_native-misc-drivers_7.0.3-0.105.22348816, VMware_bootbank_trx_7.0.3-0.105.22348816, VMware_bootbank_vdfs_7.0.3-0.105.22348816, VMware_bootbank_vmware-esx-esxcli-nvme-plugin_1.2.0.44-1vmw.703.0.20.19193900, VMware_bootbank_vsan_7.0.3-0.105.22348816, VMware_bootbank_vsanhealth_7.0.3-0.105.22348816, VMware_locker_tools-light_12.2.6.22229486-22348808
   VIBs Removed: BCM_bootbank_bnxtnet_219.0.29.0-1OEM.670.0.0.8169922, BCM_bootbank_bnxtroce_219.0.13.0-1OEM.670.0.0.8169922, BCM_bootbank_lsi-mr3_7.716.03.00-1OEM.670.0.0.8169922, ELX_bootbank_elx-esx-libelxima-8169922.so_12.0.1188.0-03, EMU_bootbank_brcmfcoe_12.0.1278.0-1OEM.670.0.0.8169922, EMU_bootbank_elxiscsi_12.0.1188.0-1OEM.670.0.0.8169922, EMU_bootbank_elxnet_12.0.1216.4-1OEM.670.0.0.8169922, EMU_bootbank_lpfc_12.8.528.14-1OEM.670.0.0.8169922, HPE_bootbank_amsd_670.11.8.0.15-1.7535516, HPE_bootbank_amshelpr_670.11.8.0.8-1.7535516, HPE_bootbank_bootcfg_6.7.0.02-06.00.14.7535516, HPE_bootbank_conrep_670.10.8.0.18-6.7.0.7535516, HPE_bootbank_cru_670.6.7.10.14-1OEM.670.0.0.7535516, HPE_bootbank_fc-enablement_670.3.80.6-7535516, HPE_bootbank_hponcfg_6.7.0.5.5-0.18.7535516, HPE_bootbank_ilo_670.10.7.5.2-1OEM.670.0.0.7535516, HPE_bootbank_ilorest_650.3.3.0.2-4240417, HPE_bootbank_oem-build_670.U3.10.9.0.1-7535516, HPE_bootbank_scsi-hpdsa_6.0.0.76-1OEM.600.0.0.2494585, HPE_bootbank_smx-provider_670.03.16.00.3-7535516, HPE_bootbank_sut_6.7.0.2.9.1.0-21, HPE_bootbank_sutesxcli_6.7.0.2.9.1.0-9, HPE_bootbank_testevent_6.7.0.02-01.00.12.7535516, INT_bootbank_i40en_1.14.3.0-1OEM.670.0.0.8169922, INT_bootbank_iavmd_2.7.0.1157-1OEM.670.0.0.8169922, INT_bootbank_icen_1.7.3.0-1OEM.670.0.0.8169922, INT_bootbank_igbn_1.5.2.0-1OEM.670.0.0.8169922, INT_bootbank_ixgben_1.10.3.0-1OEM.670.0.0.8169922, MEL_bootbank_nmlx4-core_3.17.70.1-1OEM.670.0.0.8169922, MEL_bootbank_nmlx4-en_3.17.70.1-1OEM.670.0.0.8169922, MEL_bootbank_nmlx4-rdma_3.17.70.1-1OEM.670.0.0.8169922, MEL_bootbank_nmlx5-core_4.17.71.1-1OEM.670.0.0.8169922, MEL_bootbank_nmlx5-rdma_4.17.71.1-1OEM.670.0.0.8169922, MEL_bootbank_nmst_4.12.0.105-1OEM.650.0.0.4598673, MIS_bootbank_hpessacli_5.30.6.0-6.7.0.7535516.oem, MSCC_bootbank_smartpqi_67.4150.0.119-1OEM.670.0.0.8169922, Marvell_bootbank_qlnativefc_3.1.46.0-1OEM.670.0.0.8169922, Microsemi_bootbank_nhpsa_67.0072.0.149-1OEM.670.0.0.8169922, PEN_bootbank_ionic-en_18.0.0-1OEM.650.0.0.4598673, QLC_bootbank_qcnic_1.0.60.0-1OEM.670.0.0.8169922, QLC_bootbank_qedentv-ens_3.11.43.0-1OEM.670.0.0.8169922, QLC_bootbank_qedentv_3.11.45.0-1OEM.670.0.0.8169922, QLC_bootbank_qedf_2.2.70.0-1OEM.670.0.0.8169922, QLC_bootbank_qedi_2.19.70.0-1OEM.670.0.0.8169922, QLC_bootbank_qedrntv_3.11.43.0-1OEM.670.0.0.8169922, QLC_bootbank_qfle3_1.1.22.0-1OEM.670.0.0.8169922, QLC_bootbank_qfle3f_1.1.25.0-1OEM.670.0.0.8169922, QLC_bootbank_qfle3i_1.1.5.0-1OEM.670.0.0.8169922, VMW_bootbank_ata-libata-92_3.00.9.2-16vmw.670.0.0.8169922, VMW_bootbank_ata-pata-amd_0.3.10-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-atiixp_0.4.6-4vmw.670.0.0.8169922, VMW_bootbank_ata-pata-cmd64x_0.2.5-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-hpt3x2n_0.3.4-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-pdc2027x_1.0-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-serverworks_0.4.3-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-sil680_0.4.8-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-via_0.3.3-2vmw.670.0.0.8169922, VMW_bootbank_block-cciss_3.6.14-10vmw.670.0.0.8169922, VMW_bootbank_char-random_1.0-3vmw.670.0.0.8169922, VMW_bootbank_ehci-ehci-hcd_1.0-4vmw.670.0.0.8169922, VMW_bootbank_hid-hid_1.0-3vmw.670.0.0.8169922, VMW_bootbank_ima-qla4xxx_2.02.18-1vmw.670.0.0.8169922, VMW_bootbank_ipmi-ipmi-devintf_39.1-5vmw.670.1.28.10302608, VMW_bootbank_ipmi-ipmi-msghandler_39.1-5vmw.670.1.28.10302608, VMW_bootbank_ipmi-ipmi-si-drv_39.1-5vmw.670.1.28.10302608, VMW_bootbank_iser_1.0.0.0-1vmw.670.1.28.10302608, VMW_bootbank_lpnic_11.4.59.0-1vmw.670.0.0.8169922, VMW_bootbank_lsi-msgpt2_20.00.06.00-2vmw.670.3.73.14320388, VMW_bootbank_lsi-msgpt35_09.00.00.00-5vmw.670.3.73.14320388, VMW_bootbank_lsi-msgpt3_17.00.02.00-1vmw.670.3.73.14320388, VMW_bootbank_misc-drivers_6.7.0-2.48.13006603, VMW_bootbank_mtip32xx-native_3.9.8-1vmw.670.1.28.10302608, VMW_bootbank_ne1000_0.8.4-2vmw.670.2.48.13006603, VMW_bootbank_nenic_1.0.29.0-1vmw.670.3.73.14320388, VMW_bootbank_net-cdc-ether_1.0-3vmw.670.0.0.8169922, VMW_bootbank_net-e1000_8.0.3.1-5vmw.670.3.112.16701467, VMW_bootbank_net-e1000e_3.2.2.1-2vmw.670.0.0.8169922, VMW_bootbank_net-enic_2.1.2.38-2vmw.670.0.0.8169922, VMW_bootbank_net-fcoe_1.0.29.9.3-7vmw.670.0.0.8169922, VMW_bootbank_net-forcedeth_0.61-2vmw.670.0.0.8169922, VMW_bootbank_net-libfcoe-92_1.0.24.9.4-8vmw.670.0.0.8169922, VMW_bootbank_net-nx-nic_5.0.621-5vmw.670.0.0.8169922, VMW_bootbank_net-tg3_3.131d.v60.4-2vmw.670.0.0.8169922, VMW_bootbank_net-usbnet_1.0-3vmw.670.0.0.8169922, VMW_bootbank_net-vmxnet3_1.1.3.0-3vmw.670.3.104.16075168, VMW_bootbank_nfnic_4.0.0.44-0vmw.670.3.104.16075168, VMW_bootbank_ntg3_4.1.5.0-0vmw.670.3.116.16713306, VMW_bootbank_nvme_1.2.2.28-5vmw.670.3.159.18828794, VMW_bootbank_nvmxnet3-ens_2.0.0.21-1vmw.670.0.0.8169922, VMW_bootbank_nvmxnet3_2.0.0.29-1vmw.670.1.28.10302608, VMW_bootbank_ohci-usb-ohci_1.0-3vmw.670.0.0.8169922, VMW_bootbank_pvscsi_0.1-2vmw.670.0.0.8169922, VMW_bootbank_qflge_1.1.0.11-1vmw.670.0.0.8169922, VMW_bootbank_sata-ahci_3.0-26vmw.670.0.0.8169922, VMW_bootbank_sata-ata-piix_2.12-10vmw.670.0.0.8169922, VMW_bootbank_sata-sata-nv_3.5-4vmw.670.0.0.8169922, VMW_bootbank_sata-sata-promise_2.12-3vmw.670.0.0.8169922, VMW_bootbank_sata-sata-sil24_1.1-1vmw.670.0.0.8169922, VMW_bootbank_sata-sata-sil_2.3-4vmw.670.0.0.8169922, VMW_bootbank_sata-sata-svw_2.3-3vmw.670.0.0.8169922, VMW_bootbank_scsi-aacraid_1.1.5.1-9vmw.670.0.0.8169922, VMW_bootbank_scsi-adp94xx_1.0.8.12-6vmw.670.0.0.8169922, VMW_bootbank_scsi-aic79xx_3.1-6vmw.670.0.0.8169922, VMW_bootbank_scsi-fnic_1.5.0.45-3vmw.670.0.0.8169922, VMW_bootbank_scsi-ips_7.12.05-4vmw.670.0.0.8169922, VMW_bootbank_scsi-iscsi-linux-92_1.0.0.2-3vmw.670.0.0.8169922, VMW_bootbank_scsi-libfc-92_1.0.40.9.3-5vmw.670.0.0.8169922, VMW_bootbank_scsi-megaraid-mbox_2.20.5.1-6vmw.670.0.0.8169922, VMW_bootbank_scsi-megaraid-sas_6.603.55.00-2vmw.670.0.0.8169922, VMW_bootbank_scsi-megaraid2_2.00.4-9vmw.670.0.0.8169922, VMW_bootbank_scsi-mpt2sas_19.00.00.00-2vmw.670.0.0.8169922, VMW_bootbank_scsi-mptsas_4.23.01.00-10vmw.670.0.0.8169922, VMW_bootbank_scsi-mptspi_4.23.01.00-10vmw.670.0.0.8169922, VMW_bootbank_scsi-qla4xxx_5.01.03.2-7vmw.670.0.0.8169922, VMW_bootbank_sfvmk_1.0.0.1003-7vmw.670.3.104.16075168, VMW_bootbank_shim-iscsi-linux-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-iscsi-linux-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libata-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libata-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libfc-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libfc-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libfcoe-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libfcoe-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-vmklinux-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-vmklinux-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-vmklinux-9-2-3-0_6.7.0-0.0.8169922, VMW_bootbank_uhci-usb-uhci_1.0-3vmw.670.0.0.8169922, VMW_bootbank_usb-storage-usb-storage_1.0-3vmw.670.0.0.8169922, VMW_bootbank_usbcore-usb_1.0-3vmw.670.0.0.8169922, VMW_bootbank_vmkata_0.1-1vmw.670.0.0.8169922, VMW_bootbank_vmkfcoe_1.0.0.1-1vmw.670.1.28.10302608, VMW_bootbank_vmkplexer-vmkplexer_6.7.0-0.0.8169922, VMW_bootbank_vmkusb_0.1-4vmw.670.3.159.18828794, VMW_bootbank_vmw-ahci_2.0.7-2vmw.670.3.143.17700523, VMW_bootbank_xhci-xhci_1.0-3vmw.670.0.0.8169922, VMware_bootbank_cpu-microcode_6.7.0-3.155.18812553, VMware_bootbank_esx-base_6.7.0-3.163.19195723, VMware_bootbank_esx-dvfilter-generic-fastpath_6.7.0-0.0.8169922, VMware_bootbank_esx-ui_1.33.7-15803439, VMware_bootbank_esx-update_6.7.0-3.163.19195723, VMware_bootbank_esx-xserver_6.7.0-3.73.14320388, VMware_bootbank_lsu-hp-hpsa-plugin_2.0.0-16vmw.670.1.28.10302608, VMware_bootbank_lsu-intel-vmd-plugin_1.0.0-2vmw.670.1.28.10302608, VMware_bootbank_lsu-lsi-drivers-plugin_1.0.0-1vmw.670.2.48.13006603, VMware_bootbank_lsu-lsi-lsi-mr3-plugin_1.0.0-13vmw.670.1.28.10302608, VMware_bootbank_lsu-lsi-lsi-msgpt3-plugin_1.0.0-9vmw.670.2.48.13006603, VMware_bootbank_lsu-lsi-megaraid-sas-plugin_1.0.0-9vmw.670.0.0.8169922, VMware_bootbank_lsu-lsi-mpt2sas-plugin_2.0.0-7vmw.670.0.0.8169922, VMware_bootbank_lsu-smartpqi-plugin_1.0.0-4vmw.670.3.159.18828794, VMware_bootbank_native-misc-drivers_6.7.0-3.89.15160138, VMware_bootbank_rste_2.0.2.0088-7vmw.670.0.0.8169922, VMware_bootbank_vmware-esx-esxcli-nvme-plugin_1.2.0.36-2vmw.670.3.116.16713306, VMware_bootbank_vsan_6.7.0-3.163.19184739, VMware_bootbank_vsanhealth_6.7.0-3.163.19184740, VMware_locker_tools-light_11.3.5.18557794-18812553
   VIBs Skipped: 

```

### 8 升级完成 重启

```
root@localhost:~] reboot 
[root@localhost:~] Connection closing...Socket close.

Connection closed by foreign host.

```

### 9 再次查看版本

重启后SSH会关闭，重新登陆web打开

```
[root@localhost:~] vmware -vl
VMware ESXi 7.0.3 build-22348816
VMware ESXi 7.0 Update 3
```







参考：

{% embed url="https://www.dinghui.org/upgrade-esxi-patch.html" %}
