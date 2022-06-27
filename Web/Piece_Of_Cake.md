# Piece Of Cake

Source code
```PHP
<?php 
include "flag.php"; 
highlight_file(__FILE__); 
error_reporting(0); 
$str1 = $_GET['0']; 

if(isset($_GET['0'])){ 
    if($str1 == md5($str1)){ 
        echo $onion1; 
    } 
    else{ 
        die(); 
    } 
} 
else{ 
    die();    
} 

$str2 = $_GET['1']; 
$str3 = $_GET['2']; 

if(isset($_GET['1']) && isset($_GET['2'])){ 
    if($str2 !== $str3){ 
        if(hash('md5', $salt . $str2) == hash('md5', $salt . $str3)){ 
            echo $onion2; 
        } 
        else{ 
            die(); 
        } 
    } 
    else{ 
        die(); 
    } 
} 
else{ 
    die();    
} 
?> 
```

Như bài PHP is Easy, chúng ta cần truyền vào các tham số để bypass.  
Request có 3 parameters lần lượt là 0, 1 và 2.  
Nửa đầu của flag là `$onion1`, sẽ được in ra khi giá trị tham số `0` truyền vào có giá trị bằng ( == ) với hash md5 của chính nó.  
Bằng cách tận dụng Loose comparasion của PHP và sử dụng magic hash md5 PHP (https://github.com/spaze/hashes/blob/master/md5.md), cụ thể hơn là truyền cho tham số `0` giá trị `0eRnJWi9XnKd9Z7x`, trang sẽ in ra nửa đầu của flag.

Để có được nửa sau của flag,`$onion2`, chúng ta cần truyền vào tham số `1` và `2` sao cho `(hash('md5', $salt . $str2) == hash('md5', $salt . $str3)`.
Tuy nhiên, `$salt` không được khai báo ở bất cứ đâu trong source, nên mình nghĩ nó không tồn tại,
và thực tế chúng ta đang thực hiện `(hash('md5', $str2) == hash('md5', $str3)`.

Tương tự như trên, chúng ta sử dụng ngẫu nhiên 2 string trong magic hash, hoặc là truyền vào 2 array, chúng ta đã có được nửa sau của flag  

> http://103.245.249.76:49155/?0=0e215962017&1[]=asd&2[]=asd

#### Flag: FPTUHACKING{StR1Ngs_c0nCaTeNaT1oN_1s_EaSy_!!!}
