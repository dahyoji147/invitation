---
name: commit-push
description: 변경 사항을 lint·build로 확인한 뒤 커밋하고 invitation 저장소에 푸시한다. 사용자가 "커밋해줘", "푸시해줘", "깃에 올려줘"라고 할 때 사용한다.
disable-model-invocation: true
---

# 커밋 & 푸시

사용자가 요청했을 때만 실행한다. 코딩 초보자가 쓰므로 각 단계의 결과를 쉬운 말로 알려준다.

## 순서

1. **현재 위치 확인**
   - `git rev-parse --show-toplevel`이 Invitation 폴더인지 확인한다. 상위 폴더(work-experiments)의 저장소이면 중단하고 사용자에게 알린다.
   - `git remote -v`의 `origin`이 `https://github.com/dahyoji147/invitation.git`인지 확인한다. 다르면 푸시하지 말고 물어본다.
2. **변경 내용 확인**
   - `git status --short`와 `git diff --stat`으로 바뀐 파일을 보여준다.
   - `.env` 등 비밀 값 파일이나 `node_modules`, `.next`가 포함돼 있으면 제외하고 알린다.
3. **검사**
   - `npm run lint`와 `npm run build`를 실행한다.
   - 실패하면 결과를 그대로 알리고 커밋하지 않는다. 고칠지 사용자에게 먼저 묻는다.
4. **커밋**
   - 변경된 파일을 이름으로 지정해 `git add` 한다. (`git add -A`는 확인한 뒤에만 쓴다)
   - 커밋 메시지는 한국어로, 무엇을 왜 바꿨는지 한 줄 요약으로 쓴다.
   - 메시지 끝에 현재 세션이 안내하는 Co-Authored-By 줄을 붙인다.
5. **푸시**
   - `git push origin main`을 실행한다. 강제 푸시(`--force`)는 하지 않는다.
   - 푸시가 거절되면 원인을 설명하고 사용자 확인 없이 `pull`이나 `reset`을 하지 않는다.
6. **보고**
   - 커밋 해시, 올라간 파일 수, 푸시 결과를 짧게 알린다.

## 하지 않는 것

- 요청 없이 커밋하거나 푸시하지 않는다.
- `git reset --hard`, `--force`, `--no-verify`를 쓰지 않는다.
- 변경 사항이 없으면 빈 커밋을 만들지 않고 "올릴 변경이 없다"고 알린다.
