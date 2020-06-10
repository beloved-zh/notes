# 1、权限管理

## 1.1、权限管理概念

权限管理，一般指根据系统设置的安全规则或者安全策略，用户可以访问而且只能访问自己被授权的资源。权限管 理几乎出现在任何系统里面，前提是需要有用户和密码认证的系统。 

> 在权限管理的概念中，有两个非常重要的名词： 
>
> 认证：通过用户名和密码成功登陆系统后，让系统得到当前用户的角色身份。 
>
> 授权：系统根据当前用户的角色，给其授予对应可以操作的权限资源。 

## 1.2、完成权限管理需要三个对象

- 用户：主要包含用户名，密码和当前用户的角色信息，可实现认证操作。 

- 角色：主要包含角色名称，角色描述和当前角色拥有的权限信息，可实现授权操作。 

- 权限：权限也可以称为菜单，主要包含当前权限名称，url地址等信息，可实现动态展示菜单。 注：这三个对象中，用户与角色是多对多的关系，角色与权限是多对多的关系，用户与权限没有直接关系， 二者是通过角色来建立关联关系的。

# 2、SpringSecurity

## 2.1、SpringSecurity概念

 Spring Security是spring采用AOP思想，基于servlet过滤器实现的安全框架。它提供了完善的认证机制和方法级的 授权功能。是一款非常优秀的权限管理框架。 

## 2.2、入门

### 2.2.1、jar包介绍

- Spring Security：主要jar包功能介绍 
- spring-security-core.jar ：核心包，任何Spring Security功能都需要此包。 
- spring-security-web.jar： web工程必备，包含过滤器和相关的Web安全基础结构代码。 
- spring-security-config.jar： 用于解析xml配置文件，用到Spring Security的xml配置文件的就要用到此包。 
- spring-security-taglibs.jar： Spring Security提供的动态标签库，jsp页面可以用。

```xml
<!--
   jar包具有依赖关系
   spring-security只需要导入这两个即可
-->
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
    <version>5.3.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-taglibs</artifactId>
    <version>5.3.2.RELEASE</version>
</dependency>
```

