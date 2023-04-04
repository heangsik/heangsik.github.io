# 송수신 전문 로깅

## 1 목차

- [송수신 전문 로깅](#송수신-전문-로깅)
  - [1 목차](#1-목차)
  - [2. Filter 작성](#2-filter-작성)

## 2. Filter 작성

```Java
@Slf4j
@Component
public class LoggingFilter extends OncePerRequestFilter{
    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletExceptoin, IOException{
        log.info(request.getRequestURL().toString);
        if(isAyncDispathch(request)){
            filterChain.doFilter(request, response);
        }else{
            doMessageLogging(new ContentCashingRequestWrapper(request), new ContentCashingResponseWrapper(response), filterChain);
        }
    }
    private void doMessageLogging(ContentCashingRequestWrapper request, ContentCashingResponseWrapper response, , FilterChain filterChain) throws IOExcepton{
        String queryString = request.getQueryString();
        String requestUrl = queryString == null ? request.getRequestURI() : request.getRequestURI() + "?" + queryString;
        try{
            filterChain.doFilter(request, response);
            String method = request.getMethod();
            String reqMsg = new String(request.getContentAsByArray(), request.getCharacterEncodeing());
            String resMsg = new String(response.getContentAsByeArray(), StanddardCharsets.UTF_8);
            log.info("request method : {}, uri : {}, body : {}", method, requestUrl, reqMsg);
            log.info("response : {}", resMsg);
        }
        catch(Exception ex)
        {
            throw new RuntimeException(ex);
        }
        finally{
            response.copyBodyToResponse();
        }
    }

}
```
