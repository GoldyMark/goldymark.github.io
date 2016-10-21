---
title: Vaadin学习笔记之记一次编译错误
comments: true
date: 2013-02-21 23:14:00
updated: 2013-02-21 23:14:00
tags:
  - java
  - vaadin
categories:
  - program
  - java
---

### 系统
Windows XP SP3；
### 浏览器
Chrome 版本 24.0.1312.57 m
### IDE
Eclipse 4.2 Juno

以前一直在用`GWT`，最近看到`Vaadin`，好像不错的样子，有意学习之（截至发布时最新版本为`7.0.1`）。

<!-- more -->

按官网说明，用`Maven`生成一个新的`Vaadin7`项目后，直接在`Eclipse`里面`Run on Server`（用的是`Tomcat7`），结果弹窗如下：

{% asset_img p1.jpg %}

Google一番之后找到解决办法，原来运行Vaadin项目前要先编译一次，运行`mvn vaadin:compile`之后，再Refresh一下项目，然后运行，OK！

这篇博文要说的就是在编译过程中出现的一个问题，估计也不会很多人会遇到，但我还是把它写下来吧。

刚开始编译的时候，死活编译不过，看控制台输出的信息也看不出个倪端，控制台信息如下：

```log
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[INFO] Scanning for projects...
[WARNING]
[WARNING] Some problems were encountered while building the effective model for cn.nh121:Test:war:1.0
[WARNING] 'build.plugins.plugin.version' for org.mortbay.jetty:jetty-maven-plugin is missing. @ line 133, column 12
[WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-compiler-plugin is missing. @ line 76, column 12
[WARNING]
[WARNING] It is highly recommended to fix these problems because they threaten the stability of your build.
[WARNING]
[WARNING] For this reason, future Maven versions might no longer support building such malformed projects.
[WARNING]
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Vaadin Web Application 1.0
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] >>> vaadin-maven-plugin:7.0.1:compile (default-cli) @ Test >>>
[INFO]
[INFO] --- maven-resources-plugin:2.5:resources (default-resources) @ Test ---
[debug] execute contextualize
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\development\workspace\Test\src\main\resources
[INFO]
[INFO] --- vaadin-maven-plugin:7.0.1:resources (default) @ Test ---
[INFO] auto discovered modules [cn.nh121.AppWidgetSet]
[INFO] 1 source files from GWT module cn.nh121.AppWidgetSet
[INFO]
[INFO] <<< vaadin-maven-plugin:7.0.1:compile (default-cli) @ Test <<<
[INFO]
[INFO] --- vaadin-maven-plugin:7.0.1:compile (default-cli) @ Test ---
[INFO] auto discovered modules [cn.nh121.AppWidgetSet]
[INFO] Compiling module cn.nh121.AppWidgetSet
[INFO] Computing all possible rebind results for 'com.vaadin.client.metadata.ConnectorBundleLoader'
[INFO] Rebinding com.vaadin.client.metadata.ConnectorBundleLoader
[INFO] Invoking generator com.vaadin.server.widgetsetutils.ConnectorBundleLoaderFactory
[INFO] Populating eager bundle
[INFO] Visiting VerticalLayoutConnector with ConnectorInitVisitor
[INFO] VerticalLayoutConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.orderedlayout.VerticalLayoutState as a bean
[INFO] Will serialize class java.util.HashMap<com.vaadin.shared.Connector, com.vaadin.shared.ui.orderedlayout.AbstractOrderedLayoutState.ChildComponentData> as a bean
[INFO] Will serialize class com.vaadin.shared.ui.orderedlayout.AbstractOrderedLayoutState.ChildComponentData as a bean
[INFO] Will serialize class com.vaadin.shared.communication.URLReference using URLReference_Serializer
[INFO] Visiting UIConnector with ConnectorInitVisitor
[INFO] UIConnector will be in the eager bundle
[INFO] Visiting UIConnector with WidgetInitVisitor
[INFO] [WARN] com.vaadin.client.ui.ui.UIConnector has overridden getTooltipInfo
[INFO] Will serialize class com.vaadin.shared.ui.ui.UIState as a bean
[INFO] Visiting TableConnector with ConnectorInitVisitor
[INFO] TableConnector will be in the eager bundle
[INFO] Visiting TableConnector with WidgetInitVisitor
[INFO] [WARN] com.vaadin.client.ui.table.TableConnector has overridden getTooltipInfo
[INFO] Will serialize class com.vaadin.shared.ui.table.TableState as a bean
[INFO] Visiting NativeSelectConnector with ConnectorInitVisitor
[INFO] NativeSelectConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.AbstractFieldState as a bean
[INFO] Visiting CustomComponentConnector with ConnectorInitVisitor
[INFO] CustomComponentConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.AbstractComponentState as a bean
[INFO] Visiting WindowConnector with ConnectorInitVisitor
[INFO] WindowConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.window.WindowState as a bean
[INFO] Visiting PasswordFieldConnector with ConnectorInitVisitor
[INFO] PasswordFieldConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.textfield.AbstractTextFieldState as a bean
[INFO] Visiting InlineDateFieldConnector with ConnectorInitVisitor
[INFO] InlineDateFieldConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.datefield.InlineDateFieldState as a bean
[INFO] Visiting PanelConnector with ConnectorInitVisitor
[INFO] PanelConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.panel.PanelState as a bean
[INFO] Visiting CustomFieldConnector with ConnectorInitVisitor
[INFO] CustomFieldConnector will be in the eager bundle
[INFO] Visiting DragAndDropWrapperConnector with ConnectorInitVisitor
[INFO] DragAndDropWrapperConnector will be in the eager bundle
[INFO] Visiting LinkConnector with ConnectorInitVisitor
[INFO] LinkConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.link.LinkState as a bean
[INFO] Visiting BrowserWindowOpenerConnector with ConnectorInitVisitor
[INFO] BrowserWindowOpenerConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.BrowserWindowOpenerState as a bean
[INFO] Visiting ImageConnector with ConnectorInitVisitor
[INFO] ImageConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.image.ImageState as a bean
[INFO] Visiting LabelConnector with ConnectorInitVisitor
[INFO] LabelConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.label.LabelState as a bean
[INFO] Will serialize class com.vaadin.shared.ui.label.ContentMode as an enum
[INFO] Visiting BrowserFrameConnector with ConnectorInitVisitor
[INFO] BrowserFrameConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.browserframe.BrowserFrameState as a bean
[INFO] Visiting TabsheetConnector with ConnectorInitVisitor
[INFO] TabsheetConnector will be in the eager bundle
[INFO] Visiting TabsheetConnector with WidgetInitVisitor
[INFO] [WARN] com.vaadin.client.ui.tabsheet.TabsheetConnector has overridden getTooltipInfo
[INFO] Will serialize class com.vaadin.shared.ui.tabsheet.TabsheetState as a bean
[INFO] Visiting TreeConnector with ConnectorInitVisitor
[INFO] TreeConnector will be in the eager bundle
[INFO] Visiting TreeConnector with WidgetInitVisitor
[INFO] [WARN] com.vaadin.client.ui.tree.TreeConnector has overridden getTooltipInfo
[INFO] Will serialize class com.vaadin.shared.ui.tree.TreeState as a bean
[INFO] Visiting NativeButtonConnector with ConnectorInitVisitor
[INFO] NativeButtonConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.button.NativeButtonState as a bean
[INFO] Visiting PopupDateFieldConnector with ConnectorInitVisitor
[INFO] PopupDateFieldConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.datefield.PopupDateFieldState as a bean
[INFO] Visiting EmbeddedConnector with ConnectorInitVisitor
[INFO] EmbeddedConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.embedded.EmbeddedState as a bean
[INFO] Visiting FlashConnector with ConnectorInitVisitor
[INFO] FlashConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.flash.FlashState as a bean
[INFO] Visiting CssLayoutConnector with ConnectorInitVisitor
[INFO] CssLayoutConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.csslayout.CssLayoutState as a bean
[INFO] Visiting AudioConnector with ConnectorInitVisitor
[INFO] AudioConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.AbstractMediaState as a bean
[INFO] Visiting VideoConnector with ConnectorInitVisitor
[INFO] VideoConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.video.VideoState as a bean
[INFO] Visiting CustomLayoutConnector with ConnectorInitVisitor
[INFO] CustomLayoutConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.customlayout.CustomLayoutState as a bean
[INFO] Visiting AbsoluteLayoutConnector with ConnectorInitVisitor
[INFO] AbsoluteLayoutConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.absolutelayout.AbsoluteLayoutState as a bean
[INFO] Visiting HorizontalLayoutConnector with ConnectorInitVisitor
[INFO] HorizontalLayoutConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.orderedlayout.HorizontalLayoutState as a bean
[INFO] Visiting TextFieldConnector with ConnectorInitVisitor
[INFO] TextFieldConnector will be in the eager bundle
[INFO] Visiting ListSelectConnector with ConnectorInitVisitor
[INFO] ListSelectConnector will be in the eager bundle
[INFO] Visiting FileDownloaderConnector with ConnectorInitVisitor
[INFO] FileDownloaderConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.communication.SharedState as a bean
[INFO] Visiting ButtonConnector with ConnectorInitVisitor
[INFO] ButtonConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.button.ButtonState as a bean
[INFO] Visiting GridLayoutConnector with ConnectorInitVisitor
[INFO] GridLayoutConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.gridlayout.GridLayoutState as a bean
[INFO] Will serialize class com.vaadin.shared.ui.gridlayout.GridLayoutState.ChildComponentData as a bean
[INFO] Visiting TwinColSelectConnector with ConnectorInitVisitor
[INFO] TwinColSelectConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.twincolselect.TwinColSelectState as a bean
[INFO] Visiting JavaScriptExtension with ConnectorInitVisitor
[INFO] JavaScriptExtension will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.JavaScriptExtensionState as a bean
[INFO] Visiting FormConnector with ConnectorInitVisitor
[INFO] FormConnector will be in the eager bundle
[INFO] Visiting FormConnector with WidgetInitVisitor
[INFO] [WARN] com.vaadin.client.ui.form.FormConnector has overridden getTooltipInfo
[INFO] Will serialize class com.vaadin.shared.ui.form.FormState as a bean
[INFO] Visiting TextAreaConnector with ConnectorInitVisitor
[INFO] TextAreaConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.textarea.TextAreaState as a bean
[INFO] Visiting UploadConnector with ConnectorInitVisitor
[INFO] UploadConnector will be in the eager bundle
[INFO] Visiting FormLayoutConnector with ConnectorInitVisitor
[INFO] FormLayoutConnector will be in the eager bundle
[INFO] Visiting FormLayoutConnector with WidgetInitVisitor
[INFO] [WARN] com.vaadin.client.ui.formlayout.FormLayoutConnector has overridden getTooltipInfo
[INFO] Will serialize class com.vaadin.shared.ui.orderedlayout.AbstractOrderedLayoutState as a bean
[INFO] Visiting JavaScriptManagerConnector with ConnectorInitVisitor
[INFO] JavaScriptManagerConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.extension.javascriptmanager.JavaScriptManagerState as a bean
[INFO] Visiting ProgressIndicatorConnector with ConnectorInitVisitor
[INFO] ProgressIndicatorConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.progressindicator.ProgressIndicatorState as a bean
[INFO] Visiting SliderConnector with ConnectorInitVisitor
[INFO] SliderConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.slider.SliderState as a bean
[INFO] Will serialize class com.vaadin.shared.ui.slider.SliderOrientation as an enum
[INFO] Visiting ComboBoxConnector with ConnectorInitVisitor
[INFO] ComboBoxConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.combobox.ComboBoxState as a bean
[INFO] Visiting AccordionConnector with ConnectorInitVisitor
[INFO] AccordionConnector will be in the eager bundle
[INFO] Visiting CheckBoxConnector with ConnectorInitVisitor
[INFO] CheckBoxConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.checkbox.CheckBoxState as a bean
[INFO] Visiting VerticalSplitPanelConnector with ConnectorInitVisitor
[INFO] VerticalSplitPanelConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.splitpanel.VerticalSplitPanelState as a bean
[INFO] Will serialize class com.vaadin.shared.ui.splitpanel.AbstractSplitPanelState.SplitterState as a bean
[INFO] Visiting JavaScriptComponentConnector with ConnectorInitVisitor
[INFO] JavaScriptComponentConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.JavaScriptComponentState as a bean
[INFO] Visiting OptionGroupConnector with ConnectorInitVisitor
[INFO] OptionGroupConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.optiongroup.OptionGroupState as a bean
[INFO] Visiting PopupViewConnector with ConnectorInitVisitor
[INFO] PopupViewConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.popupview.PopupViewState as a bean
[INFO] Visiting TreeTableConnector with ConnectorInitVisitor
[INFO] TreeTableConnector will be in the eager bundle
[INFO] Visiting TreeTableConnector with WidgetInitVisitor
[INFO] [WARN] com.vaadin.client.ui.treetable.TreeTableConnector has overridden getTooltipInfo
[INFO] Will serialize class com.vaadin.shared.ui.treetable.TreeTableState as a bean
[INFO] Visiting MenuBarConnector with ConnectorInitVisitor
[INFO] MenuBarConnector will be in the eager bundle
[INFO] Visiting MenuBarConnector with WidgetInitVisitor
[INFO] [WARN] com.vaadin.client.ui.menubar.MenuBarConnector has overridden getTooltipInfo
[INFO] Will serialize class com.vaadin.shared.ui.menubar.MenuBarState as a bean
[INFO] Visiting HorizontalSplitPanelConnector with ConnectorInitVisitor
[INFO] HorizontalSplitPanelConnector will be in the eager bundle
[INFO] Will serialize class com.vaadin.shared.ui.splitpanel.HorizontalSplitPanelState as a bean
[INFO] Will serialize class com.vaadin.shared.MouseEventDetails as a bean
[INFO] Will serialize class com.vaadin.shared.MouseEventDetails.MouseButton as an enum
[INFO] Populating RichTextAreaConnector bundle
[INFO] Visiting RichTextAreaConnector with ConnectorInitVisitor
[INFO] RichTextAreaConnector will be in the RichTextAreaConnector bundle
[INFO] Populating ColorPickerGradientConnector bundle
[INFO] Visiting ColorPickerGradientConnector with ConnectorInitVisitor
[INFO] ColorPickerGradientConnector will be in the ColorPickerGradientConnector bundle
[INFO] Will serialize class com.vaadin.shared.ui.colorpicker.ColorPickerGradientState as a bean
[INFO] Populating ColorPickerAreaConnector bundle
[INFO] Visiting ColorPickerAreaConnector with ConnectorInitVisitor
[INFO] ColorPickerAreaConnector will be in the ColorPickerAreaConnector bundle
[INFO] Will serialize class com.vaadin.shared.ui.colorpicker.ColorPickerState as a bean
[INFO] Populating ColorPickerConnector bundle
[INFO] Visiting ColorPickerConnector with ConnectorInitVisitor
[INFO] ColorPickerConnector will be in the ColorPickerConnector bundle
[INFO] Will serialize class com.vaadin.shared.ui.colorpicker.ColorPickerState as a bean
[INFO] Populating ColorPickerGridConnector bundle
[INFO] Visiting ColorPickerGridConnector with ConnectorInitVisitor
[INFO] ColorPickerGridConnector will be in the ColorPickerGridConnector bundle
[INFO] Will serialize class com.vaadin.shared.ui.colorpicker.ColorPickerGridState as a bean
[INFO] Will serialize java.lang.String[] as an array
[INFO] Computing all possible rebind results for 'com.vaadin.client.ui.dd.VAcceptCriterionFactory'
[INFO] Rebinding com.vaadin.client.ui.dd.VAcceptCriterionFactory
[INFO] Invoking generator com.vaadin.server.widgetsetutils.AcceptCriteriaFactoryGenerator
[INFO] Detecting available criteria ...
[INFO] creating mapping for com.vaadin.ui.AbstractSelect.TargetItemIs
[INFO] creating mapping for com.vaadin.ui.Table.TableDropCriterion
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.SourceIs
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.SourceIsTarget
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.AcceptAll
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.Not
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.And
[INFO] creating mapping for com.vaadin.ui.AbstractSelect.AcceptItem
[INFO] creating mapping for com.vaadin.ui.Tree.TreeDropCriterion
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.ServerSideCriterion
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.Or
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.TargetDetailIs
[INFO] creating mapping for com.vaadin.ui.Tree.TargetInSubtree
[INFO] creating mapping for com.vaadin.event.dd.acceptcriteria.ContainsDataFlavor
[INFO] Done. (0seconds)
[INFO] Compiling 6 permutations
[INFO] Compiling permutation 2...
[INFO] Process output
[INFO] Compiling
[INFO] Compiling permutation 1...
[INFO] Process output
[INFO] Compiling
[INFO] Compiling permutation 3...
[INFO] Process output
[INFO] Compiling
[INFO] Compiling permutation 0...
[INFO] [ERROR] Unexpected internal compiler error
[INFO] java.lang.RuntimeException: Unable to write to byte cache
[INFO] at com.google.gwt.dev.util.DiskCache.writeByteArray(DiskCache.java:219)
[INFO] at com.google.gwt.core.ext.linker.SyntheticArtifact.<init>(SyntheticArtifact.java:49)
[INFO] at com.google.gwt.core.ext.linker.SyntheticArtifact.<init>(SyntheticArtifact.java:41)
[INFO] at com.google.gwt.soyc.io.ArtifactsOutputDirectory$OutputStreamForArtifact.close(ArtifactsOutputDirectory.java:52)
[INFO] at sun.nio.cs.StreamEncoder.implClose(Unknown Source)
[INFO] at sun.nio.cs.StreamEncoder.close(Unknown Source)
[INFO] at java.io.OutputStreamWriter.close(Unknown Source)
[INFO] at java.io.BufferedWriter.close(Unknown Source)
[INFO] at java.io.PrintWriter.close(Unknown Source)
[INFO] at com.google.gwt.soyc.MakeTopLevelHtmlForPerm.makeCodeTypeClassesHtmls(MakeTopLevelHtmlForPerm.java:805)
[INFO] at com.google.gwt.soyc.SoycDashboard.makeHTMLFiles(SoycDashboard.java:435)
[INFO] at com.google.gwt.soyc.SoycDashboard.generateForOnePermutation(SoycDashboard.java:340)
[INFO] at com.google.gwt.dev.jjs.JavaToJavaScriptCompiler.makeSoycArtifacts(JavaToJavaScriptCompiler.java:1230)
[INFO] at com.google.gwt.dev.jjs.JavaToJavaScriptCompiler.compilePermutation(JavaToJavaScriptCompiler.java:503)
[INFO] at com.google.gwt.dev.jjs.UnifiedAst.compilePermutation(UnifiedAst.java:134)
[INFO] at com.google.gwt.dev.CompilePerms.compile(CompilePerms.java:195)
[INFO] at com.google.gwt.dev.CompilePermsServer.compilePermutation(CompilePermsServer.java:305)
[INFO] at com.google.gwt.dev.CompilePermsServer.run(CompilePermsServer.java:274)
[INFO] at com.google.gwt.dev.CompilePermsServer.main(CompilePermsServer.java:237)
[INFO] Caused by: java.io.IOException: ���̿ռ䲻�㡣
[INFO] at java.io.RandomAccessFile.writeBytes(Native Method)
[INFO] at java.io.RandomAccessFile.write(Unknown Source)
[INFO] at com.google.gwt.dev.util.DiskCache.writeByteArray(DiskCache.java:216)
[INFO] ... 18 more
[INFO] [ERROR] Error from external worker
[INFO] com.google.gwt.core.ext.UnableToCompleteException: (see previous log entries)
[INFO] at com.google.gwt.dev.javac.CompilationProblemReporter.logAndTranslateException(CompilationProblemReporter.java:96)
[INFO] at com.google.gwt.dev.jjs.JavaToJavaScriptCompiler.compilePermutation(JavaToJavaScriptCompiler.java:523)
[INFO] at com.google.gwt.dev.jjs.UnifiedAst.compilePermutation(UnifiedAst.java:134)
[INFO] at com.google.gwt.dev.CompilePerms.compile(CompilePerms.java:195)
[INFO] at com.google.gwt.dev.CompilePermsServer.compilePermutation(CompilePermsServer.java:305)
[INFO] at com.google.gwt.dev.CompilePermsServer.run(CompilePermsServer.java:274)
[INFO] at com.google.gwt.dev.CompilePermsServer.main(CompilePermsServer.java:237)
[INFO] [ERROR] Unrecoverable exception, shutting down
[INFO] com.google.gwt.core.ext.UnableToCompleteException: (see previous log entries)
[INFO] at com.google.gwt.dev.ExternalPermutationWorkerFactory$ExternalPermutationWorker.compile(ExternalPermutationWorkerFactory.java:155)
[INFO] at com.google.gwt.dev.PermutationWorkerFactory$Manager$WorkerThread.run(PermutationWorkerFactory.java:73)
[INFO] at java.lang.Thread.run(Unknown Source)
[INFO] [ERROR] Not all permutation were compiled , completed (0/6)
[INFO] [WARN] Lost communication with remote process
[INFO] java.net.SocketException: socket closed
[INFO] at java.net.SocketInputStream.socketRead0(Native Method)
[INFO] at java.net.SocketInputStream.read(Unknown Source)
[INFO] at java.net.SocketInputStream.read(Unknown Source)
[INFO] at java.io.ObjectInputStream$PeekInputStream.peek(Unknown Source)
[INFO] at java.io.ObjectInputStream$BlockDataInputStream.peek(Unknown Source)
[INFO] at java.io.ObjectInputStream$BlockDataInputStream.peekByte(Unknown Source)
[INFO] at java.io.ObjectInputStream.readObject0(Unknown Source)
[INFO] at java.io.ObjectInputStream.readObject(Unknown Source)
[INFO] at com.google.gwt.dev.ExternalPermutationWorkerFactory$ExternalPermutationWorker.compile(ExternalPermutationWorkerFactory.java:152)
[INFO] at com.google.gwt.dev.PermutationWorkerFactory$Manager$WorkerThread.run(PermutationWorkerFactory.java:73)
[INFO] at java.lang.Thread.run(Unknown Source)
[INFO] [WARN] Lost communication with remote process
[INFO] java.net.SocketException: socket closed
[INFO] at java.net.SocketInputStream.socketRead0(Native Method)
[INFO] at java.net.SocketInputStream.read(Unknown Source)
[INFO] at java.net.SocketInputStream.read(Unknown Source)
[INFO] at java.io.ObjectInputStream$PeekInputStream.peek(Unknown Source)
[INFO] at java.io.ObjectInputStream$BlockDataInputStream.peek(Unknown Source)
[INFO] at java.io.ObjectInputStream$BlockDataInputStream.peekByte(Unknown Source)
[INFO] at java.io.ObjectInputStream.readObject0(Unknown Source)
[INFO] at java.io.ObjectInputStream.readObject(Unknown Source)
[INFO] at com.google.gwt.dev.ExternalPermutationWorkerFactory$ExternalPermutationWorker.compile(ExternalPermutationWorkerFactory.java:152)
[INFO] at com.google.gwt.dev.PermutationWorkerFactory$Manager$WorkerThread.run(PermutationWorkerFactory.java:73)
[INFO] at java.lang.Thread.run(Unknown Source)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1:49.312s
[INFO] Finished at: Thu Feb 21 23:06:18 CST 2013
[INFO] Final Memory: 7M/18M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal com.vaadin:vaadin-maven-plugin:7.0.1:compile (default-cli) on project Test: Command [[
[ERROR] C:\Program Files\Java\jre6\bin\java -Xmx512M -Xss1024k -classpath D:\development\workspace\Test\target\classes;D:\development\workspace\Test\src\main\java;D:\development\workspace\Test\src\main\resources;D:\development\lib\com\vaadin\vaadin-server\7.0.1\vaadin-server-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-shared\7.0.1\vaadin-shared-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-shared-deps\1.0.2\vaadin-shared-deps-1.0.2.jar;D:\development\lib\com\vaadin\vaadin-theme-compiler\7.0.1\vaadin-theme-compiler-7.0.1.jar;D:\development\lib\org\apache\commons\commons-jexl\2.1.1\commons-jexl-2.1.1.jar;D:\development\lib\commons-logging\commons-logging\1.1.1\commons-logging-1.1.1.jar;D:\development\lib\net\sourceforge\cssparser\cssparser\0.9.5\cssparser-0.9.5.jar;D:\development\lib\commons-cli\commons-cli\1.2\commons-cli-1.2.jar;D:\development\lib\org\jsoup\jsoup\1.6.3\jsoup-1.6.3.jar;D:\development\lib\com\vaadin\vaadin-client-compiled\7.0.1\vaadin-client-compiled-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-client\7.0.1\vaadin-client-7.0.1.jar;D:\development\lib\org\w3c\css\sac\1.3\sac-1.3.jar;D:\development\lib\javax\validation\validation-api\1.0.0.GA\validation-api-1.0.0.GA.jar;D:\development\lib\javax\validation\validation-api\1.0.0.GA\validation-api-1.0.0.GA-sources.jar;D:\development\lib\com\vaadin\vaadin-themes\7.0.1\vaadin-themes-7.0.1.jar;D:\development\lib\javax\servlet\servlet-api\2.4\servlet-api-2.4.jar;D:\development\lib\com\vaadin\vaadin-client\7.0.1\vaadin-client-7.0.1.jar;D:\development\lib\xerces\xmlParserAPIs\2.6.2\xmlParserAPIs-2.6.2.jar;D:\development\lib\commons-digester\commons-digester\1.6\commons-digester-1.6.jar;D:\development\lib\commons-beanutils\commons-beanutils\1.7.0\commons-beanutils-1.7.0.jar;D:\development\lib\commons-logging\commons-logging\1.1.1\commons-logging-1.1.1.jar;D:\development\lib\commons-collections\commons-collections\3.2\commons-collections-3.2.jar;D:\development\lib\xml-apis\xml-apis\1.3.02\xml-apis-1.3.02.jar;D:\development\lib\oro\oro\2.0.8\oro-2.0.8.jar;D:\development\lib\com\vaadin\vaadin-client-compiler\7.0.1\vaadin-client-compiler-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-shared\7.0.1\vaadin-shared-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-shared-deps\1.0.2\vaadin-shared-deps-1.0.2.jar;D:\development\lib\com\vaadin\vaadin-server\7.0.1\vaadin-server-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-theme-compiler\7.0.1\vaadin-theme-compiler-7.0.1.jar;D:\development\lib\org\apache\commons\commons-jexl\2.1.1\commons-jexl-2.1.1.jar;D:\development\lib\org\w3c\css\sac\1.3\sac-1.3.jar;D:\development\lib\net\sourceforge\cssparser\cssparser\0.9.5\cssparser-0.9.5.jar;D:\development\lib\commons-cli\commons-cli\1.2\commons-cli-1.2.jar;D:\development\lib\org\jsoup\jsoup\1.6.3\jsoup-1.6.3.jar;D:\development\lib\org\codehaus\plexus\plexus-compiler-javac\1.5.3\plexus-compiler-javac-1.5.3.jar;D:\development\lib\org\codehaus\plexus\plexus-utils\2.0.5\plexus-utils-2.0.5.jar;D:\development\lib\org\codehaus\plexus\plexus-compiler-api\1.5.3\plexus-compiler-api-1.5.3.jar;D:\development\lib\org\codehaus\plexus\plexus-container-default\1.0-alpha-8\plexus-container-default-1.0-alpha-8.jar;D:\development\lib\junit\junit\4.8.1\junit-4.8.1.jar;D:\development\lib\org\apache\maven\doxia\doxia-logging-api\1.1.4\doxia-logging-api-1.1.4.jar;D:\development\lib\org\codehaus\plexus\plexus-interpolation\1.13\plexus-interpolation-1.13.jar;D:\development\lib\org\apache\maven\reporting\maven-reporting-api\3.0\maven-reporting-api-3.0.jar;D:\development\lib\org\apache\maven\doxia\doxia-sink-api\1.1.4\doxia-sink-api-1.1.4.jar;D:\development\lib\org\apache\maven\maven-archiver\2.4.1\maven-archiver-2.4.1.jar;D:\development\lib\org\apache\maven\maven-artifact\2.0.6\maven-artifact-2.0.6.jar;D:\development\lib\org\apache\maven\maven-model\2.0.6\maven-model-2.0.6.jar;D:\development\lib\org\codehaus\plexus\plexus-archiver\1.0-alpha-9\plexus-archiver-1.0-alpha-9.jar;D:\development\lib\org\codehaus\plexus\plexus-io\1.0-alpha-1\plexus-io-1.0-alpha-1.jar;D:\development\lib\jakarta-regexp\jakarta-regexp\1.4\jakarta-regexp-1.4.jar;D:\development\lib\org\apache\bcel\bcel\5.2\bcel-5.2.jar;D:\development\lib\jaxen\jaxen\1.1.1\jaxen-1.1.1.jar;D:\development\lib\dom4j\dom4j\1.6.1\dom4j-1.6.1.jar;D:\development\lib\jdom\jdom\1.0\jdom-1.0.jar;D:\development\lib\xerces\xercesImpl\2.9.1\xercesImpl-2.9.1.jar;D:\development\lib\xom\xom\1.0\xom-1.0.jar;D:\development\lib\xalan\xalan\2.6.0\xalan-2.6.0.jar;D:\development\lib\com\ibm\icu\icu4j\2.6.1\icu4j-2.6.1.jar;D:\development\lib\org\apache\maven\surefire\surefire-api\2.4.3\surefire-api-2.4.3.jar;D:\development\lib\org\apache\velocity\velocity\1.5\velocity-1.5.jar;D:\development\lib\commons-lang\commons-lang\2.5\commons-lang-2.5.jar;D:\development\lib\commons-httpclient\commons-httpclient\3.1\commons-httpclient-3.1.jar;D:\development\lib\commons-codec\commons-codec\1.3\commons-codec-1.3.jar;D:\development\lib\org\codehaus\plexus\plexus-component-api\1.0-alpha-15\plexus-component-api-1.0-alpha-15.jar;D:\development\lib\org\codehaus\plexus\plexus-classworlds\1.2-alpha-6\plexus-classworlds-1.2-alpha-6.jar;D:\development\lib\com\vaadin\vaadin-maven-plugin\7.0.1\vaadin-maven-plugin-7.0.1.jar;D:\development\lib\org\apache\maven\maven-project\2.0.9\maven-project-2.0.9.jar;D:\development\lib\org\apache\maven\maven-settings\2.0.10\maven-settings-2.0.10.jar;D:\development\lib\org\apache\maven\maven-profile\2.0.10\maven-profile-2.0.10.jar;D:\development\lib\org\apache\maven\maven-artifact-manager\2.0.10\maven-artifact-manager-2.0.10.jar;D:\development\lib\org\apache\maven\maven-repository-metadata\2.0.10\maven-repository-metadata-2.0.10.jar;D:\development\lib\org\apache\maven\wagon\wagon-provider-api\1.0-beta-2\wagon-provider-api-1.0-beta-2.jar;D:\development\lib\org\apache\maven\maven-plugin-registry\2.0.10\maven-plugin-registry-2.0.10.jar;D:\development\lib\org\apache\maven\maven-plugin-api\2.0.9\maven-plugin-api-2.0.9.jar;D:\development\lib\org\apache\maven\reporting\maven-reporting-impl\2.1\maven-reporting-impl-2.1.jar;D:\development\lib\org\apache\maven\doxia\doxia-core\1.1.2\doxia-core-1.1.2.jar;D:\development\lib\org\apache\maven\doxia\doxia-site-renderer\1.1.4\doxia-site-renderer-1.1.4.jar;D:\development\lib\org\apache\maven\doxia\doxia-decoration-model\1.1.4\doxia-decoration-model-1.1.4.jar;D:\development\lib\org\apache\maven\doxia\doxia-module-xhtml\1.1.4\doxia-module-xhtml-1.1.4.jar;D:\development\lib\org\apache\maven\doxia\doxia-module-fml\1.1.4\doxia-module-fml-1.1.4.jar;D:\development\lib\org\codehaus\plexus\plexus-i18n\1.0-beta-7\plexus-i18n-1.0-beta-7.jar;D:\development\lib\org\codehaus\plexus\plexus-velocity\1.1.7\plexus-velocity-1.1.7.jar;D:\development\lib\commons-validator\commons-validator\1.2.0\commons-validator-1.2.0.jar;D:\development\lib\org\springframework\spring-core\3.0.5.RELEASE\spring-core-3.0.5.RELEASE.jar;D:\development\lib\org\springframework\spring-asm\3.0.5.RELEASE\spring-asm-3.0.5.RELEASE.jar;D:\development\lib\com\thoughtworks\qdox\qdox\1.12\qdox-1.12.jar;D:\development\lib\freemarker\freemarker\2.3.6\freemarker-2.3.6.jar;D:\development\lib\velocity\velocity\1.4\velocity-1.4.jar;D:\development\lib\velocity\velocity-dep\1.4\velocity-dep-1.4.jar;D:\development\lib\org\mortbay\jetty\jetty-util\6.1.11\jetty-util-6.1.11.jar;D:\development\lib\org\mortbay\jetty\servlet-api-2.5\6.1.11\servlet-api-2.5-6.1.11.jar;D:\development\lib\org\mortbay\jetty\jetty\6.1.11\jetty-6.1.11.jar;D:\development\lib\org\apache\maven\surefire\surefire-booter\2.4.3\surefire-booter-2.4.3.jar;D:\development\lib\org\jdesktop\swing-worker\1.1\swing-worker-1.1.jar;D:\development\lib\org\jdom\jdom\1.1\jdom-1.1.jar;D:\development\lib\org\apache\james\apache-mime4j\0.6\apache-mime4j-0.6.jar;D:\development\lib\javax\validation\validation-api\1.0.0.GA\validation-api-1.0.0.GA-sources.jar;D:\development\lib\commons-io\commons-io\2.0.1\commons-io-2.0.1.jar;D:\development\lib\javax\validation\validation-api\1.0.0.GA\validation-api-1.0.0.GA.jar;D:\development\lib\ant\ant\1.6.5\ant-1.6.5.jar;D:\development\lib\ant\ant-launcher\1.6.5\ant-launcher-1.6.5.jar;D:\development\lib\com\vaadin\vaadin-client-compiler\7.0.1\vaadin-client-compiler-7.0.1.jar;D:\development\lib\xerces\xmlParserAPIs\2.6.2\xmlParserAPIs-2.6.2.jar;D:\development\lib\commons-digester\commons-digester\1.6\commons-digester-1.6.jar;D:\development\lib\commons-beanutils\commons-beanutils\1.7.0\commons-beanutils-1.7.0.jar;D:\development\lib\commons-logging\commons-logging\1.1.1\commons-logging-1.1.1.jar;D:\development\lib\commons-collections\commons-collections\3.2\commons-collections-3.2.jar;D:\development\lib\xml-apis\xml-apis\1.3.02\xml-apis-1.3.02.jar;D:\development\lib\oro\oro\2.0.8\oro-2.0.8.jar;D:\development\lib\org\codehaus\plexus\plexus-compiler-javac\1.5.3\plexus-compiler-javac-1.5.3.jar;D:\development\lib\org\codehaus\plexus\plexus-utils\2.0.5\plexus-utils-2.0.5.jar;D:\development\lib\org\codehaus\plexus\plexus-compiler-api\1.5.3\plexus-compiler-api-1.5.3.jar;D:\development\lib\org\codehaus\plexus\plexus-container-default\1.0-alpha-8\plexus-container-default-1.0-alpha-8.jar;D:\development\lib\junit\junit\4.8.1\junit-4.8.1.jar;D:\development\lib\org\apache\maven\doxia\doxia-logging-api\1.1.4\doxia-logging-api-1.1.4.jar;D:\development\lib\org\codehaus\plexus\plexus-interpolation\1.13\plexus-interpolation-1.13.jar;D:\development\lib\org\apache\maven\reporting\maven-reporting-api\3.0\maven-reporting-api-3.0.jar;D:\development\lib\org\apache\maven\doxia\doxia-sink-api\1.1.4\doxia-sink-api-1.1.4.jar;D:\development\lib\org\apache\maven\maven-archiver\2.4.1\maven-archiver-2.4.1.jar;D:\development\lib\org\apache\maven\maven-artifact\2.0.6\maven-artifact-2.0.6.jar;D:\development\lib\org\apache\maven\maven-model\2.0.6\maven-model-2.0.6.jar;D:\development\lib\org\codehaus\plexus\plexus-archiver\1.0-alpha-9\plexus-archiver-1.0-alpha-9.jar;D:\development\lib\org\codehaus\plexus\plexus-io\1.0-alpha-1\plexus-io-1.0-alpha-1.jar;D:\development\lib\jakarta-regexp\jakarta-regexp\1.4\jakarta-regexp-1.4.jar;D:\development\lib\org\apache\bcel\bcel\5.2\bcel-5.2.jar;D:\development\lib\jaxen\jaxen\1.1.1\jaxen-1.1.1.jar;D:\development\lib\dom4j\dom4j\1.6.1\dom4j-1.6.1.jar;D:\development\lib\jdom\jdom\1.0\jdom-1.0.jar;D:\development\lib\xerces\xercesImpl\2.9.1\xercesImpl-2.9.1.jar;D:\development\lib\xom\xom\1.0\xom-1.0.jar;D:\development\lib\xalan\xalan\2.6.0\xalan-2.6.0.jar;D:\development\lib\com\ibm\icu\icu4j\2.6.1\icu4j-2.6.1.jar;D:\development\lib\org\apache\maven\surefire\surefire-api\2.4.3\surefire-api-2.4.3.jar;D:\development\lib\net\sourceforge\cssparser\cssparser\0.9.5\cssparser-0.9.5.jar;D:\development\lib\org\w3c\css\sac\1.3\sac-1.3.jar;D:\development\lib\org\apache\velocity\velocity\1.5\velocity-1.5.jar;D:\development\lib\commons-lang\commons-lang\2.5\commons-lang-2.5.jar;D:\development\lib\commons-httpclient\commons-httpclient\3.1\commons-httpclient-3.1.jar;D:\development\lib\commons-codec\commons-codec\1.3\commons-codec-1.3.jar;D:\development\lib\org\codehaus\plexus\plexus-component-api\1.0-alpha-15\plexus-component-api-1.0-alpha-15.jar;D:\development\lib\org\codehaus\plexus\plexus-classworlds\1.2-alpha-6\plexus-classworlds-1.2-alpha-6.jar;D:\development\lib\com\vaadin\vaadin-maven-plugin\7.0.1\vaadin-maven-plugin-7.0.1.jar;D:\development\lib\org\apache\maven\maven-project\2.0.9\maven-project-2.0.9.jar;D:\development\lib\org\apache\maven\maven-settings\2.0.10\maven-settings-2.0.10.jar;D:\development\lib\org\apache\maven\maven-profile\2.0.10\maven-profile-2.0.10.jar;D:\development\lib\org\apache\maven\maven-artifact-manager\2.0.10\maven-artifact-manager-2.0.10.jar;D:\development\lib\org\apache\maven\maven-repository-metadata\2.0.10\maven-repository-metadata-2.0.10.jar;D:\development\lib\org\apache\maven\wagon\wagon-provider-api\1.0-beta-2\wagon-provider-api-1.0-beta-2.jar;D:\development\lib\org\apache\maven\maven-plugin-registry\2.0.10\maven-plugin-registry-2.0.10.jar;D:\development\lib\org\apache\maven\maven-plugin-api\2.0.9\maven-plugin-api-2.0.9.jar;D:\development\lib\org\apache\maven\reporting\maven-reporting-impl\2.1\maven-reporting-impl-2.1.jar;D:\development\lib\org\apache\maven\doxia\doxia-core\1.1.2\doxia-core-1.1.2.jar;D:\development\lib\org\apache\maven\doxia\doxia-site-renderer\1.1.4\doxia-site-renderer-1.1.4.jar;D:\development\lib\org\apache\maven\doxia\doxia-decoration-model\1.1.4\doxia-decoration-model-1.1.4.jar;D:\development\lib\org\apache\maven\doxia\doxia-module-xhtml\1.1.4\doxia-module-xhtml-1.1.4.jar;D:\development\lib\org\apache\maven\doxia\doxia-module-fml\1.1.4\doxia-module-fml-1.1.4.jar;D:\development\lib\org\codehaus\plexus\plexus-i18n\1.0-beta-7\plexus-i18n-1.0-beta-7.jar;D:\development\lib\org\codehaus\plexus\plexus-velocity\1.1.7\plexus-velocity-1.1.7.jar;D:\development\lib\commons-validator\commons-validator\1.2.0\commons-validator-1.2.0.jar;D:\development\lib\org\springframework\spring-core\3.0.5.RELEASE\spring-core-3.0.5.RELEASE.jar;D:\development\lib\org\springframework\spring-asm\3.0.5.RELEASE\spring-asm-3.0.5.RELEASE.jar;D:\development\lib\com\thoughtworks\qdox\qdox\1.12\qdox-1.12.jar;D:\development\lib\freemarker\freemarker\2.3.6\freemarker-2.3.6.jar;D:\development\lib\com\vaadin\vaadin-client\7.0.1\vaadin-client-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-shared\7.0.1\vaadin-shared-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-shared-deps\1.0.2\vaadin-shared-deps-1.0.2.jar;D:\development\lib\com\vaadin\vaadin-server\7.0.1\vaadin-server-7.0.1.jar;D:\development\lib\com\vaadin\vaadin-theme-compiler\7.0.1\vaadin-theme-compiler-7.0.1.jar;D:\development\lib\org\apache\commons\commons-jexl\2.1.1\commons-jexl-2.1.1.jar;D:\development\lib\commons-cli\commons-cli\1.2\commons-cli-1.2.jar;D:\development\lib\org\jsoup\jsoup\1.6.3\jsoup-1.6.3.jar;D:\development\lib\javax\validation\validation-api\1.0.0.GA\validation-api-1.0.0.GA.jar;D:\development\lib\javax\validation\validation-api\1.0.0.GA\validation-api-1.0.0.GA-sources.jar;D:\development\lib\velocity\velocity\1.4\velocity-1.4.jar;D:\development\lib\velocity\velocity-dep\1.4\velocity-dep-1.4.jar;D:\development\lib\org\mortbay\jetty\jetty-util\6.1.11\jetty-util-6.1.11.jar;D:\development\lib\org\mortbay\jetty\servlet-api-2.5\6.1.11\servlet-api-2.5-6.1.11.jar;D:\development\lib\org\mortbay\jetty\jetty\6.1.11\jetty-6.1.11.jar;D:\development\lib\org\apache\maven\surefire\surefire-booter\2.4.3\surefire-booter-2.4.3.jar;D:\development\lib\org\jdesktop\swing-worker\1.1\swing-worker-1.1.jar;D:\development\lib\org\jdom\jdom\1.1\jdom-1.1.jar;D:\development\lib\org\apache\james\apache-mime4j\0.6\apache-mime4j-0.6.jar;D:\development\lib\commons-io\commons-io\2.0.1\commons-io-2.0.1.jar;D:\development\lib\ant\ant\1.6.5\ant-1.6.5.jar;D:\development\lib\ant\ant-launcher\1.6.5\ant-launcher-1.6.5.jar com.google.gwt.dev.Compiler -logLevel INFO -style OBF -war D:\development\workspace\Test\src\main\webapp\VAADIN\widgetsets -localWorkers 4 -strict -XfragmentCount -1 -extra D:\development\workspace\Test\target\extra -compileReport -gen D:\development\workspace\Test\target\.generated cn.nh121.AppWidgetSet
[ERROR] ]] failed with status 1
[ERROR] -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
```

好吧，输出信息非常长，从中我留意到程序抛出的一个异常：`java.lang.RuntimeException: Unable to write to byte cache`，一番Google之后也找不到解决方法。这就奇怪了，今天白天我在公司明明还能编译成功的，怎么回到家就不行了呢？最后细想了一下公司机器和家里机器的区别，开始怀疑是临时空间的问题。
家里的机器装了一个叫`Vsuite Ramdisk`的软件，将4G内存里的600M划出来作为Ramdisk，然后把环境变量中的`TEMP`和`TMP`变量都指向这个Ramdisk盘，后来在编译的时候观察这个盘，果然，**容量降到接近没有的时候就编译中断了**。看来问题所在就是临时空间不够，于是赶紧把环境变量修改为原来的位置，再编译，Done！

### 总结
问题虽然不大，但也卡了我接近一晚的时间，看来有时候有些设置也不是随便可以乱改啊。
