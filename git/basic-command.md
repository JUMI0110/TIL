# Git command

> git 기본 명령어

## init
- 현재 위치에 `.git` 폴더 생성 명령어

```bash
git init
```
관리하고자 하는 폴더에만 `.git` 생성 폴더별로 생성시 에러 발생

## add
- working directory -> staging area

```bash
git add .
```

## status
- 현재 git 상태 확인

```bash
git status
```

## commit
- staging area 에 올라간 내용 스냅샷
    - `-m` 옵션을 통해 커밋메시지 바로 입력 가능

```bash
git commit -m(현재 상황에 대한 메모옵션) "first commit(코드의 변화 단계)"
```

## remote add

- 원격저장소 주소를 저장하는 명령어

```bash
git remote add {remote_name} {remote_url}
git remote add origin https://github...
```

## push

- 원격저장소로 branch를 업로드하는 명령어

```bash
git push {remote_name} {branch_name}
git push origin master
```