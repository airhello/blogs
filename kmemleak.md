# kmemleak

## 功能开启
Linux Kernel配置将以下功能宏打开 
> CONFIG_DEBUG_KMEMLEAK=y  
> CONFIG_DEBUG_KMEMLEAK_EARLY_LOG_SIZE=20000  

## 功能使用
- 挂载debug目录
  > mount -t debugfs nodev /sys/kernel/debug/
- 查看内存泄漏位置
  > cat /sys/kernel/debug/kmemleak
- 手动开启扫描
  > echo scan > /sys/kernel/debug/kmemleak

## FAQ
### mount后在/sys/kernel/debug目录下未看到kmemleak
确认CONFIG_DEBUG_KMEMLEAK_EARLY_LOG_SIZE值是否够大, 若不够大，则会自动disable掉  
确认方法：dmesg看是否有以下内容  
> kmemleak: Early log buffer exceeded (506), please increase DEBUG_KMEMLEAK_EARLY_LOG_SIZE

