# 高通随身WIFI，短信转发自动开启WIFI，关闭数据流量

短信转发自动开启WIFI 关闭数据流量和热点

### 前提

**1.WIFI 首先root**

### 开始

#### 1.连接ADB 上传脚本文件

```
# 上传enable_wifi.sh文件到wifi棒子的/data/local/tmp/目录
adb push enable_wifi.sh /data/local/tmp/
```

#### 2.挂载 system 分区可读写

```
# 挂载 system 分区可读写
adb shell   #通过 ADB 连接到 Android 设备的命令行界面
su    #切换到超级用户 (root) 权限，这需要设备已经获取 root 权限
mount -o remount,rw /system   #将 /system 分区重新挂载为可读写模式（默认通常是只读的）


adb shell 
su
mount -o remount,rw /system
```

#### 3.配置WIFI自启动脚本 并修改权限

```
# 配置WIFI自启动脚本 并修改权限
cp -a /data/local/tmp/enable_wifi.sh /data/adb/service.d/
chmod 777 /data/adb/service.d/enable_wifi.sh
chown root:root /data/adb/service.d/enable_wifi.sh


cp -a /data/local/tmp/enable_wifi.sh /data/adb/service.d/
将/data/local/tmp/目录下的enable_wifi.sh脚本，复制到/data/adb/service.d/目录
-a参数表示保持文件的原有属性（如权限、时间戳等）进行递归复制
/data/adb/service.d/是 Magisk 等 root 工具的启动脚本目录，该目录下的脚本会在系统启动时自动执行
chmod 777 /data/adb/service.d/enable_wifi.sh
为复制后的脚本设置权限：777表示所有用户（所有者、所属组、其他用户）都拥有读、写、执行权限
这是为了确保脚本能够被系统执行（但777权限过于宽松，实际使用中755通常已足够，且更安全）
chown root:root /data/adb/service.d/enable_wifi.sh
将脚本的所有者和所属组设置为root（系统超级用户）
符合 Android 系统中系统级脚本的权限规范，避免因权限不足导致脚本无法执行
```

#### 4.选择模式

```
# 默认脚本刷入后采用热点和卡自带流量模式，如果需要使用WiFi，那么在
 
adb shell 
su
 
echo 1 > /data/adb/enable_wifi
 
# 重启设备后讲默认开启wif，关闭热点、禁用4G移动数据
 
# 1 开启wif，关闭热点、禁用4G移动数据
 
# 0 关闭WiFi，开启热点 (设置--移动数据自行ARDC开启，避免无流量的卡出问题)


adb shell连接到 Android 设备的命令行界面。
su切换到 root 权限（需要设备已 root），否则后续写入文件的操作会因权限不足失败。
echo 1 > /data/adb/enable_wifi
在/data/adb/目录下创建（或覆盖）一个名为enable_wifi的文件
向该文件写入内容1（通常1表示 “启用”，0表示 “禁用”，具体逻辑由读取该文件的程序定义）
```

```shell
# 上传enable_wifi.sh文件到wifi棒子的/data/local/tmp/目录
adb push enable_wifi.sh /data/local/tmp/
 
# 挂载 system 分区可读写
adb shell - 通过 ADB 连接到 Android 设备的命令行界面
su - 切换到超级用户 (root) 权限，这需要设备已经获取 root 权限
mount -o remount,rw /system - 将 /system 分区重新挂载为可读写模式（默认通常是只读的）


adb shell 
su
mount -o remount,rw /system
# 配置WIFI自启动脚本 并修改权限
cp -a /data/local/tmp/enable_wifi.sh /data/adb/service.d/
chmod 777 /data/adb/service.d/enable_wifi.sh
chown root:root /data/adb/service.d/enable_wifi.sh
 
 
 
# 默认脚本刷入后采用热点和卡自带流量模式，如果需要使用WiFi，那么在
 
adb shell 
su
 
echo 1 > /data/adb/enable_wifi
 
# 重启设备后讲默认开启wif，关闭热点、禁用4G移动数据
 
# 1 开启wif，关闭热点、禁用4G移动数据
 
# 0 关闭WiFi，开启热点 (设置--移动数据自行ARDC开启，避免无流量的卡出问题)
```

### enable\_wifi.sh 文件内容

enable\_wifi.sh

```enable_wifi.sh
#!/system/bin/sh

# 开关
file_path="/data/adb/enable_wifi"

# 检查文件是否存在
if [ ! -f "$file_path" ]; then
  # 如果文件不存在，创建文件并写入 "0"
  echo "0" > "$file_path"
fi

file_content=$(cat $file_path)

LOGF=/data/local/tmp/service.d/enable_wifi.log
mkdir -p $(busybox dirname "$LOGF")
rm -f $LOGF
echo "Env info:" >> $LOGF
export >> $LOGF

SVC=/system/bin/svc

switch_data(){
    while true; do
        $SVC data enable  # 启用移动数据
        sleep 3         # 等待3秒
        $SVC data disable # 禁用移动数据
        sleep 250       # 等待250秒
    done
}

wifiMode() {
    echo "Enabling Wifi..."
    $SVC data disable # avoid expensive data usage!
    service call wifi 29 i32 0 i32 0 # name=null, enable=false
    $SVC wifi disable
    sleep 2
    $SVC wifi enable
    sleep 2
    $SVC wifi prefer
    # svc usb setFunction diag,serial_smd,rmnet_bam,adb  rndis
    # svc usb setFunction diag,serial_smd,rndis,adb  
    # $SVC usb setFunction diag,serial_smd,adb
}

tetherMode() {
    echo "Enabling Tether..."
    # $SVC data disable # avoid expensive data usage!
    $SVC wifi disable
    service call wifi 29 i32 0 i32 1 # name=null, enable=true
}


checkWifi() {
    if ip a show wlan0 | grep inet | grep -v "scope link" | grep -v "169.254" >/dev/null; then
        echo "Has WiFi IP"
        return 0
    else
        echo "No WiFi IP"
        return 1
    fi
}


mainFunc() {
    set -x
    if [ -z "$DEBUG" ]; then
        echo "Waiting for bootup!"
        sleep 60
    fi
    
    for i in $(busybox seq 1 20); do # 1min not connected then go hotspot
        wifiMode;
        if checkWifi; then
            echo "Wifi Connected, good to go!"
            break
        fi
        if [ $i = 20 ]; then
            echo "Failed to connect to WiFi! Going to tether mode!"
            tetherMode;
            break
        fi
        sleep 3
    done
    
    echo "Updating clock!"
    for i in $(busybox seq 1 20); do
        am broadcast -a android.intent.action.NETWORK_SET_TIME -f 536870912
        sleep 2
    done
}


# 检查文件内容
if [ "$file_content" = "1" ]; then
  mainFunc >> $LOGF 2>&1
else
  tetherMode >> $LOGF 2>&1
  $SVC data enable
fi
```
