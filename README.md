# React study<br>

<Vanilla_js><br>
Shopping mall Searchform_code study<br><br>
### MVC패턴<br>
화면 개발에서 많이 쓰이는 디자인 패턴<br>
1. Model : 데이터 관리<br>
2. View  : 사용자가 보는 화면 관리<br>
3. Controller : Model과 View를 연결하고 움직이는 주체<br>
---
### 가상돔<br>
* 화면에 그려지는 돔을 다뤄야 함<br>
* 상태변화의 횟수만큼 돔 api를 호출해야 함<br>
* js로 돔 구조를 변경시키면, 레이아웃이 다시 계산되어 픽셀로 화면에 그려짐<br>
* 돔을 수정한 만큼 작업이 반복<br>
* 페이지 렌더링 성능에 영향을 줌<br>
**  개선 방법 : 가상돔을 이용해 돔 api 호출을 줄여야 함<br><br>
    가상돔은<br>
    리액트 어플리케이션과 실제돔 사이에 위치하는 계층으로 이전돔과 현재돔을 비교해서 차이점만을 반영해 최소한의 연산으로 UI를 만든다
---
### 리액트 특성
1. 상태만 변경해도 자동으로 UI를 변경하도록 함
2. 매번 돔에 접근하지 않고, 꼭 필요한 경우에만 접근하는 방식으로 렌더링 성능을 개선(**가상돔**)
---
### 템플릿 언어 & JSX
* 돔은 트리형태의 element 구성<br>
-> 부모자식관계 코드 반복 (appendChild)<br>
* 단점 : 부모-자식 관계를 한눈에 파악하기 어려움<br>
** 개선 방법 : 템플릿 언어<br>
* 하지만, 리액트는 UI만 담당하는 작은 라이브러리이기 때문에 템플릿 언어 제공x<br>
** 개선 방법 : **JSX** (js 확장문법)<br>
    JSX은<br>
    Babal 라이브러리를 통해 jsx 문법 사용가능하고, UI 가독성을 높여준다. createElement 호출이 아니라 일반 HTML 코드를 작성하듯이 리액트 element를 작성할 수 있다<br>
    class 이름 선언시 "className" 사용해야 함
---
### 리액트에서 이벤트 처리 setState({});
* 리액트 컴포넌트는 state의 변화를 인지하고, render 함수를 자동으로 호출할 수 있다<br><br>
* 기본적으로 input element의 상태는 브라우저가 관리한다<br>
* 리액트에서 input을 관리하려면 브라우저의 역할을 리액트로 넘겨야 한다<br>
* 이때, 리액트의 **컴포넌트**를 사용한다 (state)<br>
* state값을 초기화 -> input의 value로 선언 -> state가 변경될때마다 input value로 자동 변경된다<br>
* (!) state를 변경하는 방법에 forceUpdate() 사용하면 안된다<br>
* **setState**를 사용해 state의 변화를 감지하고 render를 호출해 UI를 변경한다<br>
* setState는 비동기이다<br>
* setState가 완료되는 시점을 만들어줘야 한다<br>
--> input element 자체의 상태관리를 사용하지 않고,<br>
    컴포넌트가 관리하는 것을 **제어 컴포넌트**라고 한다<br>
---
### 조건부 렌더링
1. 엘리먼트 변수 사용
```
render() {
    let resetButton=null;
    if(this.state.searchKeyword.length>0){
      resetButton= <button type="reset" className="btn-reset"></button>
   }
```
실제 버튼 위치에 {resetButton} 입력<br>
2. 삼항연산자 사용
```
    return (
      <>
        <header>
          <h2 className="container">검색</h2>
        </header>
        <div className="container">
          <form>
            <input
              type="text"
              placeholder="검색어를 입력하세요"
              autoFocus
              value={this.state.searchKeyword}
              onChange={(event) => this.handleChangeInput(event)}
            />
            {this.state.searchKeyword.length>0?<button type="reset" className="btn-reset"></button>:null}
          </form>
        </div>
      </>
   );
```
3. &&연산자 사용
```
return (
      <>
        <header>
          <h2 className="container">검색</h2>
        </header>
        <div className="container">
          <form>
            <input
              type="text"
              placeholder="검색어를 입력하세요"
              autoFocus
              value={this.state.searchKeyword}
              onChange={(event) => this.handleChangeInput(event)}
            />
            {this.state.searchKeyword.length>0&&(<button type="reset" className="btn-reset"></button>)}
          </form>
        </div>
      </>
   );
```
첫번째 조건(length)가 만족되어야 두번째 조건(button)이 실행된다<br><br>
---
* 검색결과를 리스트 렌더링을 사용해 구현했다<br
* 리액트는 가상돔의 전 버전과 후 버전을 비교해서 변경된 부분만을 찾아 최소한의 돔 조작을 해서 렌더링 성능을 높인다<br>
* 트리구조를 비교하는 것은 상당히 무거운 구조이다<br>
* 계산 복잡도를 줄이기 위해 가정을 하고 렌더링을 한다<br>
1. 리스트 렌더링일때 "key"를 설정해서 가상돔 차이를 계산한다
2. state에 의존하기 때문에 state만 조정해서 UI를 쉽게 예측할 수 있다
---
### 컴포넌트 생명주기
* 리액트 컴포넌트는 생성부터 소멸까지의 생명주기를 가진다<br>
1. 컴포넌트 상태를 초기화
2. 컴포넌트 객체가 생성됨(constructor - state 초기화)
3. 리액트 엘리먼트를 이용해 가상돔 구현(render)하고 화면에 출력
4. (돔에 반영되는 것 = 마운드) 마운드가 완료되면 componentDidMount (event를 바인딩하거나 외부 데이터를 가져오는 일 수행)
5. 컴포넌트가 사라지기 전에 componentWillUnmount (event handle 제거, 리소스 정리 수행)
6. 컴포넌트 소멸
