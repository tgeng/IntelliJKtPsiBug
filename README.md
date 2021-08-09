# IntelliJKtPsiBug

## IDE Versions
```
IntelliJ IDEA 2021.2 (Community Edition)
Build #IC-212.4746.92, built on July 27, 2021
Runtime version: 11.0.11+9-b1504.13 amd64
VM: OpenJDK 64-Bit Server VM by JetBrains s.r.o.
Linux 5.10.40-1rodete2-amd64
GC: G1 Young Generation, G1 Old Generation
Memory: 20480M
Cores: 72
Registry: undo.documentUndoLimit=10000, analyze.exceptions.on.the.fly=true, undo.globalUndoLimit=100, suppress.focus.stealing.disable.auto.request.focus=true, debugger.watches.in.variables=false, ide.balloon.shadow.size=0, focus.follows.mouse.workarounds=true, ide.images.show.chessboard=true
Non-Bundled Plugins: IdeaVIM (0.68), com.jetbrains.embeddedProjectJdk (2.1), String Manipulation (8.15.203.000.3), ms.konovalov.intellij.hidpi-profiles (2017.1.3), navigate-from-literal (1.8), me.artspb.hackathon.git.bisect.run (0.7.3), io.wisetime.plugins.window.branch (0.6.1), org.turbanov.run.configuration.as.action (1.4.5), izhangzhihao.rainbow.brackets (6.19)
Kotlin: 212-1.5.10-release-IJ4746.92
Current Desktop: Undefined
```

## Reproduce step:

1. Open the project in IntelliJ and confgiure Kotlin if needed
2. Open `Main.kt` and click the green triangle to run or debug. It should work but may not work.
3. If 2 does work fine, open `foo.kt` and make any edits.
4. Go back to `Main.kt` and try run or debug again. It should no longer work now.
5. Delete `foo.kt` and restart the IDE and repeat step 2. Everything should work

