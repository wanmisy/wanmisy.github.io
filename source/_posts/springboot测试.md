---
title: springboot测试
date: 2018-12-25 14:49:25
tags: springboot,spring
categories: springboot
---
通常都是写 post测试，比较麻烦，这里我们介绍springboot test
<!-- more -->
```(java)
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-test</artifactId>
	<scope>test</scope>
</dependency>
```


```(java)
@RunWith(SpringRunner.class)
@SpringBootTest
public class BootApplicationTests {

    private MockMvc mockMvc;

    @Before
    public void setUp(){
        mockMvc = MockMvcBuilders.standaloneSetup(new TestController()).build();
    }

    @Test
    public void contextLoads() throws Exception {
        mockMvc.perform(MockMvcRequestBuilders.post("/test?name=neo")
        .accept(MediaType.APPLICATION_JSON_UTF8)).andDo(MockMvcResultHandlers.print());
    }

}
```
