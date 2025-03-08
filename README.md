# WebSocket com Spring Boot

## 📌 Introdução ao WebSocket
WebSockets são um protocolo de comunicação full-duplex que permite interações bidirecionais entre um cliente e um servidor em tempo real. Diferente do HTTP, onde cada requisição do cliente exige uma resposta separada do servidor, WebSockets permitem a criação de uma conexão persistente, reduzindo a latência e melhorando a eficiência da comunicação.

### 🖥️ Características do WebSocket:
- **Conexão persistente**: Mantém uma conexão aberta entre o cliente e o servidor.
- **Baixa latência**: Evita a necessidade de polling contínuo.
- **Comunicação em tempo real**: Ideal para chats, notificações, dashboards, e sistemas interativos.
- **Protocolo baseado em eventos**: Utiliza um modelo assíncrono para troca de mensagens.

## 🚀 Sobre este Projeto
Este projeto é uma aplicação Spring Boot que implementa WebSockets para comunicação em tempo real usando STOMP (Simple Text Oriented Messaging Protocol). O exemplo apresentado é um chat simples, onde múltiplos clientes podem se conectar e trocar mensagens em tempo real.

## 🏗️ Tecnologias Utilizadas
- **Java 17+**
- **Spring Boot 3+**
- **Spring WebSocket**
- **STOMP (Simple Text Oriented Messaging Protocol)**
- **SockJS** (para fallback quando WebSockets não estiverem disponíveis)
- **HTML + JavaScript (Cliente Web)**

## 📜 Estrutura do Projeto
```
websocket-springboot
│── src/main/java/br/com/example/websocket
│   ├── config/WebSocketConfig.java       # Configuração do WebSocket
│   ├── controller/ChatController.java    # Controlador para envio/recebimento de mensagens
│   ├── model/ChatMessage.java            # Classe de modelo para mensagens
│── src/main/resources/static/chat.html  # Cliente web para testes
│── pom.xml                               # Dependências do projeto
```

## 🔧 Configuração do WebSocket
A configuração do WebSocket no Spring Boot é feita através da classe `WebSocketConfig`:

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws").setAllowedOriginPatterns("*").withSockJS();
    }

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.enableSimpleBroker("/topic");
        registry.setApplicationDestinationPrefixes("/app");
    }
}
```

## 💬 Como Funciona o Chat
### **Fluxo de Mensagens:**
1. O cliente se conecta ao servidor WebSocket via `/ws`.
2. As mensagens enviadas pelo cliente são direcionadas para o endpoint `/app/chat.sendMessage`.
3. O servidor recebe a mensagem e a repassa para todos os clientes conectados no tópico `/topic/public`.
4. Os clientes recebem as mensagens em tempo real e as exibem na interface.

### **Exemplo de Payload JSON:**
```json
{
    "sender": "Usuário1",
    "content": "Olá, WebSocket!"
}
```

## 🛠️ Como Executar o Projeto
### **Pré-requisitos:**
- Java 17+
- Maven

### **Passos para rodar:**
1. Clone o repositório:
   ```sh
   git clone https://github.com/MayconCarvalho/websocket-chat.git
   ```
2. Acesse o diretório do projeto:
   ```sh
   cd websocket-springboot
   ```
3. Compile e execute a aplicação:
   ```sh
   mvn spring-boot:run
   ```
4. Abra o arquivo `index.html` no navegador e teste o chat.

## 🧪 Testando a Aplicação
### **Testando com o Postman:**
1. Abra o Postman e vá até a aba **WebSocket Request**.
2. Conecte-se a `ws://localhost:8080/ws`.
3. Envie uma mensagem JSON para `/app/chat.sendMessage`.
4. O servidor irá retransmitir a mensagem para os clientes conectados no tópico `/topic/public`.

### **Testando com WebSocket Debugger:**
- Acesse [https://www.piesocket.com/websocket-tester](https://www.piesocket.com/websocket-tester)
- Conecte-se a `ws://localhost:8080/ws`
- Envie mensagens e veja a resposta.

## 📌 Considerações Finais
Este projeto demonstra como implementar WebSockets em aplicações Spring Boot de forma simples e eficaz. Ele pode ser expandido para aplicações mais complexas, como sistemas de notificações, multiplayer games, e monitoramento em tempo real.

🔗 Para mais informações, consulte a [documentação oficial do Spring WebSocket](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#websocket).

---