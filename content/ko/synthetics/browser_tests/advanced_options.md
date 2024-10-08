---
aliases:
- /ko/synthetics/guide/browser-tests-switch-tabs/
- /ko/synthetics/guide/browser-test-self-maintenance/
description: 브라우저 테스트 단계 고급 옵션 설정하기
further_reading:
- link: https://www.datadoghq.com/blog/browser-tests/
  tag: 블로그
  text: 신서틱(Synthetic) 브라우저 테스트로 사용자 경험 모니터링
- link: /synthetics/browser_tests/actions/
  tag: 설명서
  text: 브라우저 테스트 단계 더 알아보기
title: 브라우저 테스트 단계 고급 옵션
---

## 개요

본 페이지에서는 신서틱(Synthetic) 브라우저 테스트의 고급 옵션에 대해 설명합니다. 


## 요소 찾기

### Datadog 알고리즘

테스트가 실패할 수 있기 때문에 엔드투엔드 테스트에서의 불안정함은 문제가 됩니다. 프론트엔드 팀이 변경 사항을 구현할 때 실제 애플리케이션 문제 대신 테스트의 식별자가 이를 경고할 수 있습니다.

Datadog은 테스트 오류를 방지하기 위해 일련의 로케이터를 활용하여 브라우저 테스트에서 요소를 타겟팅하는 알고리즘을 사용합니다. UI를 조금만 변경해도 요소가 수정될 수 있습니다(예: 다른 위치로 이동). 브라우저 테스트는 변경의 영향을 받지 않는 참조 포인트를 기반으로 요소를 자동으로 다시 찾아냅니다.

테스트가 성공적으로 실행되면 브라우저 테스트는 손상된 로케이터를 업데이트된 값으로 다시 계산(또는 "자체 복구")합니다. 이렇게 하면 테스트가 간단한 UI 업데이트로 인해 중단되지 않고 애플리케이션의 UI에 자동으로 적용됩니다.

브라우저 테스트 시 예상치 못한 변경 사항을 검증하지 않도록 테스트 생성 시 [어서션][5]을 활용합니다. 어서션으로 테스트 단계 경로와 연관된 예상되는 동작과 그렇지 않은 동작을 정의할 수 있습니다.

### 사용자 지정 로케이터

브라우저 테스트는 Datadog 로케이터 시스템을 기본으로 활용합니다. 테스트에서 상호 작용할 특정 요소(예: 결제 버튼)를 검색할 때 특정 XPath 또는 특정 CSS 선택기를 갖춘 요소를 확인하는 대신, 다양한 참조 포인트(예: XPath, 텍스트, 클래스 및 주변 요소)을 사용하여 해당 요소를 찾아냅니다. 

해당 참조 포인트는 일련의 로케이터가 되며, 각 로케이터는 요소를 고유하게 정의합니다. 본 Datadog 로케이터 시스템을 사용하면 테스트를 자체적으로 유지 관리할 수 있으므로 특수한 케이스에서만 커스텀 선택기를 사용해야 합니다.

커스텀 선택기는 페이지의 모든 요소에서 레코더의 관심 단계(예: **클릭**, **호버링** 또는 **어서션**)를 수행하여 생성됩니다. 이는 수행해야 하는 단계의 특성을 지정합니다.

특정 식별자를 사용하려면(예: 요소의 콘텐츠와는 무관하게 드롭다운 메뉴에서 `nth` 요소를 클릭하는 경우):

1. 레코딩하거나 [단계][1]를 레코딩에 수동 추가합니다.
2. 레코딩한 단계를 클릭한 후 **고급 옵션**을 클릭합니다.
3. **사용자 지정 로케이터** 하단에 XPath 1.0 선택기 또는 CSS 클래스/ID를 입력합니다. 예를 들어 HTML 요소의 경우 `div`, `h1` 또는 `.hero-body`입니다.
4. 요소를 정의한 다음 **테스트**를 눌러 오른쪽의 레코딩에서 해당 요소를 강조 표시합니다.

기본값으로 **사용자가 지정한 로케이터가 실패하면 테스트 실패** 확인란이 체크되어 있습니다. 정의된 로케이터가 실패하면 테스트는 실패한 것으로 간주됩니다.

{{< img src="synthetics/browser_tests/advanced_options/css.mp4" alt="강조 표시된 요소 테스트" video=true >}}

