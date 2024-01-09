# MockMvc

```java
package com.example.demo;

import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.http.MediaType.APPLICATION_JSON;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest
@Slf4j
class DemoControllerTest {

    @Autowired
    private MockMvc mockMvc;
    @Autowired
    private ObjectMapper mapper;

    @Test
    void demoGet() {
        try {
            mockMvc.perform(get("/demoGet")).andExpect(status().isOk()).andDo(print());
        } catch(Exception e) {
            log.error("에러!");
        }
    }

    @Test
    void demoPost() {
        DemoDTO demoDTO = DemoDTO.builder()
                .id("test001")
                .pw("test001")
                .build();
        try {
            String content = mapper.writeValueAsString(demoDTO);
            log.info("content = {}", content);
            mockMvc.perform(post("/demoPost").contentType(APPLICATION_JSON).content(content)).andExpect(status().isOk()).andDo(print());
        } catch(Exception e) {
            log.error("에러!");
        }
    }
}
```