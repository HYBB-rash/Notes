# 使用idea，GitHub时，push和clone出现的一些问题

## 报错：No anonymous write access

这个的原因是在idea记住的用户名和GitHub登录的不一样，导致报错。笔者的GitHub开了很久没用过，现在在学重新用，想给自己的GitHub换一个用户名，结果我先配置了idea再去换，后来再想push的时候就报这个错。

### 解决办法：

1. ![image-20200317182939173](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317182939173.png)

2. ![image-20200317183131828](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317183131828.png)

3. ![image-20200317183157341](F:\管理平板\run\我的坚果云\新概念3册讲义\md\image-20200317183157341.png)
4. ![image-20200317183224339](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317183224339.png)
5. ![image-20200317183300215](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317183300215.png)
6. 成功以后，再重新按上面差不多的步骤登录
7. 由于笔者没能还原场景，接下来没有图了。
8. 然后点pull或者push，会提示你要你重新登录的，登完就解决了

## 将自己的web项目push上去之后，再pull下来项目会卡loading

这个是因为没把`.idea` 文件夹里的artifacts文件夹push上去导致的。

所以直接把`.idea`文件夹传好再重新克隆就好了。

另外记得编译的文件不用上传。就是out文件夹

## 克隆的操作

1. ![image-20200317184031174](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317184031174.png)

2. ![image-20200317184108775](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317184108775.png)

   用第一个还是第二个都可以的，这里贪方便选第二个

3. ![image-20200317184310578](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317184310578.png)

   选择一个空夹子

4. ![image-20200317185045607](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317185045607.png)
5. ![image-20200317185135880](C:\Users\12076\AppData\Roaming\Typora\typora-user-images\image-20200317185135880.png)
6. 接下来全部默认next