---
title: vue集成百度Ueditor富文本编辑器
comments: true
date: 2017-07-25 21:54
tags:
---

---

有使用vue来开发项目可能使用一下较为简单的富文本编辑器，功能不是很全面。如果涉及到自媒体运营类的系统Ueditor再适合不过了。网上的富文本编辑器众多，比如：[wangEditor ][1]、[Simditor ][2]、[tinymce][3]等。富文本编辑器设计到的知识面特别广，如果你想自己做简单的nname也可以、利用HTML5中的属性contenteditable，把他放在div上设为true时便可以编辑，但是坑也很多。你可以点击[为什么说富文本编辑器是天坑][4]这里来看看大神们的回答。这里只讲讲在vue项目中如何集成ueditor，如果你有这个需求希望可以帮助到你
<!--more-->
效果图如下：
![demo][5]
   1、 首先使用vue-cli脚手架建立一个vue工程，然后去ueditor官网下载源码。地址：http://ueditor.baidu.com/website/ 。把项目复制到vue项目的static文件下。目的是让服务可以访问到里面的文件，打开UEditor目录文件，我这里下载的是nodejs版本的；
    项目结构如下：
    ![此处输入图片的描述][6]
  2、然后再main.js中引入js文件

    import '../static/ueditor/ueditor.config'
    import '../static/ueditor/ueditor.all'
    import '../static/ueditor/lang/zh-cn/zh-cn'
    import '../static/ueditor/ueditor.parse.min'
 3、创建组件。在components文件里面创建.vue文件，命名随便你。我这里是命名为ueditor.vue。

    <template>
      <el-row>
        <el-col :span="18" :offset="3">
          <div class=gird-content>
            <div id="editor" type="text/plain" style="width:1024px;height:500px;"></div>
            <el-button type="info" @click="getUedtorContent">保存</el-button>
          </div>
        </el-col>
      </el-row>
    </template>
    <script>
      import ElButton from "../../node_modules/element-ui/packages/button/src/button";
      export default{
        components: {ElButton}, name: 'addnew',
        data () {
          return {
            input: '',
            editor: null
          }
        },
        mounted () {
            this.editor =UE.getEditor('editor', {
              UEDITOR_HOME_URL: '/static/ueditor/',//配置资源路径
              toolbars: [
                [
                  'anchor', //锚点
                  'undo', //撤销
                  'redo', //重做
                  'bold', //加粗
                  'indent', //首行缩进
                  'snapscreen', //截图
                  'italic', //斜体
                  'underline', //下划线
                  'strikethrough', //删除线
                  'subscript', //下标
                  'fontborder', //字符边框
                  'superscript', //上标
                  'formatmatch', //格式刷
                  'source', //源代码
                  'blockquote', //引用
                  'pasteplain', //纯文本粘贴模式
                  'selectall', //全选
                  'print', //打印
                  'preview', //预览
                  'horizontal', //分隔线
                  'removeformat', //清除格式
                  'time', //时间
                  'date', //日期
                  'unlink', //取消链接
                  'insertrow', //前插入行
                  'insertcol', //前插入列
                  'mergeright', //右合并单元格
                  'mergedown', //下合并单元格
                  'deleterow', //删除行
                  'deletecol', //删除列
                  'splittorows', //拆分成行
                  'splittocols', //拆分成列
                  'splittocells', //完全拆分单元格
                  'deletecaption', //删除表格标题
                  'inserttitle', //插入标题
                  'mergecells', //合并多个单元格
                  'deletetable', //删除表格
                  'cleardoc', //清空文档
                  'insertparagraphbeforetable', //"表格前插入行"
                  'insertcode', //代码语言
                  'fontfamily', //字体
                  'fontsize', //字号
                  'paragraph', //段落格式
                  'simpleupload', //单图上传
                  'insertimage', //多图上传
                  'edittable', //表格属性
                  'edittd', //单元格属性
                  'link', //超链接
                  'emotion', //表情
                  'spechars', //特殊字符
                  'searchreplace', //查询替换
                  'map', //Baidu地图
                  'gmap', //Google地图
                  'insertvideo', //视频
                  'help', //帮助
                  'justifyleft', //居左对齐
                  'justifyright', //居右对齐
                  'justifycenter', //居中对齐
                  'justifyjustify', //两端对齐
                  'forecolor', //字体颜色
                  'backcolor', //背景色
                  'insertorderedlist', //有序列表
                  'insertunorderedlist', //无序列表
                  'fullscreen', //全屏
                  'directionalityltr', //从左向右输入
                  'directionalityrtl', //从右向左输入
                  'rowspacingtop', //段前距
                  'rowspacingbottom', //段后距
                  'pagebreak', //分页
                  'insertframe', //插入Iframe
                  'imagenone', //默认
                  'imageleft', //左浮动
                  'imageright', //右浮动
                  'attachment', //附件
                  'imagecenter', //居中
                  'wordimage', //图片转存
                  'lineheight', //行间距
                  'edittip ', //编辑提示
                  'customstyle', //自定义标题
                  'autotypeset', //自动排版
                  'webapp', //百度应用
                  'touppercase', //字母大写
                  'tolowercase', //字母小写
                  'background', //背景
                  'template', //模板
                  'scrawl', //涂鸦
                  'music', //音乐
                  'inserttable', //插入表格
                  'drafts', // 从草稿箱加载
                  'charts', // 图表
                ]
              ],
              autoHeightEnabled: true,
              autoFloatEnabled: true,
              minFrameHeight: 500,
            });
        },
        destroyed(){
            this.editor.destroy();
        },
        methods:{
            getUedtorContent: function () {
              console.log(this.editor.getContent());
            }
        }
      }
    </script>

