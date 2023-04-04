# ENUM Validation

## 1. 목차

- [ENUM Validation](#enum-validation)
  - [1. 목차](#1-목차)
  - [2. Validator 작성](#2-validator-작성)
  - [2. Annotation 작성](#2-annotation-작성)
  - [3. Enum 작성](#3-enum-작성)
  - [4. Dto에 적용](#4-dto에-적용)

## 2. Validator 작성

```Java
public class EnumValidator implements ConstraintValidator<EnumAnnotation, Enum<?>>{
    private Pattern pattern;

    @Override
    public void initialize(EnumAnnotaion enumAnnotation)
    {
        try{
            pattern = Pattern.compile(enumAnnotation.regexp());
        } catch (PatternSyntaxExceptoin e) {
            throw new IlligalArgumentException("pattern regex is invalid", e);
        }
    }

    @Overide
    public boolean isValid(Enum<?> value, ConstraintValidatorContext context){
        if(value==null){
            return false;
        }

        Matcher m = pattern.matcher(value.name());
        return m.matches();
    }
}
```

## 2. Annotation 작성

```Java
@Constraint(validatedBy = EnumValidator.class)
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface EnumAnnotation {
    String regexp();
    String message() default "does not match {regexp}";
    Class<?>[] groupos() default {};
    class<? extends Payload>[] payload() default {};
}
```

## 3. Enum 작성

```Java
@Getter
public enum EncodeType{
    BASE64("BASE64"),
    HEX("HEX");
    private String code;
    EncodeType(String code){ this.code = code; }

    @JsonCreator(mode = JsonCreator.Mode.DELEGATING)
    public static EncodeType findByCode(String code){
        return Stream.of(EncodeType.values()).filter(c->c.code.equals(code)).findFirst().orElse(null);
    }
}
```

## 4. Dto에 적용

```Java
public class ValidDto{
    @EnumAnnotation(regexp = "BASE64|HEX", message = "허용된 값만 넣어야 한다.")
    private EncodeType type;
}
```
