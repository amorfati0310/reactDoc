### 01 Hello World 

```js
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```
여기는 단지 문서 소개 (js 선수지식 필요 etc ...)및 ReactDOM.render 함수를 이용해 jsx를 출력하는 예제가 있었다.

### 02 JSX 소개

1. js 표현식을 {} 안에 넣을 수 있다. 
2. ()로 묶는 편이 ; 입력도 그렇고 보기 좋다 
3. 값으로 함수, if,for 어디서나 쓰일 수 있다 
4. 속성값은 리터럴 혹은 {} 표현식으로 넣을 수 있다. 
5. class -> className , tabindex -> tabIndex  class는 예약어여서 그랬다고 들은 것 같다.
6. 태그가 비어 있다면 />
7. 렌더링 전에 주입되기 때문에 XSS 공격방지된다. 
8. babel이 jsx를 변환하여 React.createElement()로 컴파일 한다. 
example 
```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);

```

### 3.Rendering Element 

ReactElement plain object 
ReactDom이 ReactElement와 일치하도록 DOM을 업데이트 한다! 

DOM에 rendering 하기 위해서 ReactDom.render함수로 React element 전달 

React Element는 불변객체 


```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);

```

다시 render를 계속해도 변하는 부분 {new Date().toLocaleTimeString()} 이쪽 부분만 update 되고 있다! 
How? 

### Components & Props 

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation


js 함수 일종의 ui jsx(React.element) return 해주는 함수 

함수 컴포넌트와 class 컴포넌트가 있다. 

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
1. element를 ReactDom.render 호출 
2. props를 건내주고 welcome함수 호출 
3. <h1>Hello, Sara</h1> element 반환 
4. ReactDOM -> element 일치를 보며 DOM을 업데이트 

#### Composing Component

컴포넌트 안에서 다른 컴포넌트들을 참조할 수 있다. 조립해서 쓰기 용이! 
```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}


function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}

function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```
함수 SOC 에 따라 분리 -> 재사용성 높아지는 것 처럼 
컴포넌트도 자주 쓰는 UI 관점으로 분리해놓으면 좋음 ! 

-props는 읽기 전용

props를 다룰때 순수함수처럼 -> props 값을 직접 변경 조작 X 

### State and Lifecycle


```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);

```
class component state 
기존에 props로 update한 부분을 state로 변경해서 자체 update하도록 

#### Adding Lifecycle Methods to a Class

render mounted
removed unmounted 

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
this (this.timerID)에서 어떻게 타이머 ID를 제대로 저장하는지 주의해주세요.

this.props가 React에 의해 스스로 설정되고 this.state가 특수한 의미가 있지만, 타이머 ID와 같이 데이터 흐름 안에 포함되지 않는 어떤 항목을 보관할 필요가 있다면 자유롭게 클래스에 수동으로 부가적인 필드를 추가해도 됩니다.
- 데이터 흐름에 포함되지 않고,  컴포넌트가 자체적으로 가지고 있을만한 애도 state로 관리 ! 


class Component 호출 constructor -> props , state 초기화 -> render() -> componentDidmount 
setState 변경점 -> render() 메소드 재호출 

#### State 주의사항 

비동기적으로 처리되기 때문에 다음 state를 계산할 때 해당 값에 의존해서는 안 됩니다.
```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});

계산 할 때 

// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```
state는 캡슐화 혹은 로컬 data 이를 props로 전달할 수 있다. 

### 이벤트 처리하기 

DOM과 유사 
onClick camelCase 사용하고 함수로 연결 
addEventListener 호출 필요 없음


```js
// this binding 번거로움 해결 방법 
class component에서는 이 방법 권장

handleClick = () => {
    console.log('this is:', this);
  }

// 익명함수는  render시 마다 매번 새로운 함수 할당 안 해도 되는 하위 props rerendering이 일어날 수도 있음 
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 이 문법은 `this`가 handleClick 내에서 바인딩되도록 합니다.
    return (
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
}
```

인자 전달하기 
```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

### 조건부 렌더링 

or 조건 분기 render
 ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />
논리연산자 활용 &&

render하지 않도록 하려면 null 
```js
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}
```

### 리스트와 키 