**사용자가 지정한 로케이터가 실패하면 테스트 실패** 확인란을 체크 해제하여 일반 브라우저 테스트 알고리즘으로 돌아가도록 할 수 있습니다.

{{< img src="synthetics/browser_tests/advanced_options/fail_test.png" alt="테스트 실패 옵션" style="width:70%">}}

## 시간 초과

브라우저 테스트에서 요소를 찾을 수 없는 경우 60초 동안 해당 단계를 재시도합니다.

테스트가 해당 단계의 대상 요소를 찾을 때까지 더 짧게 또는 길게 대기하도록 최대 300초까지 시간 제한을 증가 또는 감소시킬 수 있습니다.

{{< img src="synthetics/browser_tests/advanced_options/time_before_fail.png" alt="실패 전 대기 시간" style="width:50%">}}

## 옵션 단계

팝업 이벤트와 같은 경우, 옵션 단계 몇 가지를 더 실행해야 할 수도 있습니다. 해당 옵션을 설정하려면  **이 단계가 실패하도록 허용**을 선택하세요. 시간 초과 옵션으로 지정한 시간(기본값: 60초)이 지난 후에 해당 단계가 실패할 경우 테스트가 계속 진행되며 다음 단계가 실행됩니다.

{{< img src="synthetics/browser_tests/advanced_options/timeout.png" alt="시간 초과" style="width:25%">}}

## 스크린샷 캡처 방지

테스트 실행 시 단계 스크린샷이 캡처되지 않도록 설정할 수 있습니다. 본 기능으로 테스트 결과에 민감한 데이터가 포함되지 않도록 도와드립니다. 장애 트러블슈팅을 더 어렵게 만들 수 있으므로 신중하게 사용하세요. 자세한 내용을 확인하려면 [신서틱(Synthetic) 모니터링 데이터 보안][2]을 참조하세요.

{{< img src="synthetics/browser_tests/advanced_options/screenshot_capture_option.png" alt="스크린샷 캡처 옵션" style="width:50%">}}

**참고: ** 본 기능은 브라우저 테스트 설정의 [고급 옵션][3]으로 전역 테스트 수준에서도 사용할 수 있습니다.

## 하위 테스트

[하위 테스트][4]의 고급 옵션에서는 하위 테스트를 실행할 지점을 선택하고 해당 하위 테스트가 실패할 경우의 브라우저 테스트 동작을 설정합니다.

{{< img src="synthetics/browser_tests/advanced_options/subtest_advanced.png" alt="브라우저 테스트의 하위 테스트용 고급 옵션" style="width:60%">}}

### 하위 테스트 창 설정하기

* **메인(기본값)**: 하위 테스트를 메인 창에서 다른 단계와 같이 순서대로 실행합니다.
* **신규**: 하위 테스트를 새 창에서 실행하고 테스트가 종료되면 창이 닫힙니다. 해당 창을 재사용할 수 없습니다.
* **특정 창**: 번호를 붙인 창에서 하위 테스트를 실행하며, 해당 창을 다른 하위 테스트를 실행하는 데 재사용할 수 있습니다.

하위 테스트를 메인 창에서 열면 해당 하위 테스트는 이전 단계의 URL을 사용하게 되므로, 이는 메인 테스트의 연속으로 간주됩니다. 신규 창 또는 특정 창에서 하위 테스트를 열면 하위 테스트 시작 URL에서 해당 테스트가 실행됩니다.

### 실패 동작 설정하기

**본 단계가 실패하더라도 계속 테스트 진행** 및 **본 단계가 실패할 경우 전체 테스트 실패로 간주**를 클릭하여 하위 테스트가 실패한 경우 브라우저 테스트를 계속 진행하거나 전체 테스트가 실패한 것으로 간주합니다.

### 하위 테스트에서 변수 재정의

브라우저 테스트의 하위 테스트에서 변수 값을 재정의하려면 하위 테스트에서 변수 이름을 지정하고 부모 테스트에서 동일한 변수 이름을 사용합니다. 이렇게 하면 브라우저 테스트가 하위 테스트 값을 재정의합니다.

자세한 내용을 확인하려면 [브라우저 테스트 단계][4]를 참조하세요.

## 참고 자료

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ko/synthetics/browser_tests/actions/
[2]: /ko/data_security/synthetics/
[3]: /ko/synthetics/browser_tests/?tab=privacy#test-configuration
[4]: /ko/synthetics/browser_tests/actions/#subtests
[5]: /ko/synthetics/browser_tests/actions/#assertion