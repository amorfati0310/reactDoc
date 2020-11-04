보통 리액트 컴포넌트를 정의할 때 JavaScript의 class를 사용한다면 이와 같을 겁니다.
```jsx
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
아직 ES6를 사용하지 않는다면, 그 대신 create-react-class 모듈을 사용할 수도 있습니다.

var createReactClass = require('create-react-class');
var Greeting = createReactClass({
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});
ES6 class의 API는 몇몇 차이점을 제외하고는 createReactClass()와 비슷합니다.
```

props나 state 기본 값은 함수에서 미리 정의하고 적용
```jsx
 getDefaultProps: function() {
    return {
      name: 'Mary'
    };
  },
  getInitialState: function() {
    return {count: this.props.initialCount};
  },
  // 
```
this binding은 class에서는 따로 해줘야 되는 반면 createReactClass 함수로 인자 넘겨주는 경우 
binding을 알아서 한다. 

반면에 createReactClass()를 사용한다면, 알아서 모든 메서드를 바인딩하기 때문에 위의 과정이 필요하지는 않습니다.
Class Properties 문법을 Babel을 사용하면 화살표함수로 안 적어주어도 된다. 
```js
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
  }
  // 경고: 이 문법은 실험적입니다!
  // 화살표 함수를 통해 메서드를 바인딩합니다.
  handleClick = () => {
    alert(this.state.message);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}

```

### mixin
- [mixin에 아쉬운 점들 읽어볼 것](https://ko.reactjs.org/blog/2016/07/13/mixins-considered-harmful.html) 
  

#### 믹스인 예제 
```js
var SetIntervalMixin = {
  componentWillMount: function() {
    this.intervals = [];
  },
  setInterval: function() {
    this.intervals.push(setInterval.apply(null, arguments));
  },
  componentWillUnmount: function() {
    this.intervals.forEach(clearInterval);
  }
};

var createReactClass = require('create-react-class');

var TickTock = createReactClass({
  mixins: [SetIntervalMixin], // mixin을 사용
  getInitialState: function() {
    return {seconds: 0};
  },
  componentDidMount: function() {
    this.setInterval(this.tick, 1000); // mixin에서 메서드를 호출
  },
  tick: function() {
    this.setState({seconds: this.state.seconds + 1});
  },
  render: function() {
    return (
      <p>
        React has been running for {this.state.seconds} seconds.
      </p>
    );
  }
});

```
life cycle이나 반복되는 처리 공용으로 쓸 수 있다. 
질문: ...이 부분 어떻게 실행되는 거지?


