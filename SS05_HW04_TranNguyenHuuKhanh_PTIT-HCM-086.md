# BÀI 4: Phân tích & Lựa chọn Prompt học Spring AOP

## 1. Đáp án lựa chọn

**Chọn phương án B.**

Phương án B là prompt tốt nhất vì nó không chỉ yêu cầu ví dụ code, mà còn hướng dẫn AI giải thích kiến thức theo nhiều cấp độ, so sánh khái niệm và đưa ra ví dụ thực tiễn. Với một chủ đề trừu tượng như Spring AOP, cách học này giúp người học hiểu bản chất trước khi áp dụng.

## 2. Vì sao phương án B tối ưu nhất

Spring AOP có nhiều thuật ngữ khó như `Aspect`, `JoinPoint`, `Pointcut`, `Advice`, `Cross-cutting Concerns`. Nếu chỉ yêu cầu viết code, người học có thể copy được nhưng không hiểu vì sao cần dùng AOP và dùng trong trường hợp nào.

Phương án B tối ưu vì có 3 kỹ thuật học kiến thức nâng cao:

| Kỹ thuật | Cách phương án B áp dụng |
|---|---|
| Level-based Explanation | Giải thích ở 2 cấp độ: người mới và Senior Developer |
| Comparative Analysis | So sánh AOP với OOP để thấy mục đích và phạm vi ứng dụng |
| Practical Examples | Cung cấp ví dụ Spring Boot dùng `@Aspect`, `@Before`, `@After` |

Điểm mạnh của prompt B là nó tạo cầu nối từ tư duy đời thường sang tư duy kỹ thuật. Người học trước tiên hiểu AOP giống như một lớp xử lý phụ tự động áp vào nhiều luồng xử lý chính, sau đó mới học các thuật ngữ chuyên sâu và code.

## 3. Lý do loại trừ các phương án còn lại

### Phương án A

Phương án A có thể tạo ra câu trả lời đúng nhưng còn chung chung. Nó không yêu cầu giải thích theo cấp độ, không yêu cầu so sánh AOP và OOP, cũng không yêu cầu chú thích chi tiết bằng tiếng Việt. Với người mới học, phương án này có thể chưa đủ sâu.

### Phương án C

Phương án C thiên về code và cấu hình `pom.xml`, nhưng không giúp hiểu bản chất AOP. Người học có thể biết thêm dependency nào cần dùng nhưng vẫn mơ hồ về các khái niệm `Pointcut`, `Advice`, `JoinPoint` và vấn đề cắt ngang.

## 4. Giải thích ngắn gọn về Spring AOP

AOP là kỹ thuật tách các logic dùng chung, lặp đi lặp lại ở nhiều nơi ra khỏi nghiệp vụ chính. Ví dụ: logging, security, transaction, audit, đo thời gian thực thi. Thay vì viết log trong từng service, ta viết một `Aspect` và cấu hình để nó tự động chạy trước hoặc sau các method phù hợp.

## 5. Mã nguồn Java ví dụ Aspect ghi log

```java
package com.example.demo.aop;

import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Aspect
@Component
@Slf4j
public class ServiceLoggingAspect {

    // Pointcut: chọn tất cả method nằm trong các class thuộc package service.
    @Pointcut("within(com.example.demo.service..*)")
    public void serviceLayerMethods() {
    }

    // Advice @Before: chạy trước khi method service được gọi.
    @Before("serviceLayerMethods()")
    public void logBefore(JoinPoint joinPoint) {
        log.info("Starting method: {} with args: {}",
                joinPoint.getSignature().toShortString(),
                Arrays.toString(joinPoint.getArgs()));
    }

    // Advice @AfterReturning: chạy sau khi method service hoàn thành thành công.
    @AfterReturning(pointcut = "serviceLayerMethods()", returning = "result")
    public void logAfterReturning(JoinPoint joinPoint, Object result) {
        log.info("Completed method: {} with result: {}",
                joinPoint.getSignature().toShortString(),
                result);
    }

    // Advice @AfterThrowing: chạy khi method service ném exception.
    @AfterThrowing(pointcut = "serviceLayerMethods()", throwing = "ex")
    public void logAfterThrowing(JoinPoint joinPoint, Throwable ex) {
        log.error("Exception in method: {}, message: {}",
                joinPoint.getSignature().toShortString(),
                ex.getMessage(),
                ex);
    }
}
```

Ví dụ service được ghi log tự động:

```java
package com.example.demo.service;

import org.springframework.stereotype.Service;

@Service
public class PaymentService {

    public String pay(Long orderId) {
        return "Payment success for order " + orderId;
    }
}
```

Dependency Maven cần có:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
