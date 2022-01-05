# Dựng Mysql version 5.5 trên Ubuntu 20.04

### Trường hợp đã cài đặt MySQL khác loại 

Phải xóa MySQL bản cũ ấy đi xong mới tới quá trình cài đặt 

```
sudo apt-get remove --purge mysql-server mysql-client mysql-common
sudo apt-get autoremove
sudo apt-get autoclean

sudo rm -rf /var/lib/mysql
sudo rm -rf /etc/mysql
sudo apt-get autoremove
sudo apt-get autoclean
```

### Quá trình cài đặt 

- Tạo folder chưa file và tiến hành dơwnload về 

```
mkdir mysql
wget https://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.56-linux-glibc2.5-x86_64.tar.gz
sudo tar -xvf mysql-5.5.56-linux-glibc2.5-x86_64.tar.gz
```

- Tạo group, add user 
  
```
sudo groupadd mysql
sudo useradd -g mysql mysql
```

- Di chuyển tới /usr/local/ , đổi tê
```
sudo mv mysql-5.5.56-linux-glibc2.5-x86_64 /usr/local/
cd /usr/local
sudo mv mysql-5.5.56-linux-glibc2.5-x86_64 mysql
```

- Set quyền 
```
cd mysql
sudo chown -R mysql:mysql *
```

- Cài đặt các gói lib cần thiết (cũng hoạt động với Mysql 5.6) và thực thi các lệnh cài đặt mysql
```
sudo apt-get install libaio1
sudo scripts/mysql_install_db --user=mysql
```
- Phân quyền, copy file cấu hình 
```
sudo chown -R root .
sudo chown -R mysql data
sudo cp support-files/my-medium.cnf /etc/my.cn
```

- Khởi động dịch vụ 
```
sudo bin/mysqld_safe --user=mysql &
sudo cp support-files/mysql.server /etc/init.d/mysql.server
```

- Thiết lập user password
```
sudo bin/mysqladmin -u root password '[Enter your new password]'
```

- Thêm đường dẫn MySQL vào hệ thống
```
sudo ln -s /usr/local/mysql/bin/mysql /usr/local/bin/mysql
```
- Start, stop, status, enabled, disabled ...

```
sudo /etc/init.d/mysql.server start
sudo /etc/init.d/mysql.server stop
sudo /etc/init.d/mysql.server status
sudo update-rc.d -f mysql.server defaults
sudo update-rc.d -f mysql.server remove
```

* Chú ý: Nếu dựng xong mà báo lỗi ntn
```
mysql: error while loading shared libraries: libncurses.so.5: cannot open shared object file: No such file or directory
```

Cài thêm libncurses5 là được 
```
sudo apt-get install libncurses5
```

### Nguồn: https://www.vetechno.in/2021/04/how-to-install-mysql-5-5-on-ubuntu-20-04-LTS.html