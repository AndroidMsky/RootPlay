# RootPlay
安卓手机秒变网络摄像头,自动接起QQ视频。

今天大概是兴趣加技术篇，程序员不写点有趣的代码，怕是很难在女票和家人面前秀出科技感。
GITHUB：
https://github.com/AndroidMsky/RootPlay

![这里写图片描述](http://img.blog.csdn.net/20161107142642108)

如GIF所示，自动接起QQ电话。

QQ视频来电自动接起来，微信视频电自动接起来。


首先你需要两个硬件设备
1.一步Root了的，并且安装手机QQ的安卓手机。
2.如果像文档一点你可能需要一个手机支架。

两步逻辑很简单：
1.通过BroadcastReceiver获取亮屏幕的广播。
2.通过shell input 命令去滑动接起视频电话。 

1.写一个BroadcastReceiver监控的广播是Intent.ACTION_SCREEN_ON也就是屏幕被点亮后并执行我们设定好的shell命令：

```
BroadcastReceiver mBatInfoReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(final Context context, final Intent intent) {
                Log.d(TAG, "onReceive");
                String action = intent.getAction();

                if (Intent.ACTION_SCREEN_ON.equals(action)) {
                    Log.d(TAG, "screen on");
                    try {
                        if (KAI)
                            Tools.doCmds("input swipe 170 1200 600 1200");
                    } catch (Exception e) {
                        e.printStackTrace();
                    }


                } 
            }
        };

        registerReceiver(mBatInfoReceiver, filter);
    }
```
2.就是让手机去执行shell脚本直接调工具类就好啦。
Tools.doCmds("input swipe 170 1200 600 1200");

```
public static void doCmds(String cmds) throws Exception {
        Process process = Runtime.getRuntime().exec("su");
        DataOutputStream os = new DataOutputStream(process.getOutputStream());
        os.writeBytes(cmds + "\n");
        os.writeBytes("exit\n");
        os.flush();
        os.close();

        process.waitFor();
    }

```

解释一下：
input swipe 170 1200 600 1200意思就是从（170，1200）滑动到（600，1200），大概就是来电话那个滑动按钮，手机分辨率不通大家可以根据不通的分辨率去获取一下这个坐标值。如果你想更友好的话也可以去让用户去手动设置这两个坐标值，因为shell input命令是个字符串，根据用户的输入去拼接一下就好了。
分享一些常用的input命令：

```
／／休眠3秒
adb shell sleep 3
／／按下home键还有很多物理按键都是这么调用
adb shell input keyevent 3
／／从550 1000滑动到550 1100
adb shell input swipe 550 1000 550 1100
／／点击事件
adb shell input tap 118 1800
／／输入字符串 这个貌似不支持中文，一般会唤起手机输入法肯定会改变
／／其它节目元素的位置，所以使用时候已定要小心哦。
adb shell input text zaiganmane
```
这是我写的一小段QQ聊天命令。没时间陪GF聊天的可以好好发掘发掘。
```
adb shell input text zainma
adb shell input tap 118 1800
adb shell input tap 967 1600
adb shell sleep 30
adb shell input text haode
adb shell input tap 118 1800
adb shell input tap 967 1600
adb shell sleep 10
adb shell input text wufanchilama
adb shell input tap 118 1800
adb shell input tap 967 1600
adb shell sleep 10
adb shell input text nabucuoo
adb shell input tap 118 1800
adb shell input tap 967 1600
adb shell sleep 10
adb shell input text heihei
adb shell input tap 118 1800
adb shell input tap 967 1600

```

不要忘了加个是否自动接听的开关一个布尔值控制一下就好了：

```
    public void on1(View v) {


        KAI = true;
        mTextView.setText("is on");


    }

    public void on2(View v) {

        KAI = false;
        mTextView.setText("is off");

    }
```

然后是一定让QQ和我们都应用都在后台白名单里，避免被杀死。笔者用自己小米MAX和MX2，红米note3。亲测24全体小时有效
![这里写图片描述](http://img.blog.csdn.net/20161107145135053)

由于没有判断是谁来视频电话建议用QQ小号，只有自己为好友，免得谁来电都会接起来。
另外使用Accessibility可能可以优化该一些问题，这里不做详解。
不过笔者认为，打造一个网络摄像头，秀一下科技。这篇的技术就够啦。
看看家中的阿猫阿狗，检查检查你加班的时候女票在干嘛。




家中一爱犬小葡萄，上个月走啦，T T

笔者和家人心里甚是难过，也借此文悼念一下我的小葡萄一路走好。



欢迎关注作者。欢迎评论讨论。欢迎拍砖。

如果觉得这篇文章对你有帮助 欢迎打赏，

欢迎star，Fork我的github。
https://github.com/AndroidMsky
喜欢作者的也可以Follow。也算对作者的一种支持。 

本文Github代码链接 
https://github.com/AndroidMsky/RootPlay



![这里写图片描述](http://img.blog.csdn.net/20161028111556438)

博主原创未经允许不许转载。
