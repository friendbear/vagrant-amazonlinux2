# AmazonLinux2 

Local Startup

## Vagrant keygen

```
PKEY=$(vagrant ssh-config | grep IdentityFile | awk '{print $2}')

PUBKEY=$(ssh-keygen -yf ${PKEY})
```

## create seed.iso
`$ hdiutil makehybrid -o seed.iso -hfs -joliet -iso -default-volume-name cidata seedconfig/`

# Links

[amazon-linux-2-vm](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html)
