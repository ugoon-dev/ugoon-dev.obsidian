# 오류 방지 및 정정 가이드라인

## 목차
1. [오류 방지의 중요성](#오류-방지의-중요성)
2. [입력 유효성 검사](#입력-유효성-검사)
3. [오류 메시지](#오류-메시지)
4. [오류 복구](#오류-복구)
5. [구현 가이드](#구현-가이드)

## 오류 방지의 중요성

오류 방지 및 정정은 사용자가 실수를 방지하고, 실수가 발생했을 때 쉽게 복구할 수 있도록 돕습니다.

### 주요 원칙
- 오류 발생 예방
- 명확한 오류 안내
- 쉬운 오류 수정
- 데이터 보호

## 입력 유효성 검사

### 1. 실시간 유효성 검사
```html
<form class="validated-form" novalidate>
  <div class="form-group">
    <label for="email">이메일</label>
    <input type="email" id="email" 
           required
           pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$"
           aria-describedby="email-hint email-error">
    
    <span id="email-hint" class="hint">
      예: example@domain.com
    </span>
    
    <span id="email-error" class="error" role="alert" hidden>
      올바른 이메일 주소를 입력해주세요
    </span>
  </div>
  
  <div class="form-group">
    <label for="password">비밀번호</label>
    <input type="password" id="password"
           required
           minlength="8"
           aria-describedby="password-hint password-error">
    
    <span id="password-hint" class="hint">
      8자 이상, 영문, 숫자, 특수문자 포함
    </span>
    
    <span id="password-error" class="error" role="alert" hidden>
      비밀번호 형식이 올바르지 않습니다
    </span>
  </div>
</form>

<script>
class FormValidator {
  constructor(form) {
    this.form = form;
    this.inputs = form.querySelectorAll('input');
    
    this.init();
  }

  init() {
    this.inputs.forEach(input => {
      input.addEventListener('input', () => {
        this.validateInput(input);
      });
      
      input.addEventListener('blur', () => {
        this.validateInput(input);
      });
    });
    
    this.form.addEventListener('submit', (e) => {
      if (!this.validateForm()) {
        e.preventDefault();
      }
    });
  }

  validateInput(input) {
    const errorElement = document.getElementById(
      input.getAttribute('aria-describedby').split(' ')[1]
    );
    
    if (!input.checkValidity()) {
      input.setAttribute('aria-invalid', 'true');
      errorElement.hidden = false;
      this.showError(input, errorElement);
    } else {
      input.setAttribute('aria-invalid', 'false');
      errorElement.hidden = true;
    }
  }

  showError(input, errorElement) {
    let message = '';
    
    if (input.validity.valueMissing) {
      message = '이 필드는 필수입니다';
    } else if (input.validity.typeMismatch) {
      message = '올바른 형식이 아닙니다';
    } else if (input.validity.tooShort) {
      message = `최소 ${input.minLength}자 이상 입력해주세요`;
    }
    
    errorElement.textContent = message;
  }
}
</script>

<style>
.form-group {
  margin-bottom: 1em;
}

.hint {
  display: block;
  font-size: 0.875em;
  color: #666;
  margin-top: 0.25em;
}

.error {
  display: block;
  font-size: 0.875em;
  color: #d32f2f;
  margin-top: 0.25em;
}

input[aria-invalid="true"] {
  border-color: #d32f2f;
}
</style>
```

### 2. 데이터 형식 가이드
```html
<div class="input-guide">
  <label for="phone">전화번호</label>
  <input type="tel" id="phone"
         pattern="[0-9]{3}-[0-9]{4}-[0-9]{4}"
         placeholder="010-1234-5678"
         aria-describedby="phone-format">
  
  <div id="phone-format" class="format-guide">
    <h3>입력 형식</h3>
    <ul>
      <li>숫자만 입력 가능</li>
      <li>하이픈(-) 포함</li>
      <li>예: 010-1234-5678</li>
    </ul>
  </div>
</div>

<style>
.format-guide {
  background: #f5f5f5;
  padding: 1em;
  border-radius: 4px;
  margin-top: 0.5em;
}

.format-guide ul {
  margin: 0;
  padding-left: 1.2em;
}
</style>
```

## 오류 메시지

### 1. 접근성 있는 오류 표시
```javascript
class AccessibleErrorMessage {
  constructor() {
    this.errorContainer = document.createElement('div');
    this.errorContainer.setAttribute('role', 'alert');
    this.errorContainer.setAttribute('aria-live', 'polite');
    document.body.appendChild(this.errorContainer);
  }

  showError(message, field = null) {
    this.errorContainer.textContent = message;
    
    if (field) {
      field.setAttribute('aria-invalid', 'true');
      field.setAttribute('aria-describedby', 
        this.errorContainer.id);
    }
    
    // 오류 위치로 포커스 이동
    field?.focus();
  }

  clearError(field = null) {
    this.errorContainer.textContent = '';
    
    if (field) {
      field.removeAttribute('aria-invalid');
      field.removeAttribute('aria-describedby');
    }
  }
}
```

### 2. 오류 요약
```html
<form class="complex-form" novalidate>
  <!-- 폼 필드들 -->
  
  <div class="error-summary" role="alert" hidden>
    <h2>입력 오류가 발생했습니다</h2>
    <ul class="error-list">
      <!-- 오류 목록이 여기에 추가됨 -->
    </ul>
  </div>
</form>

<script>
class ErrorSummary {
  constructor(form) {
    this.form = form;
    this.summary = form.querySelector('.error-summary');
    this.errorList = this.summary.querySelector('.error-list');
    this.errors = new Map();
  }

  addError(fieldId, message) {
    this.errors.set(fieldId, message);
    this.updateSummary();
  }

  removeError(fieldId) {
    this.errors.delete(fieldId);
    this.updateSummary();
  }

  updateSummary() {
    if (this.errors.size === 0) {
      this.summary.hidden = true;
      return;
    }

    this.errorList.innerHTML = '';
    this.errors.forEach((message, fieldId) => {
      const li = document.createElement('li');
      const link = document.createElement('a');
      link.href = `#${fieldId}`;
      link.textContent = message;
      li.appendChild(link);
      this.errorList.appendChild(li);
    });

    this.summary.hidden = false;
  }
}
</script>
```

## 오류 복구

### 1. 자동 저장
```javascript
class AutoSave {
  constructor(form, interval = 30000) {
    this.form = form;
    this.interval = interval;
    this.storageKey = `autosave_${form.id}`;
    
    this.init();
  }

  init() {
    // 저장된 데이터 복원
    this.restoreData();
    
    // 주기적 저장
    setInterval(() => this.saveData(), this.interval);
    
    // 폼 제출 시 저장 데이터 삭제
    this.form.addEventListener('submit', () => {
      localStorage.removeItem(this.storageKey);
    });
  }

  saveData() {
    const formData = new FormData(this.form);
    const data = Object.fromEntries(formData);
    
    localStorage.setItem(this.storageKey, 
      JSON.stringify({
        timestamp: new Date().toISOString(),
        data: data
      })
    );
  }

  restoreData() {
    const saved = localStorage.getItem(this.storageKey);
    if (!saved) return;
    
    const { data } = JSON.parse(saved);
    
    Object.entries(data).forEach(([name, value]) => {
      const field = this.form.elements[name];
      if (field) {
        field.value = value;
      }
    });
  }
}
```

### 2. 실행 취소/다시 실행
```javascript
class UndoManager {
  constructor() {
    this.states = [];
    this.currentIndex = -1;
  }

  saveState(state) {
    // 현재 상태 이후의 기록 제거
    this.states.splice(this.currentIndex + 1);
    
    // 새 상태 추가
    this.states.push(state);
    this.currentIndex++;
  }

  undo() {
    if (this.currentIndex > 0) {
      this.currentIndex--;
      return this.states[this.currentIndex];
    }
    return null;
  }

  redo() {
    if (this.currentIndex < this.states.length - 1) {
      this.currentIndex++;
      return this.states[this.currentIndex];
    }
    return null;
  }
}
```

## 구현 체크리스트

### 필수 항목
- [ ] 모든 필수 필드 표시
- [ ] 실시간 유효성 검사
- [ ] 명확한 오류 메시지
- [ ] 오류 복구 방법 제공

### 권장 항목
- [ ] 자동 저장 기능
- [ ] 실행 취소/다시 실행
- [ ] 입력 형식 가이드
- [ ] 오류 요약 제공

### 테스트
- [ ] 모든 유효성 검사 규칙 테스트
- [ ] 오류 메시지 접근성 확인
- [ ] 자동 저장 기능 검증
- [ ] 복구 기능 테스트

## 참고 자료
- [WCAG 2.1 오류 방지](https://www.w3.org/WAI/WCAG21/Understanding/error-prevention-legal-financial-data)
- [폼 유효성 검사 모범 사례](https://www.w3.org/WAI/tutorials/forms/validation/)
- [WAI-ARIA 오류 처리](https://www.w3.org/WAI/ARIA/apg/patterns/alert/)