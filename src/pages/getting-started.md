---
title: Getting Started
layout: ../layouts/Main.astro
---

## Trying out Authorizer

This guide helps you practice using Authorizer to evaluate it before you use it in a production environment. It includes instructions for installing the Authorizer server in standalone mode.

## Installing a simple instance of Authorizer

Deploy Authorizer using [heroku](https://github.com/authorizerdev/authorizer-heroku) and quickly play with it in 30seconds
<br/><br/>
[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/authorizerdev/authorizer-heroku)

### Things to consider

- For social logins, you will need respective social platform key and secret
- For having verified users, you will need an SMTP server with an email address and password using which system can send emails. The system will send a verification link to an email address. Once an email is verified then, only able to access it. _(Note: One can always disable the email verification to allow open sign up, which is not recommended for production as anyone can use anyone's email address 😅 )_
- For persisting user sessions, you will need Redis URL. If you do not configure a Redis server, sessions will be persisted until the instance is up or not restarted. For better response time on authorization requests/middleware, we recommend deploying Redis on the same infra/network as your authorizer server.

## Integrating into your website

This example demonstrates how you can use `@authorizerdev/authorizer-js` CDN version and have login ready for your site in few seconds. You can also use the ES module version of `@authorizerdev/authorizer-js` or framework-specific versions like `@authorizerdev/authorizer-react`

### Copy the following code in `html` file

> **Note:** Change AUTHORIZER_URL in the below code with your authorizer URL. Also, you can change the logout button component

```html
<script src="https://unpkg.com/@authorizerdev/authorizer-js/lib/authorizer.min.js"></script>

<script type="text/javascript">
	const authorizerRef = new Authorizer.Authorizer({
		authorizerURL: `AUTHORIZER_URL`,
		redirectURL: window.location.origin,
	});

	// use the button selector as per your application
	const logoutBtn = document.getElementById('logout');
	logoutBtn.addEventListener('click', async function () {
		await authorizerRef.logout();
		window.location.href = '/';
	});

	async function onLoad() {
		const res = await authorizerRef.fingertipLogin();
		if (res && res.user) {
			// you can use user information here, eg:
			/**
			const userSection = document.getElementById('user');
			const logoutSection = document.getElementById('logout-section');
			logoutSection.classList.toggle('hide');
			userSection.innerHTML = `Welcome, ${res.user.email}`;
			*/
		}
	}
	onLoad();
</script>
```