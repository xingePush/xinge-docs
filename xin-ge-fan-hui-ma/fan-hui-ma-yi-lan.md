#信鸽返回码一览
##服务端返回码
<hr>
<table style="width:740px;" cellpadding="2" cellspacing="0" border="1" bordercolor="#000000">
		<tbody>
			<tr>
				<td style="text-align:center;">
					<span style="font-size:14px;"><strong>值</strong></span> 
				</td>
				<td style="text-align:center;">
					<span style="font-size:14px;"><strong>含义</strong></span> 
				</td>
				<td style="text-align:center;">
					<span style="font-size:14px;"><strong>可采取措施</strong></span> 
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">0</span> 
				</td>
				<td>
					<span style="font-size:14px;">调用成功</span> 
				</td>
				<td>
					<br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">-1</span> 
				</td>
				<td>
					<span style="font-size:14px;">参数错误</span> 
				</td>
				<td>
					<span style="font-size:14px;">检查参数配置</span> 
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">-2</span> 
				</td>
				<td>
					<span style="font-size:14px;">请求时间戳不在有效期内</span> 
				</td>
				<td>
					<span style="font-size:14px;">检查设备当前时间</span> 
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">-3</span> 
				</td>
				<td>
					<span style="font-size:14px;">sign校验无效</span> 
				</td>
				<td>
					<span style="font-size:14px;">检查Access ID和Secret Key（注意不是Access Key）</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">2</span> 
				</td>
				<td>
					<span style="font-size:14px;">参数错误</span><br>
				</td>
				<td>
					<span style="font-size:14px;">检查参数配置</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">14</span> 
				</td>
				<td>
					<span style="font-size:14px;">收到非法token，例如iOS终端没能拿到正确的token</span><br>
				</td>
				<td>
					<p>
						<span style="font-size:14px;">Android Token长度为40位</span> 
					</p>
					<p>
						<span style="font-size:14px;">iOS Token长度为64位</span><span style="line-height:1.5;"></span> 
					</p>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">15</span> 
				</td>
				<td>
					<span style="font-size:14px;">信鸽逻辑服务器繁忙</span><br>
				</td>
				<td>
					<span style="font-size:14px;">稍后重试</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">19</span> 
				</td>
				<td>
					<span style="font-size:14px;">操作时序错误。例如进行tag操作前未获取到deviceToken</span><br>
				</td>
				<td>
					<p>
						<span style="font-size:14px;">没有获取到deviceToken的原因：</span> 
					</p>
					<p>
						<span style="font-size:14px;">1.没有注册信鸽或者苹果推送</span> 
					</p>
					<p>
						<span style="font-size:14px;">2.provisioning profile制作不正确</span><span style="line-height:1.5;"></span> 
					</p>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">20</span> 
				</td>
				<td>
					<span style="font-size:14px;">鉴权错误，可能是由于Access ID和Access Key不匹配</span><br>
				</td>
				<td>
					<span style="font-size:14px;">检查Access ID和Access Key</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">40</span> 
				</td>
				<td>
					<span style="font-size:14px;">推送的token没有在信鸽中注册</span><br>
				</td>
				<td>
					<p>
						<span style="font-size:14px;">检查token是否注册</span> 
					</p>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">48</span> 
				</td>
				<td>
					<span style="font-size:14px;">推送的账号没有绑定token</span><br>
				</td>
				<td>
					<p>
						<span style="font-size:14px;">检查帐号和token是否绑定</span> 
					</p>
					<p>
						<span style="font-size:14px;">见推送指南：绑定/设置账号</span> 
					</p>
					<p>
						<span style="font-size:14px;">见热门问题解答：账号和设备未绑定</span> 
					</p>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">63</span> 
				</td>
				<td>
					<span style="font-size:14px;">标签系统忙</span><br>
				</td>
				<td>
					<p>
						<span style="font-size:14px;">检查标签是否设置成功</span> 
					</p>
					<p>
						<span style="font-size:14px;">见推送指南：设置标签</span><span style="line-height:1.5;"></span> 
					</p>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">71</span> 
				</td>
				<td>
					<span style="font-size:14px;">APNS服务器繁忙</span><br>
				</td>
				<td>
					<span style="font-size:14px;">苹果服务器繁忙，稍后重试</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">73</span> 
				</td>
				<td>
					<span style="font-size:14px;">消息字符数超限</span><br>
				</td>
				<td>
					<span style="font-size:14px;">iOS最新支持1000字节左右，苹果的额外推送设置如角标，也会占用字节数</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">76</span> 
				</td>
				<td>
					<span style="font-size:14px;">请求过于频繁，请稍后再试</span><br>
				</td>
				<td>
					<span style="font-size:14px;">全量广播限频为每3秒一次</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">78</span> 
				</td>
				<td>
					<span style="font-size:14px;">循环任务参数错误</span><br>
				</td>
				<td>
					<br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">100</span> 
				</td>
				<td>
					<span style="font-size:14px;">APNS证书错误。请重新提交正确的证书</span><br>
				</td>
				<td>
					<span style="font-size:14px;">证书格式是pem的，另外，注意区分生产证书、开发证书的区别</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">其他</span> 
				</td>
				<td>
					<span style="font-size:14px;">其他错误</span><br>
				</td>
				<td>
					<br>
				</td>
			</tr>
		</tbody>
	</table>
	
##客户端返回码
<hr>

<table style="width:740px;" cellpadding="2" cellspacing="0" border="1" bordercolor="#000000">
		<tbody>
			<tr>
				<td style="text-align:center;">
					<span style="font-size:14px;"><strong>值</strong></span> 
				</td>
				<td style="text-align:center;">
					<span style="font-size:14px;"><strong>含义</strong></span> 
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">0</span> 
				</td>
				<td>
					<span style="font-size:14px;">调用成功</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">2</span> 
				</td>
				<td>
					<span style="font-size:14px;">参数错误，例如绑定了单字符的别名，或是ios的token长度不对，应为64个字符</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">20</span> 
				</td>
				<td>
					<span style="font-size:14px;">鉴权错误</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10000</span> 
				</td>
				<td>
					<span style="font-size:14px;">起始错误</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10001</span> 
				</td>
				<td>
					<span style="font-size:14px;">操作类型错误码，例如参数错误时将会发生该错误</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10002</span> 
				</td>
				<td>
					<span style="font-size:14px;">正在执行注册操作时，又有一个注册操作到来，则回调此错误码</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10003</span> 
				</td>
				<td>
					<span style="font-size:14px;">权限配错或者缺少所需权限</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10004</span> 
				</td>
				<td>
					<span style="font-size:14px;">so库没有正确导入</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10005</span> 
				</td>
				<td>
					<span style="font-size:14px;">AndroidManifest文件的XGRemoteService节点没有配置或者的该节点的action包名配错</span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10100</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">当前网络不可用</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10101</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">创建链路失败</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10102</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">请求处理过程中， 链路被主动关闭</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10103</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">请求处理过程中，服务器关闭链接</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10104</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">请求处理过程中，客户端产生异常</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10105</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">请求处理过程中，发送或接收报文超时</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10106</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">请求处理过程中， 等待发送请求超时</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10107</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">请求处理过程中， 等待接收请求超时</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10108</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">服务器返回异常报文</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10109</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">未知异常，请在QQ群中直接联系管理员，或在官网反馈入口反馈</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
			<tr>
				<td>
					<span style="font-size:14px;">10110</span><span style="font-size:14px;"></span><br>
				</td>
				<td>
					<span style="font-size:14px;">创建链路的handler为null</span><span style="font-size:14px;"></span><br>
				</td>
			</tr>
		</tbody>
	</table>