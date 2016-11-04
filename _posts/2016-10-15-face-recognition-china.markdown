---
layout: post
title:  "人脸识别"
date:   2016-11-04 Fri 09:24:24
categories: FaceRecognition
---

1. Face++
    - 网站：[http://www.faceplusplus.com.cn/](http://www.faceplusplus.com.cn/)
        - 隶属于旷视科技: [https://megvii.com/](https://megvii.com/)
        - 北京市海淀区科学院南路2号融科资讯中心A座3层
        - 同一个公司还有一个叫 FaceID 的产品：[https://faceid.com/](https://faceid.com/)
            - 在线的用户身份验证，用户证件验证，从而达到在线核身的效果
    - 技术
        - 人脸检测、人脸关键点检测和人脸分析
        - 基础版API供免费调用测试和小规模使用
        - 企业版API相对基础版API在算法和稳定性方面会有更好的表现
    - 应用
        - 美图秀秀、Camera360、魔漫相机、神拍手等移动应用厂商使用了他们的服务
        - 刷脸门禁
        - 天眼监控
        - 陌生人识别
        - VIP 客户识别
        - 车牌识别

3. 商汤科技
    - 网站：[http://www.sensetime.com/](http://www.sensetime.com/)
        - 深圳分部地址：深圳市南山区招商局发展中心大厦13层
    - 技术
        - 人脸识别
        - 关键点定位
        - 身份认证
        - 人脸聚类
        - 真人检测
        - 美颜
    - 应用
        - 金融（身份认证，票据电子化）
        - 门禁，迎宾
        - 安防、布控
        - 智能相册

4. 腾讯优图人脸识别
    - 网站：[https://www.qcloud.com/product/fr.html](https://www.qcloud.com/product/fr.html)
        - 公测期间免费
    - 技术
        - 人脸比对，人脸验证，人脸识别，活体检测，OCR识别
        - 多语言接口: 常见的语言都支持
    - 应用
        - WeBank 微众银行（人脸识别身份核实）
        - 天天P图

5. 百度人脸识别 BFR
    - 网站：[https://cloud.baidu.com/product/bfr.html](https://cloud.baidu.com/product/bfr.html)
        - 推广期免费
    - 技术：
        - 人脸检测、人脸识别、关键点定位、属性识别和活体检测

5. 阿里
    - 网站：[https://data.aliyun.com/product/face](https://data.aliyun.com/product/face)
        - 作为阿里云平台的一部分，人脸识别的技术服务商既包括阿里自己，也包括旷视科技、汉王科技等
    - 技术
        - 人脸检测、人脸特征提取、人脸年龄估计和性别识别、人脸关键点定位
    - 应用
        - 人脸美化、人脸识别和认证、大规模人脸检索、照片管理

6. 科大讯飞
    - 网站：[http://www.xfyun.cn/services/face?type=face](http://www.xfyun.cn/services/face?type=face)
    - 技术
        - 快速判定两张照片是否为同一个人；在目前公开的 [LWF](http://vis-www.cs.umass.edu/lfw/) 测试中，人脸验证可达到世界第一的99.15%准确率 (注：这种准确率仅供参考，应用于现实场景时没有这么高)
        - 支持离线使用
        - 多角度人脸检测
        - 实时人脸定位

1. 格灵深瞳
    - 网站: [http://www.deepglint.com/](http://www.deepglint.com/)
    - 技术
        - 三维计算机视觉
        - 深度学习
    - 应用
        - 着重于安防领域 (特别是银行安防)
    - 评价
        - 知乎上的评价不高: [https://www.zhihu.com/question/28666475](https://www.zhihu.com/question/28666475)

1. 其它相关
    - 海康威视：监控
        - 网站：[http://www.hikvision.com/](http://www.hikvision.com/)
    - 大华：监控
        - 网站：[http://www.dahuatech.com/](http://www.dahuatech.com/)
    - 诺亦腾：动作捕捉
        - 网站：[http://www.noitom.com.cn/](http://www.noitom.com.cn/)


6. 自己做
    - 有许多开源框架可用
    - 基于深度学习的算法需要较高的硬件支持 (主要是显卡 GPU)
        - DEEPID: [https://github.com/hqli/face_recognition](https://github.com/hqli/face_recognition)
    - GaussianFace: [https://arxiv.org/abs/1404.3840](https://arxiv.org/abs/1404.3840)
    - 基于 PCA
        - OpenCV: [http://docs.opencv.org/2.4/modules/contrib/doc/facerec/facerec_tutorial.html](http://docs.opencv.org/2.4/modules/contrib/doc/facerec/facerec_tutorial.html)