![image-20200609151055549](http://image.beloved.ink/Typora/image-20200609151055549.png)

### 2.2.2、配置web.xml

```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">
	<display-name>Archetype Created Web Application</display-name>

    <!--Spring Security核心过滤器-->
    <!--注意过滤器名称必须叫springSecurityFilterChain-->
    <filter>
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

### 2.2.3、编写SpringSecurity核心配置文件

创建`spring-security.xml`文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:security="http://www.springframework.org/schema/security"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/security
       http://www.springframework.org/schema/security/spring-security.xsd">


    <!--配置SpringSecurity-->
    <!--
        auto-config="true" : 表示自动加载SpringSecurity的配置文件
        use-expressions="true" : 表示使用Spring的el表达式配置SpringSecurity
    -->
    <security:http auto-config="true" use-expressions="true">
        <!--拦截资源-->
        <!--
            pattern="/**" : 表示拦截所有资源
            access="hasAnyRole('ROLE_USER')" : 表示只有ROLE_USER角色才能访问资源
        -->
        <security:intercept-url pattern="/**" access="hasAnyRole('ROLE_USER')"/>
    </security:http>

    <!--模拟数据-->
    <!--设置Spring Security认证用户信息的来源-->
    <!--
        {noop} : SpringSecurity默认认证是加密的，{noop}表示不加密
    -->
    <security:authentication-manager>
        <security:authentication-provider>
            <security:user-service>
                <security:user name="user" password="{noop}user"
                               authorities="ROLE_USER" />
                <security:user name="admin" password="{noop}admin"
                               authorities="ROLE_ADMIN" />
            </security:user-service>
        </security:authentication-provider>
    </security:authentication-manager>

</beans>
```

### 2.2.4、 将spring-security.xml配置文件引入到applicationContext.xml中

```xml
<!--引入SpringSecurity主配置文件-->
<import resource="classpath:spring-security.xml"/>
```

### 2.2.5、启动测试

访问：http://localhost:8080被重定向到http://localhost:8080/login

![image-20200609163457938](http://image.beloved.ink/Typora/image-20200609163457938.png)

这个页面是SpringSecurity默认的登录页面

使用测试用户数据登录才能访问正常页面

控制台加载了很多默认的SpringSecurity过滤器

![image-20200609153659255](http://image.beloved.ink/Typora/image-20200609153659255.png)

# 3、Spring Security过滤器链

## 3.1、Spring Security常用过滤器介绍

**过滤器是一种典型的AOP思想**

- org.springframework.security.web.context.SecurityContextPersistenceFilter 

  > 首当其冲的一个过滤器，作用之重要，自不必多言。 SecurityContextPersistenceFilter主要是使用SecurityContextRepository在session中保存或更新一个 SecurityContext，并将SecurityContext给以后的过滤器使用，来为后续filter建立所需的上下文。 SecurityContext中存储了当前用户的认证以及权限信息。

- org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter 

  > 此过滤器用于集成SecurityContext到Spring异步执行机制中的WebAsyncManager 

- org.springframework.security.web.header.HeaderWriterFilter 

  > 向请求的Header中添加相应的信息,可在http标签内部使用security:headers来控制 

- org.springframework.security.web.csrf.CsrfFilter

  > csrf又称跨域请求伪造，SpringSecurity会对所有post请求验证是否包含系统生成的csrf的token信息， 如果不包含，则报错。起到防止csrf攻击的效果。 

- org.springframework.security.web.authentication.logout.LogoutFilter

  > 匹配URL为/logout的请求，实现用户退出,清除认证信息。

- org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter

  >  认证操作全靠这个过滤器，默认匹配URL为/login且必须为POST请求。

- org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter

  >  如果没有在配置文件中指定认证页面，则由该过滤器生成一个默认认证页面。

- org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter

  >  由此过滤器可以生产一个默认的退出登录页面

- org.springframework.security.web.authentication.www.BasicAuthenticationFilter 

  > 此过滤器会自动解析HTTP请求中头部名字为Authentication，且以Basic开头的头信息。

- org.springframework.security.web.savedrequest.RequestCacheAwareFilter 

  > 通过HttpSessionRequestCache内部维护了一个RequestCache，用于缓存HttpServletRequest 

- org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter 

  > 针对ServletRequest进行了一次包装，使得request具有更加丰富的API 

- org.springframework.security.web.authentication.AnonymousAuthenticationFilter

  > 当SecurityContextHolder中认证信息为空,则会创建一个匿名用户存入到SecurityContextHolder中。 spring security为了兼容未登录的访问，也走了一套认证流程，只不过是一个匿名的身份。 

- org.springframework.security.web.session.SessionManagementFilter 

  > SecurityContextRepository限制同一用户开启多个会话的数量 

- org.springframework.security.web.access.ExceptionTranslationFilter 

  > 异常转换过滤器位于整个springSecurityFilterChain的后方，用来转换整个链路中出现的异常 

- org.springframework.security.web.access.intercept.FilterSecurityInterceptor 

  > 获取所配置资源访问的授权信息，根据SecurityContextHolder中存储的用户信息来决定其是否有权 限。

## 3.2、spring security过滤器链加载原理

### 3.2.1、 DelegatingFilterProxy

我们在web.xml中配置了一个名称为springSecurityFilterChain的过滤器DelegatingFilterProxy，接下我直接对 DelegatingFilterProxy源码里重要代码进行说明，其中删减掉了一些不重要的代码。

```java
public class DelegatingFilterProxy extends GenericFilterBean {
    @Nullable
    private String contextAttribute;
    @Nullable
    private WebApplicationContext webApplicationContext;
    @Nullable
    private String targetBeanName;
    private boolean targetFilterLifecycle;
    @Nullable
    private volatile Filter delegate;//注：这个过滤器才是真正加载的过滤器
    private final Object delegateMonitor;
    
    //注：doFilter才是过滤器的入口，直接从这看！
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        Filter delegateToUse = this.delegate;
        if (delegateToUse == null) {
            synchronized(this.delegateMonitor) {
                delegateToUse = this.delegate;
                if (delegateToUse == null) {
                    WebApplicationContext wac = this.findWebApplicationContext();
                    if (wac == null) {
                        throw new IllegalStateException("No WebApplicationContext found: no
                        ContextLoaderListener or DispatcherServlet registered?");
                    }
                    //第一步：doFilter中最重要的一步，初始化上面私有过滤器属性delegate
                    delegateToUse = this.initDelegate(wac);
                }
                this.delegate = delegateToUse;
            }
        }
        //第三步：执行FilterChainProxy过滤器
        this.invokeDelegate(delegateToUse, request, response, filterChain);
     }
                                        
     //第二步：直接看最终加载的过滤器到底是谁
     protected Filter initDelegate(WebApplicationContext wac) throws ServletException {
         //debug得知targetBeanName为：springSecurityFilterChain
         String targetBeanName = this.getTargetBeanName();
         Assert.state(targetBeanName != null, "No target bean name set");
         //debug得知delegate对象为：FilterChainProxy
         Filter delegate = (Filter)wac.getBean(targetBeanName, Filter.class);
         if (this.isTargetFilterLifecycle()) {
             delegate.init(this.getFilterConfig());
         }
         return delegate;
    }
                                        
    protected void invokeDelegate(Filter delegate, ServletRequest request, ServletResponse
        response, FilterChain filterChain) throws ServletException, IOException {
        delegate.doFilter(request, response, filterChain);
    }
}

```

第二部debug结果

![image-20200609155813121](http://image.beloved.ink/Typora/image-20200609155813121.png)

由此可知，DelegatingFilterProxy通过springSecurityFilterChain这个名称，得到了一个FilterChainProxy过滤器， 最终在第三步执行了这个过滤器。

### 3.2.2、FilterChainProxy

```java
public class FilterChainProxy extends GenericFilterBean {
    private static final Log logger = LogFactory.getLog(FilterChainProxy.class);
    private static final String FILTER_APPLIED = FilterChainProxy.class.getName().concat(".APPLIED");
    private List<SecurityFilterChain> filterChains;
    private FilterChainProxy.FilterChainValidator filterChainValidator;
    private HttpFirewall firewall;
    
    //可以通过一个叫SecurityFilterChain的对象实例化出一个FilterChainProxy对象
    //这FilterChainProxy又是何方神圣？会不会是真正的过滤器链对象呢？先留着这个疑问！
    public FilterChainProxy(SecurityFilterChain chain) {
   	 	this(Arrays.asList(chain));
    }
    
    //又是SecurityFilterChain这家伙！嫌疑更大了！
    public FilterChainProxy(List<SecurityFilterChain> filterChains) {
        this.filterChainValidator = new FilterChainProxy.NullFilterChainValidator();
        this.firewall = new StrictHttpFirewall();
        this.filterChains = filterChains;
    }
    
    //注：直接从doFilter看
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        boolean clearContext = request.getAttribute(FILTER_APPLIED) == null;
        if (clearContext) {
            try {
                request.setAttribute(FILTER_APPLIED, Boolean.TRUE);
                this.doFilterInternal(request, response, chain);
            } finally {
                SecurityContextHolder.clearContext();
                request.removeAttribute(FILTER_APPLIED);
            }
        } else {
            //第一步：具体操作调用下面的doFilterInternal方法了
            this.doFilterInternal(request, response, chain);
        }
    }
    private void doFilterInternal(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        FirewalledRequest fwRequest = this.firewall.getFirewalledRequest((HttpServletRequest)request);
        HttpServletResponse fwResponse = this.firewall.getFirewalledResponse((HttpServletResponse)response);
        //第二步：封装要执行的过滤器链，那么多过滤器就在这里被封装进去了！
        List<Filter> filters = this.getFilters((HttpServletRequest)fwRequest);
        if (filters != null && filters.size() != 0) {
            FilterChainProxy.VirtualFilterChain vfc = new
            FilterChainProxy.VirtualFilterChain(fwRequest, chain, filters);
            //第四步：加载过滤器链
            vfc.doFilter(fwRequest, fwResponse);
        } else {
            if (logger.isDebugEnabled()) {
                logger.debug(UrlUtils.buildRequestUrl(fwRequest) + (filters == null ? " has no
                matching filters" : " has an empty filter list"));
            }
            fwRequest.reset();
            chain.doFilter(fwRequest, fwResponse);
        }
    }
                                                                    
    private List<Filter> getFilters(HttpServletRequest request) {
        Iterator var2 = this.filterChains.iterator();
        //第三步：封装过滤器链到SecurityFilterChain中！
        SecurityFilterChain chain;
        do {
            if (!var2.hasNext()) {
            	return null;
            }
            chain = (SecurityFilterChain)var2.next();
        } while(!chain.matches(request));
        return chain.getFilters();
    }
}

```

第二步debug结果如下图所示

![image-20200609160427410](http://image.beloved.ink/Typora/image-20200609160427410.png)

再看第三步，怀疑这么久！原来这些过滤器还真是都被封装进SecurityFilterChain中了。

### 3.2.3 SecurityFilterChain 

最后看SecurityFilterChain，这是个接口，实现类也只有一个，这才是web.xml中配置的过滤器链对象！

```java
//接口
public interface SecurityFilterChain {
    boolean matches(HttpServletRequest var1);
    List<Filter> getFilters();
}
//实现类
public final class DefaultSecurityFilterChain implements SecurityFilterChain {
    private static final Log logger = LogFactory.getLog(DefaultSecurityFilterChain.class);
    private final RequestMatcher requestMatcher;
    private final List<Filter> filters;
    
    public DefaultSecurityFilterChain(RequestMatcher requestMatcher, Filter... filters) {
    	this(requestMatcher, Arrays.asList(filters));
    }
    
    public DefaultSecurityFilterChain(RequestMatcher requestMatcher, List<Filter> filters) {
        logger.info("Creating filter chain: " + requestMatcher + ", " + filters);
        this.requestMatcher = requestMatcher;
        this.filters = new ArrayList(filters);
    }
    public RequestMatcher getRequestMatcher() {
    	return this.requestMatcher;
    }
    
    public List<Filter> getFilters() {
    	return this.filters;
    }
    
    public boolean matches(HttpServletRequest request) {
    	return this.requestMatcher.matches(request);
    }
    
    public String toString() {
    	return "[ " + this.requestMatcher + ", " + this.filters + "]";
    }
}
```

# 4、SpringSecurity使用自定义认证页面

## 4.1、配置自定义认证信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:security="http://www.springframework.org/schema/security"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/security
       http://www.springframework.org/schema/security/spring-security.xsd">


    <!--配置SpringSecurity-->

    <!--释放静态资源-->
    <security:http pattern="/css/**" security="none" />
    <security:http pattern="/img/**" security="none" />
    <security:http pattern="/plugins/**" security="none" />

    <!--
        auto-config="true" : 表示自动加载SpringSecurity的配置文件
        use-expressions="true" : 表示使用Spring的el表达式配置SpringSecurity
    -->
    <security:http auto-config="true" use-expressions="true">
        <!--让登录页面可以匿名访问-->
        <security:intercept-url pattern="/login.jsp" access="permitAll()" />


        <!--
            拦截资源
            pattern="/**" : 表示拦截所有资源
            access="hasAnyRole('ROLE_USER')" : 表示只有ROLE_USER角色才能访问资源
        -->
        <security:intercept-url pattern="/**" access="hasAnyRole('ROLE_USER')"/>

        <!--
            配置认证信息。自定义登录页面
            login-page ： 登录页面
            login-processing-url ： 登录请求 使用SpringSecurity默认提供的
            default-target-url ：认证成功的页面
            authentication-failure-url ： 认证失败页面
            username-parameter ： 登录页面的用户名name  默认username
            password-parameter ： 登录页面的密码name  默认password
        -->
        <security:form-login login-page="/login.jsp"
                             login-processing-url="/login"
                             default-target-url="/index.jsp"
                             authentication-failure-url="/failer.jsp"
                             username-parameter="username"
                             password-parameter="password" />

        <!--
            配置退出登录信息
            logout-url ： 退出请求  SpringSecurity默认提供
            logout-success-url ： 退出之后的页面
        -->
        <security:logout logout-url="/logout"
                         logout-success-url="/login.jsp" />

    </security:http>

    <!--模拟数据-->
    <!--设置Spring Security认证用户信息的来源-->
    <!--
        {noop} : SpringSecurity默认认证是加密的，{noop}表示不加密
    -->
    <security:authentication-manager>
        <security:authentication-provider>
            <security:user-service>
                <security:user name="user" password="{noop}user"
                               authorities="ROLE_USER" />
                <security:user name="admin" password="{noop}admin"
                               authorities="ROLE_ADMIN" />
            </security:user-service>
        </security:authentication-provider>
    </security:authentication-manager>

</beans>
```

修改`login.jsp`页面

![image-20200609163103493](http://image.beloved.ink/Typora/image-20200609163103493.png)

重新启动项目，测试

![image-20200609163146770](http://image.beloved.ink/Typora/image-20200609163146770.png)

使用user用户登录

![image-20200609163241603](http://image.beloved.ink/Typora/image-20200609163241603.png)

这是SpringSecurity中的权限不足！这个异常怎么来的？SpringSecurity内置认证页面源 码中的那个_csrf隐藏input。问题就在这了！

## 4.2、SpringSecurity的csrf防护机制

**CSRF（Cross-site request forgery）跨站请求伪造，是一种难以防范的网络攻击方式。**

### 4.2.1 SpringSecurity中CsrfFilter过滤器说明

```java
public final class CsrfFilter extends OncePerRequestFilter {
    public static final RequestMatcher DEFAULT_CSRF_MATCHER = new CsrfFilter.DefaultRequiresCsrfMatcher();
    private final Log logger = LogFactory.getLog(this.getClass());
    private final CsrfTokenRepository tokenRepository;
    private RequestMatcher requireCsrfProtectionMatcher;
    private AccessDeniedHandler accessDeniedHandler;
    
    public CsrfFilter(CsrfTokenRepository csrfTokenRepository) {
        this.requireCsrfProtectionMatcher = DEFAULT_CSRF_MATCHER;
        this.accessDeniedHandler = new AccessDeniedHandlerImpl();
        Assert.notNull(csrfTokenRepository, "csrfTokenRepository cannot be null");
        this.tokenRepository = csrfTokenRepository;
    }
    
    //通过这里可以看出SpringSecurity的csrf机制把请求方式分成两类来处理
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response,FilterChain filterChain) throws ServletException, IOException {
        request.setAttribute(HttpServletResponse.class.getName(), response);
        CsrfToken csrfToken = this.tokenRepository.loadToken(request);
        boolean missingToken = csrfToken == null;
        
        if (missingToken) {
            csrfToken = this.tokenRepository.generateToken(request);
            this.tokenRepository.saveToken(csrfToken, request, response);
        }
        
        request.setAttribute(CsrfToken.class.getName(), csrfToken);
        request.setAttribute(csrfToken.getParameterName(), csrfToken);
        //第一类："GET", "HEAD", "TRACE", "OPTIONS"四类请求可以直接通过
        if (!this.requireCsrfProtectionMatcher.matches(request)) {
        	filterChain.doFilter(request, response);
        } else {
            //第二类：除去上面四类，包括POST都要被验证携带token才能通过
            String actualToken = request.getHeader(csrfToken.getHeaderName());
            if (actualToken == null) {
            	actualToken = request.getParameter(csrfToken.getParameterName());
            }
            
            if (!csrfToken.getToken().equals(actualToken)) {
                if (this.logger.isDebugEnabled()) {
                    this.logger.debug("Invalid CSRF token found for " +UrlUtils.buildFullRequestUrl(request));
                }
                if (missingToken) {
                    this.accessDeniedHandler.handle(request, response, new
                    MissingCsrfTokenException(actualToken));
                } else {
                    this.accessDeniedHandler.handle(request, response, new
                    InvalidCsrfTokenException(csrfToken, actualToken));
                }
            } else {
            	filterChain.doFilter(request, response);
            }
        }
    }
    
    public void setRequireCsrfProtectionMatcher(RequestMatcher requireCsrfProtectionMatcher) {
        Assert.notNull(requireCsrfProtectionMatcher, "requireCsrfProtectionMatcher cannot be null");
        this.requireCsrfProtectionMatcher = requireCsrfProtectionMatcher;
    }
    
    public void setAccessDeniedHandler(AccessDeniedHandler accessDeniedHandler) {
        Assert.notNull(accessDeniedHandler, "accessDeniedHandler cannot be null");
        this.accessDeniedHandler = accessDeniedHandler;
    }
    
    private static final class DefaultRequiresCsrfMatcher implements RequestMatcher {
        private final HashSet<String> allowedMethods;
        private DefaultRequiresCsrfMatcher() {
        	this.allowedMethods = new HashSet(Arrays.asList("GET", "HEAD", "TRACE", "OPTIONS"));
        }
        public boolean matches(HttpServletRequest request) {
        	return !this.allowedMethods.contains(request.getMethod());
        }
    }
}
```

通过源码分析，明白了，**自己的认证页面，请求方式为POST，但却没有携带token，所以才出现了403权限不 足的异常。**

那么如何处理这个问题呢？ 

**方式一：直接禁用csrf，不推荐。**

**方式二：在认证页面携带token请求。**

### 4.2.2 禁用csrf防护机制

在SpringSecurity主配置文件中添加禁用crsf防护的配置。

```xml
<!--取出csrf拦截过滤器-->
<security:csrf disabled="true" />
```

### 4.2.3 在认证页面携带token请求

```jsp
<%@ taglib prefix="security" uri="http://www.springframework.org/security/tags" %>
<security:csrfInput/>
```

![image-20200609165041952](http://image.beloved.ink/Typora/image-20200609165041952.png)

**注：HttpSessionCsrfTokenRepository对象负责生成token并放入session域中。**

## 4.3、退出登录

```xml
<!--
    配置退出登录信息
    logout-url ： 退出请求  SpringSecurity默认提供
    logout-success-url ： 退出之后的页面
-->
<security:logout logout-url="/logout"
                 logout-success-url="/login.jsp" />
```

![image-20200609165831562](http://image.beloved.ink/Typora/image-20200609165831562.png)

**注意：开启csrf防护，退出登录必须是post请求，携带token，不能使用get请求**

## 4.4、显示当前认证用户名 

- 前端获取

  ```html
  <span class="hidden-xs">
  	<security:authentication property="principal.username" />
  </span>
  或者
  <span class="hidden-xs">
  	<security:authentication property="name" />
  </span>
  ```

- 后端获取

  ```java
  // 方式一
  SecurityContextHolder.getContext().getAuthentication().getName();
  // 方式二
  String username = ((SysUser)SecurityContextHolder.getContext().getAuthentication().getPrincipal()).getUsername();
  ```

# 5、SpringSecurity使用数据库数据完成认证

## 5.1 认证流程分析

### UsernamePasswordAuthenticationFilter

先看主要负责认证的过滤器UsernamePasswordAuthenticationFilter，有删减，注意注释

```java
public class UsernamePasswordAuthenticationFilter extends AbstractAuthenticationProcessingFilter{
    public static final String SPRING_SECURITY_FORM_USERNAME_KEY = "username";
    public static final String SPRING_SECURITY_FORM_PASSWORD_KEY = "password";
    private String usernameParameter = "username";
    private String passwordParameter = "password";
    private boolean postOnly = true;
    
    public UsernamePasswordAuthenticationFilter() {
    	super(new AntPathRequestMatcher("/login", "POST"));
    }
    
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException {
        //必须为POST请求
        if (this.postOnly && !request.getMethod().equals("POST")) {
            throw new AuthenticationServiceException("Authentication method not supported: " + request.getMethod());
        } else {
            String username = this.obtainUsername(request);
            String password = this.obtainPassword(request);
            if (username == null) {
            	username = "";
            }
            
            if (password == null) {
            	password = "";
            }
            
            username = username.trim();
            //将填写的用户名和密码封装到了UsernamePasswordAuthenticationToken中
            UsernamePasswordAuthenticationToken authRequest = new
            UsernamePasswordAuthenticationToken(username, password);
            this.setDetails(request, authRequest);
            //调用AuthenticationManager对象实现认证
            return this.getAuthenticationManager().authenticate(authRequest);
        }
    }
}
```

### AuthenticationManager

由上面源码得知，真正认证操作在AuthenticationManager里面！

 然后看AuthenticationManager的实现类ProviderManager：

```java
public class ProviderManager implements AuthenticationManager, MessageSourceAware,InitializingBean {
    private static final Log logger = LogFactory.getLog(ProviderManager.class);
    private AuthenticationEventPublisher eventPublisher;
    private List<AuthenticationProvider> providers;
    protected MessageSourceAccessor messages;
    private AuthenticationManager parent;
    private boolean eraseCredentialsAfterAuthentication;
    
    //注意AuthenticationProvider这个对象，SpringSecurity针对每一种认证，什么qq登录啊，
    //用户名密码登陆啊，微信登录啊都封装了一个AuthenticationProvider对象。
    public ProviderManager(List<AuthenticationProvider> providers) {
    	this(providers, (AuthenticationManager)null);
    }
    
    public Authentication authenticate(Authentication authentication) throwsAuthenticationException {
        Class<? extends Authentication> toTest = authentication.getClass();
        AuthenticationException lastException = null;
        AuthenticationException parentException = null;
        Authentication result = null;
        Authentication parentResult = null;
        boolean debug = logger.isDebugEnabled();
        Iterator var8 = this.getProviders().iterator();
        
        //循环所有AuthenticationProvider，匹配当前认证类型。
        while(var8.hasNext()) {
            AuthenticationProvider provider = (AuthenticationProvider)var8.next();
            if (provider.supports(toTest)) {
                if (debug) {
                    logger.debug("Authentication attempt using " + provider.getClass().getName());
                }
                try {
                    //找到了对应认证类型就继续调用AuthenticationProvider对象完成认证业务。
                    result = provider.authenticate(authentication);
                    if (result != null) {
                        this.copyDetails(authentication, result);
                        break;
                    }
                } catch (AccountStatusException var13) {
                    this.prepareException(var13, authentication);
                    throw var13;
                } catch (InternalAuthenticationServiceException var14) {
                    this.prepareException(var14, authentication);
                    throw var14;
                } catch (AuthenticationException var15) {
                	lastException = var15;
                }
            }
        }
        
        if (result == null && this.parent != null) {
            try {
            	result = parentResult = this.parent.authenticate(authentication);
            } catch (ProviderNotFoundException var11) {
            } catch (AuthenticationException var12) {
                parentException = var12;
                lastException = var12;
            }
        }
        
        if (result != null) {
            if (this.eraseCredentialsAfterAuthentication && result instanceof CredentialsContainer) {
            	((CredentialsContainer)result).eraseCredentials();
            }
            
            if (parentResult == null) {
            	this.eventPublisher.publishAuthenticationSuccess(result);
            }
            return result;
        } else {
            if (lastException == null) {
                lastException = new ProviderNotFoundException(this.messages.getMessage("ProviderManager.providerNotFound", new Object[]{toTest.getName()}, "No AuthenticationProvider found for {0}"));
            }
            
            if (parentException == null) {
            	this.prepareException((AuthenticationException)lastException, authentication);
            }
            throw lastException;
        }
    }
}
```

### AbstractUserDetailsAuthenticationProvider

咱们继续再找到AuthenticationProvider的实现类AbstractUserDetailsAuthenticationProvider：

```java
public class DaoAuthenticationProvider extends AbstractUserDetailsAuthenticationProvider {
    private static final String USER_NOT_FOUND_PASSWORD = "userNotFoundPassword";
    private PasswordEncoder passwordEncoder;
    private volatile String userNotFoundEncodedPassword;
    private UserDetailsService userDetailsService;
    private UserDetailsPasswordService userDetailsPasswordService;
    
    protected final UserDetails retrieveUser(String username,UsernamePasswordAuthenticationToken authentication) throws AuthenticationException {
        this.prepareTimingAttackProtection();
        try {
            //重点来了！主要就在这里了！
            //可别忘了，咱们为什么要翻源码，是想用自己数据库中的数据实现认证操作啊！
            //UserDetails就是SpringSecurity自己的用户对象。
            //this.getUserDetailsService()其实就是得到UserDetailsService的一个实现类
            //loadUserByUsername里面就是真正的认证逻辑
            //也就是说我们可以直接编写一个UserDetailsService的实现类，告诉SpringSecurity就可以了！
            //loadUserByUsername方法中只需要返回一个UserDetails对象即可
            UserDetails loadedUser = this.getUserDetailsService().loadUserByUsername(username);
            
            //若返回null，就抛出异常，认证失败。
            if (loadedUser == null) {
                throw new InternalAuthenticationServiceException("UserDetailsService returned null, which is an interface contract violation");
            } else {
                //若有得到了UserDetails对象，返回即可。
                return loadedUser;
            }
        } catch (UsernameNotFoundException var4) {
            this.mitigateAgainstTimingAttack(authentication);
            throw var4;
        } catch (InternalAuthenticationServiceException var5) {
        	throw var5;
        } catch (Exception var6) {
        	throw new InternalAuthenticationServiceException(var6.getMessage(), var6);
        }
    }
}
```

### AbstractUserDetailsAuthenticationProvider中authenticate返回值

按理说到此已经知道自定义认证方法的怎么写了，但咱们把返回的流程也大概走一遍，上面不是说到返回了一个 UserDetails对象对象吗？跟着它，就又回到了AbstractUserDetailsAuthenticationProvider对象中authenticate方 法的最后一行了。

```java
public abstract class AbstractUserDetailsAuthenticationProvider implements AuthenticationProvider, InitializingBean, MessageSourceAware {
    
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        //最后一行返回值，调用了createSuccessAuthentication方法，此方法就在下面！
        return this.createSuccessAuthentication(principalToReturn, authentication, user);
    }
    
    //咿！？怎么又封装了一次UsernamePasswordAuthenticationToken，开局不是已经封装过了吗？
    protected Authentication createSuccessAuthentication(Object principal, Authentication authentication, UserDetails user) {
        //那就从构造方法点进去看看，这才干啥了。
        UsernamePasswordAuthenticationToken result = new
        UsernamePasswordAuthenticationToken(principal, authentication.getCredentials(),
        this.authoritiesMapper.mapAuthorities(user.getAuthorities()));
        result.setDetails(authentication.getDetails());
        return result;
    }
}
```

### UsernamePasswordAuthenticationToken

来到UsernamePasswordAuthenticationToken对象发现里面有两个构造方法

```java
public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {
    private static final long serialVersionUID = 510L;
    private final Object principal;
    private Object credentials;
    
    //认证成功前，调用的是这个带有两个参数的。
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
        super((Collection)null);
        this.principal = principal;
        this.credentials = credentials;
        this.setAuthenticated(false);
    }
    
    //认证成功后，调用的是这个带有三个参数的。
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials,Collection<? extends GrantedAuthority> authorities) {
        //看看父类干了什么！
        super(authorities);
        this.principal = principal;
        this.credentials = credentials;
        super.setAuthenticated(true);
    }
}
```

### AbstractAuthenticationToken

再点进去super(authorities)看看：

```java
public abstract class AbstractAuthenticationToken implements Authentication,CredentialsContainer {
    private final Collection<GrantedAuthority> authorities;
    private Object details;
    private boolean authenticated = false;
    
    public AbstractAuthenticationToken(Collection<? extends GrantedAuthority> authorities) {
        //这时两个参数那个分支！
        if (authorities == null) {
        	this.authorities = AuthorityUtils.NO_AUTHORITIES;
        } else {
            //三个参数的，看这里！
            Iterator var2 = authorities.iterator();
            //原来是多个了添加权限信息的步骤
            GrantedAuthority a;
            do {
                if (!var2.hasNext()) {
                    ArrayList<GrantedAuthority> temp = new ArrayList(authorities.size());
                    temp.addAll(authorities);
                    this.authorities = Collections.unmodifiableList(temp);
                    return;
                }
                a = (GrantedAuthority)var2.next();
            } while(a != null);
            //若没有权限信息，是会抛出异常的！
            throw new IllegalArgumentException("Authorities collection cannot contain any null elements");
        }
    }
}
```

由此，咱们需要牢记自定义认证业务逻辑返回的UserDetails对象中一定要放置权限信息啊！

咱们回到最初的地方UsernamePasswordAuthenticationFilter，你看好看了，这可是个过滤器，咱们分析这么 久，都没提到doFilter方法，你不觉得心里不踏实？

可是这里面也没有doFilter呀？那就从父类找！

### AbstractAuthenticationProcessingFilter

点开AbstractAuthenticationProcessingFilter，删掉不必要的代码！

```java
public abstract class AbstractAuthenticationProcessingFilter extends GenericFilterBean implements ApplicationEventPublisherAware, MessageSourceAware {
    
    //doFilter再次！
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest)req;
        HttpServletResponse response = (HttpServletResponse)res;
        if (!this.requiresAuthentication(request, response)) {
        	chain.doFilter(request, response);
        } else {
            if (this.logger.isDebugEnabled()) {
            	this.logger.debug("Request is to process authentication");
            }
            Authentication authResult;
            try {
                authResult = this.attemptAuthentication(request, response);
                if (authResult == null) {
                	return;
                }
                
                this.sessionStrategy.onAuthentication(authResult, request, response);
            } catch (InternalAuthenticationServiceException var8) {
                this.logger.error("An internal error occurred while trying to authenticate the user.", var8);
                this.unsuccessfulAuthentication(request, response, var8);
                return;
            } catch (AuthenticationException var9) {
                this.unsuccessfulAuthentication(request, response, var9);
                return;
            }
            
            if (this.continueChainBeforeSuccessfulAuthentication) {
            	chain.doFilter(request, response);
            }
            
            this.successfulAuthentication(request, response, chain, authResult);
        }
    }
    
    protected boolean requiresAuthentication(HttpServletRequest request, HttpServletResponse response) {
    	return this.requiresAuthenticationRequestMatcher.matches(request);
    }
    
    //成功走successfulAuthentication
    protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response, FilterChain chain, Authentication authResult) throws IOException, ServletException {
        if (this.logger.isDebugEnabled()) {
            this.logger.debug("Authentication success. Updating SecurityContextHolder to contain: " + authResult);
        }
        
        //认证成功，将认证信息存储到SecurityContext中！
        SecurityContextHolder.getContext().setAuthentication(authResult);
        //登录成功调用rememberMeServices
        this.rememberMeServices.loginSuccess(request, response, authResult);
        if (this.eventPublisher != null) {
            this.eventPublisher.publishEvent(new InteractiveAuthenticationSuccessEvent(authResult, this.getClass()));
        }
        this.successHandler.onAuthenticationSuccess(request, response, authResult);
    }
    
    //失败走unsuccessfulAuthentication
    protected void unsuccessfulAuthentication(HttpServletRequest request, HttpServletResponse response, AuthenticationException failed) throws IOException, ServletException {
        SecurityContextHolder.clearContext();
        if (this.logger.isDebugEnabled()) {
            this.logger.debug("Authentication request failed: " + failed.toString(), failed);
            this.logger.debug("Updated SecurityContextHolder to contain null Authentication");
            this.logger.debug("Delegating to authentication failure handler " + this.failureHandler);
        }
        this.rememberMeServices.loginFail(request, response);
        this.failureHandler.onAuthenticationFailure(request, response, failed);
    }
}
```

可见AbstractAuthenticationProcessingFilter这个过滤器对于认证成功与否，做了两个分支，成功执行 

successfulAuthentication，失败执行unsuccessfulAuthentication。 

在successfulAuthentication内部，将认证信息存储到了SecurityContext中。并调用了loginSuccess方法，这就是 常见的“记住我”功能！此功能具体应用，咱们后续再研究！

## 5.2、初步实现认证功能

### 5.2.1、让我们自己的UserService接口继承UserDetailsService

毕竟SpringSecurity是只认UserDetailsService的：

```java
public interface UserService extends UserDetailsService {

}
```

### 5.2.2、编写loadUserByUsername业务

```java
@Service
@Transactional
public class UserServiceImpl implements UserService {
    @Autowired
    private UserDao userDao;

