# 避免阻塞I/O的操作

阻塞I/O的操作将使App能够进行下一步操作前会强制使其等待结果的返回。在UI线程上执行一个阻塞操作将强制使UI卡住，这将直接产生不好的用户体验。

我们激活`StrictMode`后，我们开始收到了关于我们的App错误操作磁盘I/O的不友好信息。

```java
D/StrictMode  StrictMode policy violation; ~duration=998 ms: android.os.StrictMode$StrictModeDiskReadViolation: policy=31 violation=2
at android.os.StrictMode$AndroidBlockGuardPolicy.onReadFromDisk (StrictMode.java:1135)
at libcore.io.BlockGuardOs.open(BlockGuardOs.java:106) at libcore.io.IoBridge.open(IoBridge.java:393)
at java.io.FileOutputStream.<init>
(FileOutputStream.java:88) at
android.app.ContextImpl.openFileOutput(ContextImpl.java:918) at
android.content.ContextWrapper.openFileOutput(ContextWrapper. java:185)
at com.packtpub.apps.rxjava_essentials.Utils.storeBitmap (Utils.java:30)

```