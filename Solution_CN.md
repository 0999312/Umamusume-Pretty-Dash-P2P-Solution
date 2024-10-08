# 《赛马娘Pretty Derby 热血喧闹大感谢祭！》 多设备联机游玩参考文档

## 概述
本文档用于解决游戏《赛马娘Pretty Derby 热血喧闹大感谢祭！》Steam版本中出现的无法正常在线联机游戏的严重问题。本文档重点内容在于简单介绍该游戏的在线联机游玩的实现方法和可供参考的复数个解决方案，不保证所有解决方案的实际效果如何，但可以保证至少有一种重点方案在可用平台不足时能独立自行解决联机问题。  
由于游戏联机方式原理的局限性，本文档默认需要联机的所有玩家能够正常登录Steam账户并已经购买了本游戏。无法连接至外网与盗版等基本问题请读者自行解决。本文档也无法解决跨平台联机的问题，还请见谅。  

## 注意事项  
本文不适用于以下类型的玩家：
> 1. 非Steam平台的玩家，例如NS平台玩家。  
> 2. 自身网络条件差的玩家——即便使用了工具也改变不了网络条件不足的事实。 
> 3. 学习版玩家——本游戏使用Steam大厅，若无法获取到Steam用户信息则无法进行在线游戏。   
> 4. 使用Steam Desk的玩家——无法使用工具。

本文尚未对以下类型的问题进行测试验证，但在理论上也可以解决：
> 1. 使用快速匹配进行游玩的玩家  
> 2. 希望和非同一地区玩家，例如大陆玩家和香港玩家之间进行联机

## 联机原理
《赛马娘Pretty Derby 热血喧闹大感谢祭！》在Steam版本中的在线游戏是一份使用Steamworks API当中的P2P联机游玩的案例。同类型联机游戏的案例在Steam当中并不少见，例如胡闹厨房等经典游戏案例。  
由于各地网络环境，软件设计的遗漏和其他相关复杂因素，本游戏在Steam平台当中几乎无法做到正常使用纯P2P联机模式进行在线联机游玩。很多游戏会提供一些其他的补救措施来让P2P连接问题带来的影响最小化，其中包括P2P中转等技术。但本游戏由于考虑到其他平台设计等原因，实际上无法由玩家解决联机问题。甚至本游戏试图修复过P2P问题，但是很显然这次修复失败了。  
因为确定了本游戏的联机方式是P2P连接 + Steam在线大厅的设计，本游戏可以轻松通过相关的P2P联机工具来解决无法正常联机游戏的问题。同类型的问题在其他游戏尤其是中小工作室或个人等开发的独立或商业游戏当中出现的概率也并不低，故只要了解这些游戏当中是否有人已经注意到联机问题并且解决了问题，对于玩家而言都是合理的方案。  
本游戏理论来说没有使用厂商自己的服务器作为本游戏的P2P相关服务器使用，所以应该只和Steam平台，联机的代码设计以及各个P2P节点状况有很大关联。实际体验表明本游戏的P2P连接的速率和可靠性等性能指标都表现不佳。根据对本游戏进行抓包来分析连接情况的初步判断，最有可能造成可靠性不佳的原因之一是STUN打洞过程中出现的UDP端口不可达问题。  
开发者在最近试图改善P2P连接的稳定性，但是他们实际的做法表现来看似乎是单纯多开了许多实际上根本没有使用上的冗余端口。同时即便存在冗余端口，UDP端口不可达问题也没有解决。所以实质上联机问题从未得到任何有效的解决。理论上虽然可以通过额外架设服务器来缓解问题，但是似乎开发者不太会有效利用Steamworks API。  
为什么这一问题在Steam版本尤其突出，其原因估计和玩家数量和程序员在Steamworks API上的实现有关。根据这一猜想，本游戏无法跨平台在线游戏的本质是因为连具体的联机平台都不统一，各个平台都很有可能只使用了该平台单独提供的多人游戏API。
最后总结一下：本游戏是一个典型的Steam P2P联机游戏，P2P过程中间由于各种原因导致速率和稳定性等性能指标都表现不佳。本游戏开发者尚未实现也无法实现对该问题进行修复。所以我们需要自行利用可用的联机工具来修复这一问题。

## 联机方案  
在今日，解决绝大部分使用P2P连接的游戏当中存在的方案已经算得上成熟。主要以搭建虚拟网络为主。本文中的联机方案基本都是该技术的具体实现，有兴趣的同志们可以自行研究并构建更方便的工具等。  
本文推荐使用以下联机工具：
> 1. Radmin LAN，用于搭建通用虚拟本地网络以改善P2P稳定性。  
> 2. UsbEAm LAN Party，基于OpenVPN实现的局域网隧道工具。  
> 3. EasyN2N 或其他基于N2N的虚拟本地网络工具。  
  
这些联机工具的具体使用方式详见软件发布界面的使用说明。后两者的使用简单易上手，可以很快完成设置并实现稳定联机。  
经过实验，利用UsbEAm LAN Party进行本游戏的在线联机可以有效解决本游戏联机当中出现的速率和稳定性问题。使用该工具之后，本游戏可以在房间人数全满的情况下顺利完成一局长度最大化的障碍赛，多局其他小游戏和一局完整的大感谢祭，解决了本游戏联机过程当中原有的问题。并且该工具具有体积小，连接时延低和高度可自定义化等特性，十分适合用于本游戏的联机。  
Radmin LAN是目前相当流行的虚拟本地网络工具，但该工具的问题在于：实际游戏体验仍旧会造成大量延迟，体验不佳。故本文并不推荐用户使用该工具进行联机。  

## 有关线下联机的说明  
考虑到“本游戏是一款赛马娘IP下的高游戏性和对抗性的派对游戏”这一特殊性，本游戏在未来将必然成为各个马娘Only展会当中的必备环节。所以一套有效的线下体验解决方案将会是必然要考虑的。  
有人提出过单机分屏联机作为展会舞台环节的方案，这一方案在实际实验过程当中被证明是“过于伤眼和困难”的。在可预见的未来，本游戏将不可避免诞生各种高水平玩家之间的对抗，单机分屏联机的思路并不能为这些高水平玩家提供任何帮助。  
网络部曾负责参与过第二届长沙马娘Only的MC专区网络规划和配置。在此次展会的配置当中我们意识到：场地方能提供的网络资源很有可能十分有限，在场地条件恶劣时有可能无法提供有效的宽带连接等保证稳定接入到互联网。本游戏在网络对战中因为设计原因强制要求接入互联网且能够正常登入Steam大厅等，对于先天条件不足的展会而言可能会有些困难。  
理论上最佳方案是四台设备连接到有线网络并统一由现场的路由器通入外网实现在线联机。该方案可以通过线下的实体局域网，绕过使用工具搭建虚拟本地网络的步骤，从物理意义上解决P2P连接当中存在的问题。该方案最大的好处是允许玩家最大程度发挥自己的真实水平。  
由于本游戏的设计问题，切换玩家档案最佳的方式是切换Steam账号。本游戏还涉及到键位配置等问题，即便全部玩家使用手柄进行对战也会存在该问题。在这一方面来说，比赛的设计和键位等问题需要经过进一步的研究和讨论。如果允许参赛玩家自行携带设备接入到网络来进行对战的话该问题也可以得到有效解决，但不是所有玩家愿意扛着笔记本电脑参加展会。故有必要在此说明  

## 致谢名单
1. 羽翼城(Dogfight360)，UsbEAm LAN Party 作者。
2. 特雷森学院Minecraft部组员，参与了本文联机方案的实验。
