### BigImage + ImageView + ViewPager = BigImageViewPager
一个图片浏览器，支持超大图、超长图、支持手势放大、支持查看原图、下载、加载百分比进度显示。采用区块复用加载，优化内存占用，有效避免OOM。

# 截图

# gif查看不流畅，可扫描底部二维码进行安装体验
![v1.2.6新增可下拉/上拉关闭](https://upload-images.jianshu.io/upload_images/1710902-053efdeaebd4f91d.gif?imageMogr2/auto-orient/strip)

![Screenshot_2018-10-19-16-36-40-375.png](https://upload-images.jianshu.io/upload_images/1710902-d94610737445a4e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Screenshot_2018-10-19-16-36-43-337.png](https://upload-images.jianshu.io/upload_images/1710902-9fc595044d2ad332.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Gif](https://upload-images.jianshu.io/upload_images/1710902-3d57165ed0785475.gif)

![v1.2.0增加多种加载策略设置](https://upload-images.jianshu.io/upload_images/1710902-0a66fe87b5b8e20b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![截图1.jpg](https://upload-images.jianshu.io/upload_images/1710902-11827e4c9c08fc86.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![截图2.jpg](https://upload-images.jianshu.io/upload_images/1710902-213bed170b37f027.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



# 功能
- 支持多张图片（网络图片、本地图片均支持）滑动浏览，支持手势放大、双击放大、下拉关闭。
- 支持下载，支持自定义下载目录文件夹名称。
- 查看原图支持加载进度条显示
- 支持超大图、超长图，sample中测试的大图尺寸分别是：2280 * 22116、5760 * 3840。
- 采用区块加载，不用担心OOM的风险。

# 更新日志
- v0.0.5新增：可设置缩放比例、缩放动画时间。
- v1.2.0新增：可设置多种加载策略（仅普清、仅原图、手动模式、网络自适应）
- v1.2.1修复可能与app冲突的部分
- v1.2.2新增：长图（宽高比大于等于3的图）默认宽度放大到手机屏幕的宽度
- v1.2.3优化内存泄漏问题，优化查看原图时更新闪烁问题，简单优化进场退场动画
- v1.2.5新增：是否显示关闭页面按钮，21以上透明化状态栏
- v1.2.6新增：可选择 点击关闭、上拉/下拉关闭

# 用法
#### 添加依赖
Step 1. 在你project层级的build.gradle中，添加仓库地址:
```
allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
}
```
Step 2. 在你主module的build.gradle中添加依赖：

# 此处显示的是本框架的最新版本号：
## glide4.x : v4_1.2.6
## glide3.x : v3_1.2.6

```
dependencies {

  // 针对glide v4 版本：如果您的app中没有使用glide任何版本，或者使用了glide，且glide版本号为4.x，请依赖以下库：

  // 主库，必须添加！
  implementation 'com.github.SherlockGougou:BigImageViewPager:v4_1.2.6'
  // v7支持库，必须添加！
  implementation 'com.android.support:appcompat-v7:27.1.1'
  // 由于本框架使用了glide和okhttp3，所以还请增加依赖以下框架，必须添加！
  // 如果您app中已经依赖某一个的话，可以略过那一个，但要保证以下这些库的版本号一致：
  implementation 'com.github.bumptech.glide:glide:4.7.1'
  implementation 'com.github.bumptech.glide:annotations:4.7.1'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.7.1'
  implementation "com.github.bumptech.glide:okhttp3-integration:4.7.1"

============================分割线==================================

  // 针对glide v3 版本：如果您的app中已经使用了glide，且glide版本号为3.x，仅需要依赖以下库：
  implementation 'com.github.SherlockGougou:BigImageViewPager:v3_1.2.6'
  implementation 'com.android.support:appcompat-v7:27.1.1'
}
```
Step 3. 在您的主module里，添加自定义AppGlideModule，例如：

```
@GlideModule
public class MyAppGlideModule extends AppGlideModule {
  @Override
  public void registerComponents(@NonNull Context context, @NonNull Glide glide,
      @NonNull Registry registry) {
    super.registerComponents(context, glide, registry);

    // 替换底层网络框架为okhttp3，这步很重要！
    // 如果您的app中已经存在了自定义的GlideModule，您只需要把这一行代码，添加到对应的重载方法中即可。
    registry.replace(GlideUrl.class, InputStream.class, new OkHttpUrlLoader.Factory(ProgressManager.getOkHttpClient()));
  }

}
```

Step 4. 以上操作完成后，请点击顶部按钮：Build->Rebuild Project，等待重建完成，至此，框架添加完成。如遇到任何问题，请附带截图提issues，我会及时回复，或添加底部QQ群，进行交流。


#### 调用方式

根据需求生成图片源：
```
ImageInfo imageInfo;
		final List<ImageInfo> imageInfoList = new ArrayList<>();
		for (int i = 0; i < images.length; i++) {
			imageInfo = new ImageInfo();
			imageInfo.setOriginUrl(images[i]);// 原图
			imageInfo.setThumbnailUrl(images[i].concat("-1200"));// 缩略图，实际使用中，根据需求传入缩略图路径。如果没有缩略图url，可以将两项设置为一样，查看原图按钮会自动隐藏。
			imageInfoList.add(imageInfo);
			imageInfo = null;
		}
```

链式调用，多种配置
```
		// 仅加载普清
		findViewById(R.id.buttonThumb).setOnClickListener(new View.OnClickListener() {
			@Override public void onClick(View v) {
				ImagePreview
					.getInstance()
					.setContext(MainActivity.this)
					.setIndex(0)// 默认索引，从0开始
					.setImageInfoList(imageInfoList)// 图片源
					.setShowDownButton(true)// 是否显示下载按钮
					.setLoadStrategy(ImagePreview.LoadStrategy.AlwaysThumb)// 加载策略，具体看下面介绍
					.setFolderName("BigImageViewDownload")// 保存图片到SD卡根目录的文件夹名称
					.setScaleLevel(1, 3, 8)// 最小、中等、最大的缩放比例
					.setZoomTransitionDuration(300)// 缩放的动画时长
					.setShowCloseButton(true)// 是否在查看大图页面显示关闭页面按钮（在左下角)
					.setEnableDragClose(true)// 是否启用上拉/下拉关闭，默认不启用
					.setEnableClickClose(true)// 是否启用点击图片关闭，默认启用
					.start();
			}
		});

		// 仅加载原图
		findViewById(R.id.buttonOrigin).setOnClickListener(new View.OnClickListener() {
			@Override public void onClick(View v) {
				ImagePreview
					.getInstance()
					.setContext(MainActivity.this)
					.setIndex(0)
					.setImageInfoList(imageInfoList)
					.setShowDownButton(true)
					.setLoadStrategy(ImagePreview.LoadStrategy.AlwaysOrigin)
					.setFolderName("BigImageViewDownload")
					.setScaleLevel(1, 3, 8)
					.setZoomTransitionDuration(300)
                    .setShowCloseButton(true)
					.setEnableDragClose(true)// 是否启用上拉/下拉关闭，默认不启用
					.setEnableClickClose(true)// 是否启用点击图片关闭，默认启用
					.start();
			}
		});

		// 手动模式：默认普清，手动高清
		findViewById(R.id.buttonDefault).setOnClickListener(new View.OnClickListener() {
			@Override public void onClick(View v) {
				ImagePreview
					.getInstance()
					.setContext(MainActivity.this)
					.setIndex(0)
					.setImageInfoList(imageInfoList)
					.setShowDownButton(true)
					.setLoadStrategy(ImagePreview.LoadStrategy.Default)
					.setFolderName("BigImageViewDownload")
					.setScaleLevel(1, 3, 8)
					.setZoomTransitionDuration(500)
                    .setShowCloseButton(true)
					.setEnableDragClose(true)// 是否启用上拉/下拉关闭，默认不启用
					.setEnableClickClose(true)// 是否启用点击图片关闭，默认启用
					.start();
			}
		});

		// 网络自适应（WiFi原图，流量普清）
		findViewById(R.id.buttonAuto).setOnClickListener(new View.OnClickListener() {
			@Override public void onClick(View v) {
				ImagePreview
					.getInstance()
					.setContext(MainActivity.this)
					.setIndex(0)
					.setImageInfoList(imageInfoList)
					.setShowDownButton(true)
					.setLoadStrategy(ImagePreview.LoadStrategy.NetworkAuto)
					.setFolderName("BigImageViewDownload")
					.setScaleLevel(1, 3, 5)
					.setZoomTransitionDuration(300)
                    .setShowCloseButton(true)
					.setEnableDragClose(true)// 是否启用上拉/下拉关闭，默认不启用
					.setEnableClickClose(true)// 是否启用点击图片关闭，默认启用
					.start();
			}
		});
```

# 加载策略介绍

```
  public enum LoadStrategy {
    /**
     * 仅加载原图；会强制隐藏查看原图按钮
     */
    AlwaysOrigin,

    /**
     * 仅加载普清；会强制隐藏查看原图按钮
     */
    AlwaysThumb,

    /**
     * 根据网络自适应加载，WiFi原图，流量普清；会强制隐藏查看原图按钮
     */
    NetworkAuto,

    /**
     * 手动模式：默认普清，点击按钮再加载原图；会根据原图、缩略图url是否一样来判断是否显示查看原图按钮
     */
    Default
  }

  以上所有方式，如果原图缓存存在的情况，会默认加载原图缓存保证清晰度，且原图缓存只要存在，就不会显示查看原图按钮
```


# DEMO体验
![扫码下载demo](https://upload-images.jianshu.io/upload_images/1710902-0073c2f34a714fe2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




# GitHub源码
https://github.com/SherlockGougou/BigImageViewPager

# 致谢
- 本框架核心是开源作者 [davemorrissey](https://github.com/davemorrissey) 的 [subsampling-scale-image-view](https://github.com/davemorrissey/subsampling-scale-image-view)，在此感谢他的付出！
对原作感兴趣的，可以去研究学习 ---> [传送门点我](https://github.com/davemorrissey/subsampling-scale-image-view)
- okhttp 进度监听部分代码，借鉴使用了[GlideImageView](https://github.com/sunfusheng/GlideImageView)，在此对其表示感谢，喜欢其作品的可以移步去查看学习


# Bug反馈、增加需求，加 QQ 交流群：271127803（大话安卓）
### 欢迎加入“大话安卓”技术交流群，一起分享，共同进步##

![欢迎加入“大话安卓”技术交流群，互相学习提升](https://upload-images.jianshu.io/upload_images/1710902-5cdeb8c1f58dd425.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

