<div align="center">

# Bài tập Linux ngày 29/06

**Lời giải Đề của Tuấn**

| Họ và tên | Mã sinh viên |
| --- | --- |
| Bùi Tuấn Anh | 2300742 |

</div>

## Câu 1 (1 điểm)

Kiểm tra xem hệ thống có cài đặt **NFS** hay không. Nếu chưa được cài đặt thì dùng lệnh `rpm` để cài.

```bash
if ! rpm -q nfs-utils >/dev/null 2>&1; then
    rpm -ivh /path/to/nfs-utils.rpm
fi
```

## Câu 2 (1 điểm)

Kiểm tra xem hệ thống có cài đặt **PORTMAP** hay không. Nếu chưa được cài đặt thì dùng lệnh `rpm` để cài.

```bash
if ! rpm -q portmap >/dev/null 2>&1 && ! rpm -q rpcbind >/dev/null 2>&1; then
    rpm -ivh /path/to/portmap.rpm
fi
```
Code: 
systemctl status nfs-kernel-server

systemctl status rpcbind
<img width="1155" height="882" alt="z7986981479466_9d6baa66b7a2208e91c4468cd843aa53" src="https://github.com/user-attachments/assets/212b1f7e-b249-4b44-aa2b-6676208b7e13" />

## Câu 3 (1 điểm)

Export thư mục `/usr/share` chỉ cho phép máy có địa chỉ `192.168.xx.yy` mount vào mount point `/mnt/share` để sử dụng.

```bash
mkdir -p /mnt/share
grep -q "^/usr/share " /etc/exports || echo "/usr/share 192.168.xx.yy(ro,sync,no_subtree_check)" >> /etc/exports
```
Code: 
cat /etc/exports
<img width="603" height="251" alt="z7986991127388_e94ee6821c2ecacf9716f02d4b11829e" src="https://github.com/user-attachments/assets/e5893701-56c7-4291-9d75-9ccf755472ce" />

## Câu 4 (1 điểm)

Export thư mục `/soft` với quyền **RW** và chỉ cho phép các máy trong mạng `192.168.xx.0/24` mount vào mount point `/mnt/soft`.

```bash
mkdir -p /soft
grep -q "^/soft " /etc/exports || echo "/soft 192.168.xx.0/24(rw,sync,no_subtree_check)" >> /etc/exports

service rpcbind start
service nfs-server start
exportfs -ra
exportfs -v
```

## Câu 5 (1 điểm)

Dùng lệnh `rpcinfo` để kiểm tra dịch vụ **NFS** có đang hoạt động trong hệ thống hay không.

```bash
rpcinfo -p localhost | grep -E "nfs|100003"
```
Code:
sudo exportfs -v
<img width="1152" height="270" alt="z7986991689205_1a19277c7b24d199828110c4d0824c22" src="https://github.com/user-attachments/assets/626d1d69-5757-413b-85ca-32a92f44095c" />

## Câu 6 (1 điểm)

Dùng lệnh `rpcinfo` để kiểm tra dịch vụ **PORTMAP** có đang hoạt động trong hệ thống hay không.

```bash
rpcinfo -p localhost | grep -E "portmapper|100000"
```
Code:
rpcinfo -p | grep nfs
rpcinfo -p | grep portmapper

<img width="547" height="436" alt="z7986996644882_4dd7d28e8dcea919e769837096bfcf7f" src="https://github.com/user-attachments/assets/9329bc17-c91a-4cfe-801a-3201a12ce8b1" />

## Câu 7 (1 điểm)

Kiểm tra và xử lý các sự cố thống kê lỗi trên **NFS Server**.

```bash
systemctl status nfs-server --no-pager
journalctl -u nfs-server -n 30 --no-pager
nfsstat -s
```

## Câu 8 (1 điểm)

Liệt kê các filesystem của hệ thống đã được mount.

```bash
findmnt
```
Code:
df -h
<img width="947" height="523" alt="z7987003095442_1a915357f400f22e7ee3746c275de34f" src="https://github.com/user-attachments/assets/3b1cc66d-8251-4253-85d5-26175711838e" />

## Câu 9 (1 điểm)

Xem các export directory.

```bash
exportfs -v
showmount -e localhost
```

## Câu 10 (1 điểm)

Theo dõi và thống kê sử dụng tài nguyên hệ thống của User.

```bash
ps -eo user,pid,%cpu,%mem,comm --sort=user | head -50
top -b -n 1 | head -30
```
Code:
top
<img width="1052" height="907" alt="anh6" src="https://github.com/user-attachments/assets/15738cbd-fb21-4422-a395-53dd749ef55b" />