    @Autowired
    private RoleService roleService;

    /**
     *
     * @param username 用户在浏览器输入的用户名
     * @return UserDetails 是SpringSecurity自己的用户对象
     * return null 就是认证失败
     * @throws UsernameNotFoundException
     */
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        try {
            // 根据用户名查询
            SysUser sysUser = userDao.findByName(username);

            if (sysUser == null){
                return null;
            }

            List<GrantedAuthority> authorities = new ArrayList<GrantedAuthority>();
            // 模拟权限
            authorities.add(new SimpleGrantedAuthority("ROLE_USER"));

            UserDetails userDetails = new User(sysUser.getUsername(),"{noop}"+sysUser.getPassword(),authorities);

            return userDetails;
        }catch (Exception e){
            e.printStackTrace();

            //认证失败
            return null;
        }
    }
}
```

### 5.2.3、在SpringSecurity主配置文件中指定认证使用的业务对象

```xml
<!--设置Spring Security认证用户信息的来源-->
<!--
    {noop} : SpringSecurity默认认证是加密的，{noop}表示不加密
-->
<security:authentication-manager>
    <security:authentication-provider user-service-ref="userServiceImpl">
    </security:authentication-provider>
</security:authentication-manager>
```

## 5.3、加密认证

### 5.3.1、在IOC容器中提供加密对象

```xml
<!--加密对象放入ioc容器-->
<bean id="passwordEncoder"
      class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>

