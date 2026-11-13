# xiangqi-app-public

바른앱 공방(Bareun App Studio)의 Android 앱 **「Xiangqi & Cờ Úp - Play AI」**(`com.appforbaby.xiangqi`, 베트남어 「Cờ Tướng · Cờ Úp: Đấu máy」, 번체 중국어 「象棋．揭棋 - 人機對弈」)에 들어 있는 오픈소스 엔진 **Fairy-Stockfish**의 대응 소스(GNU GPL-3.0)와 앱의 개인정보처리방침을 공개하는 저장소입니다. 앱 본체 코드는 이 저장소에 없습니다.

This repository provides the GPL-3.0 corresponding source for the Fairy-Stockfish engine bundled in the Android app "Xiangqi & Cờ Úp - Play AI" by Bareun App Studio, plus the app's privacy policy. It does not contain the app's own code.

## 개인정보처리방침 / Privacy policy

- English: https://minimaljjang.github.io/xiangqi-app-public/privacy_en.html
- Tiếng Việt: https://minimaljjang.github.io/xiangqi-app-public/privacy_vi.html
- 繁體中文: https://minimaljjang.github.io/xiangqi-app-public/privacy_zh-Hant.html

## 엔진 / Engine

| 항목 / Item | 값 / Value |
|------|----|
| 상위 프로젝트 / Upstream | https://github.com/fairy-stockfish/Fairy-Stockfish (GPL-3.0) |
| 커밋 / Commit | [`53c1f2bd14a2d3b685904ccf8ec9dec8401800ff`](https://github.com/fairy-stockfish/Fairy-Stockfish/tree/53c1f2bd14a2d3b685904ccf8ec9dec8401800ff) |
| 수정 / Modifications | **없음 / none** (원본 소스 그대로, [`patches/`](patches/README.md) 참고) |
| 소스 / Source | 서브모듈 [`fairy-stockfish/`](fairy-stockfish) (위 커밋 고정) + 같은 커밋의 소스 아카이브 [`source/Fairy-Stockfish-53c1f2bd….zip`](source/) (SHA-256은 같은 폴더의 `.sha256`) |
| 빌드 스크립트 / Build script | [`tool/build_engine.sh`](tool/build_engine.sh) (앱 저장소에서 쓰는 파일과 같음 / identical to the app's) |
| 평가 방식 / Evaluation | 클래식 평가, NNUE 신경망 파일 없음 (`-DNNUE_EMBEDDING_OFF`) |
| 포함 ABI / ABIs | `arm64-v8a`, `x86_64` |

앱은 엔진을 수정하지 않고 별도 프로세스로 실행하며, UCI 텍스트 프로토콜(표준 입출력)로만 주고받습니다. 앱은 엔진의 `xiangqi` 변형을 사용합니다.
The app runs the unmodified engine as a separate process and talks to it only over UCI (stdin/stdout), using the engine's `xiangqi` variant.

## 앱 버전 ↔ 엔진 대응표 / Version table

| 앱 버전 / App version (versionName+versionCode) | 엔진 커밋 / Engine commit | NDK | `libfairystockfish.so` SHA-256 |
|------|------|------|------|
| 1.0.0+1 | `53c1f2bd14a2d3b685904ccf8ec9dec8401800ff` | r28 (`28.2.13676358`) | arm64-v8a `b1767b868d0a4ef20320d564911dcc7912d78ba59aff3f0f41844c9fc01097f3`<br>x86_64 `a0f41ac31a049b83857ab91163898cd508b1ea01091162c66d30b34e2042dd44` |

엔진이 바뀌는 앱 업데이트는 새 소스와 이 표를 먼저 올린 뒤에 배포합니다.
App updates that change the engine are released only after the new source and this table are published here.

## 빌드 방법 / How to build

필요한 것 / Requirements: Android NDK r28 (`28.2.13676358`), Bash (Linux, macOS or Windows Git Bash), git.

```bash
git clone https://github.com/minimaljjang/xiangqi-app-public.git
cd xiangqi-app-public
bash tool/build_engine.sh /path/to/android-ndk-r28
```

- 스크립트는 상위 저장소를 `../_engine_src/fsf`에 받아 위 커밋으로 체크아웃합니다(위치는 `FSF_WORK_DIR`로 바꿀 수 있음). 상위 저장소를 쓸 수 없으면 `source/`의 zip을 풀고, 스크립트의 `SRC`를 풀린 폴더의 `src`로 바꾼 뒤 스크립트 끝의 컴파일 부분만 실행하면 같은 결과물이 나옵니다. 서브모듈(`git submodule update --init`)로 받은 `fairy-stockfish/src`도 같은 소스입니다.
- The script fetches upstream into `../_engine_src/fsf` (override with `FSF_WORK_DIR`) and checks out the commit above. Without network access, unzip the archive in `source/`, point `SRC` at its `src` folder and run only the compile steps at the end of the script.
- 결과물 / Output: `android/app/src/main/jniLibs/{arm64-v8a,x86_64}/libfairystockfish.so` (실행 파일을 Android JNI 라이브러리 이름으로 포장한 것 / the engine executable packaged under a JNI library name).
- 컴파일 옵션 / Flags: `-std=c++17 -O3 -DNDEBUG -fno-exceptions -fPIE -stdlib=libc++ -DIS_64BIT -DUSE_POPCNT -DLARGEBOARDS -DPRECOMPUTED_MAGICS -DNNUE_EMBEDDING_OFF`, arm64 `-DUSE_NEON`, x86_64 `-msse4.1 -mpopcnt`, API 21, `-static-libstdc++`, then `llvm-strip`.

## 라이선스 / License

- Fairy-Stockfish와 이 저장소의 빌드 스크립트 / Fairy-Stockfish and the build script: GNU General Public License v3.0 ([`LICENSE`](LICENSE)).
- `docs/`의 개인정보처리방침 문서는 앱 안내 문서입니다. / The privacy policy pages in `docs/` are app documentation.

## 문의 / Contact

소스 요청이나 문의 / Source requests and questions: ecosketch21@gmail.com (바른앱 공방 / Bareun App Studio)
