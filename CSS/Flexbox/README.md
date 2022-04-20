# CSS / Flexbox

## flex-basis, flex-shrink, flex-grow

Flexbox는 자손들에게 `width: fit-content;` 과 같은 효과를 자동으로 적용시킨다.(flex-direction이 기본이라는 가정)

만약 container가 item들을 다 그리고 나서 남는 공간은 어떻게 할까?

child element에 `flex-grow: 1;` 을 설정하면 해당 element가 남는 공간을 차지하게 할 수 있다. 만약 여러 child element에 이를 설정하면? 설정된 child element들이 각각 남는 공간을 균등하게 분배해서 차지한다.

이때 균등하게 분배할 것이 아니라 특정 child element만 많이 차지하고 싶도록 하고 싶다면 `flex-basis`를 사용하면 된다.

```css
/* Specify <'width'> */
flex-basis: 10em;
flex-basis: 3px;
flex-basis: auto;

/* Intrinsic sizing keywords */
flex-basis: fill;
flex-basis: max-content;
flex-basis: min-content;
flex-basis: fit-content;

/* Automatically size based on the flex item's content */
flex-basis: content;
```

참고로 `flex-basis`는 몇 가지 특징이 있는데
1. flex-direction에 따라 `너비`가 되기도 하고 `높이`가 되기도 한다.
  `flex-basis: 200px;`로 설정하면, `flex-direction: row;` 에서는 너비를 의미하고, `flex-direction: column;`에서는 높이를 의미한다.
1. `flex-basis`는 `width`나 `height`보다 우선순위가 높다.
  `width` 혹은 `height`가 `flex-basis`보다 나중에 정의되더라도 `flex-basis` 속성이 사용된다. <br/>
  그렇기 때문에 **flex-basis와 width 혹은 height을 같이 써야 할 경우에는 주의가 필요하다**.
1. `flex-grow` 는 `flex-basis` 보다 우선순위가 높다.
  예를 들어 child element에 `flex-grow: 1; flex-basis: 100px;`을 선언하고 container는 `500px` 만큼의 너비를 갖고 있다면 child element는 500px만큼 그려지게 된다.(`flex-grow: 1`이 적용됐기 때문)
1. width나 height과 다르게 minimum size보다 작게 그려질 수 없다. <br/>
  예를 들어 긴 텍스트의 minimum size가 100px이라고 했을 때 `width: 50px;`을 설정하면 초과하는 영역은 container를 벗어나서 그려지게 된다. 하지만 `flex-basis: 50px;`을 설정하면 텍스트는 minimum size인 100px 만큼의 너비로 그려진다. <br/>
  https://codepen.io/sundicide/pen/KKZEggK

반대로 모든 child element의 size가 flex container를 child elements들은 shrink되는데 이를 `flex-shrink`로 조절할 수 있다.

```css
/* <number> values */
flex-shrink: 2;
flex-shrink: 0.6;
```

`flex-shrink` 값이 `2`라는 것은 값이 `1(default)`인 다른 flex-item에 비해 2배의 속도로 빠르게 shrink된다는 의미이고, `0`은 shrink하지 않는다는 의미이다.

## Links
- [MDN - Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)
- [MDN - flex-basis](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-basis)
- [MDN - flex-shrink](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-shrink)