<!--设置Spring Security认证用户信息的来源-->
<!--
    {noop} : SpringSecurity默认认证是加密的，{noop}表示不加密
-->
<security:authentication-manager>
    <security:authentication-provider user-service-ref="userServiceImpl">
        <!--指定认证使用的加密对象-->
        <security:password-encoder ref="passwordEncoder"/>
    </security:authentication-provider>
</security:authentication-manager>
```

### 5.3.2、修改认证方法

去掉{noop}

```java
/**
 *
 * @param username 用户在浏览器输入的用户名
 * @return UserDetails 是SpringSecurity自己的用户对象
 * @throws UsernameNotFoundException
 */
@Override
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
    try {
        // 根据用户名查询
        SysUser sysUser = userDao.findByName(username);

        if (sysUser == null){
            return null;
        }

        List<GrantedAuthority> authorities = new ArrayList<GrantedAuthority>();
        List<SysRole> roles = sysUser.getRoles();
        for (SysRole role : roles) {
            authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
        }

        UserDetails userDetails = new User(sysUser.getUsername(),sysUser.getPassword(),authorities);

        return userDetails;
    }catch (Exception e){
        e.printStackTrace();

        //认证失败
        return null;
    }
}
```

### 5.3.3、修改添加用户的操

```java
@Service
@Transactional
public class UserServiceImpl implements UserService {
    @Autowired
    private UserDao userDao;
    
