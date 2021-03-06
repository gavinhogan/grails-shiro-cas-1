# Shiro CAS Grails Plugin

[Shiro](http://shiro.apache.org/) is a flexible authentication and authorization framework for Java that has a corresponding [Grails plugin](http://grails.org/plugin/shiro) for simplifying access control in Grails applications. This plugin extends the base Shiro plugin with support for authentication using the [Shiro CAS integration](https://shiro.apache.org/cas.html).

[![Build Status](https://travis-ci.org/commercehub-oss/grails-shiro-cas.png?branch=master)](https://travis-ci.org/commercehub-oss/grails-shiro-cas)

# Usage

This assumes that you've already configured the [Shiro plugin](http://grails.org/plugin/shiro).

To install the plugin, add declaration to your `BuildConfig.groovy` plugins section in the form `compile ":shiro-cas:VERSION"`.

Next, you need to re-configure your `ShiroSecurityFilters`.  The easiest way to accomplish this is to make your class extend `org.apache.shiro.cas.grails.ShiroCasSecurityFilters`.  If that's not an option, you can copy the `onNotAuthenticated` handler into your class.

Finally, you need a CAS-enabled Shiro realm.  Run `grails create-cas-realm` to create such a realm based on a template.  If desired, you can customize the name by specifying `--prefix=PACKAGE.` or `--prefix=PACKAGE.CLASSPREFIX`.  Modify the generated class as needed for your application.

# Configuration

* `security.shiro.cas.serverUrl` (REQUIRED): The URL of the CAS instance to authenticate against.  This should be an HTTPS URL.
* `security.shiro.cas.serviceUrl` (REQUIRED): The URL to pass to CAS as the `service` parameter (see [CAS Protocol](http://www.jasig.org/cas/protocol) for more details on how this is used).  This should be the URL at which end-users can reach the `/shiro-cas` path within the current application (which is automatically registered by this plugin).
* `security.shiro.cas.failureUrl` (RECOMMENDED): The URL that users are redirected to if ticket validation fails.  If this is not specified, ticket validation failures will result in `NullPointerException`s being thrown.
* `security.shiro.cas.loginUrl` (OPTIONAL): The URL that users are redirected to when login is required.  By default, this directs users to `/login` within the `serverUrl`, passing along the service as a query parameter.
* `security.shiro.cas.logoutUrl` (OPTIONAL): The URL that users are redirected to when logging out.  By default, this directs users to `/logout` within the `serverUrl`, passing along the service as a query parameter.
* `security.shiro.cas.loginParameters` (OPTIONAL): Key, Value pairs to be added to the generated or explicitly set loginUrl. (see [CAS Parameters](http://www.jasig.org/cas/protocol#parameters) for more details on how this is used) 

## Example configuration

Assuming that:
* your CAS instance is deployed at context path `/cas` on `sso.example.com` on the default HTTPS port
* your Grails application is accessed via a load-balancer at https://apps.example.com/my-app

A valid configuration would be:

```groovy
security.shiro.cas.serverUrl='https://sso.example.com/cas'
security.shiro.cas.serviceUrl='https://apps.example.com/my-app/shiro-cas'
```
