---
title: think of testing
date: 2022-05-28 19:52:14
tags: [lambdaTest, testing, automation, manual, real device, selenium]
categories: testing
---

# 测试(testing)

## 跨终端自动化测试

  在介绍自动化测试之前,我们先来说一下跨终端测试工具,基于全方位测试需求的考虑,跨终端测试应该是最重要的类型之一.如今,各种类型的浏览器、操作系统、品牌手机以及设备可谓是琳琅满目.因此,需要确保用户在通过不同种类的浏览器、操作系统、品牌手机以及设备访问平台服务时,不会产生较大的体验落差.
  
  在市面上,诸如LambdaTest之类的在线工具,就能够帮助以一种轻松互动的方式,解决此方面的问题.LambdaTest是一种非常流行的在线工具,可以通过它对超过3000多个真正的浏览器、操作系统、品牌手机以及设备进行跨终端式的测试.测试人员甚至可以使用该工具来自动捕捉屏幕上的截图,以加速对于目标平台网络布局的测试.另外,其他同类型比较流行的测试工具还有:Browserstack和Saucelabs.

### lambdaTest

  lambdaTest能够为3000多种浏览器、不同的Web应用操作系统、品牌手机以及设备的组合提供支持,可以在线执行手动测试(Manual testing)、自动化测试(Automatic testing)以及真机测试Real Device testing).

> 手动测试(Manual testing)

  - Browser Testing.

    可在线在不同的浏览器种类、版本、操作系统以及分辨率对线上网站进行实时交互式测试,每次真实浏览器sessions可测试10分钟,在测试期间可切换配置浏览器种类、版本、操作系统以及分辨率、可录屏(录屏可下载)、可标记bug(编辑bug截图、下载bug截图以及上传到lambdaTest衍生的SLACK、JIRA和ASANA项目管理系统).

    - 配置.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/browser/Manual_home.png)

    - 测试.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/browser/Manual_testing.png)

    - Debug.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/browser/Manual_debug.png)

    - 录屏.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/browser/Manual_video.png)

  - App Testing.

    可在线自由搭配不同的品牌手机、版本设备对App包或者App Url进行实时交互式跨真机测试,每次真实真机测试sessions可测试10分钟,在测试期间可切换配置品牌手机以及版本设备、可录屏(录屏可下载)、可标记bug(编辑bug截图、下载bug截图以及上传到lambdaTest衍生的SLACK、JIRA和ASANA项目管理系统).

    - 配置.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/app/Manual_home.png)

    - 测试.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/app/Manual_testing.png)

    - Debug.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/app/Manual_debug.png)

    - 录屏.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/app/Manual_video.png)

  - Browser Testing开发环境在线测试.

    可配合lambdaTest桌面应用(LT_Windows or LT_Macs)配合在线网站进行开发环境在线测试.

    - 配置.

      <a href='https://www.lambdatest.com/support/docs/testing-locally-hosted-pages/'>Browser Testing开发环境在线测试</a>. PS:注意看视频中的介绍步骤.

  - 结论.

    lambdaTest手动测试简洁、易用且容易理解,测试过程流畅、无障碍性困难,其产品包含其衍生的生态是成熟且完备的,例如其衍生的一套项目管理系统SLACK、JIRA和ASANA.对于测试人员来说,无论是生产环境、开发环境和App包,如果尝试自己查看每个浏览器、操作系统、品牌手机以及设备,一定是成为一项繁琐的工作,但lambdaTest都可在线简捷做各种搭配兼容测试,提高了效率,大大节约了时间成本.

  - 问题.

    现阶段lambdaTest上手动测试中大部分浏览器版本、操作系统、品牌手机以及版本设备都是不开放的,开放的很小的部分仅仅也是为了给用户进行体验的,如果需要开放就需要付费,另外支出成本.

