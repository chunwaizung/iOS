#iOS开发实习笔记

##AVFoundation
使用AVFoundation定制简单的头像录入界面

###createThumbnail
创建缩略图的方法-->图形上下文

##MBProgressHUD
使用MBProgressHUD显示短暂的提示性语句

##数组随机重排的方法

##关于UICollectionView的一些小事


- UICollectionView不一定要依赖于UICollectionViewController使用，可以嵌入任何一个VC，只要实现了`<UICollectionViewDataSource,UICollectionViewDelegate>`.

- 关于重用cell：重用时不会清除附在cell中的其它UI控件
- 使用代码的方式添加双击手势，而不是使用IB添加到cell上。双击手势直接添加到collectionview
 