    @Autowired
    private BCryptPasswordEncoder passwordEncoder;

    @Override
    public void save(SysUser user) {
        //对密码进行加密，然后再入库
        user.setPassword(passwordEncoder.encode(user.getPassword()));
        userDao.save(user);
    }
}
```

### 5.3.4、手动将数据库中用户密码改为加密后的密文

**使用此方法，获得密文，随便一个都可以**

```java
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

public class MyTest {

    //$2a$10$.fNiFRZLUBEI1gVkB.ZGdO1nHtlQ/VdCuEG9PX11cOtm9HqmhvnWu
    //$2a$10$UQKFXB6LBMqJ1/fpHrDMPeKwJSz4DNIHNB034ds2wEm5FxHVfsuWS
    //$2a$10$0H/HwJxcee2A5Vp0av1yMenpGSGMjcsbUfD2B6/iq2mj1v9h0tQeq
    //相同数据每次加密结果不一致
    public static void main(String[] args) {

        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();

        System.out.println(passwordEncoder.encode("123456"));

    }

}
```

![image-20200609183325447](http://image.beloved.ink/Typora/image-20200609183325447.png)

# 6、设置用户状态

## 6.1、源码分析

用户认证业务里，我们封装User对象时，选择了三个构造参数的构造方法，其实还有另一个构造方法：

```java
public User(String username, String password, boolean enabled,
      boolean accountNonExpired, boolean credentialsNonExpired,
      boolean accountNonLocked, Collection<? extends GrantedAuthority> authorities) {

   if (((username == null) || "".equals(username)) || (password == null)) {
      throw new IllegalArgumentException(
            "Cannot pass null or empty values to constructor");
   }

   this.username = username;
   this.password = password;
   this.enabled = enabled;
   this.accountNonExpired = accountNonExpired;
   this.credentialsNonExpired = credentialsNonExpired;
   this.accountNonLocked = accountNonLocked;
   this.authorities = Collections.unmodifiableSet(sortAuthorities(authorities));
}
```

可以看到，这个构造方法里多了四个布尔类型的构造参数，其实我们使用的三个构造参数的构造方法里这四个布尔 值默认都被赋值为了true，那么这四个布尔值到底是何意思呢？ 

- boolean enabled 是否可用 
- boolean accountNonExpired 账户是否失效 
- boolean credentialsNonExpired 秘密是否失效 
- boolean accountNonLocked 账户是否锁定

## 6.2、改造loadUserByUsername

是否可用使用数据库字段，其余默认true

```java
/**
 *
 * @param username 用户在浏览器输入的用户名
 * @return UserDetails 是SpringSecurity自己的用户对象
 * @throws UsernameNotFoundException
 */
