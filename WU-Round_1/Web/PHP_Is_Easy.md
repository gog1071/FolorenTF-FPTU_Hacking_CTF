# PHP Is Easy !!!

### Source code 
```PHP
<?php
require "flag.php";
error_reporting(0);
 $md51 = md5( '0gdVIdSQL8Cm' ); 
 $a = @$_GET[ '0ni0n' ]; 
 $md52 = @md5($a); 
 if ( isset ($a)){ 
    if ($a != '0gdVIdSQL8Cm' && $md51 == $md52) { 
        echo "<script>alert('$flag')</script>"; 
    } else { 
        echo "0ni0n{F3k4_Fl4G}"; 
    } 
} 
show_source(__FILE__);
?>
```
Chúng ta thấy sẽ có parameter 0ni0n trong request, và để lấy được flag, chúng ta cần phải gửi request sao cho para 0ni0n có giá trị khác '0gdVIdSQL8Cm' nhưng có giá trị $md51 == $md52
```PHP
 if ($a != '0gdVIdSQL8Cm' && $md51 == $md52) { 
        echo "<script>alert('$flag')</script>"; 
    }
```
Ta có md5( '0gdVIdSQL8Cm' ) = 0e366928091944678781059722345471.  
Ở đây chúng ta lợi dụng Loose comparasion của PHP ( 0eN == 0eM trả về True với N và M là các số khác nhau) để bypass.
Sử dụng một string trong magic hash https://github.com/spaze/hashes/blob/master/md5.md để truyền vào para 0ni0n, chúng ta đã lấy được flag.

> http://103.245.249.76:49154/?0ni0n=240610708

#### Flag: FPTUHACKING{Md5_bY_pAAs_eZ_H4_H4}
