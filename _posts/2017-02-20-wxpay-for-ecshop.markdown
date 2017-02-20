---
layout:     post
title:      "微信支付V3 Sdk接入ecshop"
subtitle:   "使用微信支付V3为ecshop开通微信支付"
date:       2017-02-17 11:00:00
author:     "yang"
header-img: "img/post-bg-06.jpg"
---
# 微信公众平台的配置

###     设置js安全域名
    如果不设置的话，在使用公众号支付时会提示redirect_url错误

###  设置支付目录，和支付测试目录
    比如我发起支付的文件是 http://example.com/includes/modules/payment/wxpay/example/native.php;
    
    那么我的支付目录要设置成 http://example.com/includes/modules/payment/wxpay/example/
    
    测试目录同理。

### 支付回调url
    在支付完成后，微信会向向这个地址发送请求。主要用户扫码模式一支付，比如设置成
    http://example.com/includes/modules/payment/wxpay/example/notify.php。
    其他支付模式无需设置该url,（需要在代码里设置）
    
    到此微信公众平台的配置就完成了。
    
# 将微信支付V3 Sdk接入ecshop

### 获取微信支付v3SDK
    这个在微信支付文档里很容易可以找到,然后将该SDK放进includes/modules/payment/下

### 更改配置
    打开wxpay/lib

### 编写ecshop 支付插件
    ecshop中所有的支付插件都位于includes/modules/payment/下，在该目录下创建wxpay.php文件 文件的格式可以参考该目录下其他的文件。在该文件中创建wxpay类，如下：
    
    
```
class wxpay {
	
	/**
	 * 生成支付代码
	 * 
	 * @param array $order
	 *        	订单信息
	 * @param array $payment
	 *        	支付方式信息
	 */
	function get_code($order, $payment) 
	{
	  
		if (isMobile()){
			//如果是微信浏览器
			if (strpos($_SERVER['HTTP_USER_AGENT'], 'MicroMessenger') !== false) {
				$button = '<div style="text-align:center"><input type="button" style="border:none; background:#e4393c; padding:5px 20px; color:#fff; line-height:20px;" onclick="window.open(\'/includes/modules/payment/wxpay/example/jsapi.php?&productId='.$order['order_sn'].'\')" value="' .$GLOBALS['_LANG']['wxpay_button']. '" /></div>';
			}else{
				$button = '<div style="text-align:center"><input type="button" style="border:none; background:#e4393c; padding:5px 20px; color:#fff; line-height:20px;" value="请使用微信浏览器进入我的订单进行付款" /></div>';
			}
			
		}else{
			$button = '<div style="text-align:center"><input type="button" style="border:none; background:#e4393c; padding:5px 20px; color:#fff; line-height:20px;" onclick="location.href=\'/includes/modules/payment/wxpay/example/native.php?&productId='.$order['order_sn'].'\'" value="' .$GLOBALS['_LANG']['wxpay_button']. '" /></div>';
		}

		return $button;
	}
	
	
	/**
	 * 接受通知处理订单。这里可以写接受到微信回调请求后的逻辑，我是直接写在notify.php里面了。
	 * 
	 * @param undefined $log_id
	 *        	20141125
	 *        	
	 */
	function respond() {

	}
}
```
这段代码的主要左右就是返回页面一个带调转到支付页面的按钮。手机浏览器，微信浏览器，及PC浏览器返回的按钮都是不同的。手机非微信浏览器现在还没办法用微信支付。

### PC端扫码支付模式二
    点击按钮跳转到wxpay/example/native.php  这里面有模式一和模式二，这里选择模式二，到这里按照微信提供的文档，应该就比较容易解决了。

### 回调 notify.php 
    微信发的请求会由该文件处理。我的处理逻辑如下：
    
    
```
	//重写回调处理函数
	public function NotifyProcess($data, &$msg)
	{
		$notfiyOutput = array();
		
		if(!array_key_exists("transaction_id", $data)){
			$msg = "输入参数不正确";
			return false;
		}
		//查询订单，判断订单真实性
		if(!$this->Queryorder($data["transaction_id"])){
			$msg = "订单查询失败";
			return false;
		}
		$total_fee=$data['total_fee']/100;
		//用户自定义逻辑，修改订单状态
		//检查支付的金额是否与订单相符
		$pay_log_id=get_order_id_by_sn($data['out_trade_no']);
		if(!check_money($pay_log_id,$total_fee)){
			$msg = "支付金额与订单金额不符";
			return false;
		}
		$user_id=getUserId($data['out_trade_no']);
		order_paid($pay_log_id, 2,"货款已微信支付");
		$order_id=getOrderIdBySn($data['out_trade_no']);
		Log::INFO("订单id:".$order_id."logs_id:".$pay_log_id);

		$log_msg="用户". $user_id. "已经为订单id:".$order_id."sn：" .$data['out_trade_no']. "支付了". $total_fee ." 元,并将该订单状态改为已经支付";
		Log::INFO($log_msg);
		if ($msg!='OK') {
			Log::ERROR("用户".$user_id."支付订单：".$data['out_trade_no']."时出错，错误信息为".$msg);
		}
		return true;
	}
```

### 在支付页面在支付成功后将二维码切换成成功提示消息

    这里可以设置个定时器每隔几秒就去查询订单（微信支付提供了订单查询借口），等支付成功后就提示用户 
    
#### 微信支付提供的参考文件还是很多的，只要认真看流程，然后对官方SDK进行一点修改就可满足自己的需求了，希望这篇文章能帮到需要的人。
    
    
