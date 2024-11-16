---
author: Jesse Morgan
categories:
- Projects
- SPoE
date: "2010-12-09T22:39:17Z"
guid: http://morgajel.net/?p=996
id: 996
title: 'SPoE: Performance and Errors Under Load'
url: /2010/12/09/996
views:
- "125"
---

One concern I have with SPoE is that, should it get popular, it must handle traffic to a reasonable degree. Since I deal with misbehaving Java apps at work all the time, I decided to test mine and see how it behaved under load. I’m running tomcat via eclipse, trending memory usage with VisualVM, and running the test with JMeter.

I decided to start small- simply registering a new account. With only 5 pages, the test flew by and gave me a false sense of security. I upped the ante to registering a new account, logging in, viewing the user edit account page, then creating and viewing a series of snippets.Â That’s when things fell apart. With 15 threads, 15 seconds of startup, and 100 loops, the first failure happened at around the 700 sample mark.Â The failure happened when JMeter was attempting to create a snippet. The only problem is the error code that it spat back in the tomcat eclipse log:

```

INFO: Server startup in 7158 ms
Dec 9, 2010 10:09:31 PM org.apache.catalina.core.StandardWrapperValve invoke
SEVERE: Servlet.service() for servlet dispatcher threw exception
java.lang.IndexOutOfBoundsException: Index: 0, Size: 0
 at java.util.ArrayList.RangeCheck(ArrayList.java:547)
 at java.util.ArrayList.get(ArrayList.java:322)
 at org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter$RequestMappingInfo.bestMatchedPattern(AnnotationMethodHandlerAdapter.java:1017)
 at org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter$RequestMappingInfoComparator.compare(AnnotationMethodHandlerAdapter.java:1104)
 at org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter$RequestMappingInfoComparator.compare(AnnotationMethodHandlerAdapter.java:1)
 at java.util.Arrays.mergeSort(Arrays.java:1270)
 at java.util.Arrays.sort(Arrays.java:1210)
 at java.util.Collections.sort(Collections.java:159)
 at org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter$ServletHandlerMethodResolver.resolveHandlerMethod(AnnotationMethodHandlerAdapter.java:611)
 at org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter.invokeHandlerMethod(AnnotationMethodHandlerAdapter.java:422)
 at org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter.handle(AnnotationMethodHandlerAdapter.java:415)
 at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:788)
 at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:717)
 at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:644)
 at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:549)
 at javax.servlet.http.HttpServlet.service(HttpServlet.java:690)
 at javax.servlet.http.HttpServlet.service(HttpServlet.java:803)
 at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:290)
 at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:366)
 at org.springframework.security.web.access.intercept.FilterSecurityInterceptor.invoke(FilterSecurityInterceptor.java:109)
 at org.springframework.security.web.access.intercept.FilterSecurityInterceptor.doFilter(FilterSecurityInterceptor.java:83)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.access.ExceptionTranslationFilter.doFilter(ExceptionTranslationFilter.java:97)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.session.SessionManagementFilter.doFilter(SessionManagementFilter.java:100)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.authentication.AnonymousAuthenticationFilter.doFilter(AnonymousAuthenticationFilter.java:78)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter.doFilter(SecurityContextHolderAwareRequestFilter.java:54)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.savedrequest.RequestCacheAwareFilter.doFilter(RequestCacheAwareFilter.java:35)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.authentication.www.BasicAuthenticationFilter.doFilter(BasicAuthenticationFilter.java:177)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter.doFilter(DefaultLoginPageGeneratingFilter.java:91)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter.doFilter(AbstractAuthenticationProcessingFilter.java:187)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.authentication.logout.LogoutFilter.doFilter(LogoutFilter.java:105)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.context.SecurityContextPersistenceFilter.doFilter(SecurityContextPersistenceFilter.java:79)
 at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:378)
 at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:167)
 at org.springframework.web.filter.DelegatingFilterProxy.invokeDelegate(DelegatingFilterProxy.java:237)
 at org.springframework.web.filter.DelegatingFilterProxy.doFilter(DelegatingFilterProxy.java:167)
 at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
 at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
 at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:88)
 at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:76)
 at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
 at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
 at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:233)
 at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:191)
 at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:127)
 at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:102)
 at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:109)
 at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:298)
 at org.apache.coyote.http11.Http11Processor.process(Http11Processor.java:852)
 at org.apache.coyote.http11.Http11Protocol$Http11ConnectionHandler.process(Http11Protocol.java:588)
 at org.apache.tomcat.util.net.JIoEndpoint$Worker.run(JIoEndpoint.java:489)
 at java.lang.Thread.run(Thread.java:619)
```

As you can see, there’s nothing form com.morgajel.spoe in there. Here’s the controller snippet that fires when /snippet/create is hit:

```
@RequestMapping("/create")
Â public ModelAndView createSnippetForm() {
Â Â Â Â  LOGGER.info("showing the createSnippetForm");
Â Â Â Â  ModelAndViewÂ  mav = new ModelAndView();
Â Â Â Â  mav.setViewName("snippet/editSnippet");
Â Â Â Â  mav.addObject("editSnippetForm", new EditSnippetForm());
Â Â Â Â  return mav;
 }
```

So wtf is causing the error? I don’t know. Now I’m not sure if I keep chasing this rabbit or I move on- on one hand, I hate hate HATE when there’s performance issues and devs ignore it; on the other, this is a side project that isn’t going into production anytime soon.