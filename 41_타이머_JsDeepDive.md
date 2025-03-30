## 41장. 타이머

### 41.1 호출 스케줄링

  함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 하려면 타이머 함수를 사용하는데 이를 호출 스케줄링이라 한다.

  타이머를 생성하는 setTimeout, setInterval함수 / 타이머를 제거하는 clearTimeout, clearInterval을 제공.

  자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖기 때문에 싱글 스레드로 동작한다. 그리하여 setTimeout, setInterval은 비동기 방식으로 동작한다.

### 41.2 타이머 함수

  #### 41.2.1 setTImeout / clearTimeout

  setTimeout의 콜백 함수는 두 번째 인수로 전달받은 시간 이후 단 한 번 실행되도록 호출 스케줄링 된다.

    const timeoutId = setTimeout(콜백함수, delay, param1, param2...);

  setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 부여하여 clearTImout의 인수로 전달해 타이머를 취소할 수 있다.

  #### 41.2.2 setInterval / clearInterval

  setInterval 함수의 콜백함수는 두 번째 인수로 전달받은 시간이 경과할 때마다 반복 실행되도록 호출 스케줄링된다.

    const timerId = setInterval(콜백함수, delay, param1, param2...);

  setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하며 타이머를 취소 할 수 있다.

### 41.3 디바운스와 스로틀 

  짧은 시간 간격으로 연속해서 발생하는 이벤트 (scroll. resize, mouseover..)를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.

  #### 41.3.1 디바운스

  디바운스는 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되도록 한다.

[예제]
```
<!DOCTYPE html>
<html>
<body>
    <input type="text" id="searchInput" placeholder="검색어를 입력하세요">
    <div id="result"></div>

    <script>
        const $input = document.getElementById('searchInput');
        const $msg = document.getElementById('result');

        const debounce = (callback, delay) => {
            let timerId;

            return (...args) => {
                if (timerId) clearTimeout(timerId);
                timerId = setTimeout(callback, delay, ...args);
            }
        }

        $input.oninput = debounce(e => {
            $msg.textContent = e.target.value;
        },300);
    </script>
</body>
</html>
```

#### 41.3.2 스로틀

짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.

```
<!DOCTYPE html>
<html>
<head>
    <title>스로틀링 예제</title>
</head>
<body>
    <h1>스로틀링 테스트</h1>
    <button id="throttleButton">클릭하세요</button>
    <p id="result">클릭 횟수: 0</p>

    <script>
        // 스로틀링 함수
        function throttle(func, limit) {
            let inThrottle;
            return function(...args) {
                if (!inThrottle) {
                    func.apply(this, args);
                    inThrottle = true;
                    setTimeout(() => inThrottle = false, limit);
                }
            }
        }

        let count = 0;
        const resultElement = document.getElementById('result');
        
        // 실행될 함수
        function handleClick() {
            count++;
            resultElement.textContent = `클릭 횟수: ${count}`;
        }

        // 스로틀링 적용 (1초마다 최대 1번만 실행)
        const throttledClick = throttle(handleClick, 1000);

        // 이벤트 리스너 등록
        document.getElementById('throttleButton')
            .addEventListener('click', throttledClick);
    </script>
</body>
</html>

```

