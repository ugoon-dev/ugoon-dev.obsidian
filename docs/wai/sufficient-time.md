# 충분한 시간 제공 가이드라인

## 목차
1. [충분한 시간 제공의 중요성](#충분한-시간-제공의-중요성)
2. [시간 제한 콘텐츠 처리](#시간-제한-콘텐츠-처리)
3. [자동 업데이트 제어](#자동-업데이트-제어)
4. [인터랙션 시간 관리](#인터랙션-시간-관리)
5. [구현 가이드](#구현-가이드)

## 충분한 시간 제공의 중요성

사용자가 콘텐츠를 읽고 기능을 사용하는 데 충분한 시간을 제공하는 것은 웹 접근성의 중요한 요소입니다.

### 주요 대상
- 고령자
- 인지/학습 장애인
- 운동 장애인
- 저속 인터넷 사용자
- 모바일 사용자

## 시간 제한 콘텐츠 처리

### 1. 시간 제한 해제
```html
<!-- 시간 제한 설정/해제 옵션 -->
<div class="timeout-settings">
  <h2>세션 시간 설정</h2>
  
  <div class="setting-options">
    <label>
      <input type="radio" name="timeout" value="default">
      기본 시간 (20분)
    </label>
    
    <label>
      <input type="radio" name="timeout" value="extended">
      연장된 시간 (40분)
    </label>
    
    <label>
      <input type="radio" name="timeout" value="unlimited">
      시간 제한 없음
    </label>
  </div>
</div>
```

### 2. 시간 연장 기능
```javascript
class SessionTimeout {
  constructor(options = {}) {
    this.timeoutDuration = options.duration || 1200000; // 20분
    this.warningTime = options.warningTime || 60000; // 1분
    this.timer = null;
    this.warningTimer = null;
  }

  startTimer() {
    this.resetTimer();
    this.timer = setTimeout(() => {
      this.handleTimeout();
    }, this.timeoutDuration);

    this.warningTimer = setTimeout(() => {
      this.showWarning();
    }, this.timeoutDuration - this.warningTime);
  }

  showWarning() {
    const warning = document.createElement('div');
    warning.innerHTML = `
      <div class="timeout-warning" role="alert">
        <p>세션이 곧 만료됩니다.</p>
        <button onclick="sessionTimeout.extend()">시간 연장</button>
      </div>
    `;
    document.body.appendChild(warning);
  }

  extend() {
    this.resetTimer();
    this.startTimer();
    // 경고 메시지 제거
    document.querySelector('.timeout-warning')?.remove();
  }
}
```

## 자동 업데이트 제어

### 1. 자동 슬라이더 제어
```html
<div class="carousel" role="region" aria-label="이미지 슬라이더">
  <div class="carousel-content">
    <!-- 슬라이더 내용 -->
  </div>
  
  <div class="carousel-controls">
    <button class="prev" aria-label="이전">◀</button>
    <button class="next" aria-label="다음">▶</button>
    <button class="pause-play" aria-label="일시정지">
      ⏸
    </button>
  </div>
</div>

<script>
class AccessibleCarousel {
  constructor(element) {
    this.element = element;
    this.isPlaying = true;
    this.interval = 5000; // 5초
    
    this.init();
  }

  init() {
    this.setupControls();
    this.startAutoPlay();
  }

  setupControls() {
    const pausePlay = this.element.querySelector('.pause-play');
    pausePlay.addEventListener('click', () => {
      if (this.isPlaying) {
        this.pause();
      } else {
        this.play();
      }
    });
  }

  pause() {
    this.isPlaying = false;
    clearInterval(this.autoPlayTimer);
    // 버튼 상태 업데이트
  }

  play() {
    this.isPlaying = true;
    this.startAutoPlay();
    // 버튼 상태 업데이트
  }
}
</script>
```

### 2. 자동 새로고침 제어
```javascript
class AutoRefresh {
  constructor(options = {}) {
    this.interval = options.interval || 60000; // 1분
    this.isEnabled = true;
    this.timer = null;
  }

  init() {
    this.addControls();
    if (this.isEnabled) {
      this.start();
    }
  }

  addControls() {
    const controls = document.createElement('div');
    controls.innerHTML = `
      <div class="refresh-controls">
        <label>
          <input type="checkbox" 
                 ${this.isEnabled ? 'checked' : ''}
                 onchange="autoRefresh.toggle(this.checked)">
          자동 새로고침 사용
        </label>
      </div>
    `;
    document.body.appendChild(controls);
  }

  toggle(enabled) {
    this.isEnabled = enabled;
    if (enabled) {
      this.start();
    } else {
      this.stop();
    }
  }
}
```

## 인터랙션 시간 관리

### 1. 폼 제출 시간
```html
<form id="longForm" onsubmit="return validateForm()">
  <div class="form-status">
    <p>작성 중인 내용은 자동으로 저장됩니다.</p>
    <button type="button" onclick="saveProgress()">
      임시 저장
    </button>
  </div>
  
  <!-- 폼 필드들 -->
  
  <div class="form-actions">
    <button type="submit">제출</button>
    <button type="button" onclick="requestMoreTime()">
      더 많은 시간 필요
    </button>
  </div>
</form>

<script>
function saveProgress() {
  const formData = new FormData(document.getElementById('longForm'));
  localStorage.setItem('formProgress', JSON.stringify(Object.fromEntries(formData)));
}

function loadProgress() {
  const savedData = JSON.parse(localStorage.getItem('formProgress'));
  if (savedData) {
    // 저장된 데이터로 폼 필드 채우기
  }
}

function requestMoreTime() {
  // 서버에 시간 연장 요청
  saveProgress(); // 현재 진행상황 저장
}
</script>
```

### 2. 읽기 시간 조절
```javascript
class ReadingTimeManager {
  constructor(content, options = {}) {
    this.content = content;
    this.wordsPerMinute = options.wordsPerMinute || 200;
    this.autoScroll = false;
    this.scrollSpeed = options.scrollSpeed || 'medium';
  }

  calculateReadingTime() {
    const words = this.content.textContent.trim().split(/\s+/).length;
    return Math.ceil(words / this.wordsPerMinute);
  }

  addReadingControls() {
    const controls = document.createElement('div');
    controls.innerHTML = `
      <div class="reading-controls">
        <button onclick="readingManager.toggleAutoScroll()">
          자동 스크롤 ${this.autoScroll ? '중지' : '시작'}
        </button>
        
        <div class="speed-control">
          <label>스크롤 속도</label>
          <select onchange="readingManager.setScrollSpeed(this.value)">
            <option value="slow">천천히</option>
            <option value="medium" selected>보통</option>
            <option value="fast">빠르게</option>
          </select>
        </div>
      </div>
    `;
    this.content.parentNode.insertBefore(controls, this.content);
  }
}
```

## 구현 체크리스트

### 필수 항목
- [ ] 시간 제한이 있는 모든 기능에 대해 연장/해제 옵션 제공
- [ ] 자동 업데이트 콘텐츠에 대한 제어 수단 제공
- [ ] 진행 상황 저장 기능 구현
- [ ] 시간 만료 전 충분한 경고 제공

### 권장 항목
- [ ] 사용자 설정 저장 및 복원
- [ ] 다양한 시간 옵션 제공
- [ ] 읽기 속도 조절 기능
- [ ] 자동 저장 기능

## 참고 자료
- [WCAG 2.1 충분한 시간 제공](https://www.w3.org/WAI/WCAG21/Understanding/enough-time)
- [웹 접근성 이니셔티브 시간 제한 가이드라인](https://www.w3.org/WAI/WCAG21/Understanding/timing-adjustable.html)
- [사용자 인터랙션 시간 관리 모범 사례](https://www.w3.org/WAI/WCAG21/Understanding/no-timing.html)