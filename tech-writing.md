# **Jsoup 활용하기**

<br/>

## **Jsoup 소개**

**Jsoup**은 웹 크롤링과 HTML 파싱을 위한 Java 라이브러리다. 

**Jsoup**은 웹 페이지의 HTML을 간편하게 분석, 조작할 수 있는 기능을 제공하며 HTML DOM 트리를 탐색, 데이터 추출, 변형을 쉽게 처리할 수 있다.


#### **주요 기능:**

* HTML DOM 트리 파싱
* CSS 및 jQuery 스타일의 선택자 지원
* HTML에서 데이터 추출 및 조작
* HTTP 요청을 통해 웹 페이지 가져오기


<br/>

## **기본 설정**

Jsoup 라이브러리 의존성 추가


### **Gradle**

```gradle
dependencies {
    implementation 'org.jsoup:jsoup:1.14.3'
}
```



### **Maven**


```xml
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.14.3</version>
</dependency>

```
<br/>

--- 
<br/>

## **Jsoup 기능**

### **1. 간단한 HTML 파싱**


### **HTML 파일에서 데이터 추출하기**

Jsoup으로 HTML 파일을 간단하게 파싱할 수 있다. 다음 코드는 HTML 파일을 파싱하여 특정 요소를 추출하는 예시이다.


```java
// HTML 파일을 로드하고 파싱
Document doc = Jsoup.parse(new File("sample.html"), "UTF-8");

// HTML의 title 태그 추출
String title = doc.title();

// 특정 ID를 가진 요소 추출
Element element = doc.getElementById("content");
```







* `Jsoup.parse()`를 통해 HTML 파일을 파싱한다.
* DOM 구조에서 `getElementById()`와 같은 메서드를 사용해 원하는 요소를 선택한다.



<br/>

### **2. 웹 페이지에서 데이터 가져오기**

Jsoup을 사용하면 HTTP 요청을 보내고 웹 페이지의 HTML을 직접 가져올 수 있다. 


```java
Document doc = Jsoup.connect("https://example.com").get();
Element content = doc.select("div#content").first();
System.out.println("Content: " + content.text());
```







* `Jsoup.connect()`는 주어진 URL에 HTTP 요청을 보낸다.
* `select()` 메서드를 사용하여 CSS 선택자를 기반으로 특정 요소를 선택한다.


<br/>


### **3. 선택자 사용하기**


#### **예시 HTML 문서**


```html
<html>
  <head>
    <meta property="og:title" content="Example title">
  </head>
  <body>
    <div id="content">
      <h1>Welcome to Example.com</h1>
      <p class="intro">This is an introductory paragraph with <a href="https://example.com/link">a link</a>.</p>
      <ul class="list">
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
      </ul>
    </div>
  </body>
</html>
```



#### **기본 사용법**

이 HTML 문서에서 특정 요소를 추출하려면 다음과 같이 `select()` 메서드를 사용할 수 있다.


```java
Document doc = Jsoup.connect("https://example.com").get();

// meta 태그의 og:title 속성 추출
String title = doc.select("meta[property=og:title]").attr("content");

// div#content 내부의 h1 태그 추출
String heading = doc.select("div#content h1").text();

// class가 'intro'인 p 태그 추출
String intro = doc.select("p.intro").text();

// ul 태그 내부의 모든 li 요소 추출
Elements listItems = doc.select("ul.list li");
for (Element item : listItems) {
    System.out.println("List item: " + item.text());
}
```







* `meta[property=og:title]`와 같은 CSS 선택자를 사용해 메타 태그에서 데이터를 추출할 수 있다. `attr("content")` 메서드를 사용하여 메타 태그의 `content` 속성 값을 가져온다.
* **div#content h1**: `div` 태그 중에서 `id`가 `content`인 요소 안의 `h1` 태그를 선택하여 그 내용을 추출한다. 이때 `text()` 메서드를 사용해 요소 내부의 텍스트 값을 가져온다.
* **p.intro**: `class`가 `intro`인 `p` 태그를 선택하여 그 안의 텍스트 내용을 추출한다. `class` 선택자는 마침표(`.`)로 시작한다.
* **ul.list li**: `class`가 `list`인 `ul` 태그의 모든 `li` 요소를 선택하고, 반복문을 통해 각각의 `li` 요소의 텍스트를 출력한다.
* 이 외 다양한 방법으로 링크, 이미지, 헤더 태그, 메타 태그 등 HTML 문서 내 요소들을 수집할 수 있다.


