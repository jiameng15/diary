# Vue项目Webpack打包后字体无效 

### 2020年6月17日

在dev环境中,css使用字体,`font-family:'宋体'` 生效,打包后失效.

#### 原因:

经过webpack打包后,中文汉字被转换为unicode编码格式,并在首位多加了一个斜杠,此时浏览器识别错误.

#### 示例:

`font-family:'宋体' === webpack ===> font-family://u5b8b/u4f53`

#### 解决:

使用字体的英文名字可解决.

#### 延伸:

##### 常用字体英文名:

黑体：SimHei
宋体：SimSun
新宋体：NSimSun
仿宋：FangSong
楷体：KaiTi
仿宋_GB2312：FangSong_GB2312
楷体_GB2312：KaiTi_GB2312
微軟正黑体：Microsoft JhengHei
微软雅黑体：Microsoft YaHei

