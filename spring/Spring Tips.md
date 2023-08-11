# Spring Boot Tips

### JSON null 제거
- @JsonInclude(JsonInclude.Include.NON_NULL) 으로 Null 제거
- @JsonIgnore 으로 변환 방지 변수 지정
```java
    package kr.co.yhs.authserver.dto;

    import com.fasterxml.jackson.annotation.JsonIgnore;
    import com.fasterxml.jackson.annotation.JsonInclude;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import lombok.AllArgsConstructor;
    import lombok.Builder;
    import lombok.Getter;

    @AllArgsConstructor
    @Getter
    @JsonInclude(JsonInclude.Include.NON_NULL)
    public class ResponseDto<T> {
        @JsonIgnore
        private ResponseCode resCode;
        private String code;
        private String message;
        private T responseObject;

        //
        @Builder
        ResponseDto(ResponseCode code, T responseObject) {
            this.code = code.getResponseCode();
            this.message = code.getResponseMessage();
            this.responseObject = responseObject;
        }

        public String toString() {
            Gson gson = new GsonBuilder().disableHtmlEscaping().create();
            JsonObject responseJson = new JsonObject();
            responseJson.addProperty("code", this.code);
            responseJson.addProperty("message", this.message);
            responseJson.addProperty("data", gson.toJson(responseObject));
            return gson.toJson(responseJson);
        }
    }

```

## Validation Anotation
|Anotation|조건|
|---|---|
|@NotNull|Null 불가|
|@Null|Null만 입력 가능|
|@NotEmpty|Null, 빈 문자열 불가|
|@NotBlank|Null, 빈 문자열, 스페이스만 있는 문자열 불가|
|@Size(min=,max=)|문자열, 배열등의 크기가 만족하는가?|
|@Pattern(regex=)|정규식을 만족하는가?|
|@Max(숫자)|지정 값 이하인가?|
|@Min(숫자)|지정 값 이상인가|
|@Future|현재 보다 미래인가?|
|@Past|현재 보다 과거인가?|
|@Positive|양수만 가능|
|@PositiveOrZero|양수와 0만 가능|
|@Negative|음수만 가능|
|@NegativeOrZero|음수와 0만 가능|
|@Email|이메일 형식만 가능|
|@Digits(integer=, fraction = )|대상 수가 지정된 정수와 소수 자리 수 보다 작은가?|
|@DecimalMax(value=) |지정된 값(실수) 이하인가?|
|@DecimalMin(value=)|지정된 값(실수) 이상인가?|
|@AssertFalse|false 인가?|
|@AssertTrue|true 인가?|