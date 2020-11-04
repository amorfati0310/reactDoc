### Accessibility 

WCAG
Web Content Accessibility Guidelines는 접근성을 갖춘 웹사이트를 만드는 데 필요한 지침을 제공합니다.


WAI-ARIA 

html과 마찬가지로 hypen-case(혹은 kebab-case, lisp-case 등)로 작성
```js
<input
  type="text"
  aria-label={labelText}
  aria-required="true"
  onChange={onchangeHandler}
  value={inputValue}
  name="name"
/>

```

여러 요소를 묶어줄때 Fragment | <> 적절히 사용합시다 <> <- props나 key 필요 없는 경우 

```js
function ListItem({ item }) {
  return (
    <Fragment>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </Fragment>
  );
}

```
#### 접근성 있는 폼 

```jsx
<label htmlFor="namedInput">Name:</label>
x


```
label에 for 대신 htmlFor 


여기서 보여주는 링크들 다 읽어보긴 해야 할 텐데 너무 방대하다 .... 
- 접근성 지침 
- 라벨링 방법 지침
- 사용자들에게 오류 안내하기 

여기는 상대적으로 나중에 챙겨보기 ... 

#### 키보드 포커스와 포커스 윤곽선

focus element 다른 윤곽선으로 교체할때만 outline 0 으로 놓고 제어 

#### 원하는 콘텐츠로 건너뛸 수 있는 방법