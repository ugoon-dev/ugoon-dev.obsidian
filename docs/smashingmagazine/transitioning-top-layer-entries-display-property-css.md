# CSS에서 Top-Layer 항목과 Display 속성의 전환

우리는 [스크롤 기반 애니메이션](https://www.smashingmagazine.com/2024/12/introduction-css-scroll-driven-animations/)부터 [뷰 전환](https://www.smashingmagazine.com/2023/12/view-transitions-api-ui-animations-part1/)까지 CSS의 많은 새로운 기능들로 인해 점점 더 풍부한 기능을 사용할 수 있게 되었습니다. 하지만 항상 큰 기능들만이 우리의 일상적인 작업을 더 쉽게 만드는 것은 아닙니다. 때로는 작은 편의성 기능들이 우리의 프로젝트를 진정으로 향상시키기도 합니다. 이 글에서는 `@starting-style`과 `transition-behavior`라는 두 가지 속성을 소개하겠습니다. 이 두 속성은 CSS 애니메이션 작업에 매우 유용한 추가 기능입니다.

## display: none에서의 전환

JavaScript를 사용하여 클래스를 변경하거나 다른 해결책을 사용하지 않고는 `display: none`에서 전환하는 것은 불가능했습니다. 새로운 [CSS Transitions Level 2 명세](https://www.w3.org/TR/css-transitions-2/#defining-before-change-style)에서는 이에 대해 다음과 같이 설명합니다:

> "이 명세의 Level 1에서는, 이전 스타일 변경 이벤트에 의해 설정된 변경 전 스타일이 정의된 요소에 대해서만 스타일 변경 이벤트 중에 전환을 시작할 수 있습니다. 즉, 이전 스타일 변경 이벤트에서 렌더링되지 않은 요소에 대해서는 전환을 시작할 수 없었습니다."

간단히 말해서, 숨겨져 있거나 방금 생성된 요소에 대해서는 전환을 시작할 수 없었다는 의미입니다.

## transition-behavior: allow-discrete는 무엇을 하나요?

`allow-discrete`는 CSS 속성 값으로는 조금 이상한 이름처럼 보입니다. 우리는 `display: none`의 전환에 대해 이야기하고 있는데, 왜 이것이 `transition-behavior: allow-display`라고 이름 지어지지 않았을까요? 그 이유는 이 속성이 CSS `display` 속성만을 다루는 것이 아니기 때문입니다. CSS에는 다른 "이산적(discrete)" 속성들도 있습니다. 간단한 규칙은 이산적 속성들이 전환되지 않고 보통 두 상태 사이에서 즉시 전환된다는 것입니다. 다른 예로는 `visibility`와 `mix-blend-mode`가 있습니다.

요약하면, `transition-behavior` 속성을 `allow-discrete`로 설정하면 브라우저에게 이산적 속성(예: `display`, `visibility`, `mix-blend-mode`)의 값을 전환의 0% 지점이 아닌 50% 지점에서 바꿀 수 있다고 알려주는 것입니다.

## @starting-style은 무엇을 하나요?

`@starting-style` 규칙은 요소가 페이지에 렌더링되기 직전의 스타일을 정의합니다. 이는 `transition-behavior`와 함께 매우 필요한 기능입니다. 그 이유는 다음과 같습니다:

요소가 DOM에 추가되거나 초기에 `display: none`으로 설정되었을 때, 전환을 시작할 수 있는 "시작 스타일"이 필요합니다. 예를 들어, 팝오버와 다이얼로그 요소는 문서 흐름 외부에 있는 최상위 레이어에 추가됩니다. 이는 페이지 구조에서 `<html>` 요소의 형제처럼 볼 수 있습니다. 이 다이얼로그나 팝오버를 열 때, 그들은 최상위 레이어 안에 생성되므로 전환을 시작할 스타일이 없습니다. 이것이 바로 우리가 `@starting-style`을 설정하는 이유입니다.

## 브라우저 지원 참고사항

글 작성 시점에서 `transition-behavior`는 Chrome, Edge, Safari, Firefox에서 사용 가능합니다. `@starting-style`도 마찬가지이지만, Firefox는 현재 `display: none`에서의 애니메이션을 지원하지 않습니다. 하지만 이 글의 모든 내용은 점진적 향상으로 완벽하게 사용될 수 있습니다.

## 실제 사용 예시

### 1. DOM에서 display: none으로의 전환

기본 HTML 구조:
```html
<button type="button" class="btn-add">
  Add item
</button>
<button type="button" class="btn-remove">
  Remove item
</button>
<ul role="list"></ul>
```

JavaScript 코드:
```javascript
document.addEventListener("DOMContentLoaded", () => {
  const addButton = document.querySelector(".btn-add");
  const removeButton = document.querySelector(".btn-remove");
  const list = document.querySelector('ul[role="list"]');

  addButton.addEventListener("click", () => {
    const newItem = document.createElement("li");
    list.appendChild(newItem);
  });

  removeButton.addEventListener("click", () => {
    if (list.lastElementChild) {
      list.lastElementChild.classList.add("removing");
      setTimeout(() => {
        list.removeChild(list.lastElementChild);
      }, 200);
    }
  });
});
```

CSS 스타일:
```css
ul {
    li {
      opacity: 1;
      transform: translate(0, 0);
      transition: opacity 0.2s, transform 0.2s;

      @starting-style {
        opacity: 0;
        transform: translate(0, -50%);
      }

      &.removing {
        opacity: 0;
        transform: translate(0, 50%);
      }
    }
  }
```

### 2. 최상위 레이어에서의 다이얼로그 전환

기본 HTML 구조:
```html
<button class="open-dialog" data-target="my-modal">Show dialog</button>

<dialog id="my-modal">
  <p>Hi, there!</p>
  <button class="outline close-dialog" data-target="my-modal">
    close
  </button>
</dialog>
```

CSS 스타일:
```css
dialog {
  opacity: 0;
  translate: 0 30%;
  transition-property: opacity, translate, display, overlay;
  transition-duration: 0.8s;
  transition-behavior: allow-discrete;

  &[open] {
    opacity: 1;
    translate: 0 0;

    @starting-style {
      opacity: 0;
      translate: 0 -30%;
    }
  }

  &::backdrop {
    opacity: 0;
    transition-property: opacity, display, overlay;
    transition-duration: 1s;
  }

  &[open]::backdrop {
    opacity: 0.8;

    @starting-style {
      opacity: 0;
    }
  }
}
```

## 결론

이것으로 요소를 최상위 레이어 안팎으로 전환하는 방법에 대한 개요를 마치겠습니다. 이상적인 세상에서는 "전환할 수 없는" 속성을 전환하기 위해 `transition-behavior`와 같은 완전히 새로운 속성이 필요하지 않을 것입니다. 하지만 현재 우리가 있는 곳이 여기이고, 이 속성이 있어서 기쁩니다.

또한 `@starting-style`에 대해서도 배웠고, 이것이 브라우저에게 최상위 레이어에 있는 요소의 전환 시작 시 적용할 수 있는 스타일 세트를 제공하는 방법에 대해서도 배웠습니다. 그렇지 않으면 요소는 처음 렌더링될 때 전환할 것이 없고, 최상위 레이어 안팎으로 부드럽게 전환할 방법이 없을 것입니다.

> 출처: [Smashing Magazine - Transitioning Top-Layer Entries And The Display Property In CSS](https://www.smashingmagazine.com/2025/01/transitioning-top-layer-entries-display-property-css/)