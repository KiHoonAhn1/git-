### 상황 1. fast-foward

> fast-foward는 feature 브랜치 생성된 이후 master 브랜치에 변경 사항이 없는 상황

1. feature/test branch 생성 및 이동

   ```bash
   $ git checkout -b feature/test
   (feature/test)
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch test.html
   $ git add .
   $ git commit -m 'Complete Test'
   ```


3. master 이동

   ```bash
   git checkout master
   ```


4. master에 병합

   ```bash
   $ git merge feature/test
   Updating a42dc7a..e3f6b3b
   Fast-forward
    README.md | 14 +++++++++++++-
    branch.md | 18 ++++++++++++------
    test.html |  0
    3 files changed, 25 insertions(+), 7 deletions(-)
    create mode 100644 test.html
   ```


5. 결과 -> fast-foward (단순히 HEAD를 이동)

   ```bash
   $ git log --oneline
   e3f6b3b (HEAD -> master, feature/test) test
   bec7a8e Complete Test
   a42dc7a (test2) Complete Test2
   0679c0b init
   ```

6. branch 삭제

   ```bash
   $ git branch -d feature/test
   Deleted branch feature/test (was e3f6b3b).
   ```



---

### 상황 2. merge commit

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 다른 파일이 수정되어 있는 상황
>
> git이 auto merging을 진행하고, commit이 발생된다.

1. feature/signout branch 생성 및 이동

   ```bash
   $ git checkout -b feature/signout
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch signout.html
   $ git add .
   $ git commit -m 'Complete signout'
   $ git log --oneline
   a5ac9a6 (HEAD -> feature/signout) Complete signout
   e3f6b3b (master) test
   bec7a8e Complete Test
   a42dc7a (test2) Complete Test2
   0679c0b init
   ```

3. master 이동

   ```bash
   $ git checkout master
   $ git log --oneline
   ```

4. *master에 추가 commit 이 발생시키기!!*

   * **다른 파일을 수정 혹은 생성하세요!**

   

5. master에 병합

   ```bash
   $ git merge feature/signout
   ```

6. 결과 -> 자동으로 *merge commit 발생*

   * vim 편집기 화면이 나타납니다.
   
   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
      * `w` : write
      * `q` : quit
      
   * 커밋이  확인 해봅시다.
   
7. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   ```

8. branch 삭제

   ```bash
   $ git branch -d feature/signout
   ```
   
   

---

### 상황 3. merge commit 충돌

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 동일 파일이 수정되어 있는 상황
>
> git이 auto merging을 하지 못하고, 해당 파일의 위치에 라벨링을 해준다.
>
> 원하는 형태의 코드로 직접 수정을 하고 merge commit을 발생 시켜야 한다.

1. feature/board branch 생성 및 이동

   ```bash
   $ git checkout -b feature/board
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch board.html
   # README.md 파일을 열어서 자유롭게 수정
   $ git add .
   $ git commit -m 'Complete board & Readme'
   $ git log --oneline
   ```


3. master 이동

   ```bash
   $ git checkout master
   ```


4. *master에 추가 commit 이 발생시키기!!*

   * **동일 파일을 수정 혹은 생성하세요!**
   
5. master에 병합

   ```bash
   $ git merge feature/board
   Removing branch.md
   Auto-merging README.md
   CONFLICT (content): Merge conflict in README.md
   Automatic merge failed; fix conflicts and then commit the result.
   ```


6. 결과 -> *merge conflict발생*

   ```bash
   $ git status
   On branch master
   # 병합되지 않은 파일들이 있습니다.
   You have unmerged paths.
   # 충돌을 고치고, commit 하세요.
     (fix conflicts and run "git commit")
   # 병합을 취소하려면
     (use "git merge --abort" to abort the merge)
   # staging area
   # -> 충돌나지 않은 파일
   Changes to be committed:
           new file:   board.html
           deleted:    branch.md
   # 유사 working directory...
   # -> 충돌난 파일
   Unmerged paths:
   	#해결하고 add하세요!!!
     (use "git add <file>..." to mark resolution)
           both modified:   README.md
   ```


7. 충돌 확인 및 해결

   ```bash
   # vscode로 해결하고,
   $ git add .
   ```


8. merge commit 진행

    ```bash
    $ git commit
    ```

   * vim 편집기 화면이 나타납니다.
   
   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
      * `w` : write
      * `q` : quit
      
   * 커밋이  확인 해봅시다.
   
9. 그래프 확인하기

    ```bash
   $ git log --oneline --graph
   *   6e2c4cb (HEAD -> master) Merge branch 'feature/board' into master
   |\
   | * 02d7a17 (feature/board) com
   | * 569c301 complete board&readme
   * | c60d630 master
   |/
   * d4bbd44 Complete signout
   * a5ac9a6 Complete signout
   * e3f6b3b test
   * bec7a8e Complete Test
   * a42dc7a (test2) Complete Test2
   * 0679c0b init
    ```


10. branch 삭제

    ```bash
    $ git branch -d feature/board
    Deleted branch feature/board (was 02d7a17).
    ```
    
    
