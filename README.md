
  <div class="entry">	
		<h2><a href="http://marshal.easymorse.com/archives/5025" rel="bookmark" title="Permanent Link to Android下发送和接收OBD数据">Android下发送和接收OBD数据</a></h2>
<p>OBD，On-Board Diagnostics，车载自动诊断系统。你可以把它看做汽车上的电脑。现在的汽车，如果不是出厂年份太早，基本上都带有OBD接口，是国际标准。</p>
<p>连接OBD可以获取到很多汽车状态数据，在驾驶员位置附近，有OBD接口，我的高尔夫6，接口在方向盘左下方位置。可以使用ELM327蓝牙转接口连接OBD接口，这样就可以无线蓝牙连接。我使用的ELM327转接口：</p>
<p><a href="http://marshal.easymorse.com/wp-content/uploads/2013/04/image.png" ><img title="image" style="border-top: 0px; border-right: 0px; border-bottom: 0px; border-left: 0px; display: inline" border="0" alt="image" src="http://marshal.easymorse.com/wp-content/uploads/2013/04/image_thumb.png" width="244" height="152" /></a> </p>
<p><span id="more-5025"></span>
<p>Android有连接ELM327的app，比如Torque，有功能简化的免费版本。</p>
<p>如果想编写Android连接ELM327的程序，需要解决以下几个问题：</p>
<ul>
<li>如何通过蓝牙连接到ELM327设备</li>
<li>发送和接收数据的格式</li>
</ul>
<p>好在已经有人编写了开源项目，可实现基本的ELM327通讯的app，链接见：</p>
<blockquote><p><a title="https://code.google.com/p/android-obd-reader/" href="https://code.google.com/p/android-obd-reader/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://code.google.com']);">https://code.google.com/p/android-obd-reader/</a></p>
</blockquote>
<p>该作者编写的代码，依赖maven3，比较麻烦。我改写了他的代码：</p>
<ul>
<li>不在需要依赖maven3，直接可导入到IDE工具生成项目</li>
<li>增加了手工输入命令和显示原始结果的功能</li>
</ul>
<p>效果见：</p>
<p><a href="http://marshal.easymorse.com/wp-content/uploads/2013/04/image1.png" ><img title="image" style="border-top: 0px; border-right: 0px; border-bottom: 0px; border-left: 0px; display: inline" border="0" alt="image" src="http://marshal.easymorse.com/wp-content/uploads/2013/04/image_thumb1.png" width="139" height="244" /></a> </p>
<p>代码共享在这里：</p>
<blockquote><p><a title="https://github.com/MarshalW/MyObdDemo" href="https://github.com/MarshalW/MyObdDemo" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://github.com']);">https://github.com/MarshalW/MyObdDemo</a></p>
</blockquote>
<p>因为是开源项目，对容错和自动化处理不够，要按照一定的次序执行，否则会app崩溃：</p>
<ol>
<li>启动android蓝牙</li>
<li>在android蓝牙设置中对ELM327做蓝牙配对</li>
<li>在app菜单中，选择Settings，在列表中选择Bluetooth Devices，然后在对话框中选择配对的设备（下面有截图）</li>
<li>在app菜单中，选择Start Live Data，等2秒钟左右，界面将显示发送命令接收到的内容</li>
<li>这时候，可以在上面的对话框中输入OBD命令，确切的说，应该叫OBD II PID，可参见：<a title="http://en.wikipedia.org/wiki/OBD-II_PIDs#Bitwise_encoded_PIDs" href="http://en.wikipedia.org/wiki/OBD-II_PIDs#Bitwise_encoded_PIDs" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://en.wikipedia.org']);">http://en.wikipedia.org/wiki/OBD-II_PIDs#Bitwise_encoded_PIDs</a></li>
</ol>
<p><a href="http://marshal.easymorse.com/wp-content/uploads/2013/04/image2.png" ><img title="image" style="border-top: 0px; border-right: 0px; border-bottom: 0px; border-left: 0px; display: inline" border="0" alt="image" src="http://marshal.easymorse.com/wp-content/uploads/2013/04/image_thumb2.png" width="139" height="244" /></a> </p>
<p>该项目主要代码：</p>
<ul>
<li>ObdGatewayService，是一个Android Service，可以跑在系统后台，这个Service用来连接蓝牙，并发送接收数据</li>
<li>ObdCommand，是个类族，用于封装命令和返回的结果，我写了个继承ObdCommand的子类，MyObdCommand，用于手工输入的命令和获得原始返回数据</li>
<li>MainActivity，我加了个文本框和相关界面组件，用于接收用户输入数据，然后，将数据封装为MyObdCommand，再加入到ObdGatewayService的队列中去执行</li>
</ul>
<p>如能理解这些，就可以在这个项目代码基础上，编写自己的基于读取ELM327的应用了。</p>
