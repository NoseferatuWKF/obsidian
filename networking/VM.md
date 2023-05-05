## qemu

```bash
sudo apt install qemu qemu-kvm
apt show qemu-system-x86
kvm -version
# remember to enable hardware virtualization first
qemu-image create -f qcow2 Image.img 10G
# virtual cpu, opengl, audio passthrough
qemu-system-x86 -boot d -cdrom /path/to/iso -m 2G -drive file=/path/to/vdi -smp 2 -cpu qemu64 -vga virtio -enable-kvm -display sdl,gl=on,show-cursor=on -device ich9-intel-hda,addr=1f.1 -audiodev pa,id=snd0 -device hda-output,audiodev=snd0 -machine q35
```