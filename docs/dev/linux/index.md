---
title: 关于linux的知识
toc: true
tags: 
date: 2025-04-07
---

# 关于linux的一般知识

## path

```
/usr/share/fonts/
```

```
/etc/profile
/etc/environment
/etc/zsh/zprofile
/etc/zsh/zshenv
```

```
/etc/hosts
/etc/hostname
```

```
/etc/ssh/
ssh
├── moduli
├── ssh_config
├── ssh_config.d
│   ├── 20-systemd-ssh-proxy.conf ⇒ ../../../usr/lib/systemd/ssh_config.d/20-systemd-ssh-proxy.conf
│   └── 30-libvirt-ssh-proxy.conf
├── ssh_host_ecdsa_key
├── ssh_host_ecdsa_key.pub
├── ssh_host_ed25519_key
├── ssh_host_ed25519_key.pub
├── ssh_host_rsa_key
├── ssh_host_rsa_key.pub
├── sshd_config
├── sshd_config.d
│   ├── 20-systemd-userdb.conf ⇒ ../../../usr/lib/systemd/sshd_config.d/20-systemd-userdb.conf
│   └── 99-archlinux.conf
└── sshd_config.pacnew
```
