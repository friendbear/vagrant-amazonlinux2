# AmazonLinux2 

Local Startup

## Vagrant keygen

```
PKEY=$(vagrant ssh-config | grep IdentityFile | awk '{print $2}')

PUBKEY=$(ssh-keygen -yf ${PKEY})
```

## create seed.iso
`$ hdiutil makehybrid -o seed.iso -hfs -joliet -iso -default-volume-name cidata seedconfig/`

or 
```
docker run -it --rm -v $(pwd):/data debian sh

# exec docker container
apt-get update && apt-get install -y genisoimage
cd /data; genisoimage -output seed.iso -volid cidata -joliet -rock user-data meta-data
exit
```

## Vagrant up

```
VM=amznlinux2
# 仮想マシンを作成
VBoxManage createvm --name "$VM" --ostype "RedHat_64" --register
# 仮想ストレージアレイを追加
VBoxManage storagectl "$VM" --name "SATA Controller" --add "sata" --controller "IntelAHCI"
VBoxManage storagectl "$VM" --name "IDE Controller" --add "ide"
# 仮想ディスクとISOをアタッチ
VBoxManage storageattach "$VM" --storagectl "SATA Controller" \
  --port 0 --device 0 --type hdd --medium amznlinux2.vdi
VBoxManage storageattach "$VM" --storagectl "SATA Controller" \
  --port 1 --device 0 --type dvddrive --medium seed.iso
VBoxManage storageattach "$VM" --storagectl "IDE Controller" \
  --port 0 --device 0 --type dvddrive --medium /path/to/VBoxGuestAddtions.iso
# ポートフォワードとメモリを調整
VBoxManage modifyvm "$VM" --natpf1 "ssh,tcp,127.0.0.1,2222,,22" --memory 1024 --vram 8
# 仮想マシンを起動
VBoxManage startvm "$VM" --type headless
```

# Links

[amazon-linux-2-vm](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html)