@Override
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
    try {
        // 根据用户名查询
        SysUser sysUser = userDao.findByName(username);

        if (sysUser == null){
            return null;
        }

        List<GrantedAuthority> authorities = new ArrayList<GrantedAuthority>();
        List<SysRole> roles = sysUser.getRoles();
        for (SysRole role : roles) {
            authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
        }

        UserDetails userDetails = new User(
                sysUser.getUsername(),
                sysUser.getPassword(),
                sysUser.getStatus() == 1,
                true,
                true,
                true,
                authorities);

        return userDetails;
    }catch (Exception e){
        e.printStackTrace();

        //认证失败
        return null;
    }
}
```

![image-20200610115324993](http://image.beloved.ink/Typora/image-20200610115324993.png)

测试user登录失败

# 7、remember me

## 7.1、 记住我功能原理分析

找到AbstractRememberMeServices对象的loginSuccess方法：

```java
public abstract class AbstractRememberMeServices implements RememberMeServices,InitializingBean, LogoutHandler {
    public final void loginSuccess(HttpServletRequest request, HttpServletResponse response,Authentication successfulAuthentication) {
        // 判断是否勾选记住我
        // 注意：这里this.parameter点进去是上面的private String parameter = "remember-me";
        if (!this.rememberMeRequested(request, this.parameter)) {
        	this.logger.debug("Remember-me login not requested.");
        } else {
            //若勾选就调用onLoginSuccess方法
            this.onLoginSuccess(request, response, successfulAuthentication);
        }
    }
}
```

再点进去上面if判断中的rememberMeRequested方法，还在当前类中：

```java
protected boolean rememberMeRequested(HttpServletRequest request, String parameter) {
    if (this.alwaysRemember) {
    	return true;
    } else {
        // 从上面的字parameter的值为"remember-me"
        // 也就是说，此功能提交的属性名必须为"remember-me"
        String paramValue = request.getParameter(parameter);
        // 这里我们看到属性值可以为：true，on，yes，1。
        if (paramValue != null && (paramValue.equalsIgnoreCase("true") ||
            paramValue.equalsIgnoreCase("on") || paramValue.equalsIgnoreCase("yes") ||
            paramValue.equals("1"))) {
            //满足上面条件才能返回true
            return true;
        } else {
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Did not send remember-me cookie (principal did not set parameter '" + parameter + "')");
            }
            return false;
        }
    }
}
```

如果上面方法返回true，就表示页面勾选了记住我选项了。 

继续顺着调用的方法找到PersistentTokenBasedRememberMeServices的onLoginSuccess方法：

```java
public class PersistentTokenBasedRememberMeServices extends AbstractRememberMeServices {
    protected void onLoginSuccess(HttpServletRequest request, HttpServletResponse response,Authentication successfulAuthentication) {
        // 获取用户名
        String username = successfulAuthentication.getName();
        this.logger.debug("Creating new persistent login for user " + username);
        //创建记住我的token
        PersistentRememberMeToken persistentToken = new PersistentRememberMeToken(username,this.generateSeriesData(), this.generateTokenData(), new Date());
        try {
            //将token持久化到数据库
            this.tokenRepository.createNewToken(persistentToken);
            //将token写入到浏览器的Cookie中
            this.addCookie(persistentToken, request, response);
        } catch (Exception var7) {
        	this.logger.error("Failed to save persistent token ", var7);
        }
    }
}
```

## 7.2、记住我功能页面代码

**注意name和value属性的值不要写错哦！**

![image-20200610120709375](http://image.beloved.ink/Typora/image-20200610120709375.png)

## 7.3、开启remember me过滤器

```xml
<!--设置可以用spring的el表达式配置Spring Security并自动生成对应配置组件（过滤器）-->
<security:http auto-config="true" use-expressions="true">
    <!--省略其余配置-->
    <!--开启remember me过滤器，设置token存储时间为60秒-->
    <security:remember-me token-validity-seconds="60"/>
