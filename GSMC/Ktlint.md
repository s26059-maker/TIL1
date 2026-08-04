# Ktlint

코틀린 코드 스타일을 자동으로 통일시켜주는 도구. 사람마다 중괄호 위치, 들여쓰기 스타일이 다르면 코드 읽기도 힘들고 Git conflict도 잦아짐. 그래서 스타일을 기계적으로 맞춰주는 lint(코드 스캔 도구)가 필요한데, 코틀린에서 제일 많이 쓰는 게 Ktlint.

## Gradle 적용 방법 2가지

- **Plugin 사용** (JLLeitschuh/ktlint-gradle 등): build.gradle plugins 블록에 한 줄만 추가하면 끝
- **Jar 직접 사용**: plugin 없이 ktlint jar만 받아서 Gradle task를 직접 만들어 실행

## .editorconfig

Ktlint가 검사 기준으로 삼는 설정 파일. 기본으로 안 만들어지니 프로젝트 root에 직접 생성.

```
root = true

[*]
charset = utf-8
end_of_line = lf
indent_size = 4
max_line_length = 120

[*.{kt,kts}]
disabled_rules=import-ordering
```

`disabled_rules`로 규칙을 끌 수 있음. import-ordering을 끄는 이유: IntelliJ의 자동 import 정렬 기능이 `java.*` 패키지는 ktlint가 원하는 순서대로 못 맞춰서 도구끼리 충돌하기 때문.

낮은 버전 ktlint는 .editorconfig 자체를 지원 안 할 수도 있음. 옵션이 안 먹으면 버전 문제 의심.

## ktlintCheck vs ktlintFormat

- `./gradlew ktlintCheck` → 검사만, 안 고쳐줌
- `./gradlew ktlintFormat` → 자동으로 스타일 고쳐줌

인텔리제이 우측 Gradle 탭에서도 바로 실행 가능.

## 빌드 실패 여부 설정

기본은 검사 실패하면 build도 실패. 봐주고 싶으면:

```kotlin
configure<org.jlleitschuh.gradle.ktlint.KtlintExtension> {
    ignoreFailure.set(true)
    debug.set(true) // 에러 메시지 자세히 출력
}
```

ignoreFailure 켜도 Gradle이 몇몇 ktlint task를 build에 물고 있는 경우가 있어서, 안 통과되면 task를 하나씩 찾아서 꺼줘야 할 수도 있음.

## Pre-Commit Hook

ktlint를 쓰는 이유 자체가 "여럿이 같이 쓸 때 스타일 강제하려고"인데, 각자 알아서 돌리게 하면 의미가 없음. 그래서 커밋 직전에 자동으로 검사하고, 실패하면 커밋을 막는 방법.

Git에는 커밋 전/후 시점마다 실행되는 스크립트(Git Hook)가 있고, 그중 pre-commit 훅에 아래를 넣으면 됨.

```sh
#!/bin/sh
./gradlew ktlintCheck
```

팀원 전부가 로컬에 직접 설치하라고 하면 번거로우니, Gradle task로 만들어서 build할 때 자동으로 `.git/hooks/pre-commit` 파일을 생성하도록 설정.

```kotlin
tasks.named("build").configure {
    dependsOn("installKtlintGitPreCommitHook")
}
```

## Github Actions

로컬 훅은 개인 컴퓨터에 의존하는 거라 100% 강제는 안 됨 (안 걸고 push 해버릴 수도 있음). 그래서 PR 생성 시 자동으로 검사하는 최종 방어선을 하나 더 걸어둠.

```yaml
on:
  pull_request:
    branches: [ develop, master ]

jobs:
  lint:
    steps:
      - uses: actions/checkout@v3
      - name: Set up Java (JDK 17)
        uses: actions/setup-java@v3
        with:
          distribution: corretto
          java-version: 17
      - run: ./gradlew ktlintCheck
```

로컬 pre-commit hook = 1차 방어선, Github Actions = 최종 방어선. 이중으로 걸어두면 스타일 안 맞는 코드가 develop/master로 들어갈 일이 거의 없음.

## 인텔리제이 Save Actions 플러그인

저장할 때마다 자동으로 스타일을 정리해주는 보조 플러그인. ktlint 자체 기능은 아니고, ktlintFormat을 매번 수동으로 안 돌려도 되게 해주는 편의 도구.

설치: Settings → Plugins → "Save Actions" 검색 → 설치 → 재시작 → Settings → Other Settings → Save Actions

체크할 옵션:
- Activate save actions on save
- Optimize imports
- Reformat file (프로젝트 전체 스타일이 이미 통일돼 있을 때)
- Reformat only changed code (스타일이 아직 안 맞아서, 전체를 고치면 diff가 지저분해질 때)