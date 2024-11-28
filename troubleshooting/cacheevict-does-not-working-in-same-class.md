# @CacheEvict does not working in same class

스프링 AOP(Aspect Oriented Programming) 개념과 관련

* spring annotatioin은 spring proxy 객체 대상으로 동작
* 같은 클래스의 호출하려는 메서드의 inner method에 선언된 annotation을 가진 메서드를 호출하면 프록시가 아닌 실제 객체를 호출하기 때문에 AOP 가 동작하지 않음
* 따라서 @CacheEvict가 동작하지 않은 상황

#### AS-IS

<pre class="language-java"><code class="lang-java">// Same Class
    
@CacheEvict(value = "menuListByUser", key = "#userId")
public void evictMenuWithPageAndCompInfoCacheByUser(String userId) {
    log.info(userId + "의 메뉴 캐시정보가 삭제되었습니다.");
}

private void deleteCacheByRoleId(Integer roleId) {
<strong>    List&#x3C;String> userIdList = getUserIdListByRoleId(roleId);
</strong>    for (String userId : userIdList) {
        evictMenuWithPageAndCompInfoCacheByUser(userId);
    }
}

</code></pre>

#### TO-BE

```java
// Diffrent Class

// [1] in cache service class (class name : CacheWorkService)
@CacheEvict(value = "menuListByUser", key = "#userId")
public void evictMenuWithPageAndCompInfoCacheByUser(String userId) {
    log.info(userId + "의 메뉴 캐시정보가 삭제되었습니다.");
}

// [2] in service class (class name : WorkService)
private void deleteCacheByRoleId(Integer roleId) {
    List<String> userIdList = getUserIdListByRoleId(roleId);
    for (String userId : userIdList) {
        cacheWorkService.evictMenuWithPageAndCompInfoCacheByUser(userId);
    }
}

```



\| 참고

{% embed url="https://stackoverflow.com/questions/71056348/why-does-cacheevict-not-work-when-called-from-the-same-class" %}

{% embed url="https://spring.io/blog/2012/05/23/transactions-caching-and-aop-understanding-proxy-usage-in-spring" %}
