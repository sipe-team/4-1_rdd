## 기존 형태 (기존 방식의 문제점)
기존 ERP 시스템에서는 업무 알림 기능이 일반적인 알람이 아닌 To-Do 리스트 형식에 가까운 구조로 구현되어 있어 다음과 같은 문제점이 발생함:

1. 알림이 단순한 알림이 아니라, 해당 업무가 완료되기 전까지 사라지지 않아 일반 알람 기능처럼 활용하기 어려움
2. 알림 데이터를 전용 알림 테이블이 아닌, 각 업무별 DB에서 직접 가져오는 방식으로 인해 로직이 복잡하고, 유지보수가 어려움
3. 실시간 알림 구조가 아니며, 브라우저를 새로 고침하거나 재로그인해야 알림이 갱신되어, 실시간성이 떨어짐

## 개선 목표
- 사용자에게 <b>실시간으로 업무 알림을 제공</b>하여, 업무 인지 및 대응 속도를 향상
- 알림 수신의 접근성과 편의성을 개선하여, 사용자 경험(UX) 제고
- 기존 시스템의 복잡한 데이터 연동 구조를 단순화하여 유지보수 효율성을 높임

## 개선할 형태
모든 구성원이 사용하는 Microsoft Teams 메신저와 연동된 실시간 알림 시스템을 도입

알림 전송은 Microsoft Graph API를 활용하여, ERP에서 발생하는 업무 이벤트에 따라 자동으로 Teams 메시지를 전송

메시지 내에 **업무 화면으로 바로 이동할 수 있는 링크(바로가기 버튼)**를 포함시켜, 클릭 시 해당 ERP 업무 화면으로 즉시 접근 가능

### ⚙️ 개발 및 구현 방식의 목표 
- 알림 전송 로직은 <b>공통 서비스화</b>하여 유지보수성과 재사용성이 높고, 전체 시스템에 최소한의 수정으로 손쉽게 적용 가능
- 알림 내용 구성, 사용자 지정, 링크 생성 등의 기능은 매개변수 기반으로 유연하게 처리되도록 설계
- 실제 알람 전송은 Microsoft Graph API를 통해 이루어지며, 이를 분산 서버 환경에서도 안정적으로 처리하기 위해 다음과 같은 구조를 적용
  - 비동기 큐 방식으로 알림 요청을 처리
    
    → 각 업무 서버에서는 알람 요청을 큐에 전달만 하고, 실제 전송은 전담 워커(Worker) 서버에서 처리
    
    → 이를 통해 API 호출 부하를 분산하고, 업무 처리 속도에 영향을 주지 않음
    
  - API 인증/엑세스 키 관리의 일원화
    
    → Graph API의 엑세스 토큰 및 인증 관리는 중앙의 워커 서버에서 일괄 처리
    
    → 분산 서버 간 토큰 충돌이나 인증 실패를 방지하고, 보안성과 일관성을 동시에 확보
    
## 개선 후 예상되는 성과
- 사용자 측면
  - 중요 알림 누락 방지로 업무 효율성 향상
  - 상황에 맞는 업무화면 링크로 더욱 편리하고 쾌적한 DMS 사용 경험 제공
  - 업무의 병목 구간이 줄어들고 전반적인 업무 리드타임 단축 효과 기대
- 개발 측면
  - 공통 알림 모듈을 통한 구조화된 구현으로, 유지보수 효율성 증대
  - 알람 전용 워커 서버를 통해 알림 기능의 수평 확장이 용이하고, API 호출 부담을 업무 서버로부터 분리함으로써 시스템 전체의 확장성과 안정성을 동시에 확보
