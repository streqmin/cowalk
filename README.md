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

API 키는 저장소에 커밋하지 않고 **GitHub Secret**에 두며, Actions가 배포할 때 `index.html`의
`__GMAPS_KEY__` 자리표시자에 주입합니다 (`.github/workflows/deploy.yml`).

1. [Google Cloud 콘솔](https://console.cloud.google.com/)에서 프로젝트를 만들고 다음 API를 사용 설정합니다.
   - Maps JavaScript API
   - Places API
   - Directions API
2. API 키를 만들고 **애플리케이션 제한 → HTTP 리퍼러**에 `https://streqmin.github.io/*` 를 넣고,
   **API 제한**에서 위 세 API만 허용합니다.
3. 저장소 **Settings → Secrets and variables → Actions → New repository secret**
   - Name: `GMAPS_KEY`
   - Secret: 발급받은 키
4. 저장소 **Settings → Pages → Build and deployment → Source: GitHub Actions** 로 바꿉니다.
5. `main`에 푸시하거나 Actions 탭에서 `Deploy to GitHub Pages`를 수동 실행하면
   `https://streqmin.github.io/cowalk/` 에 배포됩니다.

> 정적 페이지 특성상 배포된 HTML에는 키가 포함됩니다. Secret은 "저장소 소스에 키를 남기지 않는" 용도이고,
> 실제 오남용 방지는 2번의 리퍼러·API 제한이 담당합니다.

Secret이 없으면 지도 영역에 안내만 표시되고, 카드의 「Google 지도 앱」·「사진 보기」 링크는 그대로 작동합니다.
Places 사진 요청을 끄려면 `index.html`의 `PHOTOS_FROM_PLACES = false`.

### 로컬 테스트

```bash
GMAPS_KEY=발급키 python3 -c "import os,pathlib;p=pathlib.Path('index.html');pathlib.Path('local.html').write_text(p.read_text().replace('__GMAPS_KEY__',os.environ['GMAPS_KEY']))"
python3 -m http.server 8000   # http://localhost:8000/local.html
```

`local.html`은 `.gitignore`에 포함되어 커밋되지 않습니다.

## 배포

GitHub Actions(`deploy.yml`)가 `main` 푸시마다 자동 배포합니다. Pages Source는 **GitHub Actions**여야 합니다.

## 참고

- 운영시간·전시 정보는 2026년 11월 7일 기준이며 방문 전 확인을 권합니다.
- 장소 데이터는 `index.html`의 `P` 배열에 있습니다. `pid`는 Google Place ID이며, 없는 항목은 이름으로 검색해 사진을 찾습니다.
