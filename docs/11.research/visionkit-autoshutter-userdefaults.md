# iOS VNDocumentCameraViewController "Auto Shutter" 제어 연구

`VNDocumentCameraViewController`는 Apple의 VisionKit에서 제공하는 시스템 표준 UI 코드로, 공식적으로는 **Auto Shutter(자동 셔터) 기능을 코드로 제어할 수 있는 API를 제공하지 않습니다.** 

하지만 개발자 커뮤니티(Stack Overflow, GitHub) 및 시스템 내부 분석을 통해 알려진 비공식적인(Private API 수준의) 제어 방법과 그 한계점을 정리합니다.

## 1. UserDefaults를 이용한 상태 제어 (Private API)

VisionKit은 내부적으로 `VNDocumentCameraViewController`의 설정 상태(필터 선택, 자동/수동 셔터 모드 등)를 저장하고 관리하기 위해 `UserDefaults`를 사용합니다.

*   **관련 UserDefaults 키 (추정):**
    *   `VNScanDocumentAutoShutter`
    *   `VNDocCamAutoScanEnabled`
    *   `VNScanDocumentAutoShutterEnabled`
*   **동작 원리:** 시스템은 이 컨트롤러가 표시되기 전에 특정 키값을 참조하여 마지막 사용 상태를 복원하려 시도합니다. 개발자가 컨트롤러 초기화 직전에 이 값을 강제로 `false`로 설정함으로써 '수동(Manual)' 모드를 유도할 수 있다는 연구 결과가 있습니다.

## 2. 코드 구현 사례 (Swift)

이 방식은 공식 지원 기능이 아니므로, 앱 서비스에 적용 시 반드시 충분한 테스트가 필요합니다.

```swift
import UIKit
import VisionKit

func presentScanner(from viewController: UIViewController) {
    // 1. VisionKit이 참조하는 내부 설정을 강제로 Manual(false)로 세팅
    // 주의: 시스템 내부 도메인(com.apple.VisionKit)에 직접 접근하지 못하는 경우 
    // standard를 통해 덮어쓰기를 시도하는 방식입니다.
    UserDefaults.standard.set(false, forKey: "VNScanDocumentAutoShutter")
    UserDefaults.standard.set(false, forKey: "VNDocCamAutoScanEnabled")
    UserDefaults.standard.synchronize()

    // 2. 컨트롤러 표시
    guard VNDocumentCameraViewController.isSupported else { return }
    let scannerViewController = VNDocumentCameraViewController()
    // scannerViewController.delegate = self
    viewController.present(scannerViewController, animated: true)
}
```

## 3. iOS 버전별 동작 차이 및 한계

*   **iOS 13 ~ 15:** 해당 `UserDefaults` 트릭이 비교적 잘 동작하는 것으로 알려져 있습니다. 사용자가 수동 모드로 앱을 시작하도록 강제할 수 있는 유일한 방법 중 하나였습니다.
*   **iOS 16 ~ 17:** Apple이 `DataScannerViewController`를 도입하고 내부 보정 로직을 강화하면서, 시스템이 `UserDefaults` 값을 덮어쓰는 타이밍이나 우선순위가 변경되었습니다. 이에 따라 `UserDefaults.standard` 세팅이 무시되고 사용자의 **마지막 수동 선택 상태**가 우선되는 경우가 빈번해졌습니다.
*   **App Store 심사 이슈:** `synchronize()` 호출이나 비공식 키 사용은 드물게 심사 과정에서 'Private API 사용'으로 간주될 수 있으나, 보통 `UserDefaults` 키 사용 자체로 거절되는 경우는 적습니다. 다만, 기능의 영속성을 보장할 수 없다는 점이 가장 큰 리스크입니다.

## 4. Stack Overflow / GitHub의 주요 해결 사례 요약

| 접근 방식 | 설명 | 커뮤니티 권장도 |
| :--- | :--- | :--- |
| **순응형 (Native)** | 시스템 동작에 맡기고, 사용자가 필요시 직접 모드를 변경하도록 가이드 제공 | 최상 (안정성) |
| **UserDefaults 해킹** | `VNDocCamAutoScanEnabled` 키 등을 주입하여 초기 상태를 Manual로 고정 시도 | 중 (불안정) |
| **Custom 구현 (AVFoundation)** | `VNDocumentCameraViewController`를 사용하지 않고 직접 카메라 UI를 개발 | 하 (개발 비용 높음) |

### 커스텀 구현 시 참고 (AVFoundation + Vision)
정밀한 제어가 필수적인 금융 앱 등에서는 다음과 같은 워크플로우를 직접 구현합니다:
1. `AVCapturePhotoOutput`으로 영상 프레임 분석
2. `VNDetectDocumentSegmentationRequest`를 통해 실시간 문서 영역 검출
3. 셔터 버튼 터치 시 사진 캡처 후 `CIPerspectiveCorrection`으로 이미지 보정

---

## 결론
`VNDocumentCameraViewController`에서 자동 셔터를 끄는 **공식적인 방법은 없습니다.** 

가장 저비용으로 시도할 수 있는 방법은 `UserDefaults.standard.set(false, forKey: "VNDocCamAutoScanEnabled")`를 주입하는 것이지만, 이는 iOS 업데이트에 따라 언제든 작동하지 않을 수 있습니다. 

사용자 경험(UX) 관점에서는 **"안내 메시지"**를 제공하여 사용자가 수동 촬영을 선택하도록 유도하는 것이 가장 권장되는 방식입니다.
