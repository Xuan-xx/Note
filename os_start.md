# 操作系统启动
## operating system start
### x86 pc
1. enter real mode(CS=0XFFFF,ip=0)
2. point CS << 4 + ip = 0XFFFF0(ROM BIOS)
3. check RAM,keyboard,screen,disk....
4. read 0 track and 0 sector 0X7c00(512byte)---boot
5. set CS=0x07c0,ip=0
6. boot read setup system

