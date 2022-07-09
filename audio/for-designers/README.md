---
description: 目錄
---

# 目錄

#### Audio Plugin Development— Audio/DSP 插件開發|音樂科技網

[英文版](https://medium.com/@jeffreywang1183/audio-plugin-development-audio-dsp-music-tech-alliance-c6bdc5fec62)

[JUCE](https://en.wikipedia.org/wiki/JUCE) 是部分為 open-source 的 C++ framework，2014年被 London 的 hardware manufacturer [ROLI](https://en.wikipedia.org/wiki/ROLI) 收購。相較於其他 framework 強項在於 audio functionality

#### JUCE

[Projucer](https://juce.com/discover/projucer) 是他的程式編輯器，先看完[最初的教學](https://docs.juce.com/master/tutorial\_new\_projucer\_project.html)(下面左圖)，

Learn how to configure your build settings, create new export targets and configurations, and use the Projucer’s C++ code editor.

![](https://cdn-images-1.medium.com/max/1600/1\*0H1qmzTEk\_egsuo28zVOOg.png)

\


一開始不知道要選什麼就選 GUI application

\


#### 現在看到[教學2](https://docs.juce.com/master/tutorial\_manage\_projucer\_project.html) 的 Managing JUCE modules

不要從 Xcode, Visual Studio 調整 source files 的檔名或刪減檔案，請在 Projucer 做調整，因為在 Projucer 存擋時都會重新生成 native IDE 的專案。

教學2說明了 projects 內的檔案組成，

我看到 Managing your project

\


\


Audio Plug-In 這個，可以看教學

See [**Tutorial: Create a basic Audio/MIDI plugin, Part 1: Setting up**](https://docs.juce.com/master/tutorial\_create\_projucer\_basic\_plugin.html) 有一系列的教學

\


\


[跟 Android 的結合](https://docs.juce.com/master/tutorial\_android\_studio.html)

An introduction to the use of Android Studio for creating JUCE-based projects.

[實作白噪音](https://docs.juce.com/master/tutorial\_simple\_synth\_noise.html)

Create a basic audio app that outputs some sound. In this example, we will build a simple white noise generator.

\


[tutorial](https://juce.com/learn/tutorials)

[第三方學習資源](https://juce.com/discover/made-with-juce)

[免費下載](https://shop.juce.com/get-juce)

juce-5.3.2-osx

[完整的 document](https://juce.com/learn/documentation)

有很棒的論壇，[初學者標籤](https://forum.juce.com/c/getting-started)

[Roli](https://roli.com/) 是 JUCE 的開發商，看來還有做硬體

\


#### Wwsie

Wwise 是遊戲音效

[Audiokinet](https://www.audiokinetic.com/) 是母公司

[音頻人脈網](https://www.audiokinetic.com/directory/?list=providers)這奇妙的東西

\


像我買了一本 C++ 的書，除非一頁一頁啃書外，更有效率也更有動力的作法就是先想清楚，你今天學這個東西的目的是什麼？你想要解決什麼問題？

對我來說，因為我知道 C++ 與 Matlab 是許多音訊相關的職位，例如 audio engineering 會用到的技能，所以我想學。但在看了 Coursera 的課程或是 C++ 的教材中，往往就是理論與程式的寫法。而我的出發點則是我想做什麼專案？而專案會用到這個技術。確定專案後，我從網路上的教材，先自己列出我缺少的知識。另外再詢問相關技術人員，確認我需要學習的方向。

三家公司 ([U-He Zebra](http://www.u-he.com/cms/zebra), [Sonalksis](https://www.sonalksis.com/index.html) and [D16 Decimort](http://www.d16.pl/index.php?menu=203))

\


#### 資源

* [DSP Guide](http://www.dspguide.com/pdfbook.htm): Very good free book, covers probably more than we’ll use. Refer to it if somewhere on the way you don’t understand a DSP concept.
* [WDL-OL](https://github.com/olilarkin/wdl-ol) library. It is based on [Cockos WDL](http://www.cockos.com/wdl/)

#### &#x20;

\


#### &#x20;

\


#### Why Use Object-Oriented Design

先看這篇：[https://www.mathworks.com/help/matlab/matlab\_oop/why-use-object-oriented-design.html](https://www.mathworks.com/help/matlab/matlab\_oop/why-use-object-oriented-design.html)

\


#### Matlab 方面：Design an Audio Plugin

[https://www.mathworks.com/help/audio/gs/design-an-audio-plugin.html](https://www.mathworks.com/help/audio/gs/design-an-audio-plugin.html)

Matlab 提供的基本知識：[什麼是 DAW, MIDI, audio plugin](http://What%20Are%20DAWs,%20Audio%20Plugins,%20and%20MIDI%20Controllers?)

MATLAB Online

[https://cn.mathworks.com/products/matlab-online.html](https://cn.mathworks.com/products/matlab-online.html)

\


\


\


#### 先前[看到第二篇](http://www.martin-finke.de/blog/articles/audio-plugins-002-setting-up-wdl-ol/)

這門課會從 distortion plugin 開始

我的預備知識，可是我都沒有 XD

Some understanding of C++ (Syntax, Pointers, Basic OOP, Memory).

音樂的預備知識我有，其他的就靠這個專案補

\


步驟1：設定 Xcode，請先[下載並安裝 Xcode](https://itunes.apple.com/tw/app/xcode/id497799835?mt=12)

步驟2：然後在 [steinberg](https://www.steinberg.net/en/company/developers.html) 下載了 [VST 3 Audio Plug-Ins SDK](https://www.steinberg.net/vst3sdk)，解壓縮後出現 VST\_SDK 資料夾，下載 [RtAudio](http://www.music.mcgill.ca/\~gary/rtaudio/)&#x20;

步驟3：git clone 了 [WDL-OL library ](https://github.com/olilarkin/wdl-ol)，就會有一個 wdl-ol

步驟4：加入 Dependencies，將步驟2下載的檔案都解壓縮

_在 pluginterface資料夾的 saeffect.h_ 和_aeffectx.h_ 檔案拖到 WDL-OL 的 _VST\_SDK 資料夾裡_

_在_VST3 資料夾的 _base/source_, _pluginterfaces_ and _public.sdk/source_ 檔案要拉進 WDL-OL 的 _VST3\_SDK 資料夾裡_

在 RtAudio 的 _include 資料夾有_ .cpp/.h 檔案 要放到 WDL-OL 的 _ASIO\_SDK 資料夾內_

接著打開 _common.xcconfig_ 的檔案

接著到這一段 Running the Example Project

我執行了下面一段 command 後出現了 _MyFirstPlugin_

```
./duplicate.py IPlugEffect/ MyFirstPlugin YourName
```

接著來處理 WDL-OL 的 bug，在左側找到 app\_main.h 的程式，修改 34行，去做預設輸入音訊的裝置為內建麥克風。

```
#define DEFAULT_INPUT_DEV "Built-in Microphone"
```

![](https://cdn-images-1.medium.com/max/1600/1\*bKaD7Qe3m6pIljJcJkf0QQ.png)

按下 run 之後其實沒看到部落格上說的 linker errors，主要是遇到另一個問題，看起來就是找不到下面這個檔案

The file “CommonDebugSettings.xcconfig” couldn’t be opened because there is no such file. (/Users/junyuanwang/plugin-development/wdl-ol/AAX\_SDK/ExamplePlugIns/Common/Mac/CommonDebugSettings.xcconfig)

從文件中的[建議](https://github.com/olilarkin/wdl-ol/tree/master/AAX\_SDK)，要按照下面步驟調參數 MacOSX10.6.sdk

```
- this assumes that you have installed the MacOSX10.6.sdk in /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/  - set SDKROOT to macosx10.6 for debug and release targets of AAXLibrary static lib   - modify ExamplePlugIns/Common/Mac/CommonDebugSettings.xcconfig and ExamplePlugIns/Common/Mac/CommonReleaseSettings.xcconfig...  GCC_VERSION = com.apple.compilers.llvm.clang  SDKROOT = macosx10.6  MACOSX_DEPLOYMENT_TARGET = 10.5  ARCHS = x86_64 i386
```



You should never add, rename, and/or remove source files from JUCE projects inside your native IDE (such as Xcode, Visual Studio). These changes would be overwritten the next time you save the project in Projucer (which re-generates the native IDE projects every time). Instead, always use the Projucer itself to make such changes.\
