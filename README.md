# 서촌 가을 산책 — 식사 · 카페 · 구경

경복궁 서쪽 서촌 일대의 식당·카페·구경거리를 세 카테고리로 정리한 단일 HTML 페이지입니다.
지도·사진·길찾기는 **Google Maps Platform**을 사용합니다.

## 기능

- 카테고리 탭(전체 / 식사 / 카페 / 구경)과 해시 라우팅 (`#meal`, `#cafe`, `#see`)
- 장소 카드: 이름, 사진(Google Places), 주소, 운영시간, 대기 전망, 태그, 설명
- Google 지도에 카테고리 색 핀 표시 — 핀 클릭 → 정보창 + 카드 강조, 카드 「지도에서 보기」 → 핀으로 이동
- 「현재 위치에서 길찾기」: 위치 권한이 있으면 지도 위에 도보 경로(Directions)와 거리·시간 표시, 없으면 Google 지도 앱을 열어 기기 위치에서 경로 생성
- 「내 위치 표시 · 거리순 정렬」: 지도에 내 위치 표시, 카드별 도보 거리·분, 가까운 순 정렬
- 모바일 적응형: 상단 고정 지도(접기/펼치기), 1열 카드, `prefers-reduced-motion` 대응

## 설정 (필수)

1. [Google Cloud 콘솔](https://console.cloud.google.com/)에서 프로젝트를 만들고 다음 API를 사용 설정합니다.
   - Maps JavaScript API
   - Places API
   - Directions API
2. API 키를 만들고 **애플리케이션 제한 → HTTP 리퍼러**에 배포 주소를 넣습니다.
   예: `https://streqmin.github.io/*`, 로컬 테스트 시 `http://localhost/*`
3. `index.html` 상단의 설정에 키를 넣습니다.

   ```js
   const GMAPS_KEY = '발급받은 키';
   const PHOTOS_FROM_PLACES = true; // Places 사진 요청을 끄려면 false
   ```

키가 없으면 지도 영역에 안내만 표시되고, 카드의 「Google 지도 앱」·「사진 보기」 링크는 그대로 작동합니다.

> Places 사진은 장소마다 요청이 발생해 무료 한도를 넘기면 과금될 수 있습니다. 개인 용도의 트래픽이면 월 무료 크레딧 안에서 충분하지만, 불안하면 `PHOTOS_FROM_PLACES = false`로 두세요.

## 배포

GitHub Pages: Settings → Pages → Branch `main` / `/ (root)`.
배포 주소는 `https://streqmin.github.io/cowalk/` 입니다. `file://`로 열면 위치 권한이 동작하지 않는 브라우저가 있으니 로컬 테스트는 간단한 정적 서버(`python -m http.server`)를 권합니다.

## 참고

- 운영시간·전시 정보는 2026년 11월 7일 기준이며 방문 전 확인을 권합니다.
- 장소 데이터는 `index.html`의 `P` 배열에 있습니다. `pid`는 Google Place ID이며, 없는 항목은 이름으로 검색해 사진을 찾습니다.
