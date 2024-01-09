# MockMvc

```java
package com.example.demo;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest
class DemoControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void demoGet() {
        try {
            mockMvc.perform(get("/demoGet")).andExpect(status().isOk()).andDo(print());
        } catch(Exception e) {
            System.out.println("어 왜 안됨??");
        }
    }
}
```