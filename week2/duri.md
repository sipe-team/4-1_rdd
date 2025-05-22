## 금주 목표 
- 프로젝트 생성 및 기본구조 개발

## 진행상황 공유
### 1. 디렉토리 구조
```
Peeker/
├── Package.swift                    # SPM 메타데이터
├── Sources/
│   └── Peeker/
│       ├── Peeker.swift             # 진입점
│       ├── PeekerMonitor.swift      # 메모리 측정 로직
│       └── PeekerGraphView.swift    # UIKit 그래프 뷰
│
├── Tests/
│   └── PeekerTests/
│       └── PeekerTests.swift        # 유닛 테스트
│
├── Example/                         # 샘플 (SwiftUI 앱)
│   ├── PeekerExample.xcodeproj
│   └── PeekerExample/
│       ├── ContentView.swift        
│       ├── PeekerExampleApp.swift
│       └── Assets.xcassets
│
├── README.md                        
├── LICENSE                          # 오픈소스 라이선스 (MIT)
└── .gitignore                       
```

### 2. 작업 내역
앱의 사용 중인 메모리 측정


### 3. 차주 목표 및 느낀 점
3-1)차주 목표
1. 네트워크 상태 정보 기능 추가
2. CPU 사용률 정보 기능 추가

3-2) 느낀 점
- 생각보다 앱 배포보다 쉬..울 것 같다 ...?
- 레포지토리를 만들긴 했는데, 배포 완료 정도의 상태가 됐을 때 public으로 전환할 생각 중.. (부끄러워잇)