# ARIA 역할과 속성 가이드

## 목차
1. [ARIA 개요](#aria-개요)
2. [랜드마크 역할](#랜드마크-역할)
3. [위젯 역할](#위젯-역할)
4. [구조적 역할](#구조적-역할)
5. [라이브 리전](#라이브-리전)
6. [상태 및 속성](#상태-및-속성)
7. [컴포넌트별 ARIA 사용 예시](#컴포넌트별-aria-사용-예시)

## ARIA 개요

ARIA(Accessible Rich Internet Applications)는 웹 콘텐츠와 웹 애플리케이션을 더 접근성 있게 만들기 위한 방법을 정의하는 규격입니다.

### ARIA의 세 가지 주요 기능
1. 역할(Roles): 요소의 기능을 정의
2. 속성(Properties): 요소의 특성을 정의
3. 상태(States): 요소의 현재 상태를 정의

## 랜드마크 역할

웹 페이지의 주요 영역을 식별하는 데 사용되는 역할입니다.

### 주요 랜드마크 역할
```html
<!-- 헤더 영역 -->
<header role="banner">
    <!-- 사이트 로고, 메인 헤더 등 -->
</header>

<!-- 네비게이션 -->
<nav role="navigation" aria-label="메인 메뉴">
    <!-- 메뉴 항목들 -->
</nav>

<!-- 메인 콘텐츠 -->
<main role="main">
    <!-- 주요 콘텐츠 -->
</main>

<!-- 보조 콘텐츠 -->
<aside role="complementary" aria-label="관련 정보">
    <!-- 부가 정보 -->
</aside>

<!-- 검색 -->
<form role="search">
    <!-- 검색 폼 -->
</form>

<!-- 푸터 -->
<footer role="contentinfo">
    <!-- 저작권 정보, 연락처 등 -->
</footer>
```

## 위젯 역할

사용자 인터페이스 컴포넌트를 정의하는 역할입니다.

### 1. 버튼 및 링크
```html
<!-- 버튼 -->
<div role="button" 
     tabindex="0"
     aria-pressed="false"
     onclick="toggleButton(this)">
    토글 버튼
</div>

<!-- 링크 -->
<span role="link" 
      tabindex="0"
      onclick="navigate(this)">
    커스텀 링크
</span>
```

### 2. 폼 컨트롤
```html
<!-- 콤보박스 -->
<div role="combobox"
     aria-expanded="false"
     aria-controls="listbox1"
     aria-haspopup="listbox">
    <input type="text" 
           aria-autocomplete="list">
    <div role="listbox" id="listbox1" hidden>
        <div role="option" aria-selected="false">옵션 1</div>
        <div role="option" aria-selected="false">옵션 2</div>
    </div>
</div>

<!-- 슬라이더 -->
<div role="slider"
     aria-valuemin="0"
     aria-valuemax="100"
     aria-valuenow="50"
     aria-valuetext="50%"
     tabindex="0">
    <div class="slider-thumb"></div>
</div>
```

### 3. 복합 위젯
```html
<!-- 아코디언 -->
<div role="accordion">
    <h3>
        <button role="button"
                aria-expanded="false"
                aria-controls="panel1">
            섹션 1
        </button>
    </h3>
    <div role="region"
         id="panel1"
         aria-labelledby="header1"
         hidden>
        <!-- 패널 내용 -->
    </div>
</div>

<!-- 탭 패널 -->
<div class="tabs">
    <div role="tablist" aria-label="프로그래밍 언어">
        <button role="tab"
                aria-selected="true"
                aria-controls="panel1"
                id="tab1">
            JavaScript
        </button>
        <button role="tab"
                aria-selected="false"
                aria-controls="panel2"
                id="tab2">
            Python
        </button>
    </div>
    
    <div role="tabpanel"
         id="panel1"
         aria-labelledby="tab1">
        <!-- JavaScript 관련 내용 -->
    </div>
    <div role="tabpanel"
         id="panel2"
         aria-labelledby="tab2"
         hidden>
        <!-- Python 관련 내용 -->
    </div>
</div>
```

## 구조적 역할

문서의 구조를 설명하는 역할입니다.

```html
<!-- 문서 구조 -->
<div role="article">
    <h1>기사 제목</h1>
    <div role="toolbar" aria-label="문서 도구">
        <button>공유</button>
        <button>인쇄</button>
    </div>
    <div role="main">
        <!-- 주요 내용 -->
    </div>
    <div role="contentinfo">
        <!-- 메타 정보 -->
    </div>
</div>

<!-- 목록 구조 -->
<div role="list">
    <div role="listitem">항목 1</div>
    <div role="listitem">항목 2</div>
</div>

<!-- 표 구조 -->
<div role="table">
    <div role="rowgroup">
        <div role="row">
            <span role="columnheader">이름</span>
            <span role="columnheader">나이</span>
        </div>
    </div>
    <div role="rowgroup">
        <div role="row">
            <span role="cell">홍길동</span>
            <span role="cell">30</span>
        </div>
    </div>
</div>
```

## 라이브 리전

동적으로 업데이트되는 콘텐츠를 위한 역할입니다.

```html
<!-- 알림 메시지 -->
<div role="alert" aria-live="assertive">
    <!-- 중요한 알림 -->
</div>

<!-- 상태 메시지 -->
<div role="status" aria-live="polite">
    <!-- 상태 업데이트 -->
</div>

<!-- 진행 상태 -->
<div role="progressbar"
     aria-valuenow="75"
     aria-valuemin="0"
     aria-valuemax="100">
    75% 완료
</div>

<!-- 로그 -->
<div role="log" aria-live="polite">
    <!-- 로그 메시지 -->
</div>
```

## 상태 및 속성

### 1. 상태 속성
```html
<!-- 확장/축소 상태 -->
<button aria-expanded="false">
    메뉴 펼치기
</button>

<!-- 선택 상태 -->
<div role="option" aria-selected="true">
    선택된 옵션
</div>

<!-- 비활성화 상태 -->
<button aria-disabled="true">
    비활성화된 버튼
</button>

<!-- 필수 입력 상태 -->
<input aria-required="true">

<!-- 오류 상태 -->
<input aria-invalid="true"
       aria-errormessage="error-msg">
<div id="error-msg">
    올바른 이메일을 입력하세요
</div>
```

### 2. 관계 속성
```html
<!-- 레이블 관계 -->
<div id="label">사용자 이름</div>
<input aria-labelledby="label">

<!-- 설명 관계 -->
<button aria-describedby="description">
    저장
</button>
<div id="description">
    변경사항을 저장합니다
</div>

<!-- 컨트롤 관계 -->
<button aria-controls="panel">
    패널 열기
</button>
<div id="panel" hidden>
    <!-- 패널 내용 -->
</div>
```

## 컴포넌트별 ARIA 사용 예시

### 1. 모달 다이얼로그
```html
<div role="dialog"
     aria-labelledby="dialog-title"
     aria-describedby="dialog-desc"
     aria-modal="true">
    <h2 id="dialog-title">확인</h2>
    <p id="dialog-desc">
        변경사항을 저장하시겠습니까?
    </p>
    <div class="dialog-buttons">
        <button>확인</button>
        <button>취소</button>
    </div>
</div>

<script>
class AccessibleDialog {
    constructor(dialog) {
        this.dialog = dialog;
        this.previousFocus = null;
        this.focusableElements = dialog.querySelectorAll(
            'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
        );
    }

    show() {
        this.previousFocus = document.activeElement;
        this.dialog.hidden = false;
        this.trapFocus();
        this.focusableElements[0].focus();
    }

    hide() {
        this.dialog.hidden = true;
        this.previousFocus?.focus();
    }

    trapFocus() {
        const firstFocusable = this.focusableElements[0];
        const lastFocusable = this.focusableElements[
            this.focusableElements.length - 1
        ];

        this.dialog.addEventListener('keydown', e => {
            if (e.key !== 'Tab') return;

            if (e.shiftKey) {
                if (document.activeElement === firstFocusable) {
                    e.preventDefault();
                    lastFocusable.focus();
                }
            } else {
                if (document.activeElement === lastFocusable) {
                    e.preventDefault();
                    firstFocusable.focus();
                }
            }
        });
    }
}
</script>
```

### 2. 자동완성 검색
```html
<div class="search-container">
    <label id="search-label">검색</label>
    <input type="text"
           role="combobox"
           aria-expanded="false"
           aria-autocomplete="list"
           aria-controls="search-listbox"
           aria-labelledby="search-label">
    
    <ul id="search-listbox"
        role="listbox"
        aria-label="검색 결과"
        hidden>
        <li role="option" 
            aria-selected="false"
            id="option1">
            검색결과 1
        </li>
        <li role="option"
            aria-selected="false"
            id="option2">
            검색결과 2
        </li>
    </ul>
</div>

<script>
class AccessibleAutocomplete {
    constructor(container) {
        this.input = container.querySelector('[role="combobox"]');
        this.listbox = container.querySelector('[role="listbox"]');
        this.options = this.listbox.querySelectorAll('[role="option"]');
        
        this.setupEventListeners();
    }

    setupEventListeners() {
        this.input.addEventListener('input', () => {
            this.showResults();
        });

        this.input.addEventListener('keydown', e => {
            switch(e.key) {
                case 'ArrowDown':
                    e.preventDefault();
                    this.navigateOptions(1);
                    break;
                case 'ArrowUp':
                    e.preventDefault();
                    this.navigateOptions(-1);
                    break;
                case 'Enter':
                    this.selectOption();
                    break;
                case 'Escape':
                    this.hideResults();
                    break;
            }
        });
    }

    showResults() {
        this.listbox.hidden = false;
        this.input.setAttribute('aria-expanded', 'true');
    }

    hideResults() {
        this.listbox.hidden = true;
        this.input.setAttribute('aria-expanded', 'false');
    }

    navigateOptions(direction) {
        const current = this.listbox.querySelector('[aria-selected="true"]');
        let next;

        if (!current) {
            next = direction > 0 ? 
                this.options[0] : 
                this.options[this.options.length - 1];
        } else {
            const currentIndex = Array.from(this.options)
                .indexOf(current);
            const nextIndex = currentIndex + direction;

            if (nextIndex >= 0 && nextIndex < this.options.length) {
                next = this.options[nextIndex];
            }
        }

        if (next) {
            current?.setAttribute('aria-selected', 'false');
            next.setAttribute('aria-selected', 'true');
            next.scrollIntoView({ block: 'nearest' });
        }
    }

    selectOption() {
        const selected = this.listbox
            .querySelector('[aria-selected="true"]');
        if (selected) {
            this.input.value = selected.textContent.trim();
            this.hideResults();
        }
    }
}
</script>
```

### 3. 알림 토스트
```html
<div class="toast-container">
    <div role="alert"
         aria-live="polite"
         class="toast"
         hidden>
        <div class="toast-content">
            <!-- 토스트 메시지 -->
        </div>
        <button aria-label="닫기">×</button>
    </div>
</div>

<script>
class AccessibleToast {
    constructor(container) {
        this.container = container;
        this.queue = [];
        this.isShowing = false;
    }

    show(message, type = 'info', duration = 3000) {
        const toast = this.createToast(message, type);
        this.queue.push({ toast, duration });
        
        if (!this.isShowing) {
            this.showNext();
        }
    }

    createToast(message, type) {
        const toast = document.createElement('div');
        toast.setAttribute('role', 'alert');
        toast.setAttribute('aria-live', 'polite');
        toast.className = `toast toast-${type}`;
        
        toast.innerHTML = `
            <div class="toast-content">${message}</div>
            <button aria-label="닫기">×</button>
        `;

        toast.querySelector('button').addEventListener('click', () => {
            this.hide(toast);
        });

        return toast;
    }

    async showNext() {
        if (this.queue.length === 0) {
            this.isShowing = false;
            return;
        }

        this.isShowing = true;
        const { toast, duration } = this.queue.shift();
        
        this.container.appendChild(toast);
        toast.hidden = false;

        await new Promise(resolve => setTimeout(resolve, duration));
        await this.hide(toast);
        this.showNext();
    }

    async hide(toast) {
        toast.hidden = true;
        toast.remove();
    }
}
</script>

<style>
.toast {
    position: fixed;
    bottom: 20px;
    right: 20px;
    padding: 1rem;
    background: white;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    border-radius: 4px;
    max-width: 300px;
}

.toast-info {
    border-left: 4px solid #2196F3;
}

.toast-success {
    border-left: 4px solid #4CAF50;
}

.toast-error {
    border-left: 4px solid #F44336;
}
</style>
```

### 4. 메뉴 버튼
```html
<div class="menu-container">
    <button aria-haspopup="true"
            aria-expanded="false"
            aria-controls="menu1">
        메뉴
    </button>
    
    <ul id="menu1"
        role="menu"
        hidden>
        <li role="menuitem" tabindex="-1">
            메뉴 항목 1
        </li>
        <li role="menuitem" tabindex="-1">
            메뉴 항목 2
        </li>
        <li role="separator"></li>
        <li role="menuitem" tabindex="-1">
            메뉴 항목 3
        </li>
    </ul>
</div>

<script>
class AccessibleMenu {
    constructor(container) {
        this.button = container.querySelector('button');
        this.menu = container.querySelector('[role="menu"]');
        this.menuItems = this.menu.querySelectorAll('[role="menuitem"]');
        
        this.setupEventListeners();
    }

    setupEventListeners() {
        this.button.addEventListener('click', () => {
            this.toggleMenu();
        });

        this.button.addEventListener('keydown', e => {
            if (e.key === 'ArrowDown' || e.key === 'Enter') {
                e.preventDefault();
                this.openMenu();
                this.focusFirstItem();
            }
        });

        this.menu.addEventListener('keydown', e => {
            switch(e.key) {
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
                case 'Tab':
                    this.closeMenu();
                    break;
            }
        });

        document.addEventListener('click', e => {
            if (!container.contains(e.target)) {
                this.closeMenu();
            }
        });
    }

    toggleMenu() {
        if (this.menu.hidden) {
            this.openMenu();
        } else {
            this.closeMenu();
        }
    }

    openMenu() {
        this.menu.hidden = false;
        this.button.setAttribute('aria-expanded', 'true');
        this.focusFirstItem();
    }

    closeMenu() {
        this.menu.hidden = true;
        this.button.setAttribute('aria-expanded', 'false');
    }

    focusFirstItem() {
        this.menuItems[0]?.focus();
    }

    focusNextItem() {
        const current = document.activeElement;
        const currentIndex = Array.from(this.menuItems)
            .indexOf(current);
        const next = this.menuItems[currentIndex + 1];
        
        if (next) {
            next.focus();
        } else {
            this.menuItems[0].focus();
        }
    }

    focusPreviousItem() {
        const current = document.activeElement;
        const currentIndex = Array.from(this.menuItems)
            .indexOf(current);
        const previous = this.menuItems[currentIndex - 1];
        
        if (previous) {
            previous.focus();
        } else {
            this.menuItems[this.menuItems.length - 1].focus();
        }
    }
}
</script>
```

## 구현 체크리스트

### 필수 항목
- [ ] 모든 대화형 요소에 적절한 역할 부여
- [ ] 상태 변경 시 ARIA 속성 업데이트
- [ ] 키보드 접근성 보장
- [ ] 적절한 포커스 관리
- [ ] 오류 및 상태 메시지 제공

### 권장 항목
- [ ] 컨텍스트에 맞는 ARIA 레이블 사용
- [ ] 동적 콘텐츠 업데이트 알림
- [ ] 복잡한 위젯의 키보드 인터랙션 구현
- [ ] 상태 변경 시 적절한 피드백 제공
- [ ] 스크린 리더 사용자를 위한 추가 설명 제공

## 참고 자료
- [WAI-ARIA 1.2 명세](https://www.w3.org/TR/wai-aria-1.2/)
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [MDN ARIA 문서](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)
- [ARIA 디자인 패턴](https://www.w3.org/TR/wai-aria-practices-1.2/)