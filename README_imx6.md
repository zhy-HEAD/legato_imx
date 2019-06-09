1. 环境变量设置
export RASPI_TOOLCHAIN_DIR=/home/XXXX/SDK/usr/bin/
export RASPI_TOOLCHAIN_PREFIX=arm-linux-gnueabihf-
export LEGATO_SYSROOT=/home/XXXX/SDK/usr/arm-buildroot-linux-gnueabihf/sysroot

说明:1.SDK对应自己板子的交叉编译工具链.
     2.我的LEGATO_SYSROOT这个的设置为sysroot目录,有的是该目录的下一级子目录对应arm-linux-gnueabihf-所在的子目录,否则会报错


2. 编译配置legato
     1.配置对应平台，我用的是raspi,模板配成自己的
     2.进入legato目录make menuconfig ARCH=arm
     3.Watchdog所有的关闭
     4.vim ./.config.raspi 修改该文件,搜索/DOG注销
     5.DISABLE_SMACK=1 make raspi  

说明:不这么配置会导致板子reboot

3. 编译板级内核
     1.创建:legatocfg()
     CONFIG_CGROUPS=y
     CONFIG_CGROUP_DEBUG=y
     CONFIG_CGROUP_FREEZER=y
     CONFIG_CGROUP_CPUACCT=y
     CONFIG_CGROUP_DEVICE=y
     CONFIG_MEMCG=y
    # CONFIG_MEMCG_SWAP is not set
    # CONFIG_MEMCG_KMEM is not set
     CONFIG_CGROUP_SCHED=y
     CONFIG_FAIR_GROUP_SCHED=y
     CONFIG_RT_GROUP_SCHED=y
     CONFIG_BLK_CGROUP=y
     CONFIG_SECURITY=y
     CONFIG_SECURITY_SMACK=y
     CONFIG_DEFAULT_SECURITY_SMACK=y
    # CONFIG_DEFAULT_SECURITY_DAC is not set
     CONFIG_DEFAULT_SECURITY="smack"
     CONFIG_HOTPLUG_CPU=y
     CONFIG_ARM_CPU_SUSPEND=y
     CONFIG_SUSPEND=y
     CONFIG_PM_SLEEP=y
     CONFIG_PM_WAKELOCKS=y
     CONFIG_PM_WAKELOCKS_LIMIT=100
     CONFIG_PM_WAKELOCKS_GC=y
     CONFIG_PM_DEBUG=y
     CONFIG_PM_ADVANCED_DEBUG=y
    # CONFIG_PM_TEST_SUSPEND is not set
     CONFIG_PM_SLEEP_DEBUG=y
     CONFIG_SQUASHFS=y
     CONFIG_SQUASHFS_DECOMP_SINGLE=y
     CONFIG_SQUASHFS_EMBEDDED=y
     CONFIG_SQUASHFS_FILE_DIRECT=y
    # CONFIG_SQUASHFS_FILE_CACHE is not set
    
    2. ARCH=arm make XXXX_defconfig && ARCH=arm scripts/kconfig/merge_config.sh -O . .config legatocfg
    3. 查看结果如果没有配置的手动make menuconfig补上配置
    4. 编译内核


4. 文件系统
    说明: 如果是自己做的文件系统需要在根目录下创建/legato和/mnt/legato文件夹

5. 安装legato
    1. 安装好内核和文件系统后，将legato.squashfs传到板子上
    2. mount -t squashfs legato.squashfs /mnt/legato/ 挂载
    3. cd /mnt/legato/ && ./start
    4. 修改SSH允许root登录
    5. 完成

    


注: 该方法有些弊端 比如看门狗，还有，我的是最小系统，所以log会提示'logread: can't find syslogd buffer: No such file or directory'导致无法查看输出信息
    补救措施: cat /var/log/messages | grep "xxx"
    

     
