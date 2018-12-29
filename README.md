#### jquery  大文件上传类

##### 功能说明
- 支持多文件上传
- 支持切片上传
- 支持各种回调


##### 使用方法

```javascript
$("#thumb-file").sliceupload({
        slice:false,
        appendServer:{
            _token:document.getElementsByTagName('meta')['csrf-token'].getAttribute('content'),
        },
        checkTypeFunction:function(d){
            if(d.type.substr(0,5)!="image"){
                layer.alert(d.name+"不是图片文件");
                return false;
            }
        },
        callback:function (d) {
            layer.msg("文件上传完成");
            console.info(d);
            $("#thumb").val(d.info.fileName);
            $("#thumb_show").prop("src",d.info.thumbUrl);
        }
    })
```

```javascript
$("#pano-file").sliceupload({
        sliceSize:2*1024*1024,
        appendServer:{
            _token:document.getElementsByTagName('meta')['csrf-token'].getAttribute('content'),
            dealPano:1, //处理全景图
        },
        checkTypeFunction:function(d){
            if(d.type.substr(0,5)!="image"){
                layer.alert(d.name+"不是图片文件");
                return false;
            }
        },
        successFunction:function(d){
            layer.msg("文件上传中 "+d.percent+"%");
        },
        checkFunction:function(percent,index,total){
            console.info(percent)
            layer.msg("文件检测中 "+percent+"%");
        },
        callback:function (d) {
            layer.msg("文件上传完成");
            console.info(d);
            $("#pano_image").val(d.info.fileName);
            $("#pano_image_show").prop("src",d.info.thumbUrl);
        }
    })
```

##### 参数说明
```javascript
{
        url: "/file/upload",
        //是否支持多文件
        //multiple:false, //根据文件数量来
        //是否支持切片
        slice: true,
        //切片大小 bytes
        sliceSize: 500 * 1024,
        /**
         * 百分比小数
         */
        percentDecimal: 2,
        /**
         * 附加参数
         */
        appendServer: {aboc: 1},
        /**
         * 上传过程中的函数
         * @param d
         */
        processFunction: function (d) {
            // console.info(d)
        },
        /**
         * 上传完成后的函数
         * @param d
         */
        successFunction: function (d) {
            console.info(d)
        },
        /**
         * 所有的完成后的回调函授
         * @param d
         */
        callback: function (d) {
            // console.info(d);
        },
        /**
         * 检测函授
         * @param d
         */
        checkFunction:function (d) {
            console.info(d)
        },
        /**
         * 检测函授
         * @returns {boolean}
         */
        checkTypeFunction: function (d) {
            // console.info(d.type)
            return true;
        }
    }
```

##### 服务端返回数据
```json
{
  "status": "success",
  "message": "",
  "sliceIndex": "0",
  "fileIndex": "0",
  "info": {
    "md5": "8693410b2ea49ade4bf45fe092deb3f5",
    "fileName": "upload/201812/29/11/5c26e9607e10a_3.jpg",
    "isImage": true,
    "fileUrl": "http://xxx.cn/upload/201812/29/11/5c26e9607e10a_3.jpg",
    "thumbName": "upload/201812/29/11/s_5c26e9607e10a_3.jpg",
    "thumbUrl": "http://xxx.cn/upload/201812/29/11/s_5c26e9607e10a_3.jpg"
  }
}
```

##### 其他说明
- 如果需要支持多文件，请在file表单中增加 `multiple="true"`
- file表单请加上id
