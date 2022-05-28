---
title: think of testing
date: 2022-05-28 19:52:14
tags: [lambdaTest, testing, automation, manual, real device, selenium]
categories: testing
---

# 测试(testing)

## 跨浏览器自动化测试

  在介绍自动化测试之前,我们先来说一下跨浏览器测试工具,基于全方位测试需求的考虑,跨浏览器测试应该是最重要的类型之一.如今,各种类型的浏览器可谓是琳琅满目.因此,需要确保用户在通过不同种类的浏览器访问平台服务时,不会产生较大的体验落差.
  
  在市面上,诸如LambdaTest之类的在线工具,就能够帮助以一种轻松互动的方式,解决此方面的问题.LambdaTest是一种非常流行的在线工具,可以通过它对超过3000多个真正的浏览器、与操作系统进行跨浏览器式的测试.测试人员甚至可以使用该工具来自动捕捉屏幕上的截图,以加速对于目标平台网络布局的测试.另外,其他同类型比较流行的测试工具还有:Browserstack和Saucelabs.

### lambdaTest

  lambdaTest能够为3000多种浏览器和不同的Web应用操作系统的组合提供支持,可以在线执行手动测试(Manual testing)、自动化测试(Automatic testing)以及真机测试Real Device testing).

> 手动测试(Manual testing)

  - Browser Testing.

    可在线自由搭配不同的浏览器种类、版本、操作系统以及分辨率对线上网站进行实时交互式跨浏览器测试,每次真实浏览器sessions可测试10分钟,在测试期间可切换之前自由搭配的所有配置选项、可录屏、可标记bug(编辑bug截图、下载bug截图以及上传到lambdaTest衍生的SLACK、JIRA和ASANA项目管理系统).

    - 配置.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/browser/Manual_home.png)

    - 测试.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/browser/Manual_testing.png)

    - Debug.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/browser/Manual_debug.png)

    - 录屏.

      ![](https://image.white-than-wood.zone/lambdaTest/Manual/browser/Manual_video.png)

  - App Testing.

    可在线自由搭配不同的品牌手机、版本设备对App包或者App Url进行实时交互式跨真机测试,每次真实真机测试sessions可测试10分钟,在测试期间可切换之前自由搭配的品牌手机以及版本设备、可录屏、可标记bug(编辑bug截图、下载bug截图以及上传到lambdaTest衍生的SLACK、JIRA和ASANA项目管理系统).

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

    lambdaTest手动测试简洁、易用且容易理解,测试过程流畅、无障碍性困难,其产品包含其衍生的生态是成熟且完备的,例如其衍生的一套项目管理系统SLACK、JIRA和ASANA.对于测试人员来说,无论是生产环境、开发环境和App包,如果尝试自己查看每个浏览器、操作系统和设备,一定是成为一项繁琐的工作,但lambdaTest都可在线简捷做各种搭配兼容测试,提高了效率,大大节约了时间成本.

    - 问题.

      现阶段lambdaTest上手动测试中大部分浏览器版本、操作系统、品牌手机以及版本设备都是不开放的,开放的很小的部分仅仅也是为了给用户进行体验的,如果需要开放就需要付费,另外支出成本.

> 自动化测试(Automatic testing)

- 自动化测试工具.

  众所周知,软件测试人员平时的工作量既多且复杂.因此,为了给他们减负,以及加快测试周期,各种高效率的自动化测试工具往往是必须的.

  - Selenium.

    Selenium是自动化测试工具领域最为流行的一种测试套件.Selenium的IDE能够以插件的形式被安装到测试者的浏览器中,从而方便地实现Web界面的测试,lambdaTest在自动化测试部分也极力推荐Selenium.

    - Selenium的Remote Control可以通过录制用户的操作,来简化Web测试人员的各项重复作业.Selenium的Grid具有编写、运行和并行处理测试的功能.
    - Selenium的Core则是基于JsUnit,完全由JavaScript所编写,因此可以被运行在各种支持JavaScript的主流浏览器之上.根据《针对自动化测试各种挑战的调查》一文,九成的测试人员已经或正在使用着Selenium.

- lambdaTest配合Selenium.

  - 配置.

    使用Selenium以及配合lambdaTest的配置还是比较简单的,官方有具体的一步一步实现的文章: <a href='https://www.lambdatest.com/support/docs/run-selenium-ide-tests-on-lambdatest-selenium-cloud-grid/'>Run Selenium IDE Tests with LambdaTest Selenium Grid</a>

  - selenium-side-runner命令行配合lambdaTest。

    使用selenium-side-runner依赖包在命令行中运行导出的Selenium IDE测试套件,在lambdaTest在线平台上搭配不同的浏览器版本、浏览器类型、操作系统、品牌手机以及版本设备进行生成自动化测试录屏.

    - 可根据selenium事件步骤分帧在录屏中在线查看测试套件自动化测试的情况;
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

    Selenium确实是自动化测试工具领域最为流行的一种测试套件,作为Chrome extensions,无需编辑一行代码就可实现可视化、自动化测试,并且效果顶级.而lambdaTest在线自动化测试更是与Selenium配合的相得益彰,在不同的浏览器版本、浏览器类型、操作系统、品牌手机以及版本设备进行生成自动化测试录屏都是简洁、易用且容易理解,只能说如果作为一个测试,现在可以说"大大真的解放了头脑和双手".