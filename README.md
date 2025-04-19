# QCloudMusicServer

基于Qt实现的 网易云音乐API HTTP服务器

[![GitHub Actions CI Status](https://github.com/s12mmm3/QCloudMusicServer/actions/workflows/windows.yml/badge.svg)](https://github.com/s12mmm3/QCloudMusicServer/actions/workflows/windows.yml)

![C++ version](https://img.shields.io/badge/C++-11-00599C?logo=++)
[![Qt version](https://img.shields.io/badge/Qt-6.9+-41CD52?logo=qt)](https://www.qt.io)
![GitHub license](https://img.shields.io/github/license/s12mmm3/QCloudMusicServer)

## 简介

基于网易云API实现的HTTP服务器，使用Qt QHttpServer库实现

支持跨平台和多种编译器编译，基于[Qt版 网易云音乐 API](https://github.com/s12mmm3/QCloudMusicApi)实现

### 目录

- [需求和依赖](#需求和依赖)
- [编译方式](#编译方式)
- [License](#License)

---

## 需求和依赖

- [Qt 6.9+](https://www.qt.io/download-qt-installer)

## 使用说明

运行`QCloudMusicServer`

浏览器访问`http://localhost:3000/song/dynamic/cover?id=2101179024`

返回结果
```
{
    "code": 200,
    "data": {
        "height": 0,
        "needTransition": true,
        "videoPlayUrl": "http://dcover.music.126.net/4e94/a0a4/86a0/4f8994c7397cce4306fa3d40474863da.mp4?wsSecret=1bbd9bdee053836dae0ebc976bd1a131&wsTime=1745088713",
        "weight": 0
    },
    "message": ""
}
```

接口可参考[原项目文档](https://binaryify.github.io/NeteaseCloudMusicApi)

## 编译方式

```Shell
git clone --recursive https://github.com/s12mmm3/QCloudMusicServer.git
cd QCloudMusicServer
cmake -B build
cmake --build build -j
```

## License

[The MIT License (MIT)](https://github.com/s12mmm3/QCloudMusicServer/blob/master/LICENSE)