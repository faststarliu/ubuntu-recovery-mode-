# ubuntu-recovery-mode
                                   ubuntu的recovery mode解决用户一些实际问题
遇到的问题如下： 
1、	在当前用户下使用sudo来直接修改password等几个文件，一旦修改了passwd，用户名发生了变化，其他的用户组、密码等却没有对应的配置，就再进不了该用户了。
2、	忘记用户密码，不能进入ubuntu了。
3、	Ubuntu下普通用户用sudo执行命令时报"xxx is not in the sudoers file.This incident will be reported"错误。
如果你遇到上述问题或者在用户模式遇到类似问题，我们如何做呢？

进入ubuntu的recovery mode获取ubuntu的root权限来解决这些问题。
步骤如下：
1.	重启电脑
2.	开机时，按esc键，进入一个Grub引导页面,选择 "Ubuntu 高级选项"之后,按 回车(Enter) 键进行确认选择

 
3.	选择带有"Recover mode"的菜单,回车
 
4.	你将看到recover Menu的选项页面，然后我们选择"root drop to a root shell prompt"，回车
 
5.	在root权限下输入命令
 

6.	比如问题如Ubuntu下普通用户用sudo执行命令时报"xxx is not in the sudoers file.This incident will be reported"错误，解决方法就是在/etc/sudoers文件里给该用户添加权限，此时如果我们直接在输入命令：
chmod u+w /etc/sudoers
则会报错如下：

 


此时我们在窗口中输入命令：	
mount  -o  remount,rw / 
(这里该是重新挂载/etc分区，我的/etc是在根目录下(ubuntu 用 / 表示，所以是对/目录重新挂载为读/写)，再输入命令：
chmod u+w /etc/sudoers
 

7.	编辑sudoers文件
vi m /etc/sudoers
找到这行 root ALL=(ALL) ALL,在他下面添加xxx ALL=(ALL) ALL (这里的xxx是你的用户名)
ps:这里说下你可以sudoers添加下面四行中任意一条
abc            ALL=(ALL)                ALL
%abc          ALL=(ALL)                ALL
testr           ALL=(ALL)                NOPASSWD: ALL
%test          ALL=(ALL)                NOPASSWD: ALL
第一行:允许用户abc执行sudo命令(需要输入密码).
第二行:允许用户组abc里面的用户执行sudo命令(需要输入密码).
第三行:允许用户test执行sudo命令,并且在执行的时候不输入密码.
第四行:允许用户组test里面的用户执行sudo命令,并且在执行的时候不输入密码.
撤销sudoers文件写权限,命令:
`	chmod u-w /etc/sudoers


 


8.	根目录重新挂载为只读:
mount -o remount, ro /
9.	重启计算机:
	reboot
