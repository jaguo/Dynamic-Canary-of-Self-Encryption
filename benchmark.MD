# Benchmark
&nbsp;&nbsp;&nbsp;&nbsp;How to know performacne of you system.

## &nbsp;&nbsp;1.spec2006
&nbsp;&nbsp;&nbsp;&nbsp;   
### install :https://www.spec.org/cpu2006/Docs/install-guide-unix.html
  ```shell
  mount -t iso9660 -o ro,exec /dev/cdrom /mnt
  cd /mnt
  ./install.sh
  ```  
### configspec 
&nbsp;&nbsp;&nbsp;&nbsp;  In this config:
submit = $BIND $command
EXTRA_CFLAGS/EXTRA_CXXFLAGS


submit = LD_PRELOAD=/pathtolib/libdese.so $command
use_submit_for_speed=1

and for the flags:

EXTRA_CFLAGS       = -ffixed-r10 -fstack-protector -fplugin=/pathtodcse/dcse.so


### runspce :https://www.spec.org/cpu2006/Docs/runspec.html#section1.4.2
  ```shell
  cd /usr/ryan/cpu2006
  bash
  . ./shrc
  runspec --config my.cfg --nobuild --reportable int
  runspec --config my.cfg --nobuild --reportable fp
  runspec --config my.cfg --nobuild --reportable all
  ```
## &nbsp;&nbsp;2.apache2 benchmark
&nbsp;&nbsp;&nbsp;&nbsp;   
  ```shell
  apt-get install apache2
  ab -c 500 -n 100000 http://127.0.0.1/
  
  #start and stop apache
  /usr/sbin/apache2ctl start
  /usr/sbin/apache2ctl stop
  ```   
&nbsp;&nbsp;&nbsp;&nbsp; modification of apache2ctl
  ```shell
line139
start)
      # ssl_scache shouldn't be here if we're just starting up.
      # (this is bad if there are several apache2 instances running)
      rm -f ${APACHE_RUN_DIR:-/var/run/apache2}/*ssl_scache*
--    $HTTPD ${APACHE_ARGUMENTS} -k $ARGV
++    LD_LIBRARY_PATH=/usr/lib64/glibc-2.19/lib LD_PRELOAD=/home/wangzhilong/Desktop/dynamic_encrypt_redundant_stackguard/libredundantguard.so $HTTPD ${APACHE_ARGUMENTS} -k $ARGV
      ERROR=$?
      ;;
  stop|graceful-stop)
--    $HTTPD ${APACHE_ARGUMENTS} -k $ARGV
++    LD_LIBRARY_PATH=/usr/lib64/glibc-2.19/lib LD_PRELOAD=/home/wangzhilong/Desktop/dynamic_encrypt_redundant_stackguard/libredundantguard.so $HTTPD ${APACHE_ARGUMENTS} -k $ARGV
      ERROR=$?
      ;;
  ```  
  
  
## &nbsp;&nbsp;2.nginx benchmark
&nbsp;&nbsp;&nbsp;&nbsp;   
  ```shell
  apt-get install nginx
  ab -c 500 -n 100000 http://127.0.0.1/
  
  #start and stop nginx
  LD_LIBRARY_PATH=/usr/lib64/glibc-2.19/lib LD_PRELOAD=/home/wangzhilong/Desktop/dynamic_encrypt_redundant_stackguard/libredundantguard.so /usr/sbin/nginx -c /etc/nginx/nginx.conf
  LD_LIBRARY_PATH=/usr/lib64/glibc-2.19/lib LD_PRELOAD=/home/wangzhilong/Desktop/dynamic_encrypt_redundant_stackguard/libredundantguard.so /usr/sbin/nginx -s stop
  ```


## other benchmarks
http://www.gnutoolbox.com/linux-benchmark-tools/
