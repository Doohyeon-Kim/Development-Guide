
## Philosophy

1. 서버 사이드는 한정된 자원으로 여러 요청을 처리해야 함으로, 가독성만 신경쓰기 보다는 CPU와 메모리에 대한 고려도 적극적으로 하면서 개발한다.
2. 가독성은 메소드를 분리하면 되기 때문에 문제가 되지 않는다. 가독성이 문제라면 메소드를 분리하자.
3. 높은 확률로 가까운 미래에 추가될 것으로 예상되는 기능이 아니라면, 불필요한 기능을 구현하는 것은 피한다.
4. 수학 증명 같은 코드가 아닌, 영어로 읽히는 코드를 작성하자. 
    1. 예시 (Java는 named parameter를 지원하지 않기에 pseudo 코드로 표현함)
        ```
          Transfer(){
            call(remittance: remittance, from: myAccount, into: userAccount){
              bankRepository = BankRepositry();
              userAccount = await bankRepository.find(userAccount);
            if(userAccount.isValid()){
                if(myAccount.balance > remittance){
                  await bankRepository.transfer(amount: remittance, from: myAccount, into: userAccount);
                  return Result("SUCCESS");
                }else{
                  return ErrorMessage("Your balance is insufficient.");
                }
              }else{
                stopAction();
                return ErrorMessage("This user account is invalid.");
              }
            }
          .
          .
          .
          }
        ```
   
        
    2. 개발자가 아닌 사람이 코드를 읽어도 읽히는 코드가 가장 좋다.
        1. 함수 호출 코드
            
            ```
            transfer(remittance: 1000, from: myAccount, into: yourAccount);
            ```
            
        2. 영어
      
            ```
            Transfer 1000 from my account into your account.
            ```
            

## Architecture

1. 구성원들의 익숙함을 고려하여 Controller - Service - Repository의 구조를 갖도록 하나, DIP를 사용하여 최소한의 클린 아키텍처를 구현한다.
2. UseCase가 없는 대신에 Service의 메소드를 가능한 잘게 쪼갠다.
    1. 쪼갤 수록 테스트가 용이해지고 코드를 재사용 할 수 있게 된다.
3. Controller에서는 Service 의 호출과 Exception 처리만을 담당한다.
    1. Repository는 CustomizedRepository와 JpaRepository로 구분하고, JpaRepository는 DIP를 적용하지 않고 interface adapter 레이어에 둔다.

## Policies

1. 데이터 타입을 맞추기 위한 경우가 아니라면 클래스는 상속이 아닌 합성을 사용한다.
    1. 불필요한 상속을 막기 위해 클래스에 final 키워드를 붙여준다.
2. 모든 클래스는 가급적 immutable하게 관리한다.
    1. 유틸리티를 위한 클래스를 만들 때 패키지에서 필수적으로 요구되는 등의 예외사항이 아니라면 Setter 대신 Builder 패턴을 사용하자.
3. 오버로딩(Overloding)은 가급적 사용하지 않는다.
    1. 같은 클래스 내에 동일한 메소드명이 있는 건 코드 사용자에게 혼란을 야기한다.
    2. 오버로딩 메소드는 컴파일 타임에 결정되기 때문에, 파라미터의 갯수가 같은 메소드가 있으면 원치않는 동작을 발생시킬 수 있다.
    3. 사용하게 된다면 메소드 간 위치를 붙여놓는다.
4. Stream API는 필요한 경우에만 사용한다. (병렬처리 등)
    1. Stream API는 가독성도 떨어지고 내부적으로 iteratior와 람다식을 사용하기 때문에 성능도 더 느리다.
    2. 람다식이 많아지면 흐름 추적이 어려워 유지보수 측면에서도 더 부담이 있다.
    3. Stream은 lazy하게 동작하기 때문에 원하는 결과가 나오지 않을 수 있다.
5. 반복문을 사용할 때는 Stream API(Filter, Map) 보다는 for문을 사용하자.
    1. for문은 속도도 더 빠르고 메모리 근접성의 원리를 적용시킬 수 있다.
    2. 향상된 for문을 사용하면 배열이나 ArrayList(내부 구현이 배열)는 메모리 친화적으로 동작하고, LinkedList 같은 자료구조인 경우 Iterator를 사용한다.
    3. 특히 nested for문이나 원시타입을 사용하게 될 경우 성능차이가 매우 크게 발생 할 수 있다.
    4. Stream API를 사용하게 되면 try/catch를 통해 Exception을 한 번 더 관리해야한다.
        1. AOP로 구현된 RestControllerAdvice가 있는데 exception을 또 관리하게 된다.
        2. mapper에서 이미 exception을 발생시키고 있는 경우 이는 불필요한 중복 코드가 된다.
6. var 타입을 사용하지 않는다.
    1. 타입은 명시적으로 선언한다.
    2. Object도 되도록 피할 것을 권장하며, 차라리 casting을 사용한다.
        
        
        | Bad 😞 | Good 😊 |
        | --- | --- |
        | `<Object, Object> map` | **<String, Object> map** |
        
        이렇게 구현하면 컴파일 시 개발자의 의도와 다른 부분을 컴파일러가 캐치해서 출력해줄 수 있다.
        