<br/>


### **4. 폼 데이터 전송**

Jsoup을 사용하면 웹 폼에 데이터를 입력하고 서버에 전송할 수도 있다.


```java
Document doc = Jsoup.connect("https://example.com/login")
    .data("username", "myUsername")
    .data("password", "myPassword")
    .post();
```







* `data()` 메서드를 사용하여 폼 필드에 데이터를 추가한다.
* `post()` 메서드를 호출해 POST 요청을 서버로 전송한다.


<br/>


### **5. HTML 조작하기**

Jsoup은 HTML 문서를 파싱하는 것뿐만 아니라 문서를 조작하는 기능도 제공한다.


```java
Element newElement = doc.createElement("div").text("새로운 요소");
doc.body().appendChild(newElement);
```







* `createElement()`를 사용하여 새로운 HTML 요소를 생성하고, DOM 트리에 추가할 수 있다.


<br/>


### **6. 에러 처리 및 예외 상황**

Jsoup을 사용할 때 HTTP 요청 실패나 잘못된 HTML 구조로 인한 예외가 발생할 수 있다.


```java
try {
    Document doc = Jsoup.connect("https://invalid-url.com").get();
} catch (IOException e) {
}
```







* HTTP 요청 실패 시 `IOException`이 발생하며, 적절한 예외 처리가 필요하다.

<br/>

--- 
<br/>

## **Jsoup 기능이 포함된 로직 테스트 하는 법**


```java
@Test
void test() throws IOException {
   final String url = "https://www.google.com";
   final Document document = Jsoup.connect(url).get();
   assertThat(document.title()).isEqualTo("Google");
}
```


위와 같이 실제 서비스되고 있는 URL을 사용해 테스트 코드를 작성할 경우, 해당 서비스의 HTML 문서가 변경되면 내 테스트 코드가 깨질 위험이 있다. 이를 방지하기 위해, Fake Server를 만들어 HTML 응답을 미리 설정하고 테스트하는 방법이 더 안정적이다.


```java
public class FakeServer {
    public static int createAndStartFakeServer(final String html) {
        HttpServer server;
        try {
            server = HttpServer.create(new InetSocketAddress(0), 0);
        } catch (final IOException e) {
        }
        final int assignedPort = server.getAddress().getPort();

        server.createContext("/test", createHandler(html));
        server.setExecutor(null);
        server.start();
        return assignedPort;
    }

    private static HttpHandler createHandler(final String html) {
        return new HttpHandler() {
            @Override
            public void handle(final HttpExchange exchange) throws IOException {
                exchange.getResponseHeaders().set("Content-Type", "text/html; charset=UTF-8");
                exchange.sendResponseHeaders(200, html.getBytes(StandardCharsets.UTF_8).length);
                final OutputStream os = exchange.getResponseBody();
                os.write(html.getBytes(StandardCharsets.UTF_8));
                os.close();
            }
        };
    }
}

@Test
void test() throws IOException {
   final String html = "<html><head>" +
           "<title>타이틀</title>" +
           "</head><body></body></html>";
   final int assignedPort = FakeServer.createAndStartFakeServer(html);
   final String url = "http://localhost:" + assignedPort + "/test";


   final Document document = Jsoup.connect(url).get();


   assertThat(document.title()).isEqualTo("타이틀");
}
```







* Java의 내장 HTTP 서버를 사용하여 테스트 환경에서 실제 서비스와 비슷한 서버를 생성해 테스트 중 요청이 들어오면, 미리 정의된 HTML 문서를 반환한다.
* 실제 외부 서버가 다운되거나 변경되는 상황에서도 테스트가 영향을 받지 않아, 안정적이고 반복 가능한 테스트 환경을 제공한다
