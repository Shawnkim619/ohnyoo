# 온유 성장일기 배포 가이드

## 1. 먼저 로컬 모드로 써보기
- GitHub에 새 저장소(예: onyu)를 만들고 index.html, manifest.json, sw.js, icon-192.png, icon-512.png 5개 파일 업로드
- Settings > Pages > Branch: main / root 선택 후 저장
- 몇 분 뒤 https://shawnkim619.github.io/onyu/ 접속
- 이 상태(로컬 모드)는 기기마다 따로 저장됨

## 2. 부부 공동 모드 (Firebase)
1. Firebase 콘솔에서 새 프로젝트 생성 (책갈피와 분리 추천)
2. Authentication > 로그인 방법 > Google 사용 설정
3. Authentication > 설정 > 승인된 도메인에 shawnkim619.github.io 추가
4. Firestore Database 만들기 (프로덕션 모드)
5. 프로젝트 설정 > 웹 앱 추가 > firebaseConfig 복사
6. index.html 맨 위 설정 부분 수정
   - FIREBASE_CONFIG = { ...복사한 값... };
   - MEMBERS = { '아내메일@gmail.com': 'mom', 'kmhemma619@gmail.com': 'dad' };
7. Firestore > 규칙에 아래 내용 붙여넣고 게시 (이메일 2개 수정)

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /families/{fid}/{document=**} {
      allow read, write: if request.auth != null &&
        request.auth.token.email in ['아내메일@gmail.com', 'kmhemma619@gmail.com'];
    }
    match /families/{fid} {
      allow read, write: if request.auth != null &&
        request.auth.token.email in ['아내메일@gmail.com', 'kmhemma619@gmail.com'];
    }
  }
}

- 사진은 Firestore에 압축(약 100~200KB)해서 저장하므로 Firebase Storage(유료 요금제)는 필요 없음
- 로컬 모드에서 쓰던 기록은 설정 > 백업 파일 저장 후, 공동 모드에서 백업 불러오기로 옮기면 됨

## 3. 폰에 설치
- 안드로이드 크롬에서 주소 열기 > 메뉴 > 홈 화면에 추가

## 4. 앱 수정 후
- sw.js의 VERSION 값을 onyu-v2처럼 올리면 폰에서도 새 버전이 바로 반영됨
