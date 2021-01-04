# LeakCanary #
## API ##
[https://square.github.io/leakcanary/recipes/#matching-known-library-leaks](https://square.github.io/leakcanary/recipes/#matching-known-library-leaks "leakcanary使用")

1. LeakCanary config

    `
	val dumpHeap: Boolean = true,
	val dumpHeapWhenDebugging: Boolean = false,
	val retainedVisibleThreshold: Int = 5,
	val referenceMatchers: List<ReferenceMatcher> = AndroidReferenceMatchers.appDefaults,
	val onHeapAnalyzedListener: OnHeapAnalyzedListener = DefaultOnHeapAnalyzedListener.create(),
	val metadataExtractor: MetadataExtractor = AndroidMetadataExtractor,
	val computeRetainedHeapSize: Boolean = true,
	val maxStoredHeapDumps: Int = 7,
	val requestWriteExternalStoragePermission: Boolean = false,
	val leakingObjectFinder: LeakingObjectFinder = KeyedWeakReferenceFinder,
	val useExperimentalLeakFinders: Boolean = false
	`

2. AppWatcher config
    `    
	val watchActivities: Boolean = true,
    val watchFragments: Boolean = true,
    val watchFragmentViews: Boolean = true,
    val watchViewModels: Boolean = true,
    val watchDurationMillis: Long = TimeUnit.SECONDS.toMillis(5),
    val enabled: Boolean = true
	`

3. 分离进程分析泄漏
    `dependencies {
  // debugImplementation 'com.squareup.leakcanary:leakcanary-android:${version}'
  debugImplementation 'com.squareup.leakcanary:leakcanary-android-process:${version}'
}`

## 原理 ##


通过contentProvider实现自动初始化

初始化操作：
1. ActivityDestroyWatcher初始化：application注册监听Activity生命周期，并在Activity destroy的时候调用objectWatcher.watch()
2. FragmentDestroyWatcher初始化：
- 如果android 8.0以上，注册AndroidOFragmentDestroyWatcher监听fragment生命周期
- 注册AndroidSupportFragmentDestroyWatcher监听fragment生命周期
- 注册AndroidXFragmentDestroyWatcher监听fragment生命周期

以上三种本质上都是通过传入的Activity.fragmentManager.registerFragmentLifecycleCallbacks进行监听fragment生命周期回调


3. 通过InternalLeakCanary类的invoke函数：
- 初始化一些检测内存泄露过程中需要的对象。
- addOnObjectRetainedListener设置可能存在内存泄漏的回调。
- 通过AndroidHeapDumper进行内存泄漏之后进行 heap dump 任务。
- 通过GcTrigger 手动调用 GC 再次确认内存泄露。
- 启动内存泄漏检查的线程。
- 通过registerVisibilityListener监听应用程序的可见性。

4. 