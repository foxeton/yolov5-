# yolov5+tesseract ocr 踩坑笔记


yolov5
tesseract ocr
win10
cuda11.6 cudnn8.4.0.27


#### 安装教程
1.  
    安装anaconda先不改镜像源
    科学上网clash：
    更改用户文件夹下.condarc文件内容为以下内容：
    ssl_verify: true
    show_channel_urls: true
    report_errors: false
    proxy_servers: {http: 127.0.0.1:7890, https: 127.0.0.1:7890}

2.  
    创建环境 python=3.8
    激活环境后
    conda install pytorch torchvision torchaudio pytorch-cuda=11.6 -c pytorch -c nvidia
    期间两个包安装失败不叼他
    结束后更换清华源，再次单独安装失败的包，并适当增加时间，以防失败
    可以用conda install 或者 pip install 哪个成功哪个就牛逼!
    大功告成
3.  
    tesseract ocr 安装随便找几个教程
    1、使用jtessbox合并成多个图片为name.tif文件(name自取名，下不赘述)
    2、生成box文件	命令：tesseract name.tif name -l other_name(需要合并的)+chi_sim(默认eng，训练中文就得它) batch.nochop makebox
    3、jtessbox中修改识别错误的地方，不会就搜
    4、生成tr文件 	命令：tesseract name.tif name nobatch box.train
    5、生成字符集	命令：unicharset_extractor t1.box
    6、生成字体特征文件 里面写入：
    <fontname> <italic> <bold> <fixed> <serif> <fraktur>(参考，不写入)
    如： fontname 0 0 0 0 0        最终文件不可有后缀名fontname不是fontname.txt
    7、mftraining -F fontname -U unicharset name.tr
    生成的所有文件除了fontname，其余文件都需改名为name.***	       ***=原始名称
    8、聚集tesseract识别的训练文件
    cntraning name.tr
    (提前将unicharset,inttemp,normproto,pfftable,shapetable)加上name前缀名上一条。
    9、合并，生成字库文件
    combine_tessdata name.
    10、测试
    tessract path.bmp 13 -l name
    所有步骤应该可以写生一个文件方便调用，后续再更新，睡觉不熬了！
