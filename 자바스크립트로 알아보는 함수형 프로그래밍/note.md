
### 메서드와 함수는 다르다

+ 차이점 1. 메서드는 객체지향 프로그래밍으로 해당 클래스에 종속적이다.
map, filter 메서드는 Array에서만 사용할 수 있다. 이는 ArrayLike 객체에서는 메서드를 사용할 수 없다는 것과 같다. (ex, jQuery 셀렉터로 생성된 객체는 ArrayLike 객체)
함수를 사용하면 이런 ArrayLike 객체더라도 동작한다. 이 강의에서 만든 _map, _filter는 객체에 index가 있고, length가 있으면 동작한다.
함수를 사용하면 높은 수준의 다형성을 이용할 수 있다.

+ 차이점 2. 메서드는 데이터가 먼저 있어야 기능을 이용할 수 있다.
메서드와 함수는 데이터의 평가 시점이 다르다. 메서드는 데이터가 우선, 함수는 함수가 우선이다.

+ 차이점 3. 함수는 콜백에 따라 동작을 유연하게 가져갈 수 있다.
함수 내부에서는 데이터를 직접 접근하지 않는다. 데이터 처리는 전적으로 콜백에 위임한다.
함수를 사용하면 데이터의 타입이나 형태에 구애받지 않고 개발할 수 있다.

* * *

### Currying
함수에 필요한 인자를 하나씩 적용해나가다가 필요한 인자가 채워지면 함수 본체를 실행하는 기법.
함수의 인자를 고정적인 인자와 가변적인 인자로 묶어서 구성할 수 있다.
고정적인 인자를 먼저 채워두면 가변적인 인자를 채워 반환된 데이터는 고정적 인자에 종속적인 데이터를 얻을 수 있다.
인자를 적용하는 방향(왼쪽에서 오른쪽 or 그 반대)에 따라 오른쪽에서 왼쪽이면 함수 이름 뒤에 관례적으로 r(right)을 붙인다.
콜백 함수로 유용하게 이용할 수 있다.

    const _curry = fn => {
      return function(a, b) {
        return arguments.length === 2 ? 
          fn(a, b) : b => fn(a, b)
      }
    }

    const _curryr = fn => {
      return function(a, b) {
        return arguments.length === 2 ?
          fn(a, b) : b => fn(b, a)
      }
    }

    const _get = _curryr((obj, key) => 
      obj === null ? undefined : obj[key]
    )

    const get_name = _get('name')

    console.log(get_name(users[1]))
    console.log(
      _map(
        _filter(users, user => user.age >= 30),
        _get('name') // users 라는 배열에 있는 user 객체에서 name의 value 반환
      )
    )