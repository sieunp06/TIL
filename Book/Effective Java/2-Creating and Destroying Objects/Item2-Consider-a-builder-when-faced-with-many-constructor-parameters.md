# Item 2 생성자에 매개변수가 많다면 빌더를 고려하라
## ❓점층적 생성자 패턴
점층적 생성자 패턴(telescoping constructor pattern)은 필수 매개변수만 받는 생성자에 선택 매개변수를 갖는 생성자를 필요한 만큼 두는 패턴이다.

예를 들어, 식품 포장의 영양정보를 표현한다면 다음과 같은 필드를 가져야 한다.
- servingSize: 필수
- servings: 필수
- calories: 선택
- fat: 선택
- sodium: 선택
- carbohydrate: 선택

이런 필드를 가진 식품 포장의 영양정보를 점층적 생성자 패턴으로 나타내면 다음과 같다.

```java
public class NutritionFacts {
    private final int servingSize;  // (ml, 1회 제공량), 필수
    private final int servings;     // (회, 총 n회 제공량), 필수
    private final int calories;     // (1회 제공량당), 선택
    private final int fat;          // (g/1회 제공량), 선택
    private final int sodium;       // (mg/1회 제공량), 선택
    private final int carbohydrate; // (g/1회 제공량), 선택

    public NutritionFacts(int servingSize, int servings) {
        this(servingSize, serving, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories) {
        this(servingSize, servings, calories, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories, int fat) {
        this(servingSize, servings, calories, fat, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium) {
        this(servingSize, servings, calories, fat, sodium, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
        this.servingSize = servingSize;
        this.servings = servings;
        this.calories = calories;
        this.fat = fat;
        this.sodium = sodium;
        this.carbohydrate = carbohydrate;
    }
}
```
필요한 매개변수를 모두 포함한 생성자 중 가장 짧은 것을 골라 사용하면 된다.
```java
NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
```

### 점증적 생성자 패턴의 단점
#### 설정하기 원치 않는 매개변수에도 값을 지정해줘야 한다.

#### 매개변수 개수가 많아지면 코드를 작성하거나 읽기 어렵다.

#### 타입이 같은 매개변수가 연달아 늘어서 있으면 찾기 어려운 버그로 이어질 수 있다.

## ❓자바빈즈 패턴
