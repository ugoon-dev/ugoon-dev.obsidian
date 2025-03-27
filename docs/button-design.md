# 버튼 디자인 가이드라인

## 1. 상태별 색상 구분

### 활성/비활성 상태 표현
1. **활성 상태**
   - 선명한 주요 브랜드 색상 사용
   - 높은 대비를 통한 가시성 확보
   - 상호작용 가능 상태 명확히 표현

2. **비활성 상태**
   - 채도와 명도를 낮춘 색상 사용
   - 상호작용 불가 상태 시각적 표현
   - 비활성 사유에 대한 피드백 제공

### 구현 가이드라인
```css
/* 버튼 기본 스타일 */
.button {
    padding: 12px 24px;
    border-radius: 8px;
    font-weight: 600;
    transition: all 0.3s ease;
}

/* 활성 상태 */
.button.active {
    background-color: #007AFF;
    color: white;
    cursor: pointer;
}

/* 비활성 상태 */
.button.disabled {
    background-color: #E5E5EA;
    color: #8E8E93;
    cursor: not-allowed;
}

/* 호버 상태 */
.button.active:hover {
    background-color: #0066D6;
}
```

## 2. 버튼 레이블 작성

### 명확한 의도 표현
1. **행동 중심 레이블**
   - 동사형으로 시작
   - 사용자 액션 명확히 표현
   - 모호한 표현 지양

2. **구체적인 표현**
   - '예/아니오' 대신 구체적 액션 사용
   - 예: '이체하기/취소' 
   - 결과를 예측할 수 있는 문구

### 예시
```html
<!-- 좋은 예시 -->
<button class="button primary">결제하기</button>
<button class="button secondary">장바구니에 담기</button>

<!-- 피해야 할 예시 -->
<button class="button">확인</button>
<button class="button">OK</button>
```

## 3. 버튼 위치 설계

### 주 액션과 보조 액션 구분
1. **주 액션**
   - 화면의 주요 목적을 달성하는 버튼
   - 눈에 띄는 위치와 스타일
   - 일반적으로 우측 또는 하단 배치

2. **보조 액션**
   - 취소, 뒤로 가기 등 보조적 기능
   - 덜 강조된 스타일
   - 주 액션의 좌측에 배치

### 구현 예시
```css
/* 버튼 컨테이너 */
.button-container {
    display: flex;
    justify-content: flex-end;
    gap: 12px;
    padding: 16px;
}

/* 주 액션 버튼 */
.button.primary {
    background-color: #007AFF;
    color: white;
}

/* 보조 액션 버튼 */
.button.secondary {
    background-color: transparent;
    color: #007AFF;
    border: 1px solid #007AFF;
}
```

## 4. 되돌릴 수 없는 액션 처리

### 안전장치 설계
1. **시각적 구분**
   - 경고성 색상 사용 (예: 빨간색)
   - 아이콘을 통한 경고 표시
   - 구분되는 위치에 배치

2. **확인 절차**
   - 2단계 확인 프로세스
   - 명확한 경고 메시지
   - 취소 옵션 제공

### 구현 예시
```javascript
class DangerousAction extends Component {
    state = { showConfirm: false };

    handleAction = () => {
        this.setState({ showConfirm: true });
    };

    handleConfirm = () => {
        // 실제 삭제 로직
        this.setState({ showConfirm: false });
    };

    render() {
        return (
            <>
                <button 
                    className="button dangerous"
                    onClick={this.handleAction}
                >
                    삭제하기
                </button>

                {this.state.showConfirm && (
                    <ConfirmDialog
                        message="이 작업은 되돌릴 수 없습니다. 계속하시겠습니까?"
                        onConfirm={this.handleConfirm}
                        onCancel={() => this.setState({ showConfirm: false })}
                    />
                )}
            </>
        );
    }
}
```

## 5. 접근성 고려사항

### 주요 고려사항
1. **터치 영역**
   - 최소 44x44pt 확보
   - 충분한 여백 설정
   - 터치 피드백 제공

2. **상태 표현**
   - 상태 변화의 시각적 표현
   - 적절한 ARIA 레이블 사용
   - 스크린 리더 지원

### 구현 가이드라인
```css
/* 접근성 고려한 버튼 스타일 */
.button {
    min-width: 44px;
    min-height: 44px;
    padding: 12px 24px;
    touch-action: manipulation;
}

/* 포커스 표시 */
.button:focus {
    outline: 2px solid #007AFF;
    outline-offset: 2px;
}

/* 터치 피드백 */
.button:active {
    transform: scale(0.98);
}
```

## 참고 자료
1. Material Design - Buttons
2. Apple Human Interface Guidelines - Buttons
3. WCAG 2.1 - Interactive Elements
4. Nielsen Norman Group - Button UX Design

## 변경 이력
- 2025.03.27: 최초 작성
- 추가 업데이트 예정