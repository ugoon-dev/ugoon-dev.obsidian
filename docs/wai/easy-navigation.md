# 쉬운 내비게이션 가이드라인

## 목차
1. [쉬운 내비게이션의 중요성](#쉬운-내비게이션의-중요성)
2. [페이지 구조화](#페이지-구조화)
3. [내비게이션 컴포넌트](#내비게이션-컴포넌트)
4. [위치 정보 제공](#위치-정보-제공)
5. [구현 가이드](#구현-가이드)

## 쉬운 내비게이션의 중요성

쉬운 내비게이션은 모든 사용자가 웹사이트의 구조를 이해하고 원하는 콘텐츠에 쉽게 접근할 수 있도록 돕습니다.

### 주요 고려사항
- 일관된 구조
- 명확한 레이블
- 다양한 접근 방법
- 현재 위치 표시

## 페이지 구조화

### 1. 건너뛰기 링크
```html
<!-- 건너뛰기 링크 -->
<div class="skip-links">
  <a href="#main-content" class="skip-link">
    본문 바로가기
  </a>
  <a href="#main-nav" class="skip-link">
    메인 메뉴 바로가기
  </a>
</div>

<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
</style>
```

### 2. 랜드마크 구조
```html
<body>
  <header role="banner">
    <nav role="navigation" aria-label="메인 메뉴">
      <!-- 메인 내비게이션 -->
    </nav>
  </header>

  <main id="main-content" role="main">
    <nav aria-label="사이드 메뉴">
      <!-- 보조 내비게이션 -->
    </nav>
    
    <article>
      <!-- 주요 콘텐츠 -->
    </article>
    
    <aside role="complementary">
      <!-- 부가 정보 -->
    </aside>
  </main>

  <footer role="contentinfo">
    <!-- 푸터 내용 -->
  </footer>
</body>
```

## 내비게이션 컴포넌트

### 1. 메인 메뉴
```html
<nav class="main-nav" role="navigation" aria-label="메인 메뉴">
  <button class="menu-toggle" aria-expanded="false" aria-controls="main-menu">
    메뉴
  </button>
  
  <ul id="main-menu" class="menu-list">
    <li class="menu-item">
      <a href="/" class="menu-link">홈</a>
    </li>
    <li class="menu-item has-submenu">
      <a href="/products" class="menu-link">
        제품
        <span class="submenu-indicator" aria-hidden="true">▼</span>
      </a>
      <ul class="submenu">
        <li><a href="/products/new">신제품</a></li>
        <li><a href="/products/best">인기제품</a></li>
      </ul>
    </li>
  </ul>
</nav>

<script>
class MainNavigation {
  constructor(element) {
    this.nav = element;
    this.toggle = element.querySelector('.menu-toggle');
    this.menu = element.querySelector('.menu-list');
    this.menuItems = element.querySelectorAll('.menu-item');
    
    this.init();
  }

  init() {
    this.setupToggle();
    this.setupKeyboardNav();
    this.setupSubmenuHandling();
  }

  setupToggle() {
    this.toggle.addEventListener('click', () => {
      const isExpanded = this.toggle.getAttribute('aria-expanded') === 'true';
      this.toggle.setAttribute('aria-expanded', !isExpanded);
      this.menu.classList.toggle('is-open');
    });
  }

  setupKeyboardNav() {
    this.menuItems.forEach(item => {
      item.addEventListener('keydown', e => {
        switch(e.key) {
          case 'ArrowRight':
            this.focusNextItem(item);
            break;
          case 'ArrowLeft':
            this.focusPreviousItem(item);
            break;
          case 'ArrowDown':
            if (item.classList.contains('has-submenu')) {
              e.preventDefault();
              this.openSubmenu(item);
            }
            break;
        }
      });
    });
  }
}
</script>
```

### 2. 브레드크럼
```html
<nav aria-label="현재 위치" class="breadcrumb">
  <ol>
    <li>
      <a href="/">홈</a>
      <span aria-hidden="true">/</span>
    </li>
    <li>
      <a href="/products">제품</a>
      <span aria-hidden="true">/</span>
    </li>
    <li aria-current="page">제품 상세</li>
  </ol>
</nav>

<style>
.breadcrumb ol {
  display: flex;
  list-style: none;
  padding: 0;
}

.breadcrumb li + li::before {
  content: "/";
  padding: 0 8px;
}
</style>
```

## 위치 정보 제공

### 1. 현재 페이지 표시
```html
<nav class="site-nav">
  <ul>
    <li>
      <a href="/" class="nav-link" 
         aria-current="page">홈</a>
    </li>
    <li>
      <a href="/about" class="nav-link">소개</a>
    </li>
  </ul>
</nav>

<style>
.nav-link[aria-current="page"] {
  font-weight: bold;
  border-bottom: 2px solid currentColor;
}
</style>
```

### 2. 진행 단계 표시
```html
<div class="progress-tracker" role="navigation" 
     aria-label="진행 단계">
  <ol class="steps">
    <li class="step completed">
      <span class="step-label">
        1. 장바구니
        <span class="visually-hidden">(완료)</span>
      </span>
    </li>
    <li class="step current" aria-current="step">
      <span class="step-label">2. 배송정보</span>
    </li>
    <li class="step">
      <span class="step-label">3. 결제</span>
    </li>
  </ol>
</div>

<style>
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}

.steps {
  display: flex;
  list-style: none;
  padding: 0;
}

.step {
  flex: 1;
  text-align: center;
  position: relative;
}

.step::after {
  content: "";
  position: absolute;
  top: 50%;
  right: 0;
  width: 100%;
  height: 2px;
  background: #ccc;
}

.step.completed::after {
  background: #4CAF50;
}
</style>
```

## 구현 가이드

### 1. 검색 기능
```html
<div class="search-container">
  <form role="search" class="search-form">
    <label for="search" class="visually-hidden">검색</label>
    <input type="search" id="search" 
           placeholder="검색어를 입력하세요"
           aria-label="사이트 검색">
    <button type="submit" aria-label="검색">
      <span class="search-icon" aria-hidden="true">🔍</span>
    </button>
  </form>
  
  <div id="search-results" role="region" 
       aria-live="polite" aria-label="검색 결과">
    <!-- 검색 결과 표시 영역 -->
  </div>
</div>
```

### 2. 사이트맵
```html
<nav class="sitemap" role="navigation" 
     aria-label="사이트맵">
  <h2>사이트맵</h2>
  
  <div class="sitemap-section">
    <h3>제품</h3>
    <ul>
      <li><a href="/products/new">신제품</a></li>
      <li><a href="/products/best">인기제품</a></li>
      <li><a href="/products/sale">할인제품</a></li>
    </ul>
  </div>
  
  <div class="sitemap-section">
    <h3>고객지원</h3>
    <ul>
      <li><a href="/support/faq">자주 묻는 질문</a></li>
      <li><a href="/support/contact">문의하기</a></li>
    </ul>
  </div>
</nav>
```

## 체크리스트

### 필수 항목
- [ ] 건너뛰기 링크 제공
- [ ] 명확한 내비게이션 구조
- [ ] 현재 위치 표시
- [ ] 키보드 접근성 보장

### 권장 항목
- [ ] 검색 기능 제공
- [ ] 사이트맵 제공
- [ ] 다양한 내비게이션 방법
- [ ] 반응형 내비게이션 구현

### 테스트
- [ ] 키보드 내비게이션 테스트
- [ ] 스크린리더 사용성 테스트
- [ ] 반응형 동작 확인
- [ ] 내비게이션 일관성 검증

## 참고 자료
- [WAI 내비게이션 가이드라인](https://www.w3.org/WAI/tutorials/menus/)
- [ARIA 내비게이션 패턴](https://www.w3.org/WAI/ARIA/apg/patterns/navigation/)
- [모바일 내비게이션 접근성](https://www.w3.org/WAI/mobile/)