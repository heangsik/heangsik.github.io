# WebSocket Server

### 1. 프로젝트 설정
- dependencies
    ```java
        // build.gradle
        dependencies {
            implementation 'org.springframework.boot:spring-boot-starter-websocket'
            compileOnly 'org.projectlombok:lombok'
            annotationProcessor 'org.projectlombok:lombok'
            developmentOnly 'org.springframework.boot:spring-boot-devtools'
            testImplementation 'org.springframework.boot:spring-boot-starter-test'
        }
    ``` 
### 2. Configure
- WebSocket Config
```Java
    package com.example.demo.config;

    import com.example.demo.handler.ChatHdl;
    import lombok.RequiredArgsConstructor;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.web.socket.config.annotation.EnableWebSocket;
    import org.springframework.web.socket.config.annotation.WebSocketConfigurer;
    import org.springframework.web.socket.config.annotation.WebSocketHandlerRegistry;

    @Configuration
    @EnableWebSocket
    @RequiredArgsConstructor
    public class WsConfig implements WebSocketConfigurer {
        final ChatHdl chatHdl;
        @Override
        public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
            registry.addHandler(chatHdl, "/ws/demo").setAllowedOrigins("*"); // 중요 ** allow설정을 해야 접속이 가능하다
        }


    }

```
### 3. Handler
- Handler
```Java
    package com.example.demo.handler;

    import lombok.extern.slf4j.Slf4j;
    import org.springframework.stereotype.Component;
    import org.springframework.web.socket.*;
    import org.springframework.web.socket.handler.TextWebSocketHandler;

    @Component
    @Slf4j
    public class ChatHdl extends TextWebSocketHandler {
        @Override
        public void afterConnectionEstablished(WebSocketSession session) throws Exception {
            log.info("afterConnectionEstablished");
        }

        @Override
        public void handleMessage(WebSocketSession session, WebSocketMessage<?> message) throws Exception {
            log.info("ChatHdl.handleMessage");
        }

        @Override
        protected void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
            log.info("ChatHdl.handleTextMessage");

        }

        @Override
        protected void handlePongMessage(WebSocketSession session, PongMessage message) throws Exception {
            log.info("ChatHdl.handlePongMessage");

        }

        @Override
        public void handleTransportError(WebSocketSession session, Throwable exception) throws Exception {
            log.info("ChatHdl.handleTransportError");

        }

        @Override
        public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {
            log.info("ChatHdl.afterConnectionClosed");

        }
    }

```