</security:http>
```

**说明：RememberMeAuthenticationFilter中功能非常简单，会在打开浏览器时，自动判断是否认证，如果没有则 调用autoLogin进行自动认证。**

## 7.4、remember me安全性分析

记住我功能方便是大家看得见的，但是安全性却令人担忧。因为Cookie毕竟是保存在客户端的，很容易盗取，而且 cookie的值还与用户名、密码这些敏感数据相关，虽然加密了，但是将敏感信息存在客户端，还是不太安全。那么 这就要提醒喜欢使用此功能的，用完网站要及时手动退出登录，清空认证信息。 此外，SpringSecurity还提供了remember me的另一种相对更安全的实现机制 :在客户端的cookie中，仅保存一个 无意义的加密串（与用户名、密码等敏感数据无关），然后在db中保存该加密串-用户信息的对应关系，自动登录 时，用cookie中的加密串，到db中验证，如果通过，自动登录才算通过。

## 7.5、持久化remember me信息

**创建一张表，注意这张表的名称和字段都是固定的，是SpringSecurity提供的，不要修改。**

```mysql
CREATE TABLE `persistent_logins` (
    `username` varchar(64) NOT NULL,
    `series` varchar(64) NOT NULL,
    `token` varchar(64) NOT NULL,
    `last_used` timestamp NOT NULL,
    PRIMARY KEY (`series`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

然后将spring-security.xml中 改为：

```xml
<!--
    开启remember me过滤器
    token-validity-seconds="60"：设置token存储时间为60秒
    data-source-ref="dataSource"：持久化token到数据库。dataSource来自ioc容器
    remember-me-parameter：记住我前端name名。默认remember-me
-->
<security:remember-me token-validity-seconds="60"
                        data-source-ref="dataSource"
                        remember-me-parameter="remember-me"/>
```

测试登录。使用记住我功能。数据库持久化一条信息

![image-20200610122132787](http://image.beloved.ink/Typora/image-20200610122132787.png)

token自动过期持久化的数据会一直在。当手动退出时，该用户所有的持久化token都会删除

# 8、授权管理

## 8.1、准备工作

创建001和002用户

![image-20200610125120211](http://image.beloved.ink/Typora/image-20200610125120211.png)

创建ROLE_PRODUCT和ROLE_ORDER角色

![image-20200610125218434](http://image.beloved.ink/Typora/image-20200610125218434.png)

为001和002用户授权

![image-20200610125333877](http://image.beloved.ink/Typora/image-20200610125333877.png)

![](http://image.beloved.ink/Typora/image-20200610125401015.png)

## 8.2、动态展示菜单

修改菜单页面，对每个菜单通过SpringSecurity标签库指定访问所需角色

```html
<%-- 只有管理员可以操作系统管理 --%>
<security:authorize access="hasAnyRole('ROLE_ADMIN')">
	<li class="treeview"><a href="#"> <i class="fa fa-cogs"></i>
    	<span>系统管理</span> <span class="pull-right-container"> <i
                class="fa fa-angle-left pull-right"></i>
		</span>
    </a>
        <ul class="treeview-menu">
            <li id="system-setting"><a
                href="${pageContext.request.contextPath}/user/findAll"> <i
                class="fa fa-circle-o"></i> 用户管理
            </a></li>
            <li id="system-setting"><a
                href="${pageContext.request.contextPath}/role/findAll"> <i
                class="fa fa-circle-o"></i> 角色管理
            </a></li>
            <li id="system-setting"><a
                href="${pageContext.request.contextPath}/pages/permission-list.jsp">
                <i class="fa fa-circle-o"></i> 权限管理
            </a></li>
        </ul>
    </li>
</security:authorize>

<%-- 管理员和普通用户都可以操作基础数据 --%>
<security:authorize access="hasAnyRole('ROLE_ADMIN','ROLE_USER')">
    <li class="treeview"><a href="#"> <i class="fa fa-cube"></i>
        <span>基础数据</span> <span class="pull-right-container"> <i
            class="fa fa-angle-left pull-right"></i>
		</span>
        </a>
        <ul class="treeview-menu">
            <!-- 产品模块还需要产品角色，注意这里还需再次指定超级管理员 -->
            <security:authorize access="hasAnyRole('ROLE_ADMIN','ROLE_PRODUCT')">
                <li id="system-setting"><a
                    href="${pageContext.request.contextPath}/product/findAll">
                    <i class="fa fa-circle-o"></i> 产品管理
                </a></li>
            </security:authorize>
            <!-- 订单模块需要订单角色 -->
            <security:authorize access="hasAnyRole('ROLE_ADMIN','ROLE_ORDER')">
                <li id="system-setting"><a
                    href="${pageContext.request.contextPath}/order/findAll">
                    <i class="fa fa-circle-o"></i> 订单管理
                </a></li>
            </security:authorize>
        </ul>
    </li>
</security:authorize>
```

启动测试。使用001用户登录

![image-20200610131345969](http://image.beloved.ink/Typora/image-20200610131345969.png)

只可以看见产品模块菜单

但是通过地址栏可以访问订单模块

![image-20200610131513939](http://image.beloved.ink/Typora/image-20200610131513939.png)

**页面动态菜单的展示只是为了用户体验，并未真正控制权限！**

## 8.3、IOC容器结构说明

![image-20200610133649782](http://image.beloved.ink/Typora/image-20200610133649782.png)

**说明：SpringSecurity可以通过注解的方式来控制类或者方法的访问权限。注解需要对应的注解支持，若注解放在 controller类中，对应注解支持应该放在mvc配置文件中，因为controller类是有mvc配置文件扫描并创建的，同 理，注解放在service类中，对应注解支持应该放在spring配置文件中。由于我们现在是模拟业务操作，并没有 service业务代码，所以就把注解放在controller类中了。**

## 8.4、开启授权的注解支持

```xml
<!--
    开启权限控制的注解支持
    实际开发中只使用一种即可
    secured-annotations="enabled"  ： SpringSecurity内部的权限控制注解开关
    pre-post-annotations="enabled" ： Spring指定的权限控制的注解开关
    jsr250-annotations="enabled"   ： 开启java250注解支持
-->
<security:global-method-security
        secured-annotations="enabled"
        pre-post-annotations="enabled"
        jsr250-annotations="enabled" />
```

**因为是测试模拟业务，注解在controller层，所以注解支持要放在mvc配置文件中**

**实际开发为了安全应该在service层，注解支持可以写在SpringSecurity配置文件中**

## 8.5、在注解支持对应类或者方法上添加注解

```java
@Controller
@RequestMapping("/order")
public class OrderController {

    //@Secured({"ROLE_ADMIN","ROLE_ORDER"})  // SpringSecurity内部制定注解
    //@RolesAllowed({"ROLE_ADMIN","ROLE_ORDER"})  // jsr250注解
    @PreAuthorize("hasAnyAuthority('ROLE_ADMIN','ROLE_ORDER')")  //Spring的el表达式注解
    @RequestMapping("/findAll")
    public String findAll(){
        return "order-list";
    }
}
```

**实际开发使用一种即可**

## 8.6、权限不足异常处理

![image-20200610135412272](http://image.beloved.ink/Typora/image-20200610135412272.png)

**自定义异常页面**

### 8.6.1、在spring-security.xml配置文件中处理

```xml
<!--设置可以用spring的el表达式配置Spring Security并自动生成对应配置组件（过滤器）-->
<security:http auto-config="true" use-expressions="true">
    <!--省略其它配置-->
    <!--403异常处理-->
    <security:access-denied-handler error-page="/403.jsp"/>
</security:http>
```

### 8.6.2、在web.xml中处理

```xml
<!--处理403异常-->
<error-page>
    <error-code>403</error-code>
    <location>/403.jsp</location>
</error-page>

<!--处理404异常-->
<error-page>
    <error-code>404</error-code>
    <location>/404.jsp</location>
</error-page>

<!--处理500异常-->
<error-page>
    <error-code>500</error-code>
    <location>/500.jsp</location>
</error-page>
```

### 8.6.3、编写异常处理器

**测试有问题**

```java
@Component
public class HandlerControllerException implements HandlerExceptionResolver {

    /**
     *
     * @param httpServletRequest
     * @param httpServletResponse
     * @param o 出现的异常对象
     * @param e 出现的异常信息
     * @return ModelAndView
     */
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView mv = new ModelAndView();
        //将异常信息放入Request域中，基本不用
        mv.addObject("errorMsg",e.getMessage());
        //指定不同的异常跳转的页面
        if (e instanceof AccessDeniedException){
            mv.setViewName("redirect:/403.jsp");
        }else {
            mv.setViewName("redirect:/500.jsp");
        }
        return mv;
    }
}
```

```java
@ControllerAdvice
public class HandlerControllerAdvice {

    //只有出现AccessDeniedException异常才调转403.jsp页面
    @ExceptionHandler(AccessDeniedException.class)
    public String exceptionAdvice(){
        return "forward:/403.jsp";
    }

}
```

![image-20200610135803638](http://image.beloved.ink/Typora/image-20200610135803638.png)

# 9、SpringBoot整合SpringSecurity

## 9.1、创建项目

创建springboot项目

**编写测试接口**

```java
@Controller
public class MyController {

    @GetMapping("/")
    @ResponseBody
    public String index(){
        return "test";
    }

}
```

**启动测试**

![image-20200610182504899](http://image.beloved.ink/Typora/image-20200610182504899.png)

**导入SpringSecurity**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

**再次启动测试**

![image-20200610182643130](http://image.beloved.ink/Typora/image-20200610182643130.png)

SpringBoot已经为SpringSecurity提供了默认配置，默认所有资源都必须认证通过才能访问。

其实SpringBoot已经提供了默认用户名user，密码在项目启动时随机生成，如图：

![image-20200610182726403](http://image.beloved.ink/Typora/image-20200610182726403.png)

认证通过后可以继续访问处理器资源

## 9.2、加入jsp使用自定义认证页面

### 说明 

SpringBoot官方是不推荐在SpringBoot中使用jsp的，需要导入tomcat插件启动项目，不能再用SpringBoot默认tomcat了。 

### 导入SpringBoot的tomcat启动插件jar包

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
```

### 加入jsp页面等静态资源

在src/main目录下创建webapp目录

![image-20200610183102024](http://image.beloved.ink/Typora/image-20200610183102024.png)

这时webapp目录并不能正常使用，因为只有web工程才有webapp目录，在pom文件中修改项目为web工程

![image-20200610183204358](http://image.beloved.ink/Typora/image-20200610183204358.png)

导入静态资源，**注意WEB-INF就不用了！**

![image-20200610183637596](http://image.beloved.ink/Typora/image-20200610183637596.png)