
## 关于开启 SSH 远程登录的方法

!!! quote ""

    - ### 验证是否已安装 `SSH` 服务

        ``` bash
        ls /etc | grep ssh
        ```
        > 如果没有这个文件夹说明系统未安装 `SSH` 服务，你需要通过包管理工具安装 `openssh` 软件包

    - ### 设置允许 Root 用户登录

        ``` bash
        cat /etc/ssh/sshd_config | grep -Eq "^[# ]?PermitRootLogin " ; [ $? -eq 0 ] && sed -i 's/^[# ]\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config || echo -e "\nPermitRootLogin yes" >> /etc/ssh/sshd_config
        ```

    - ### 设置密码认证

        ``` bash
        cat /etc/ssh/sshd_config | grep -Eq "^[# ]?PasswordAuthentication " ; [ $? -eq 0 ] && sed -i 's/^[# ]\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config || echo -e "\nPasswordAuthentication yes" >> /etc/ssh/sshd_config
        ```

    - ### 启动/重启 `SSH` 服务

        ``` bash
        ps -ef | grep -q ssh ; [ $? -eq 0 ] && systemctl restart sshd || systemctl enable --now sshd
        ```


## 关于报错 Command not found

!!! quote ""

    - 如果提示 `Command 'curl' not found` 则说明当前未安装 `curl` 软件包

        === "Debian 系 Linux"

            ``` sh
            apt-get install -y curl
            ```

            > `Debian` &nbsp; `Ubuntu` &nbsp; `Kali` &nbsp; `Deepin`

        === "RedHat 系 Linux / OpenCloudOS / openEuler"

            ``` sh
            yum install -y curl || dnf install -y curl
            ```

            > `Red Hat Enterprise Linux` &nbsp; `CentOS` &nbsp; `Rocky Linux` &nbsp; `AlmaLinux` &nbsp; `Fedora` &nbsp; `OpenCloudOS` &nbsp; `openEuler`

        === "openSUSE"

            ``` sh
            zypper install curl
            ```

        === "Arch Linux"

            ``` sh
            pacman -S curl
            ```

## 还原已备份的软件源

!!! quote ""

    === "Debian 系 Linux"

        ``` sh
        cp -rvf /etc/apt/sources.list.bak /etc/apt/sources.list
        apt-get update
        ```

        > `Debian` &nbsp; `Ubuntu` &nbsp; `Kali` &nbsp; `Deepin`

    === "RedHat 系 Linux / OpenCloudOS / openEuler"

        ``` sh
        cp -rvf /etc/yum.repos.d.bak /etc/yum.repos.d
        yum makecache
        ```

        > `Red Hat Enterprise Linux` &nbsp; `CentOS` &nbsp; `Rocky Linux` &nbsp; `AlmaLinux` &nbsp; `Fedora` &nbsp; `OpenCloudOS` &nbsp; `openEuler`

    === "openSUSE"

        ``` sh
        cp -rvf /etc/zypp/repos.d.bak /etc/zypp/repos.d
        zypper ref
        ```

    === "Arch Linux"

        ``` sh
        cp -rvf /etc/pacman.d/mirrorlist.bak /etc/pacman.d/mirrorlist
        pacman -Sy
        ```

## 其它

!!! quote ""

    - 如果提示 `bash: /proc/self/fd/11: No such file or directory`，请切换至 `Root` 用户执行，切换命令为 `sudo -i` 或 `su root`
