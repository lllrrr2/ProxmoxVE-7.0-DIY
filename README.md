## Proxmox VE 7.0 换源

### SSH登录到pve后台，然后一条一条的执行命令

#### 处理掉企业源
```
rm -rf /etc/apt/sources.list.d/pve-install-repo.list
```

```
echo "#deb https://enterprise.proxmox.com/debian/pve Bullseye pve-enterprise" > /etc/apt/sources.list.d/pve-enterprise.list
```


#### 开始换源

```
wget https://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg
```

```
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/pve bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
```
```
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/ceph-pacific bullseye main" > /etc/apt/sources.list.d/ceph.list
```

```
sed -i.bak "s#http://download.proxmox.com/debian#https://mirrors.ustc.edu.cn/proxmox/debian#g" /usr/share/perl5/PVE/CLI/pveceph.pm
```
```
sed -i.bak "s#ftp.debian.org/debian#mirrors.aliyun.com/debian#g" /etc/apt/sources.list
```
```
sed -i "s#security.debian.org#mirrors.aliyun.com/debian-security#g" /etc/apt/sources.list
```
```
echo "deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription" >>  /etc/apt/sources.list
```

#### 最后更新
```
apt update && apt dist-upgrade -y
```



##  Proxmox VE 7.0 关订阅提示

编辑打开这个文件：/usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js

搜索'active'，找到这一段：
```
			.data.status.toLowerCase() !== 'active') {
			Ext.Msg.show({
			    title: gettext('No valid subscription'),
			    icon: Ext.Msg.WARNING,
			    message: Proxmox.Utils.getNoSubKeyHtml(res.data.url),
			    buttons: Ext.Msg.OK,
			    callback: function(btn) {
				if (btn !== 'ok') {
				    return;
				}
				orig_cmd();
			    },
			});
		    } else {
			orig_cmd();
		    }
```

直接删掉其中几行，变成如下：

```
			.data.status.toLowerCase() !== 'active') {
				orig_cmd();
		    } else {
			orig_cmd();
		    }

```
最后保存即可。