> 自动化测试(Automatic testing)

  众所周知,软件测试人员平时的工作量既多且复杂.因此,为了给他们减负,以及加快测试周期,各种高效率的自动化测试工具往往是必须的.

  - 浏览器自动化操作标准-WebDriver.

    WebDriver是W3C的一个标准,是一个远程控制协议,它提供了跨平台和跨语言的方式来远程操控浏览器,它提供了一系列接口来访问和操作DOM,进而控制浏览器的行为.它使得web开发者能写一些自动化脚本来测试网页.后续的Selenium、Appium都是基于WebDriver协议并进行了扩展.

    - WebDriver的工作过程.

      浏览器在启动后会在某一个端口启动基于WebDriver协议的Web Service,接下来我们调用WebDriver的任何api时,都需要借助一个CommandExecutor发送一个命令(也就是给监听端口上的Web Service发送一个http请求),这个命令会告诉浏览器接下来要做什么.

      ![](https://image.white-than-wood.zone/lambdaTest/Automatic/Automatic_webdriver.png)

  - 浏览器自动化测试工具.

    - Selenium.

      Selenium是浏览器自动化测试工具领域最为流行的一种测试套件,根据《针对自动化测试各种挑战的调查》一文,九成的测试人员已经或正在使用着Selenium;

      - Selenium支持多浏览器平台(Chrome、Firefox、IE、Opera、Safari等);
      - Selenium支持多语言(python、java、ruby、js、c#等);
      - Selenium的Remote Control可以通过录制用户的操作,来简化Web测试人员的各项重复作业;
      - Selenium的Grid具有编写、运行和并行处理测试的功能;
      - Selenium的Core则是基于JsUnit,完全由JavaScript所编写,因此可以被运行在各种支持JavaScript的主流浏览器之上;
      - Selenium开源免费;

    - Selenium IDE.

      Selenium IDE能够以插件的形式被安装到测试者的浏览器中,从而方便地实现Web界面的测试,lambdaTest在浏览器自动化测试部分也极力推荐Selenium.
  
    - lambdaTest配合Selenium IDE.

      - 配置.

        使用Selenium IDE以及配合lambdaTest的配置还是比较简单的,官方有具体的一步一步实现的文章: <a href='https://www.lambdatest.com/support/docs/run-selenium-ide-tests-on-lambdatest-selenium-cloud-grid/'>Run Selenium IDE Tests with LambdaTest Selenium Grid</a>.

      - selenium-side-runner命令行配合lambdaTest.

        使用selenium-side-runner依赖包在命令行中运行导出的Selenium IDE测试套件,在lambdaTest在线平台上搭配不同的浏览器版本、浏览器类型、操作系统、品牌手机以及版本设备进行生成自动化测试录屏.

        - 可根据selenium-webdriver事件步骤分帧在录屏中在线查看测试套件自动化测试的情况;
        - 可在线查看在自动化测试过程当中网络资源接口的加载情况;
        - 可下载自动化测试的录屏;
        - 可将标记bug(将有问题的selenium事件步骤分帧上传到lambdaTest衍生的SLACK、JIRA和ASANA项目管理系统);

      - 生成.

        ![](https://image.white-than-wood.zone/lambdaTest/Automatic/Automatic_generate.png)

        运行命令:

            npm run test

        生成自动化测试录屏列表.

        ![](https://image.white-than-wood.zone/lambdaTest/Automatic/Automatic_view.png)

        查看自动化测试录屏详情.

        ![](https://image.white-than-wood.zone/lambdaTest/Automatic/Automatic_detail.png)

      - 下载.

        ![](https://image.white-than-wood.zone/lambdaTest/Automatic/Automatic_video.png)

      - Debug.

        ![](https://image.white-than-wood.zone/lambdaTest/Automatic/Automatic_debug.png)

      - 网络资源接口加载情况.

        ![](https://image.white-than-wood.zone/lambdaTest/Automatic/Automatic_network.png)

      - 结论.

        Selenium IDE确实是测试工具领域最为流行的一种可视化、自动化测试工具,作为Chrome extensions,无需编辑一行代码就可实现可视化、自动化测试,并且效果顶级.而lambdaTest在线自动化测试更是与Selenium配合的相得益彰,在不同的浏览器版本、浏览器类型、操作系统、品牌手机以及版本设备进行生成自动化测试录屏都是简洁、易用且容易理解,只能说如果作为一个测试,现在可以说"大大真的解放了头脑和双手".
      
      - 问题
        
        其可视化测试,有时运行时会出现莫名问题,比如在寻找一些web页面上的DOM节点时查询不到,需要手动修改或者删除selenium-webdriver事件步骤,其定制性、精确性着实不如使用Jest配合selenium-webdriver编辑测试用例.
    
    - selenium-webdriver

      这是一个浏览器自动化库,它提供了许多浏览器自动化接口,用于测试web应用.除了通过npm安装selenium-webdriver之外,还需要安装浏览器相应的驱动.其相应的api和用法<a href='https://www.selenium.dev/selenium/docs/api/javascript/'>selenium-webdriver</a>.

      在new一个WebDriver的过程中,selenium首先会确认浏览器的native component是否存在可用而且匹配的版本,然后就在目标浏览器里启动一整套Web Service,这套Web Service使用了selenium自己设计定义的协议,名字叫做The WebDriver Wire Protocol.这套协议非常之强大,几乎可以操作浏览器做任何事情,包括打开、关闭、最大化、最小化、元素定位、元素点击、上传文件等等.其配合Jest写测试用例再搭配lambdaTest,可自定义各种测试套件在不同的浏览器版本以及操作系统上实现定制且容易理解的自动化测试.
    
  - App自动化测试工具.
    
    - Appium.

      Appium是一个开源的,适用于原生或混合移动应用(hybrid mobile apps)的自动化测试工具,Appium应用WebDriver:JSON wire protocol驱动安卓和iOS移动应用,也是App自动化测试工具领域最为流行的一种测试套件.
    
      - Appium支持多App平台(Android、iOS等);
      - Appium支持多语言(python、java、ruby、js、c#等),Appium选择了Client/Server的设计模式,只要client能够发送http请求给server,那么client用什么语言来实现都是可以的,这就是如何做到支持多语言的原因;
      - Appium是跨平台的,可以用在OSX，Windows以及Linux桌面系统上运行;
      - Appium扩展了WebDriver的协议,这样的好处是以前的WebDriver API能够直接被继承过来,以前的WebDriver各种语言的binding都可以拿来就用,省去了为浏览器、App端各开发一个client的工作量;
      - Appium开源免费;
    
    - lambdaTest配合Appium.

      - 配置.

        使用JS WebDriverIO With Appium以及配合lambdaTest的配置是有点小复杂的,官方有具体的一步一步实现的文章: <a href='https://www.lambdatest.com/support/docs/appium-nodejs-webdriverio/'>WebDriverIO With Appium</a>.