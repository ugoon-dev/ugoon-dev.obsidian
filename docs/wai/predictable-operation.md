# 예측 가능한 작동 가이드라인

## 목차
1. [예측 가능한 작동의 중요성](#예측-가능한-작동의-중요성)
2. [일관된 내비게이션](#일관된-내비게이션)
3. [사용자 제어](#사용자-제어)
4. [상태 변화 알림](#상태-변화-알림)
5. [구현 가이드](#구현-가이드)

## 예측 가능한 작동의 중요성

예측 가능한 작동은 사용자가 웹사이트의 기능과 동작을 쉽게 이해하고 예상할 수 있도록 합니다.

### 주요 원칙
- 일관된 레이아웃
- 명확한 피드백
- 사용자 통제권
- 예상 가능한 동작

## 일관된 내비게이션

### 1. 내비게이션 구조
```html
<!-- 일관된 내비게이션 구조 -->
<header class="site-header">
  <nav class="main-nav">
    <ul>
      <li><a href="/" class="nav-item">홈</a></li>
      <li>
        <button class="nav-item has-submenu" 
                aria-expanded="false"
                aria-controls="products-menu">
          제품
        </button>
        <ul id="products-menu" class="submenu" hidden>
          <li><a href="/products/new">신제품</a></li>
          <li><a href="/products/best">인기제품</a></li>
        </ul>
      </li>
    </ul>
  </nav>
</header>

<script>
class ConsistentNavigation {
  constructor(element) {
    this.nav = element;
    this.submenus = this.nav.querySelectorAll('.has-submenu');
    
    this.init();
  }

  init() {
    this.submenus.forEach(submenu => {
      submenu.addEventListener('click', (e) => {
        this.toggleSubmenu(submenu);
      });
      
      // 키보드 접근성
      submenu.addEventListener('keydown', (e) => {
        if (e.key === 'Enter' || e.key === ' ') {
          e.preventDefault();
          this.toggleSubmenu(submenu);
        }
      });
    });
  }

  toggleSubmenu(element) {
    const isExpanded = element.getAttribute('aria-expanded') === 'true';
    const targetId = element.getAttribute('aria-controls');
    const target = document.getElementById(targetId);
    
    element.setAttribute('aria-expanded', !isExpanded);
    target.hidden = isExpanded;
  }
}
</script>
```

### 2. 레이아웃 일관성
```css
/* 일관된 레이아웃 스타일 */
.site-layout {
  --header-height: 60px;
  --footer-height: 100px;
  --sidebar-width: 250px;
  
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: var(--sidebar-width) 1fr;
  grid-template-rows: var(--header-height) 1fr var(--footer-height);
  min-height: 100vh;
}

.site-header { grid-area: header; }
.site-sidebar { grid-area: sidebar; }
.site-main { grid-area: main; }
.site-footer { grid-area: footer; }
```

## 사용자 제어

### 1. 포커스 관리
```javascript
class FocusManager {
  constructor() {
    this.focusableElements = 'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])';
  }

  trapFocus(element) {
    const focusable = element.querySelectorAll(this.focusableElements);
    const firstFocusable = focusable[0];
    const lastFocusable = focusable[focusable.length - 1];

    element.addEventListener('keydown', e => {
      if (e.key === 'Tab') {
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
      }
    });
  }
}
```

### 2. 상태 변경 제어
```html
<div class="user-preferences">
  <h2>사용자 설정</h2>
  
  <form class="preferences-form">
    <fieldset>
      <legend>자동 재생 설정</legend>
      
      <label>
        <input type="checkbox" name="autoplay" 
               class="preference-control"
               data-preference="autoplay">
        비디오 자동 재생
      </label>
    </fieldset>
    
    <fieldset>
      <legend>알림 설정</legend>
      
      <label>
        <input type="checkbox" name="notifications"
               class="preference-control"
               data-preference="notifications">
        푸시 알림 허용
      </label>
    </fieldset>
  </form>
</div>

<script>
class PreferencesManager {
  constructor() {
    this.preferences = new Map();
    this.controls = document.querySelectorAll('.preference-control');
    
    this.init();
  }

  init() {
    // 저장된 설정 불러오기
    this.loadPreferences();
    
    // 이벤트 리스너 설정
    this.controls.forEach(control => {
      control.addEventListener('change', (e) => {
        this.updatePreference(
          control.dataset.preference,
          control.checked
        );
      });
    });
  }

  updatePreference(key, value) {
    this.preferences.set(key, value);
    localStorage.setItem('userPreferences', 
      JSON.stringify(Object.fromEntries(this.preferences))
    );
  }
}
</script>
```

## 상태 변화 알림

### 1. 실시간 피드백
```html
<div class="feedback-system" role="status" aria-live="polite">
  <div class="feedback-message" hidden>
    <!-- 피드백 메시지가 여기에 표시됨 -->
  </div>
</div>

<script>
class FeedbackSystem {
  constructor() {
    this.container = document.querySelector('.feedback-message');
    this.timeoutId = null;
  }

  showMessage(message, type = 'info', duration = 3000) {
    // 이전 메시지 제거
    clearTimeout(this.timeoutId);
    
    // 새 메시지 표시
    this.container.textContent = message;
    this.container.className = `feedback-message ${type}`;
    this.container.hidden = false;
    
    // 자동 제거 타이머 설정
    this.timeoutId = setTimeout(() => {
      this.container.hidden = true;
    }, duration);
  }
}
</script>

<style>
.feedback-message {
  padding: 1em;
  border-radius: 4px;
  margin: 1em 0;
}

.feedback-message.info {
  background-color: #e3f2fd;
  color: #1565c0;
}

.feedback-message.success {
  background-color: #e8f5e9;
  color: #2e7d32;
}

.feedback-message.error {
  background-color: #ffebee;
  color: #c62828;
}
</style>
```

### 2. 상태 변화 통지
```javascript
class StateChangeNotifier {
  constructor() {
    this.observers = new Set();
  }

  subscribe(callback) {
    this.observers.add(callback);
  }

  unsubscribe(callback) {
    this.observers.delete(callback);
  }

  notify(state) {
    this.observers.forEach(callback => callback(state));
  }
}

// 사용 예시
const notifier = new StateChangeNotifier();

// 상태 변화 구독
notifier.subscribe((state) => {
  const announcement = document.createElement('div');
  announcement.setAttribute('role', 'alert');
  announcement.textContent = `상태가 ${state}로 변경되었습니다.`;
  document.body.appendChild(announcement);
  
  // 잠시 후 알림 제거
  setTimeout(() => announcement.remove(), 3000);
});
```

## 구현 체크리스트

### 기본 요구사항
- [ ] 일관된 내비게이션 구조
- [ ] 예측 가능한 포커스 이동
- [ ] 명확한 상태 변화 알림
- [ ] 사용자 설정 유지

### 고급 기능
- [ ] 상태 변화 히스토리 관리
- [ ] 실행 취소/다시 실행 지원
- [ ] 사용자 설정 백업/복원
- [ ] 다중 기기 동기화

### 테스트
- [ ] 내비게이션 일관성 검증
- [ ] 키보드 접근성 테스트
- [ ] 스크린리더 호환성 확인
- [ ] 사용자 설정 지속성 확인

## 참고 자료
- [WCAG 2.1 예측 가능성](https://www.w3.org/WAI/WCAG21/Understanding/consistent-behavior)
- [WAI-ARIA 상태 및 속��](https://www.w3.org/WAI/ARIA/apg/patterns/)
- [사용자 인터페이스 디자인 패턴](https://www.w3.org/WAI/ARIA/apg/patterns/)