4、在你想要显示第view中引入该组件即可。


----------
关于上传地址配置需要在ueditot.config.js文件里面进行配置。这里也需要后端配置，具体实现可以在uedtor文档上进行查看。
贴上我这里的代码：
ueditot.config.js中：serverUrl: "/ue" // 服务器统一请求接口路径
后端接口：

    app.use("/ue", ueditor(path.join(__dirname,'/public'), function (req, res, next) {
      //客户端上传文件设置
      var imgDir = '/img/ueditor/';
      var ActionType = req.query.action;
      if (ActionType === 'uploadimage' || ActionType === 'uploadfile' || ActionType === 'uploadvideo') {
        var file_url = imgDir;//默认图片上传地址
        /*其他上传格式的地址*/
        if (ActionType === 'uploadfile') {
          file_url = '/file/ueditor/'; //附件
        }
        if (ActionType === 'uploadvideo') {
          file_url = '/video/ueditor/'; //视频
        }
        res.ue_up(file_url); //你只要输入要保存的地址 。保存操作交给ueditor来做
        res.setHeader('Content-Type', 'text/html');
      }
      //  客户端发起图片列表请求
      else if (req.query.action === 'listimage') {
        var dir_url = imgDir;
        res.ue_list(dir_url); // 客户端会列出 dir_url 目录下的所有图片
      }
      // 客户端发起其它请求
      else {
        // console.log('config.json')
        res.setHeader('Content-Type', 'application/json');
        res.redirect('/static/ueditor/nodejs/config.json');
      }
    }));
    需要注意的是资源路径容易搞错。使用相对路径即可
以上都是个人在开发过程中遇到的问题然后根据网友提供的方法再自己整理出来，然后记录下来的。希望对大家有所帮忙。
  [1]: http://wangeditor.github.io/
  [2]: http://simditor.tower.im/
  [3]: https://www.tinymce.com/
  [4]: https://www.zhihu.com/question/38699645
  [5]: http://101.200.187.90/img/logos/14f1add290b0d403661731ee518ad6e4.png
  [6]: http://101.200.187.90/img/logos/a2f5a3efc7e10ab02b31f4905d88726d.png