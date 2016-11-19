# simple-router
路由简单实现，原理介绍
# 实现原理
使用浏览器的hash改变事件监听，取出对应的hash值匹配到路由规则中相应的url再调用配置的回调函数

function Router(routes){
    this.routes = routes || {};
    this.currentUrl = '';
};
/**
 * 定义路由
 * @param path  路径
 * @param callback 路由回调处理
 */
Router.prototype.route = function(path,callback){
    this.routes[path] = callback || function(){};
};

/**
 *路由刷新载入
 */
Router.prototype.refresh = function(){
    this.currentUrl = location.hash.slice(1) || '/';
    var callback = this.routes[this.currentUrl] || this.routes['ERROR'];
    callback();
};

/**
 * 路由错误处理一般用于全局错误处理比如 404或者500
 */
Router.prototype.routeError = function(callback){
    this.routes['ERROR'] = callback || function(){};
};

/**
 * 路由初始化绑定事件
 */
Router.prototype.init = function(){
    //window.addEventListener('load',this.refresh.bind(this),false);
    //window.addEventListener('hashchange', this.refresh.bind(this),false);
    $(window).bind('load',this.refresh.bind(this));
    $(window).bind('hashchange',this.refresh.bind(this));
};

