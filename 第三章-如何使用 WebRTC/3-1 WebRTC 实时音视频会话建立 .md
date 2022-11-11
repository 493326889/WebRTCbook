## 如何使用WebRTC  

前面2章我们介绍了音视频基础知识以及WebRTC的前生今世，核心概念。让我们对WebRTC有了初步的了解。那么，我们该如何使用WebRTC呢，在第三章开始，将会通过一个个示例具体展开，深入了解WebRTC以及其底层原理，让我们一起开启对WebRTC的实践探索之路吧。    
### 3.1 WebRTC 实时音视频会话建立   
建立WebRTC会话只需要几个步骤即可，主要为以下4个步骤：  
1, 通过getUserMeida获取本地媒体；      
2，创建对等连接对象，并交换会话描述；   
3，绑定媒体与数据到连接；      
4，在浏览器跟其他端(浏览器/终端)建立连接
  
下面我们将一步步展开讲解，首先我们先看第一步：

**3.1.1 获取本地媒体流**  
通过javascript获取本地媒体流的方式有很多中，其中WebRTC定义了一种最常见便捷的方式，通过getUserMedia API来获取本地的媒体流。这里需要注意的是，使用MediaStream API需要站点开启https协议以及用户的授权，才会允许web应用访问用户的摄像头以及麦克风的请求。通过getUserMedia API获取本地媒体流后，我们可以获取到一个MediaStream对象。   
````
// 通过constraints指明需要获取的媒体源数据，constraints还有很多其他配置参数哦～后面会详细讲解
const constraints = {
  audio: true,
  video: true
};
try {
    const stream = await navigator.mediaDevices.getUserMedia(constraints);
    // 通过getTracks可以获取到该MediaStream下面的所有track
    stream.getTracks().forEach((track) => {
        // todo something
    });

} catch (e) {
    handleError(e);
}
````   

**3.1.2 创建对等连接并交换会话描述**     
在WebRTC中，对等连接的建立需要依赖PeerConnection对象。RTCPeerConnection对象封装了一系列方法用于对等连接建立。首先可以通过生成RTCPeerConnection对象来创建对等连接对象。
````
 const peerConnection = new RTCPeerConnection(null);
````   
这里我们推荐使用webrtc-adaptor来适配不同浏览器的RTCPeerConnection以及getUserMeida等api的前缀。   
**adaptor图**   
各端分别创建完RTCPeerConnection后，后面将进行会话描述(SDP)的交换，会话描述()