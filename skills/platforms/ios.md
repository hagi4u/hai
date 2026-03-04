# iOS 제약사항

## UI / UX
<!-- Safe Area, 노치/다이나믹 아일랜드, 시스템 제스처 간섭 등 -->

## 이미지

### SVG — URL(런타임) 렌더링 불가
- iOS는 **런타임에 URL로 전달된 SVG 렌더링을 지원하지 않음**
- Xcode의 SVG 지원(Xcode 12+)은 **빌드 타임에 래스터(PNG)로 변환**하는 방식 → 런타임과 무관
  - 앱 내장 SVG가 표시되더라도 이는 빌드 시 변환된 PNG를 보여주는 것
- Apple 내부에 `CoreSVG` 프레임워크가 존재하나 **공개 API 아님** → 사용 불가
- 오픈소스 라이브러리(SVGKit, SwiftSVG, SVGView 등)가 있으나 안정성/유지보수 불안정
- **SDUI 등 서버 드리븐 방식에서 이미지를 URL로 내려줄 때는 PNG 또는 JPG로 제한할 것**
  - KREAM: SDUI에서 내장 SVG key만 지원, URL SVG 미운영 중

## 권한 (Permissions)
<!-- 카메라, 위치, 알림 등 권한 정책 및 버전별 차이 -->

## WebView (WKWebView)
<!-- WKWebView 동작 제약, JS 브릿지 이슈, 쿠키/스토리지 제한 등 -->

## 결제 / 보안
<!-- 인앱결제(IAP) 강제, 생체인증(Face ID/Touch ID), 보안 키패드 등 -->

## OS 버전별 이슈
<!-- 특정 iOS 버전에서 발생하는 제약사항 -->

## 심사 / 배포
<!-- App Store 정책, 심사 기준, 리젝 사례 -->

## 기타
