# sweetalert-demo
bootstrap &amp; sweetalert demo

---

## 项目地址

 - 官网地址：http://t4t5.github.io/sweetalert/
 - 中文网址：http://sweetalert.getmaterialize.com/
 
## 基本用法
文档里面已经讲得十分详细，不过刚使用时没仔细看不知道**怎样响应取消的点击事件**
看下面这个例子，第9行通过```isConfirm```来判断是否保存还是取消，只需判断一下就可以了
```js
swal({
    title: "",
    text: "卡片描述尚未保存，请选择保存或者取消",
    type: "warning",
    showCancelButton: true,
    confirmButtonText: "保存",
    confirmButtonColor: "#5cb85c",
    cancelButtonText: "取消",
    }, function (isConfirm) {
        if (isConfirm) {
            alert("save");
        } else {
            alert("cancel");
        }
})      
```
![image_1b085s3c01evbj12dif17410799.png-10.4kB](http://static.zybuluo.com/Jerry-MEI/onbgbr0at68mz2zu417cpdms/image_1b085s3c01evbj12dif17410799.png)

## 样式修改
看上面这张图，保存是在左边的。原项目和这个相反，通过css可以简单的改变位置
```css
//给保存按钮加个左浮动
.sweet-alert .sa-confirm-button-container{
    display: inline-block;
    position: relative;
    float: left;
}

//取消按钮右浮动
.sweet-alert button.cancel {
    background-color: #C1C1C1;
    float: right;
}

//按钮的上一层设置一下宽度
.sa-button-container {
    width: 50%;
    margin: 0 auto;
    overflow: hidden;
}
```

## Bootstrap模态框结合使用
>在使用Bootstrap模态框做表单提交时，为防止用户不小心关掉模态框导致内容丢失，我们可以在触发模态框关闭事件时判断用户是否输入新的内容。如果输入了新的内容，则弹出sweetalert对话框，提示用户保存信息。

![image_1b11pfpkb1rjv18be1uae1piq8dg9.png-24kB](http://static.zybuluo.com/Jerry-MEI/uvrr0nqfdu0fx4invp75d8av/image_1b11pfpkb1rjv18be1uae1piq8dg9.png)

```js
var input_val_before="";
$(function(){
    $('#myModal').on('hide.bs.modal', function (e) {
        var input_val=$("#myInput").val().trim();  
        if(input_val_before!=input_val){
            e.preventDefault();
            swal({
                title: "",
                text: "编辑框内容尚未保存，请选择保存或者取消",
                type: "warning",
                allowEscapeKey:false,
                showCancelButton: true,
                confirmButtonText: "保存",
                confirmButtonColor: "#5cb85c",
                cancelButtonText: "取消"
            }, function (isConfirm) {
                if (isConfirm) {
                    input_val_before=input_val;
                    //执行保存操作...
                } else {
                    $("#myInput").val(input_val_before);
                    //执行取消操作...
                }
                $("#myModal").modal("hide");
            });
        }
    });
});
```

先定义一个全局变量`input_val_before`用来保存输入框内容，在模态框触发关闭事件的时候判断一下此时输入框的内容是否发生改变。如果没有改变，通过`e.preventDefault();`来阻止模态框关闭，然后调用sweetalert对话框

当用户点击确定按钮时，将`input_val_before`赋值为当前输入框的值；当用户点击取消按钮时，将输入框的值设置为修改前的值。为什么要这样做呢，因为最后一句`$("#myModal").modal("hide");`关闭模态框会再次触发`hide.bs.modal`事件，这样`input_val_before`和`input_val`就是相等的，不会再执行下面的操作
