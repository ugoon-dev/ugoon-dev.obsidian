# 내비게이션 바 디자인 가이드라인

## 1. 뒤로 가기 버튼 설계

### 단일 뒤로 가기 버튼 원칙
- 한 화면에 뒤로 가기 버튼은 반드시 하나만 존재
- 사용자의 혼란을 방지하고 일관된 경험 제공
- 시스템 뒤로 가기와 앱 내 뒤로 가기의 동작 일치성 유지

### 구현 예시
```swift
// iOS 예시
class NavigationController: UINavigationController {
    override func viewDidLoad() {
        super.viewDidLoad()
        // 커스텀 뒤로 가기 버튼 설정
        let backButton = UIBarButtonItem(
            image: UIImage(named: "back-arrow"),
            style: .plain,
            target: self,
            action: #selector(handleBack)
        )
        navigationItem.leftBarButtonItem = backButton
    }
}
```

## 2. 내비게이션 바와 탭바의 기능 분리

### 중복 방지 원칙
- 상단 내비게이션 바와 하단 탭바의 기능 중복 금지
- 각 요소의 역할 명확히 정의
  - 내비게이션 바: 현재 위치 표시 및 계층 이동
  - 탭바: 주요 기능 간 전환

### 기능 분리 예시
```kotlin
// Android 예시
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        // 내비게이션 바 설정
        setupNavigationBar()
        // 탭바 설정
        setupBottomNavigation()
    }
    
    private fun setupNavigationBar() {
        // 계층 이동 관련 기능만 포함
    }
    
    private fun setupBottomNavigation() {
        // 주요 섹션 전환 기능만 포함
    }
}
```

## 3. 내비게이션 바 레이아웃

### 좌우 요소 배치
1. **왼쪽 영역**
   - 뒤로 가기 버튼 배치
   - 이전 화면명을 레이블로 표시
   - 명확한 터치 영역 확보 (최소 44x44pt)

2. **중앙 영역**
   - 현재 화면 제목 표시
   - 간결하고 명확한 텍스트 사용
   - 글자 수 제한 (2줄 이내)

3. **오른쪽 영역**
   - 편집/완료/공유 등 주요 컨트롤 배치
   - 상황에 맞는 적절한 아이콘 사용
   - 중요도에 따른 우선순위 부여

### 구현 가이드라인
```css
.navigation-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 44px;
  padding: 0 16px;
}

.nav-left {
  display: flex;
  align-items: center;
  min-width: 44px;
}

.nav-title {
  text-align: center;
  font-weight: 600;
  flex: 1;
}

.nav-right {
  display: flex;
  align-items: center;
  gap: 16px;
}
```

## 4. 이전 화면명 표시

### 레이블 디자인 원칙
- 명확성: 이전 화면을 정확히 식별할 수 있는 텍스트
- 간결성: 불필요한 정보 제외
- 일관성: 전체 앱에서 동일한 패턴 유지

### 구현 예시
```swift
// iOS 예시
extension UIViewController {
    func setupBackButton(previousTitle: String) {
        let backButton = UIBarButtonItem(
            title: previousTitle,
            style: .plain,
            target: nil,
            action: nil
        )
        navigationItem.backBarButtonItem = backButton
    }
}
```

## 5. 접근성 고려사항

### 주요 고려사항
1. **터치 영역**
   - 모든 탭 가능 요소는 최소 44x44pt
   - 요소 간 충분한 간격 확보

2. **색상 대비**
   - WCAG 2.1 기준 준수
   - 다크 모드 지원

3. **음성 안내**
   - 모든 내비게이션 요소에 적절한 레이블 제공
   - 화면 전환 시 명확한 피드백

## 참고 자료
1. Apple Human Interface Guidelines - Navigation Bars
2. Material Design - Top app bars
3. WCAG 2.1 - Navigation Guidelines
4. Nielsen Norman Group - Mobile Navigation Patterns

## 변경 이력
- 2025.03.27: 최초 작성
- 추가 업데이트 예정