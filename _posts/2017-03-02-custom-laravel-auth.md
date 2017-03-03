---
layout:     post
title:      "laravel修改登录注册"
subtitle:   "在laravel5.4中使用自定义的字段登录，注册的过程"
date:       2017-03-02 17:10:00
author:     "杨"
header-img: "img/post-bg-04.jpg"
---


# 在laravel5.4中使用自定义的字段登录，注册的过程

在使用laravel自带的注册登录时，默认的登录名是email,这个有的时候不能满足需求，下面是修改的步骤

1. 首先想到使用php artisan route:list 查看现在项目的登录，注册路由执行的方法
    
    
```
                     |
|        | GET|HEAD                       | admin/login                                     | login                      | App\Http\Controllers\Auth\LoginController@showLoginForm                | web,guest                         |
|        | POST                           | admin/login                                     |                            | App\Http\Controllers\Auth\LoginController@login                        | web,guest                         |
|        | POST                           | admin/logout                                    | logout                     | App\Http\Controllers\Auth\LoginController@logout                       | web                               |
|        | GET|HEAD                       | admin/logout                                    |                            | App\Http\Controllers\Auth\LoginController@logout                       | web                               |
|        | POST                           | admin/password/email                            | password.email             | App\Http\Controllers\Auth\ForgotPasswordController@sendResetLinkEmail  | web,guest                         |
|        | GET|HEAD                       | admin/password/reset                            | password.request           | App\Http\Controllers\Auth\ForgotPasswordController@showLinkRequestForm | web,guest                         |
|        | POST                           | admin/password/reset                            |                            | App\Http\Controllers\Auth\ResetPasswordController@reset                | web,guest                         |
|        | GET|HEAD                       | admin/password/reset/{token}                    | password.reset             | App\Http\Controllers\Auth\ResetPasswordController@showResetForm        | web,guest                         |
|        | GET|HEAD                       | admin/register                                  | register                   | App\Http\Controllers\Auth\RegisterController@showRegistrationForm      | web,guest                         |
|        | POST                           | admin/register                                  |                            | App\Http\Controllers\Auth\RegisterController@register                  | web,gues
```

找到 admin/login 路由对应的方法有两个
######     App\Http\Controllers\Auth\LoginController@showLoginForm
######     App\Http\Controllers\Auth\LoginController@login
第一个方法是展示给用户登录页面，第二个是提交用户填写的登录信息。这两个方法都在App\Http\Controllers\Auth\LoginController 内，然后我就打开这个文件发现没有找到这两个方法，注意到LoginController使用了use AuthenticatesUsers；就进入AuthenticatesUsers找这两个方法。

```
 	public function showLoginForm() {
		return view('auth.login');
	}
	
```
这个方法可以改变登录页面

```
	public function login(Request $request) {
		$this->validateLogin($request);

		// If the class is using the ThrottlesLogins trait, we can automatically throttle
		// the login attempts for this application. We'll key this by the username and
		// the IP address of the client making these requests into this application.
		if ($this->hasTooManyLoginAttempts($request)) {
			$this->fireLockoutEvent($request);

			return $this->sendLockoutResponse($request);
		}

		if ($this->attemptLogin($request)) {
			return $this->sendLoginResponse($request);
		}

		// If the login attempt was unsuccessful we will increment the number of attempts
		// to login and redirect the user back to the login form. Of course, when this
		// user surpasses their maximum number of attempts they will get locked out.
		$this->incrementLoginAttempts($request);

		return $this->sendFailedLoginResponse($request);
	}
	
```
顺藤摸瓜找到


```
 	protected function validateLogin(Request $request) {
		$this->validate($request, [
			$this->username() => 'required', 'password' => 'required',
		]);
	}
```

然后

```
 	public function username() {
		return 'email';
	}
```
这里找到了登录字段系统默认返回的是email,本来直接将该字段修改为自己想要的字段就可以了，但是注意到该文件位于/vendor文件夹下，属于第三方依赖不能直接修改。想到在引入trait的文件中重新定义和被引入trait文件中同名的方法时，新方法会覆盖掉trait中的方法。那么只要在LoginController中修改就可以了。



```
	/**
	 * 登录名
	 */
	protected $username = '自定义字段';
	
	public function username() {
		return $this->username;
	}

```

这样登录的过程中就会使用自定义的字段了

登录完成后同样的去修改注册 打开RegisterController ,修改下面两个方法




```
 	protected function validator(array $data) {
		return Validator::make($data, [
			'自定义字段' => 'required|max:255',
			'password' => 'required|min:6|confirmed',
		]);
	}

	protected function create(array $data) {
		return User::create([
			'自定义字段' => $data['自定义字段'],
			'password' => bcrypt($data['password']),
		]);
	}
```

修改完成后再去修改 登录，注册页 表单里 用户名 input的name,  修改成和自定义字段一直即可。

