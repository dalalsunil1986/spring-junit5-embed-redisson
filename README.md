#### A tiny wrapper for conveniently using embedded Redis Server with JUnit5 and Spring Boot.

Mainly this is an excuse to play with JUnit5's ParameterResolver interface. 

In order to create a Spring Boot Test, you need to use JUnit5.


First add 
```
<dependency>
	<groupId>com.mercateo.oss</groupId>
	<artifactId>spring-junit5-embed-redisson</artifactId>
	<version>[0,)</version> <!-- or a particular version -->
	<scope>test</scope>
</dependency>
``` 
		
to your pom.xml and create a SpringBootRedissonTest like this:

```
// look ma, no RunWith
@SpringBootRedissonTest // provides the extensions
public class RedissonTest {

	@Test
	public void testWithInjectedRedissonParameter(RedissonClient c) {
		RDeque<Object> deque = c.getDeque("mylist");
		assertEquals(0, deque.size());
	
		deque.add("hubba");
		assertEquals("hubba", deque.pop());
	}
	
	@Test
	public void testWithoutRedissonParameter() {
		// there is no redis server being started for this test, 
		// as there is no redisson parameter
	}
}

``` 


The Extension will start a Spring Boot container and for every test that
has a parameter of type ```RedissonClient```, an embedded Redis Server is
started on a random port (and stopped after test execution) and the client passed to the test is
configured accordingly.

Example Project: [spring-junit5-embed-redisson-example](https://github.com/uweschaefer/spring-junit5-embed-redisson-example)
