# 키보드 접근성 가이드라인

## 목차
1. [키보드 접근성의 이해](#키보드-접근성의-이해)
2. [포커스 관리](#포커스-관리)
3. [키보드 인터랙션](#키보드-인터랙션)
4. [키보드 트랩 방지](#키보드-트랩-방지)
5. [구현 가이드](#구현-가이드)

## 키보드 접근성의 이해

키보드 접근성은 마우스를 사용할 수 없는 사용자들이 키보드만으로 웹사이트의 모든 기능을 이용할 수 있도록 보장하는 것입니다.

### 주요 대상
- 시각 장애인
- 운동 장애인
- 키보드 선호 사용자
- 파워 유저

## 포커스 관리

### 1. 포커스 표시
```css
/* 기본 포커스 스타일 */
:focus {
  outline: 2px solid #4A90E2;
  outline-offset: 2px;
}

/* 포커스-visible 사용 */
:focus:not(:focus-visible) {
  outline: none;
}

:focus-visible {
  outline: 2px solid #4A90E2;
  outline-offset: 2px;
  box-shadow: 0 0 0 4px rgba(74, 144, 226, 0.25);
}
```

### 2. 포커스 순서
```html
<!-- 논리적인 포커스 순서 -->
<header>
  <nav>
    <a href="#main" class="skip-link">본문 바로가기</a>
    <a href="/">홈</a>
    <a href="/about">소개</a>
  </nav>
</header>

<main id="main" tabindex="-1">
  <h1>페이지 제목</h1>
  <!-- 본문 내용 -->
</main>
```

### 3. 포커스 제어
```javascript
// 모달 다이얼로그 포커스 관리
class Modal {
  constructor(element) {
    this.element = element;
    this.focusableElements = element.querySelectorAll(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    this.firstFocusable = this.focusableElements[0];
    this.lastFocusable = this.focusableElements[this.focusableElements.length - 1];
  }

  trapFocus(e) {
    if (e.key === 'Tab') {
      if (e.shiftKey) {
        if (document.activeElement === this.firstFocusable) {
          e.preventDefault();
          this.lastFocusable.focus();
        }
      } else {
        if (document.activeElement === this.lastFocusable) {
          e.preventDefault();
          this.firstFocusable.focus();
        }
      }
    }
  }
}
```

## 키보드 인터랙션

### 1. 기본 키보드 동작
- Tab: 포커스 가능한 요소로 이동
- Shift + Tab: 역방향 이동
- Enter/Space: 선택/활성화
- 화살표 키: 메뉴, 리스트 내 이동

### 2. 커스텀 키보드 인터랙션
```javascript
// 커스텀 드롭다운 메뉴
class DropdownMenu {
  constructor(element) {
    this.element = element;
    this.button = element.querySelector('button');
    this.menu = element.querySelector('ul');
    this.menuItems = element.querySelectorAll('li');
    
    this.button.addEventListener('keydown', this.handleButtonKeydown.bind(this));
    this.menu.addEventListener('keydown', this.handleMenuKeydown.bind(this));
  }

  handleButtonKeydown(e) {
    switch (e.key) {
      case 'ArrowDown':
        e.preventDefault();
        this.openMenu();
        this.menuItems[0].focus();
        break;
      case 'ArrowUp':
        e.preventDefault();
        this.openMenu();
        this.menuItems[this.menuItems.length - 1].focus();
        break;
    }
  }

  handleMenuKeydown(e) {
    switch (e.key) {
      case 'ArrowDown':
        e.preventDefault();
        this.focusNextItem();
        break;
      case 'ArrowUp':
        e.preventDefault();
        this.focusPreviousItem();
        break;
      case 'Escape':
        this.closeMenu();
        this.button.focus();
        break;
    }
  }
}
```

## 키보드 트랩 방지

### 1. 모달 다이얼로그
```html
<div class="modal" role="dialog" aria-labelledby="modal-title">
  <h2 id="modal-title">모달 제목</h2>
  <div class="modal-content">
    <!-- 모달 내용 -->
  </div>
  <button class="close-button">닫기</button>
</div>

<script>
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') {
    closeModal();
  }
});
</script>
```

### 2. 접근 가능한 오버레이
```javascript
class AccessibleOverlay {
  constructor() {
    this.previousFocus = null;
    this.overlay = document.querySelector('.overlay');
  }

  show() {
    this.previousFocus = document.activeElement;
    this.overlay.hidden = false;
    // 첫 번째 포커스 가능한 요소로 포커스 이동
    const firstFocusable = this.overlay.querySelector('button, [href], input');
    if (firstFocusable) {
      firstFocusable.focus();
    }
  }

  hide() {
    this.overlay.hidden = true;
    // 이전 포커스 위치로 복귀
    if (this.previousFocus) {
      this.previousFocus.focus();
    }
  }
}
```

## 구현 가이드

### 1. 클릭 가능한 요소
```html
<!-- 버튼 사용 -->
<button type="button" class="card-button">
  <img src="icon.png" alt="">
  <span>카드 상세 보기</span>
</button>

<!-- div를 버튼처럼 사용할 경우 -->
<div role="button"
     tabindex="0"
     onclick="handleClick()"
     onkeydown="handleKeydown(event)"
     class="custom-button">
  커스텀 버튼
</div>
```

### 2. 복잡한 위젯
```javascript
// 탭 패널 구현
class TabPanel {
  constructor(element) {
    this.element = element;
    this.tabs = element.querySelectorAll('[role="tab"]');
    this.panels = element.querySelectorAll('[role="tabpanel"]');
    
    this.init();
  }

  init() {
    // 키보드 이벤트 처리
    this.tabs.forEach(tab => {
      tab.addEventListener('keydown', e => {
        let targetTab = null;
        
        switch (e.key) {
          case 'ArrowRight':
            targetTab = this.getNextTab();
            break;
          case 'ArrowLeft':
            targetTab = this.getPreviousTab();
            break;
          case 'Home':
            targetTab = this.tabs[0];
            break;
          case 'End':
            targetTab = this.tabs[this.tabs.length - 1];
            break;
        }
        
        if (targetTab) {
          e.preventDefault();
          this.switchTab(targetTab);
        }
      });
    });
  }
}
```

## 체크리스트

### 기본 접근성
- [ ] 모든 기능을 키보드로 사용 가능
- [ ] 포커스 표시가 시각적으로 명확함
- [ ] 논리적인 포커스 이동 순서
- [ ] 키보드 트랩이 없음

### 고급 기능
- [ ] 건너뛰기 링크 제공
- [ ] 복잡한 위젯의 키보드 인터랙션 구현
- [ ] 모달/오버레이의 적절한 포커스 관리
- [ ] 커스텀 컴포넌트의 키보드 지원

### 테스트
- [ ] 키보드로만 모든 기능 테스트
- [ ] 스크린리더 사용 테스트
- [ ] 다양한 브라우저에서 테스트
- [ ] 포커스 순서 검증

## 참고 자료
- [WAI-ARIA 키보드 인터랙션](https://www.w3.org/WAI/ARIA/apg/practices/keyboard-interface/)
- [MDN 키보드 접근성](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Keyboard-navigable_JavaScript_widgets)
- [WebAIM 키보드 접근성](https://webaim.org/techniques/keyboard/)