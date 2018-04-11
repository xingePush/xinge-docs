#三方通道功能说明
-------------
<br>

__三方通道功能验证__

 
<table class="tg">
  <tr>
    <th class="tg-c3ow" colspan="14">信鸽三方通道验证</th>
  </tr>
  <tr>
    <td class="tg-us36" rowspan="2" colspan="2">测试类型</td>
    <td class="tg-us36" colspan="2">type_activity=1</td>  
    <td class="tg-us36" colspan="2">type_url=2</td> 
    <td class="tg-us36" colspan="2">type_intent=3</td>
    <td class="tg-us36" colspan="2">透传消息</td>
  </tr>
  <tr>
    <td class="tg-us36">通知抵达，点击跳转/custom</td>
    <td class="tg-us36">回调</td>
    <td class="tg-us36">通知抵达，点击跳转/custom</td>
    <td class="tg-us36">回调</td>
    <td class="tg-us36">通知抵达，点击跳转/custom</td>
    <td class="tg-us36">回调</td>
    <td class="tg-us36">消息抵达/custom</td>
    <td class="tg-us36">回调（仅到达）</td>
  </tr>
  <tr>
    <td class="tg-us36" rowspan="2">信鸽</td>
    <td class="tg-us36">脚本</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass/pass</td>
    <td class="tg-us36">有回调</td>
  </tr>
  <tr>
    <td class="tg-us36">信鸽官网</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36" colspan="2">不支持</td>
    <td class="tg-us36">pass/pass</td>
    <td class="tg-us36">有回调</td>
  </tr>
  <tr>
    <td class="tg-us36" rowspan="3">小米</td>
    <td class="tg-us36">脚本</td>
    <td class="tg-us36">pass,不支持/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass/pass</td>
    <td class="tg-us36">有回调</td>
  </tr>
  <tr>
    <td class="tg-us36">信鸽官网</td>
    <td class="tg-us36">pass,X/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass,X/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36" colspan="2">不支持</td>
    <td class="tg-us36">pass/pass</td>
    <td class="tg-us36">有回调</td>
  </tr>
  <tr>
    <td class="tg-us36">小米官网</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击无</td>
    <td class="tg-us36">pass/pass</td>
    <td class="tg-us36">有回调</td>
  </tr>
  <tr>
    <td class="tg-us36" rowspan="3">华为</td>
    <td class="tg-us36">脚本</td>
    <td class="tg-us36">pass,X/pass</td>
    <td class="tg-us36">到达无，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达无，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">达到无，点击有</td>
    <td class="tg-us36">pass/X</td>
    <td class="tg-us36">有回调</td>
  </tr>
  <tr>
    <td class="tg-us36">信鸽官网</td>
    <td class="tg-us36">pass,X/pass</td>
    <td class="tg-us36">到达无，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达无，点击有</td>
    <td class="tg-us36" colspan="2">不支持</td>
    <td class="tg-us36">pass/X</td>
    <td class="tg-us36">有回调</td>
  </tr>
  <tr>
    <td class="tg-us36">华为官网</td>
    <td class="tg-us36">pass,不支持/pass</td>
    <td class="tg-us36">到达无，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达无，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">达到无，点击有</td>
    <td class="tg-us36">pass/不支持</td>
    <td class="tg-us36">有回调</td>
  </tr>
  <tr>
    <td class="tg-us36" rowspan="3">魅族</td>
    <td class="tg-us36">脚本</td>
    <td class="tg-us36">passs,pass/pass</td>
    <td class="tg-us36">到达有，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击有</td>
    <td class="tg-us36">pass, pass/pass</td>
    <td class="tg-us36">达到有，点击有</td>
    <td class="tg-us36">X/X</td>
    <td class="tg-us36">――</td>
  </tr>
  <tr>
    <td class="tg-us36">信鸽官网</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击有</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击有</td>
    <td class="tg-us36" colspan="2">不支持</td>
    <td class="tg-us36">X/X</td>
    <td class="tg-us36">――</td>
  </tr>
  <tr>
    <td class="tg-us36">魅族官网</td>
    <td class="tg-us36">pass,pass/pass</td>
    <td class="tg-us36">到达有，点击有</td>
    <td class="tg-us36">pass,pass/不支持</td>
    <td class="tg-us36">到达有，点击有</td>
    <td class="tg-us36">pass,pass/不支持</td>
    <td class="tg-us36">到达无，点击有</td>
    <td class="tg-us36" colspan="2">不支持</td>
  </tr>
</table>




__结果解析__

pass：通过；X：不通过；――表示无法验证；不支持：无法找到对应的输入项
信鸽通道：脚本推送时，type为url和intent时候，通知到达有回调，点击无回调；
信鸽官网web推送没intent选项。
小米通道：信鸽官方推送type为activity和url，点击跳转失败。
小米通道：通知到达有回调，点击无回调
华为通道：不支持type为activity的形式，用脚本和信鸽官网web端推，跳转失败
华为通道：通知到达无回调，点击有回调.
华为通道：透传消息不支持自定义custom
魅族通道：通知到达和点击均有回调，但回调为同一个函数，输出的参数不一致
魅族通道：自定义custom只有点击才能获得
魅族通道：不支持透传消息。
脚本推送，各家均有intent形式，但接收intent格式不一致
