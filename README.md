# Object Oriented Programming 大作业

#### Description
ucas2023面向对象程序设计课程大作业项目--**简易记事本**

#### Author
黄钰乔

#### Software Architecture
```
|-- undefined
    │  .gitignore
    │  pom.xml
    │  TimingFramework-0.55.jar
    │  tree.txt
    │  
    ├─.idea
    │  │  .gitignore
    │  │  compiler.xml
    │  │  encodings.xml
    │  │  jarRepositories.xml
    │  │  misc.xml
    │  │  vcs.xml
    │  │  workspace.xml
    │  │  
    │  └─libraries
    │          lib.xml
    │          
    ├─demo
    │  │  mvnw
    │  │  mvnw.cmd
    │  │  pom.xml
    │  │  
    │  ├─.mvn
    │  │  └─wrapper
    │  │          maven-wrapper.jar
    │  │          maven-wrapper.properties
    │  │          
    │  └─src
    │      └─main
    │          ├─java
    │          │  │  module-info.java
    │          │  │  
    │          │  └─com
    │          │      └─example
    │          │          └─demo
    │          │                  HelloApplication.java
    │          │                  HelloController.java
    │          │                  
    │          └─resources
    │              └─com
    │                  └─example
    │                      └─demo
    │                              hello-view.fxml
    │                              
    ├─pic
    │      markdownicon.png
    │      
    ├─src
    │  ├─main
    │  │  ├─java
    │  │  │  │  Function_Color.java
    │  │  │  │  Function_Edit.java
    │  │  │  │  Function_File.java
    │  │  │  │  Function_Format.java
    │  │  │  │  Function_Function.java
    │  │  │  │  Function_Insertion.java
    │  │  │  │  GUI.java
    │  │  │  │  KeyHandler.java
    │  │  │  │  
    │  │  │  ├─cloud
    │  │  │  │      AliyunOSSFileUploaderDownloader.java
    │  │  │  │      CloudStorageUIExample.java
    │  │  │  │      
    │  │  │  ├─insertpicture
    │  │  │  │      ResizableImageComponent.java
    │  │  │  │      
    │  │  │  ├─inserttable
    │  │  │  │      CustomCellRenderer.java
    │  │  │  │      JTextPaneWithTable.java
    │  │  │  │      
    │  │  │  ├─insertvideo
    │  │  │  │      VideoPlayerExample.java
    │  │  │  │      
    │  │  │  ├─markdownparser
    │  │  │  │      FlexmarkMarkdownEditor.java
    │  │  │  │      MarkdownEditor.java
    │  │  │  │      
    │  │  │  ├─org
    │  │  │  │  └─example
    │  │  │  │          Main.java
    │  │  │  │          
    │  │  │  ├─scalableimg
    │  │  │  │      JTextPaneWithScalableImage.java
    │  │  │  │      ScalableImagePanel.java
    │  │  │  │      
    │  │  │  └─scroll
    │  │  │          PolygonCorner.java
    │  │  │          ScrollBarWin11UI.java
    │  │  │          ScrollPaneWin11.java
    │  │  │          Test2.java
    │  │  │          
    │  │  └─resources
    │  │      └─org
    │  │          └─example
    │  │                  application.css
    │  │                  
    │  └─test
    │      └─java
    │          └─org
    │              └─example
    │                      AppTest.java
    │                      
    ├─target
    │  ├─classes
    │  │  │  Function_Color.class
    │  │  │  Function_Edit.class
    │  │  │  Function_File.class
    │  │  │  Function_Format$1$1$1.class
    │  │  │  Function_Format$1$1$2.class
    │  │  │  Function_Format$1$1.class
    │  │  │  Function_Format$1.class
    │  │  │  Function_Format.class
    │  │  │  Function_Function.class
    │  │  │  Function_Insertion.class
    │  │  │  GUI$1.class
    │  │  │  GUI.class
    │  │  │  KeyHandler.class
    │  │  │  
    │  │  ├─cloud
    │  │  │      AliyunOSSFileUploaderDownloader$1.class
    │  │  │      AliyunOSSFileUploaderDownloader$2.class
    │  │  │      AliyunOSSFileUploaderDownloader$CustomScrollBarUI$1.class
    │  │  │      AliyunOSSFileUploaderDownloader$CustomScrollBarUI$2.class
    │  │  │      AliyunOSSFileUploaderDownloader$CustomScrollBarUI.class
    │  │  │      AliyunOSSFileUploaderDownloader.class
    │  │  │      CloudStorageUIExample$1.class
    │  │  │      CloudStorageUIExample.class
    │  │  │      
    │  │  ├─insertpicture
    │  │  │      ResizableImageComponent$1.class
    │  │  │      ResizableImageComponent.class
    │  │  │      
    │  │  ├─inserttable
    │  │  │      CustomCellRenderer.class
    │  │  │      JTextPaneWithTable$1.class
    │  │  │      JTextPaneWithTable$MultiLineTableCellRenderer.class
    │  │  │      JTextPaneWithTable.class
    │  │  │      
    │  │  ├─markdownparser
    │  │  │      FlexmarkMarkdownEditor$1.class
    │  │  │      FlexmarkMarkdownEditor$2.class
    │  │  │      FlexmarkMarkdownEditor$3.class
    │  │  │      FlexmarkMarkdownEditor$4.class
    │  │  │      FlexmarkMarkdownEditor$5.class
    │  │  │      FlexmarkMarkdownEditor$6.class
    │  │  │      FlexmarkMarkdownEditor$7.class
    │  │  │      FlexmarkMarkdownEditor$CustomScrollBarUI$1.class
    │  │  │      FlexmarkMarkdownEditor$CustomScrollBarUI$2.class
    │  │  │      FlexmarkMarkdownEditor$CustomScrollBarUI.class
    │  │  │      FlexmarkMarkdownEditor.class
    │  │  │      MarkdownEditor$1.class
    │  │  │      MarkdownEditor$2.class
    │  │  │      MarkdownEditor$3.class
    │  │  │      MarkdownEditor.class
    │  │  │      
    │  │  ├─org
    │  │  │  └─example
    │  │  │          application.css
    │  │  │          Main.class
    │  │  │          
    │  │  ├─scalableimg
    │  │  │      JTextPaneWithScalableImage$1.class
    │  │  │      JTextPaneWithScalableImage.class
    │  │  │      ScalableImagePanel$1.class
    │  │  │      ScalableImagePanel$2.class
    │  │  │      ScalableImagePanel.class
    │  │  │      
    │  │  └─scroll
    │  │          PolygonCorner.class
    │  │          ScrollBarWin11UI$1.class
    │  │          ScrollBarWin11UI$2.class
    │  │          ScrollBarWin11UI$ScrollButton$1.class
    │  │          ScrollBarWin11UI$ScrollButton.class
    │  │          ScrollBarWin11UI.class
    │  │          ScrollPaneWin11$ScrollLayout.class
    │  │          ScrollPaneWin11.class
    │  │          Test2$1.class
    │  │          Test2.class
    │  │          
    │  ├─generated-sources
    │  │  └─annotations
    │  ├─generated-test-sources
    │  │  └─test-annotations
    │  └─test-classes
    │      └─org
    │          └─example
    │                  AppTest.class
    │                  
    └─uml
        │  clouduml.puml
        │  coloruml.puml
        │  edituml.puml
        │  fileuml.puml
        │  formatuml.puml
        │  functionuml.puml
        │  guiuml.puml
        │  insertionuml.puml
        │  keyuml.puml
        │  mdparderuml.puml
        │  pictureuml.puml
        │  scrolluml.puml
        │  tableuml.puml
        │  
        └─umlpic
                clouduml.png
                coloruml.png
                edituml.png
                fileuml.png
                formatuml.png
                functionuml.png
                guiuml.png
                insertionuml.png
                keyuml.png
                mdparderuml.png
                pictureuml.png
                scrolluml.png
                tableuml.png
```

#### Installation

1. 安装java，配置相应环境
2. 配置maven环境，方便载入依赖包或库。为了实现更好的UI界面与实现markdown功能，目前需依赖javafx与flexmark
3. External Library导入
    1. 为了实现一定的动画效果，本次项目还需要导入与动画相关的TimingFramework-0.55.jar文件。该文件在SimpleNotepad文件夹里，可以直接通过ij的settings-module导入。

#### Instructions

目前最新项目利用maven创建的MyNotepad，main函数在class GUI内，运行项目需要选择src里的GUI.java运行。

#### Contribution

1.  Fork the repository
2.  Create Feat_xxx branch
3.  Commit your code
4.  Create Pull Request


#### Gitee Feature

1.  最新项目地址：[MyNotepad](https://gitee.com/iris10026/object-oriented-programming/tree/master/ProjectWork/MyNotepad)
2.  旧版项目（非maven的简易版）地址：[SimpleNotepad](https://gitee.com/iris10026/object-oriented-programming/tree/master/ProjectWork/SimpleNotepad)
3.  Gitee blog [blog.gitee.com](https://blog.gitee.com)