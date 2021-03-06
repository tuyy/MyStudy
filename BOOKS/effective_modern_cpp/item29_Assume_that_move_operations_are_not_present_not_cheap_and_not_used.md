이동 의미론 덕분에 컴파일러는 비싼 복사 연산을 비교적 저렴한 이동 연산으로 대체할 수 있을 뿐만 아니라, 적절한 조건이 만족되는 경우에는 반드시 그렇게 대체해야 한다. (표준 준수를 위해서)

이동 연산을 명시적으로 지원하지는 않는 형식에 대해 C++11이 자동으로 이동 연산들을 작성해 주긴 하지만, 형식에 복사 연산이나 이동 연산, 소멸자가 하나라도 있으면 그러한 자동 작성은 일어나지 않는다.

또한, 이동을 명시적으로 지원하는 형식에서도, 성능상의 이득이 생각만큼 크지 않을 수 있다.
예를 들어 C++11 표준 라이브러리 중에 std::vector와 std::array를 비교해 보면,

std::vector<Widget> vw1;
auto vw2 = std::move(vw1);

vw1 -> widget들
vw1  -> nullptr  // std::move(vw1);로 이동연산이 동작하면서 vw1은 nullptr을 가리키고,
vw2 -> wdget들  // vw1이 가리키던 포인터를 vw2가 가리키게 된다.


std::array 의 경우엔 std::vector가 가지고 있는 데이터를 가리키는 포인터가 없고, 내용이 객체 자체에 직접 저장된다.

std::array<Widget, 100000> aw1;
auto aw2 = std::move(aw1);  // aw1의 각 원소가 aw2로 이동된다. 즉, 컨테이너의 모든 요소를 일일이 이동 또는 복사해야한다.


std::string은 상수 시간 이동과 선형 시간 복사를 제공한다. 이점을 생각하면 이동이 복사보다 빠를 것 같지만, 반드시 그렇지는 않을 수 있다. 문자열 구현 중에는 작은 문자열 최적화(small string optimization, SSO)를 사용하는 것들이 많다.

이러한 SSO 기반 구현을 사용하는 작은 문자열의 이동은 복사보다 빠르지 않다. 대체로 이동이 복사보다 빠른 것은 포인터 하나만 복사하면 되기 때문인데, 이 경우에는 그런 요령을 적용할 수 없기 때문이다.


빠른 이동이 보장되는 형식에서도, 겉으로 보기에는 이동이 일어날 만한 상황에서 사실은 복사가 일어나는 경우도 있다.

1) 이동 연산이 없는 경우
2) 이동이 더 빠르지 않는 경우
3) 이동을 사용할 수 없는 경우 (해당 연산이 noexcept로 선언되어 있지 않는 경우)
4) 원본 객체가 왼값이다.


기억해 둘 사항들
1) 이동 연산들이 존재하지 않고, 저렴하지 않고, 적용되지 않을 것이라고 가정하라.
2) 형식들과 이동 의미론 지원 여부를 미리 알 수 있는 경우에는 그런 가정을 둘 필요가 없다.
