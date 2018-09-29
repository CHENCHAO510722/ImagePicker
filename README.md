# 高仿微信图片选择器

#### 注意开发细节，尽可能的做到加载速度最快，目前支持图片单选，多选，多文件夹切换，自定义图片加载器等功能。


### 使用方法：

### 在项目下的build.gradle文件中引入（注意gradle的版本）：
```
compile 'com.lcw.library:imagepicker:1.0.1'//gradle版本在3.0以下引入

implementation 'com.lcw.library:imagepicker:1.0.1'//gradle版本在3.0以上引入
```

### 调用方式非常简单，只需要简单一行代码：
```
  ImagePicker.getInstance()
                        .setTitle("标题")//设置标题
                        .showCamera(true)//设置是否显示拍照按钮
                        .setMaxCount(9)//设置最大选择图片数目(默认为1，单选)
                        .setImageLoader(new GlideLoader())//设置自定义图片加载器
                        .start(mContext, REQUEST_SELECT_IMAGES_CODE);//REQEST_SELECT_IMAGES_CODE为Intent调用的requestCode
                        
```
### 获取选择图片返回的数据：
```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == REQUEST_SELECT_IMAGES_CODE && resultCode == RESULT_OK) {
            List<String> imagePaths = data.getStringArrayListExtra(ImagePicker.EXTRA_SELECT_IMAGES);
        }
    }
```

### 关于自定义图片加载器，不具体指定图片加载框架，让开发者更加灵活的定制，只需要去实现ImageLoader接口即可：
```
public class GlideLoader implements ImageLoader {

    @Override
    public void loadImage(ImageView imageView, String imagePah) {
        //小图加载，这里以Glide图片加载框架为例
        RequestOptions options = new RequestOptions()
                .centerCrop()
                .placeholder(R.mipmap.icon_image_default)
                .error(R.mipmap.icon_image_error);
        Glide.with(imageView.getContext()).load(imagePah).apply(options).into(imageView);
    }

}
```

### 最后别忘了权限，6.0以后需要动态申请，本Library不提供，需要自行处理
```
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.CAMERA" />
```

###  其他
根据业务的需求，有时候我们在选择一部分图片后，再次跳转图片选择器的时候，需要去保存已经勾选的图片，这边也提供了对应的方法，只需要把onActivityResult返回的图片路径List集合，重新设置进来即可，代码如下：
```
    ImagePicker.getInstance()
                        .setTitle("标题")
                        .showCamera(true)
                        .setMaxCount(9)
                        .setImagePaths(mImagePaths)
                        .setImageLoader(new GlideLoader())
                        .start(MainActivity.this, REQUEST_SELECT_IMAGES_CODE);
 ```

版本会持续迭代完善，未完待续。。。（欢迎Star，欢迎Fork）

 

