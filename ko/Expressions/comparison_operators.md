# [비교 연산자](#comparison-operators)

* Go 버전: 1.9
* 원문: [Comparison operators](https://golang.org/ref/spec#Comparison_operators)
* 번역자: Jhonghee Park (@jhonghee)

비교 연산자로 피연산자 2개를 비교한 결과는 미지정 타입의(untyped) 불리언 상수값으로 제공된다.

```
==    같다
!=    같지 않다
<     작다
<=    작거나 같다
>     크다
>=    크거나 같다
```

어떤 비교 연산에서든, 첫번째 피연산자는 두번째 피연산자의 타입에 [할당 가능(assignable)](/Properties%20of%20types%20and%20values/assignability.html)해야 하며, 그 반대도 마찬가지이다.

동등 연산자인 `==`와 `!=`의 피연산자들은 *서로 비교할 수(comparable)* 있어야 한다. 순서 연산자인 `<`, `<=`, `>`, `>=`의 피연산자들은 확실하게 *우선 순위가 구별될 수(ordered)* 있어야 한다. 비교항목에 대한 정의와 비교연산을 수행한 결과는 다음과 같다:

 * 불리언 값들은 비교할 수 있다. 2개의 불리언 값이 모두 `true` 거나 `false`일때 2개의 값이 같다라고 말할 수 있다.
 * 정수 값들은 일반적으로 비교할 수 있으며 순서가 있다.
 * IEEE-754 표준에 따라 정의된 부동 소수점 값들은 비교할 수 있으며 순서가 있다.
 * 2개의 복소수 값 `u`와 `v`는 `real(u) == real(v)`와 `imag(u) == imag(v)`를 동시에 만족할 경우 서로 같다라고 말할 수 있다.
 * 문자열 값들은 어휘 바이트 단위(lexically byte-wise)로 비교할 수 있고 순서가 있다.
 * 2개의 포인터 값이 모두 `nil`이거나 같은 변수를 가리킬 때 서로 같다고 말할 수 있다. 서로 다른 [zero-size](/System%20considerations/size_and_alignment_guarantees.html) 변수에 대한 포인터는 같거나 다를 수 있다.
 * 만약 2개의 채널이 모두 `nil`값을 갖거나 같은 [make](/making_slices,_maps_and_channels.html)의 호출을 통해 만들어 졌다면 같다고 말할 수 있다.
 * 2개의 인터페이스가 [같은(identical)](/Properties%20of%20types%20and%20values/type_identity.html) 동적 타입, 같은 동적 값을 가지고 있거나 둘 다 `nil`값을 갖고 있을 때 같다고 말할 수 있다.
 * 인터페이스가 아닌 타입 `X`의 값 `x`와 인터페이스 타입 `T`의 값 `t`는 `X`가 비교 가능한 타입이면서 `T`를 구현하고 있는 경우 서로 비교할 수 있다.
 * 모든 필드가 비교 가능한 경우, struct 값들을 비교할 수 있다. 2개의 struct 값들은 상응하는 필드([blank 식별자](/Declarations%20and%20scope/blank_identifier.html)가 아닌)가 같을 경우 같다고 말할 수 있다.
 * 배열 요소 타입의 값들이 비교 가능하면 배열 값들은 비교할 수 있다. 상응하는 요소가 같다면 2개의 배열 값은 같다고 말할 수 있다.

같은 동적 타입을 갖는 2개의 인터페이스 값들을 비교할 때, 동적 타입의 값을 비교할 수 없다면 [런타임 패닉](/Run-time%20panics/)이 발생한다. 이 규칙은 인터페이스간의 직접 비교뿐만 아니라 인터페이스 값을 지닌 배열 비교와 인터페이스 값을 갖고 있는 필드를 지닌 struct간의 비교에도 적용된다.

원칙적으로는 슬라이스, map, 함수 값들을 비교할 수 없지만, 미리 선언된 식별자 `nil`과는 비교할 수 있다. 포인터, 채널, 그리고 인터페이스 값을 `nil`과 비교하는 것도 허용되며 위에 명시된 일반적인 규칙을 따른다.

```
const c = 3 < 4            // c는 미지정 타입의 불리언 상수 true

type MyBool bool
var x, y int
var (
	// 비교연산의 결과는 미지정 타입의 불리언이다.
	// 보통의 할당 규칙들이 적용된다.
	b3        = x == y // b3는 불리언 타입을 갖는다
	b4 bool   = x == y // b4는 불리언 타입을 갖는다
	b5 MyBool = x == y // b5는 MyBool 타입을 갖는다
)
```