Looking at idea.log, it looks `foo.kt` has crashed the Kotlin plugin.
```
2021-08-09 16:23:24,117 [  54589]  ERROR - nSystem.impl.ActionToolbarImpl - file=KtFile: foo.kt; tree=Element(kotlin.FILE)
 each of class class org.jetbrains.kotlin.psi.KtValueArgument; valid=true; ref=com.intellij.psi.impl.source.SubstrateRef$StubRef@2e9f29ad
 each of class class org.jetbrains.kotlin.psi.KtValueArgumentList; valid=true; ref=com.intellij.psi.impl.source.SubstrateRef$StubRef@c6f4e16
 each of class class org.jetbrains.kotlin.psi.KtAnnotationEntry; valid=true; ref=com.intellij.psi.impl.source.SubstrateRef$StubRef@f25e662
 each of class class org.jetbrains.kotlin.psi.KtDeclarationModifierList; valid=true; ref=com.intellij.psi.impl.source.SubstrateRef$StubRef@692da17a
 each of class class org.jetbrains.kotlin.psi.KtFile; valid=true; same file=true; current tree= Element(kotlin.FILE); stubTree=null; physical=false
 each stub KotlinStub$VALUE_ARGUMENT[isSpread=false]
 each stub KotlinStub$VALUE_ARGUMENT_LIST
 each stub KotlinStub$ANNOTATION_ENTRY[hasValueArguments=true, shortName=JvmName]
 each stub KotlinStub$MODIFIER_LIST[]
 each stub PsiJetFileStubImpl[package=](file='KtFile: foo.kt', invalidationReason=null, stubRoots='[PsiJetFileStubImpl[package=]]', stubTree='null') 
java.lang.AssertionError: file=KtFile: foo.kt; tree=Element(kotlin.FILE)
 each of class class org.jetbrains.kotlin.psi.KtValueArgument; valid=true; ref=com.intellij.psi.impl.source.SubstrateRef$StubRef@2e9f29ad
 each of class class org.jetbrains.kotlin.psi.KtValueArgumentList; valid=true; ref=com.intellij.psi.impl.source.SubstrateRef$StubRef@c6f4e16
 each of class class org.jetbrains.kotlin.psi.KtAnnotationEntry; valid=true; ref=com.intellij.psi.impl.source.SubstrateRef$StubRef@f25e662
 each of class class org.jetbrains.kotlin.psi.KtDeclarationModifierList; valid=true; ref=com.intellij.psi.impl.source.SubstrateRef$StubRef@692da17a
 each of class class org.jetbrains.kotlin.psi.KtFile; valid=true; same file=true; current tree= Element(kotlin.FILE); stubTree=null; physical=false
 each stub KotlinStub$VALUE_ARGUMENT[isSpread=false]
 each stub KotlinStub$VALUE_ARGUMENT_LIST
 each stub KotlinStub$ANNOTATION_ENTRY[hasValueArguments=true, shortName=JvmName]
 each stub KotlinStub$MODIFIER_LIST[]
 each stub PsiJetFileStubImpl[package=](file='KtFile: foo.kt', invalidationReason=null, stubRoots='[PsiJetFileStubImpl[package=]]', stubTree='null')
	at com.intellij.extapi.psi.StubBasedPsiElementBase.notBoundInExistingAst(StubBasedPsiElementBase.java:212)
	at com.intellij.extapi.psi.StubBasedPsiElementBase.getNode(StubBasedPsiElementBase.java:125)
	at com.intellij.extapi.psi.ASTDelegatePsiElement.getFirstChild(ASTDelegatePsiElement.java:99)
	at com.intellij.psi.impl.PsiElementBase.findChildByClass(PsiElementBase.java:299)
	at org.jetbrains.kotlin.psi.KtValueArgument.getArgumentExpression(KtValueArgument.java:73)
	at org.jetbrains.kotlin.fileClasses.JvmFileClassUtil.getLiteralStringFromAnnotation(JvmFileClassUtil.kt:102)
	at org.jetbrains.kotlin.idea.stubindex.IndexUtilsKt.indexJvmNameAnnotation(IndexUtils.kt:143)
	at org.jetbrains.kotlin.idea.stubindex.IdeStubIndexService.indexAnnotation(IdeStubIndexService.java:231)
	at org.jetbrains.kotlin.psi.stubs.elements.KtAnnotationEntryElementType.indexStub(KtAnnotationEntryElementType.java:66)
	at org.jetbrains.kotlin.psi.stubs.elements.KtAnnotationEntryElementType.indexStub(KtAnnotationEntryElementType.java:34)
	at com.intellij.psi.stubs.ObjectStubTree.indexStubTree(ObjectStubTree.java:57)
	at com.intellij.psi.stubs.SerializedStubTree.indexTree(SerializedStubTree.java:191)
	at com.intellij.psi.stubs.SerializedStubTree.serializeStub(SerializedStubTree.java:60)
	at com.intellij.psi.stubs.StubUpdatingIndex$1.computeValue(StubUpdatingIndex.java:193)
	at com.intellij.psi.stubs.StubUpdatingIndex$1.computeValue(StubUpdatingIndex.java:156)
	at com.intellij.psi.stubs.StubUpdatingIndex$1.computeValue(StubUpdatingIndex.java:123)
	at com.intellij.util.indexing.SingleEntryIndexer.map(SingleEntryIndexer.java:30)
	at com.intellij.util.indexing.SingleEntryIndexer.map(SingleEntryIndexer.java:19)
	at com.intellij.util.indexing.impl.MapReduceIndex.mapByIndexer(MapReduceIndex.java:328)
	at com.intellij.util.indexing.impl.MapReduceIndex.mapInput(MapReduceIndex.java:318)
	at com.intellij.util.indexing.impl.storage.VfsAwareMapReduceIndex.mapInput(VfsAwareMapReduceIndex.java:182)
	at com.intellij.util.indexing.impl.storage.VfsAwareMapReduceIndex.mapInput(VfsAwareMapReduceIndex.java:47)
	at com.intellij.util.indexing.impl.MapReduceIndex.mapInputAndPrepareUpdate(MapReduceIndex.java:263)
	at com.intellij.psi.stubs.StubUpdatingIndexStorage.mapInputAndPrepareUpdate(StubUpdatingIndexStorage.java:60)
	at com.intellij.psi.stubs.StubUpdatingIndexStorage.mapInputAndPrepareUpdate(StubUpdatingIndexStorage.java:19)
	at com.intellij.indexing.composite.CompositeInvertedIndexBase.updateBaseIndex(CompositeInvertedIndexBase.java:232)
	at com.intellij.indexing.composite.CompositeInvertedIndexBase.mapInputAndPrepareUpdate(CompositeInvertedIndexBase.java:55)
	at com.intellij.indexing.composite.CompositeInvertedIndexBase.mapInputAndPrepareUpdate(CompositeInvertedIndexBase.java:26)
	at com.intellij.util.indexing.FileBasedIndexImpl.updateSingleIndex(FileBasedIndexImpl.java:1520)
	at com.intellij.util.indexing.FileBasedIndexImpl.lambda$doIndexFileContent$23(FileBasedIndexImpl.java:1394)
	at com.intellij.openapi.fileTypes.impl.FileTypeManagerImpl.freezeFileTypeTemporarilyIn(FileTypeManagerImpl.java:638)
	at com.intellij.util.indexing.FileBasedIndexImpl.doIndexFileContent(FileBasedIndexImpl.java:1361)
	at com.intellij.util.indexing.FileBasedIndexImpl.indexFileContent(FileBasedIndexImpl.java:1315)
	at com.intellij.util.indexing.FileBasedIndexImpl.processRefreshedFile(FileBasedIndexImpl.java:1280)
	at com.intellij.util.indexing.FileBasedIndexImpl$VirtualFileUpdateTask.doProcess(FileBasedIndexImpl.java:1621)
	at com.intellij.util.indexing.FileBasedIndexImpl$VirtualFileUpdateTask.doProcess(FileBasedIndexImpl.java:1618)
	at com.intellij.util.indexing.UpdateTask.process(UpdateTask.java:63)
	at com.intellij.util.indexing.UpdateTask.processAll(UpdateTask.java:32)
	at com.intellij.util.indexing.FileBasedIndexImpl.forceUpdate(FileBasedIndexImpl.java:1640)
	at com.intellij.util.indexing.FileBasedIndexImpl.ensureUpToDate(FileBasedIndexImpl.java:792)
	at com.intellij.util.indexing.FileBasedIndexEx.processExceptions(FileBasedIndexEx.java:244)
	at com.intellij.util.indexing.FileBasedIndexEx.collectFileIdsContainingAllKeys(FileBasedIndexEx.java:512)
	at com.intellij.util.indexing.FileBasedIndexEx.processFilesContainingAllKeysInPhysicalFiles(FileBasedIndexEx.java:385)
	at com.intellij.util.indexing.FileBasedIndexEx.processFilesContainingAllKeys(FileBasedIndexEx.java:413)
	at com.intellij.psi.impl.cache.impl.IndexCacheManagerImpl.processVirtualFilesWithAllWords(IndexCacheManagerImpl.java:83)
	at com.intellij.psi.impl.cache.impl.IndexCacheManagerImpl.collectVirtualFilesWithWord(IndexCacheManagerImpl.java:99)
	at com.intellij.psi.impl.cache.impl.IndexCacheManagerImpl.processFilesWithWord(IndexCacheManagerImpl.java:106)
	at com.intellij.psi.impl.search.PsiSearchHelperImpl.processAllFilesWithWordInLiterals(PsiSearchHelperImpl.java:688)
	at org.jetbrains.idea.devkit.testAssistant.TestLocationDataRule.collectRelativeLocations(TestLocationDataRule.java:67)
	at org.jetbrains.idea.devkit.testAssistant.TestLocationDataRule.getData(TestLocationDataRule.java:37)
	at com.intellij.ide.impl.DataManagerImpl.lambda$getDataRule$4(DataManagerImpl.java:116)
	at com.intellij.ide.impl.DataManagerImpl.getDataFromProvider(DataManagerImpl.java:73)
	at com.intellij.openapi.actionSystem.impl.PreCachedDataContext.getData(PreCachedDataContext.java:126)
	at com.intellij.openapi.actionSystem.DataContext.getData(DataContext.java:42)
	at com.intellij.openapi.actionSystem.impl.ActionUpdater.ensureSlowDataKeysPreCached(ActionUpdater.java:344)
	at com.intellij.openapi.actionSystem.impl.ActionUpdater.lambda$expandActionGroupAsync$14(ActionUpdater.java:273)
	at com.intellij.openapi.application.impl.ApplicationImpl.tryRunReadAction(ApplicationImpl.java:1078)
	at com.intellij.openapi.actionSystem.impl.ActionUpdater.lambda$expandActionGroupAsync$16(ActionUpdater.java:298)
	at com.intellij.openapi.progress.util.ProgressIndicatorUtils.runActionAndCancelBeforeWrite(ProgressIndicatorUtils.java:161)
	at com.intellij.openapi.actionSystem.impl.ActionUpdater.lambda$expandActionGroupAsync$17(ActionUpdater.java:295)
	at com.intellij.openapi.progress.impl.CoreProgressManager.lambda$runProcess$2(CoreProgressManager.java:183)
	at com.intellij.openapi.progress.impl.CoreProgressManager.registerIndicatorAndRun(CoreProgressManager.java:705)
	at com.intellij.openapi.progress.impl.CoreProgressManager.executeProcessUnderProgress(CoreProgressManager.java:647)
	at com.intellij.openapi.progress.impl.ProgressManagerImpl.executeProcessUnderProgress(ProgressManagerImpl.java:63)
	at com.intellij.openapi.progress.impl.CoreProgressManager.runProcess(CoreProgressManager.java:170)
	at com.intellij.openapi.progress.util.BackgroundTaskUtil.runUnderDisposeAwareIndicator(BackgroundTaskUtil.java:270)
	at com.intellij.openapi.actionSystem.impl.ActionUpdater.lambda$expandActionGroupAsync$18(ActionUpdater.java:294)
	at com.intellij.codeWithMe.ClientId$Companion.withClientId(ClientId.kt:135)
	at com.intellij.codeWithMe.ClientId.withClientId(ClientId.kt)
	at com.intellij.openapi.actionSystem.impl.ActionUpdater.lambda$expandActionGroupAsync$19(ActionUpdater.java:292)
	at com.intellij.util.concurrency.BoundedTaskExecutor.doRun(BoundedTaskExecutor.java:216)
	at com.intellij.util.concurrency.BoundedTaskExecutor.access$200(BoundedTaskExecutor.java:27)
	at com.intellij.util.concurrency.BoundedTaskExecutor$1.execute(BoundedTaskExecutor.java:195)
	at com.intellij.util.ConcurrencyUtil.runUnderThreadName(ConcurrencyUtil.java:213)
	at com.intellij.util.concurrency.BoundedTaskExecutor$1.run(BoundedTaskExecutor.java:184)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1$1.run(Executors.java:668)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1$1.run(Executors.java:665)
	at java.base/java.security.AccessController.doPrivileged(Native Method)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1.run(Executors.java:665)
	at java.base/java.lang.Thread.run(Thread.java:829)
```
