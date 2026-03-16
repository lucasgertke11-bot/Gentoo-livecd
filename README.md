# Gentoo Live CD

Arquivo base do Gentoo Linux para criação de Live CD.

## Download

Arquivo: `stage3-amd64-openrc-20260315T160058Z.tar.xz` (264MB)

## Como Extrair

```bash
# Extrair o stage3
tar xf stage3-amd64-openrc-20260315T160058Z.tar.xz -C /path/to/gentoo

# Entrar no ambiente chroot
cd /path/to/gentoo
mount -t proc /proc proc
mount -t sysfs /sys sys
mount --rbind /dev dev
chroot . /bin/bash
source /etc/profile
```

## Como Criar Live CD

### 1. Configure o ambiente Gentoo

Após extrair e entrar no chroot:

```bash
# Configure o make.conf
echo 'MAKEOPTS="-j4"' >> /etc/portage/make.conf
echo 'EMERGE_DEFAULT_OPTS="--ask --verbose"' >> /etc/portage/make.conf

# Atualize o sistema base
emerge --sync
emerge --oneshot portage

# Instale ferramentas necessárias
emerge sys-kernel/linux-firmware
emerge sys-boot/grub
emerge sys-boot/genkernel
```

### 2. Configure o kernel

```bash
# Compile o kernel
genkernel all

# Ou use kernel pré-compilado
emerge sys-kernel/installkernel-gentoo
```

### 3. Configure o bootloader

```bash
# Instale o GRUB
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

### 4. Configure o sistema

```bash
# Configure rede
ln -sf /etc/init.d/net.lo /etc/init.d/net.eth0
rc-update add net.eth0 default

# Configure locale
echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen
locale-gen

# Configure senha root
passwd
```

### 5. Crie a ISO

```bash
# Instale ferramentas de livecd
emerge sys-boot/livecd-tools

# Crie a ISO
livecd --help
# ou use genkernel com opções de livecd
```

### Alternativa: Usar Calculate Linux

Se preferir uma distribuição baseada em Gentoo já pronta para Live CD:
- [Calculate Linux](https://www.calculate-linux.org/)
- Ferramenta: `cl-builder` ou `calculate-builder`

## Especificações

- **Arquitetura**: amd64 (x86-64)
- **Init**: OpenRC
- **Tamanho compactado**: ~264MB
- **Tamanho extraído**: ~1.3GB
- **Data**: 2026-03-15

## Requisitos

- Processador 64-bit (AMD/Intel)
- Mínimo 2GB de RAM
- 10GB de espaço em disco para compilação

## Links Úteis

- [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64)
- [Gentoo Downloads](https://www.gentoo.org/downloads/)
- [Gentoo Wiki - Stage File](https://wiki.gentoo.org/wiki/Stage_file)