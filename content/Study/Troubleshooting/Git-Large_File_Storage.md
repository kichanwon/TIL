---
tags:
  - Git
draft: false
---
## GitHub 대용량 파일 Push 오류
>GitHub은 저장소에 허용되는 파일 크기를 제한
>이 제한을 초과하는 파일을 추적하려면 Git Large File Storage를 사용하여 추적 가능
---
### _문제 상황_
- `git push origin main` 수행 시
``` git-error
remote: error: File [File_name] is 175.63 MB; 
this exceeds GitHub's file size limit of 100.00 MB 

remote: error: GH001: Large files detected. 
You may want to try Git Large File Storage - https://git-lfs.github.com. 
To github.com:[git_repo].git !

[remote rejected] main -> main (pre-receive hook declined) 
error: failed to push some refs to 'github.com:[git_repo].git'
```

---
### _문제 원인_
GitHub push 용량 
- 단일 push 당 최대 **2GB**
- 개별 파일은 **100MB** 이상이면 reject

---
### _문제 해결_
1. Git LFS 사용
2. `.gitignore` 추가
3. [large object promisor](https://digitalbourgeois.tistory.com/1802)

`.gitignore`에 추가
\> 멍청하게 commit을 삭제하는 것 까먹음
\> 계속 push를 시도하다가 결국 Git LFS를 사용

#### **[[Git LFS]]**
Git LFS는 저장소에 있는 파일에 대한 참조를 저장하여 대용량 파일을 처리하지만 실제 파일 자체는 저장하지 않음

Git 아키텍처를 해결하기 위해 Git LFS는 실제 파일(다른 곳에 저장됨)에 대한 참조 역할을 하는 포인터 파일을 생성

GitHub은 저장소에서 이 포인터 파일을 관리
저장소를 복제하면 GitHub은 포인터 파일을 지도처럼 사용하여 큰 파일을 찾음

GitHub 플랜에 따라 Git LFS의 최대 크기 제한

| Product                 | Maximum file size |
| ----------------------- | ----------------- |
| GitHub Free             | 2 GB              |
| GitHub Pro              | 2 GB              |
| GitHub Team             | 4 GB              |
| GitHub Enterprise Cloud | 5 GB              |
Settings > Billing and licensing > Overview > Git LFS 사용량 확인 가능
- Git LFS bandwidth
	- 전송 한도 10 GB included (외부에서 LFS 파일을 다운로드한 누적 트래픽 )
- Git LFS storage
	- 저장 한도10 GB included (Git LFS 서버에 업로드된 실제 파일 크기)