7. Exception이 필요한 이유가 명확한 게 아니면 if, else를 사용하여 예외처리를 한다.
    1. 코드가 직관적이고 성능상의 이점도 있다.
8. 하나의 메소드와 클래스는 하나의 목적을 두게 만든다.
    1. 하나의 메소드 안에서는 하나의 일을 해야 한다.
    2. 하나의 클래스 안에서는 같은 목적을 둔 코드들의 집합이어야 한다.
9. 메소드와 클래스는 최대한 작게 만든다.
    1. 비대해진 클래스는 여러동작이 추가 될 소지가 크다.
10. 도메인 서비스를 만드는것을 지양한다.
    1. pass 이라는 도메인이 있을때 passService 로 만드는 것을 피한다.
    2. 해당 서비스는 목적을 잃고 책임이 커지게 된다.
11. 2회 이상 반복되는 함수는 공통으로 사용할 수 있게 한다.
12. `booleanBuilder`를 repository에 나열하지 않고, Predicate 패턴을 사용한다.
13. 하나의 데이터에 두 가지 상태를 저장 하지 않는다. 
    
    
    | Bad 😞 | Good 😊 |
    | --- | --- |
    | `limit이 1 이상이면 제한, 0이면 무제한` | **limit은 1부터 시작, 무제한은 별도 상태 값을 추가 후 먼저 확인** |
14. null은 사용하지 않는다. 사용하게 되면 주석이나 로그를 남겨 맥락을 표현한다.
    1. null은 의미가 지나치게 함축적이라 유지보수가 매우 어려워진다.
    2. null을 잘못 다루는 경우 런타임 에러가 발생할 수 있다.
    3. java는 default argument 설정이 되지 않아서 맥락 처리를 위한 메소드를 만들어야 한다.

## Coding Convention

1. 명시된 내용이 아니면 구글 자바 스타일 가이드를 참조한다. (https://google.github.io/styleguide/javaguide.html)
2. 파일 구조
    1. 라이센스 (존재하는 경우)
    2. package
    3. import
    4. 최상위 class
3. 줄바꿈
    1. 코드가 길어져서 줄바꿈이 필요할 때는 higher syntactic level의 앞에서 바꿔준다.
        1. . (dot separator)
        2. :: (람다)
        3. & (<T extends Foo & Bar>)
        4. | (FooException | BarException e)
    2. ,(comma)는 앞의 token의 뒤에 붙여서 적는다.
    3. 예시
       
       ![Screenshot 2024-08-27 at 19 02 29](https://github.com/user-attachments/assets/62f1b806-b372-4a8c-a42c-1efa535e9eb5)            
    
4. double negative(이중 부정문) 사용 금지
    
    
    | Bad 😞 | Good 😊 |
    | --- | --- |
    | `disabled = false` | **enabled = true** |
    | `hidden = false` | **visible = true** |

## Naming Convention

1. 네이밍은 길어지더라도 최대한 이름만 보고 뜻을 알 수 있게 작성한다.
    
    
    | Bad 😞 | Good 😊 |
    | --- | --- |
    | `isOkayUser` | **isUserEligibleForMembership** |
    | `check` | **checkIfUserIsEligibleForPremiumMembership** |
    | `procOrder` | **processOrder** |
    
    관용적인 용어가 아니면 약어 사용을 피한다.
    
    | Bad 😞 | Good 😊 |
    | --- | --- |
    | `i` | **index** |
    | `horizontalPosition` | **x** |
    | `Calc` | **MathOperationExecutor** |
2. 변수 네이밍은 형태/종류, 의미/속성, 숫자, 상태로 조합하여 작성한다.
    
    
    | Bad 😞 | Good 😊 |
    | --- | --- |
    | `quantity01Order` | **orderQuantity01** |
    | `activeUser` | **userActivated** |
3. Java 파일 이름은 파일의 최상위 클래스 이름과 .java 확장자로 구성한다.
4. 디렉토리 이름에 띄어쓰기 표시가 필요한 경우 _(underscore)로 표시한다.
    1. 예시: interface_adapter
5. 함수는 동사로 네이밍한다.
6. 약어는 CamelCase를 사용한다.
    
    
    | Bad 😞 | Good 😊 |
    | --- | --- |
    | `WEBAPIDTO` | **WebApiDto** |
7. 경로는 끝에 /를 붙이지 않으며, 띄어쓰기가 필요할 때는 -를 붙인다.
    
    
    | Bad 😞 | Good 😊 |
    | --- | --- |
    | `/path/path/` | **/path/path-path** |
8. ENUM , 상수는 대문자로 사용한다.
9. Collection 타입인 경우 타입이 List면 List, 아니면 s/es를 사용한다.
    1. 예시
        1. Set, Stream: members
        2. ArrayList, LinkedList: memberList
10. DB 쿼리와 구분하기 위해, 서버에서는 아래 키워드를 사용한다.
    1. get: 단순 조회(다건, 단건)
        1. getMember, getMembers, getMemberList
    2. search: 검색
    3. register: 등록
    4. modify: 수정
    5. remove: 삭제

## etc

1. 기본적인 encoding은 UTF-8을 사용한다.
2. 함수의 코드가 200줄이 넘어가면 뭔가 잘못 되었을 확률이 매우 높다.
