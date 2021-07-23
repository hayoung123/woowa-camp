# 0717 라우트 구현 중 마주친 문제사항

## 문제사항 

### appendChild + innerHTML 시 appendChild의 이벤트 안 걸림

`appendChild` 중간에 `innerHTML`을 섞어서 사용했는데 

```javascript
 this.$target.appendChild(topBar);
    this.$target.innerHTML += `
        <div class='location-cautions'>
          <div>${this.CAUTION_LINE1}</div>
          <div>${this.CAUTION_LINE2}</div>
        </div>
    `;
```

위의 `appendChild`했던게 `+= `과정에서새로 복사? 및 덮어 씌워지면서 `topBar`에 적용해 놨던 이벤트가 날라가게 된다,

> 이벤트를 걸어둔 target이 변하지 않도록 조심해야 된다.



### 초기에 history.state를 어떻게 적용할까

`history.pushState`를 활용하면 history가 하나 쌓이기 때문에 초기로는 걸어줄 수 없다. 

이때 `history.replaceState`를 활용하면 초기값을 history를 쌓지 않고 설정할 수 있다. 



### 뒤로가기 앞으로가기 구별하는 법

`Router`에 `currentIndex`를 두고 `history.state`에 `index`를 넣어서 `pushState`를 해준다.

그렇게 되면 뒤로가기 혹은 앞으로 가기를 눌렀을 때 `currentIndex`와 `history.state.index`를 비교해서 구별할 수 있다.



### body에 이벤트 걸어주는 것을 조심하자

컴포넌트 내에서 body에 이벤트를 걸어줬는데 호출하는 클래스 컴포넌트를 호출해 인스턴스를 생성할 때 마다 body의 이벤트가 중첩되어서 이벤트 동작이 중복으로 발동된다. 이를 조심해야된다.

