---
title: Java后端实现token的生成和验证
date: 2022-06-27 16:47:58
author: Evan
categories: 笔记
tags:
---

# Java后端实现token的生成和验证

在Java后端实现token登录功能

## 什么是token？

token的意思是“令牌”，是服务端生成的一串字符串，作为客户端进行请求的一个标识。

当用户第一次登录后，服务器生成一个token并将此token返回给客户端，以后客户端只需带上这个token前来请求数据即可，无需再次带上用户名和密码。

简单token的组成；uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign（签名，token的前几位以哈希算法压缩成的一定长度的十六进制字符串。为防止token泄露）。

## 为什么要用token？

Token的目的是为了验证用户登录情况以及减轻服务器的压力，减少频繁的查询数据库，使服务器更加健壮。

避免CSRF跨站伪造攻击，支持跨域，适合前后端分离项目。

## 使用方法

**基于JWT的token认证实现**

**1.引入依赖**

```
<dependency>
      <groupId>com.auth0</groupId>
      <artifactId>java-jwt</artifactId>
      <version>3.8.2</version>
    </dependency>
```

**2.创建工具类TokenUtil**

```java
public class TokenUtil {
	//设置过期时间
    private static final long EXPIRE_TIME= 15*60*1000;
    //token秘钥
    private static final String TOKEN_SECRET="tokenqkj";  

    //实现签名方法
    public static String sign(Admin admin){

        String token = null;
        try {
            Date expiresAt = new Date(System.currentTimeMillis() + EXPIRE_TIME);
            token = JWT.create()
                    .withIssuer("auth0")
                    .withClaim("name", admin.getName())
                    .withExpiresAt(expiresAt)
                    // 使用了HMAC256加密算法。
                    .sign(Algorithm.HMAC256(TOKEN_SECRET));
        } catch (Exception e){
            e.printStackTrace();
        }
        return token;
    }

    
    //签名验证
    public static boolean verify(String token){
      try {
            JWTVerifier verifier = JWT.require(Algorithm.HMAC256(TOKEN_SECRET)).withIssuer("auth0").build();
            DecodedJWT jwt = verifier.verify(token);
            System.out.println("认证通过：");//控制台打印信息
            System.out.println("issuer: " + jwt.getIssuer());
            System.out.println("name: " + jwt.getClaim("name").asString());
            System.out.println("过期时间：      " + jwt.getExpiresAt());
            return true;
        } catch (Exception e){
            return false;
        }
    }
}

```

**3.创建token拦截器TokenInterceptor** **

```java
@Component
public class TokenInterceptor implements HandlerInterceptor {



    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)throws Exception{

        if(request.getMethod().equals("OPTIONS")){
            response.setStatus(HttpServletResponse.SC_OK);
            return true;
        }

        response.setCharacterEncoding("utf-8");

        String token = request.getHeader("token");
        if(token != null){
            boolean result = TokenUtil.verify(token);
            if(result){
                System.out.println("通过拦截器");
                return true;
            }
        }
        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json; charset=utf-8");
        PrintWriter out = null;
        try{
            JSONObject json = new JSONObject();
            json.put("success","false");
            json.put("msg","认证失败，未通过拦截器");
            json.put("code","50000");
            response.getWriter().append(json.toJSONString());
            System.out.println("认证失败，未通过拦截器");
            //        response.getWriter().write("50000");
        }catch (Exception e){
            e.printStackTrace();
            response.sendError(500);
            return false;
        }

        return false;

    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        response.setHeader("Access-Control-Allow-Credentials", "true");
        response.setHeader("Access-Control-Allow-Headers", "Authorization,Content-Type,X-Requested-With,token");
        response.setHeader("Access-Control-Allow-Methods", "GET,HEAD,OPTIONS,POST,PUT,DELETE");
        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Access-Control-Max-Age", "3600");
//        System.out.println("方法处理后==========");
    }
}
```

**4.创建入口拦截器IntercepterConfig**

```java
@Configuration
public class IntercepterConfig implements WebMvcConfigurer {

    private TokenInterceptor tokenInterceptor;

    //构造方法
    public IntercepterConfig(TokenInterceptor tokenInterceptor){
        this.tokenInterceptor = tokenInterceptor;
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry){
        List<String> excludePath = new ArrayList<>();//excludePath开放不需要token验证的资源
        excludePath.add("/register"); //注册
        excludePath.add("/api/login"); //登录
        excludePath.add("/logout"); //登出
        excludePath.add("/static/**");  //静态资源
        excludePath.add("/swagger-ui.html/**");  //静态资源
        excludePath.add("/assets/**");  //静态资源

        registry.addInterceptor(tokenInterceptor)
                .addPathPatterns("/**")
                .excludePathPatterns(excludePath);
        WebMvcConfigurer.super.addInterceptors(registry);

    }
    
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowCredentials(true)
                .allowedMethods("GET", "POST", "DELETE", "PUT", "PATCH", "OPTIONS", "HEAD")
                .maxAge(3600 * 24);
    }
}
```

**5.控制类的编写**

```Java
@PostMapping(value = "/api/login")
    @CrossOrigin
    public Result login (String name, String password, HttpServletResponse response){
        String token=null;
        Admin admin = service.login(name, password);
        if (admin != null) {
            token = TokenUtil.sign(admin);//用户存在就生成token
        }
        if(token != null){
            return new Result(200,"",token);
        }
        else {
            return new Result(400,"账号密码错误","");
        }

    }
```

