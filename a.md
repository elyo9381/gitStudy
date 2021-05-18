## 왜 add를 사용하는가?
 - 특정한 원하는 파일만 저장하기 위해서이다.

## log & diff
    - git log 를 진행하면 역사를 볼수있다. 
    - git log - p : 최신 커밋과 이전커밋의 차이점을 볼수있다.
     - +++는 추가된 내용이며, ---는 이전 버전의 내용이다. 
     - git log에는 커밋it가 나와있다. 
 
   - git diff 커밋id, 커밋id : 커밋들의 차이를 보여준다. 소스코드의 차이점을 보여줌
   - git diff : 현재 내가 작업한(수정한) 내용을 볼수있다. 이전코드와 수정한 코드의 차이를 볼수있다.

## Commit cancel
    - reset : 특정 커밋을 최신상태로 하고싶을때 -> git reset 커밋id --hard
      - 리셋은 공유환경에서 사용하면 안된다. 
      - --hard : 

    - revert : 특정 커밋을 취소하고 해당 커밋을 새로운 버전으로 생성한다.
  
## git add 원리
    - git의 파일의 이름은 index에 담겨있고 파일의 내용은 objects파일에 들어있다.
    - index, objects 파일은 .git 폴더안에 들어있다.
    - git은 파일의 이름이 다르더라도 내용이 같다면 같은 인덱스를 가진다. 그리고 이 인덱스는 같은 오브젝트를 가르킨다.
    - git은 해쉬값 + 몇가지 부가적인 값을 통해서 add 를 진행하고 커밋을 객체를 만든다.

## git commit 원리
    - 커밋파일에는 커밋 메시지, tree , 사용자정보, parent 가 들어있다. 
    - tree는 우리가 작성한 파일의 이름이 존재한다. 각각의 버전마다 tree의 값이 다르다. 
      - 이를 스냅샷을 찍었다라고 한다. 
    - parent는 이전 커밋을 의미한다. 

    - 중요정보 parent, tree의 관계가 중요하다. 

## index 파일 지레짐작
    - status의 상태를 어떻게 알까?
    - object와 index의 내용이 같으면 status가 같다고 보는것같다. 
    - 이는 해쉬값을 통해서 비교하는것 같다. 인덱스, 트리, 그리고 local데이터를 비교하여 다르면 status를 바꾼다.


## git branch
    - git branch : 브랜치를 조회할수있는 명령어
    - git branch filename : 브랜치 생성
    - git checkout filename : 브랜치 이동

    - git log --branches --decorate : 모든 브랜치를 볼수있는 명령어
    - git log --branches --decorate --graph : 그래프 기능이 추가된다. 
    - git log --branches --decorate --graph --oneline : 한눈에 보기 싶다. 

    - git log master..exp : 마스터에는 없고 exp에는 존재하는 커밋을 보여줌
    - git log exp..master -p : exp에는 없고 마스터에는 존재하는것 그리고 -p를 통해서 상세하게 비교해준다. 

    - git diff master..exp : exp에 추가적인 정보가 어떤게 있는지 비교하여 보여준다.
    
## git Merge
    - 마스터로 머지하려면 마스터로 checkout해야한다. 
    - git merge exp : 현재 브랜치에 exp를 머지한다.
    - git merge master : exp의 브랜치를 master로 시점을 맞춰주는 역할을한다.

    만약에 병합이 잘되었다면 기존의 exp branch는 삭제 해도된다. 
    - git branch - d exp : 브런치를 삭제하는 명령어


## git 연습
    - git tutorial 을 통해서 연습하자. 
    
    마스터 브랜치가 존재하고 이에 따른 커밋을 3번하였다.

    이때 새로운 브런치를 생성한다. 
    ```
    git checkout -b issue53; 
    ```

    이렇게 되면git은 두가지의 브랜치를 갖는다. 그리고 브런치를 커밋하면 브랜치는 새로운 브랜치를 가르킨다. 

    이때 또다른 B 브랜치를 생성하고 이를 마스터 브랜치에 머지한다. 

    이럴때 FastForward가 발생한다. 
    FastForward : B 브랜치를 마스터에 병합할때 마스터가 B 브런치를 가르키게 하는 역할을 한다. 
     - 즉 빨리감기를 한다. 

    그리고 마지막 브런치 A(issue53)을 마스터에 병합할때 merge recursion이 발생한다. 

    이때 깃은 마스터와 A의 공통의 parent를 찾고 후에 3wayMerge방법을 이용해서 브랜치들을 병합하고 
    별도의 커밋을 생성한다. 그리고 새로운 커밋을 마스터가 가르킨다.


## git stash
    - 작업이 끝나지 않았을때 다른 브랜치로 이동해서 급하게 일을 수행하는경우 사용한다. 
    - 브랜치를 특정 공간에 숨길수있다. 그리고 새로운 업무를 진행하고 다시 숨긴 브랜치를 사용할수있는것이다. 

## git branch 의 원리
    - git을 init하면 head 파일이 생성된다. 
    - 그리고 커밋을 하면 refs/heads/master 가 생성된다. 이는 가장 최신 커밋을 가르킨다.
    
    - git의 head는 최신 커밋을 가리키고 이는 object파일의 내용을 알수있다. 또한 object,tree,index 등의 해쉬값을 통해서 최신 커밋의 상태또한 알수있다. 
    이전커밋은 parent를 통해서 이전 커밋도 알수잇다. 

    - 브랜치를 생성하면 ./.git/refs/heads/exp가 생성된다. 이는 일반 텍스트 파일이다. 

    - HEAD의 역할을 checkout을 한 최신 커밋을 가르킨다.

## git conflict
    - 마스터와 브랜치가 하나의 파일의 같은 라인소스를 수정할때 충돌이 일어난다. 
    - 같은 코드의 다른 라인을수정한다면 이는 문제가 되지 않고 git이 recursionMerge한다. 

    ```
     <<<<<<<<<<
     function(master){
     ========
     function(exp){
     >>>>>>>>>>
    ```

    - == 는 코드의 다른 부분을 보여주는 분기이다. 
    - 충돌해결은 재주것 해야한다. 리팩토링의 관점에서 

## git reset checkout 원리
    - git reset --hard 커밋id : HEAD이 커밋ID를 보게 되고 이는 최신커밋ID 바꾸는 행위이다. 
    - git은 rest을 하여도 삭제된것은 아니다. resert을 취소할수도 있기 떄문이다. 

    - git log --hard ORIG_HEAD : reset을 취소할수있다. 

    - git reflog : 각각의 로그를 볼수있다. 

    - --hard, --soft, --mixed 개념 알기 
      1. working directory(workgin tree, working copy)  
      2. index(stating area,cache) 
      3. repository
      soft,mixed,hard는 각각 초기화되는 부분이 다르다.   
      soft(3), mixed(2,3), hard(1,2,3)의 범위를 갖는다.    
      
    - soft : 레포지토리의 영역만 초기화된다.
    - index : add영역(index)의 영역 + 레포지토리 초기화된다.
    - hard : local(working)까지 모든 영역을 초기화한다.

    - ORIG_HEAD : reset을 취소할때도 hard, soft, mixed를 설정해야한다.

## conflict 원리
    - 3way merge 를 통해서 오토 커밋이 가능하다. 
      - 3Way merge : base,a,b 셋을 고려해서 병합하는 방법
      - 2way merge : a,b (branch)를 둘을 이용해서 병합하는 방법

      3Way는 4가지 출돌 경우의 수가 존재한다.
        1. a,base가 같은 코드이고 b가 수정했을때 B로 병합된다.
        2. a,base,b가 모두 같은 코드를 가르킬때 같은 데이터로 병합된다.
        3. base와 a,b가 모두 다를때 충돌이 발생한다.
        4. b,base가 같고 a가 코드를 수정했을때 a의 코드로 병합된다. 

        3Way는 모두 다를때만 충돌 나고 ,2way는 같을때 뺴고는 모두 충돌난다. 

    - 병합을 전문적으로 하는 kdiff3가 존재한다. intellij, sourtree 등 다양하다. 

    - 충돌이 났을때는 base, local, remote 가 존재한다. 
      - base는 common파일이며, local,remote는 같은코드를 접근한 파일이다. 


## remote repository
    - git init --bare : 저장소로서의 기능만 수능하는 init bare는 불변의 특징을 갖게하는것이다. 
    
    - git remote add 경로 : 현재 디렉터리에 원격저장소 경로를 연결한다
    - git push  : 원격 저장소에 데이터를 푸시(보낸다)
      - --set-upstream origin master : 마스터에서 푸시하면 자동으로 오리진 마스터에 푸시하겠다는 뜻
        : 로컬브랜치와 원격브랜치사이의 명시적인 설정 set-upstream 
    
    - fork : fork를 하면 내가 특정 프로젝트를 내컴퓨터에서 사용할수있다. (복제)
      : 원격저장소에 복사된다. 
    - git clone : 특정 원격 레포지토리를 다운로드한다. 
    
## 원격 저장소의 원리 
    - 로컬 master를 원격 저장소의 마스터에 연결해야한다. 
    - remote를 하면 원격저장소를 위한 폴더(파일)이 생긴다. 
    - 그리고 push --set-upstream origin master 를 하게 되면 각각 로컬과 원격저장소를 가르키는 파일들이 같은 커밋을 가르키게 된다. 
    - 원격의 오리진과 지역의 마스터,HEAD르가 다를때 master를 orgin master에 push 하면 같은 커밋을 가르키게 되는것이다. 

## git pull 과 fetch의 차이
    - pull : 원격과 로컬의 데이터가 차이가 없게 다운한다. HEAD, origin/master 가 같은 커밋을 가르킨다. 
      다운받고병합까지 할것이면 pull!!
    - fetch : 원격저장소가 로컬저장소를 앞서게 된다. 
      이말은 무엇인가? : 원격저장소와 지역 저장소의 차이를 확인할수있다.    
      git diff등을 통하여 ex) git diff HEAD origin/master   
      그리고 이를 병합할수있다. git merge origin/master 같은 커밋을 가르키도록 병합할수있다.
      소스코드는 다운받지 않지만 차이점을 확인할때 fetch!!!


## git tag 
    branch와 비슷하지만 다르다. 

    releases : 사용자들에게 제공되어도 되는 각각의 버전을 뜻한다. 
    tab : 특정 커밋을 사용자가 다운로드하거나 기억할수있도록 하는것
    
    - git tag 1.0.0 커밋id or 브랜치이름-> checkout을 통해서 특정 버전으로 갈수있다. 
    - git tag - a 1.1.0 -m "bug fix" : annotated 를 설정할수잇다.  -a
      tag에 상세한 주석을 첨부할수있다.

    - 원격 저장소에tag를 올릴수도 있다. git push --tag 
    - tag 삭제 방법 : git tag -d 버전 

## git tag 원리
    tag를 생성하면 refs/tags/tag이름 파일이 생성된다.    
    즉 tag는 텍스트 파일이다.   

    이를 원격저장소 또는 로컬의 git에서 내부적으로 적용되는 방법은 workingtree, index, repository의 원리에 의해서 작동된다. 

## git rebase
    -rebase : 소스코드를 병합하는 하나의 방법이다.
      1. merge 병렬적으로 병합하지만 rebase는 임시저장소를 이용해 직렬적으로 병합한다. 
      2. 공통 base를 토대로 브런치를 직렬화하면서 병합한다. 

    - git rebase branchName
    - git rebase continue

## git flow
    - master, develop, release, feature, hotfix 의 흐름으로 개발이 진행되어야한다. 
    
    1. master : 마스터브랜치를 뜻하고 이는 최종 버전을 의미한다.
    2. develop : master에서 브랜치를생성해서 develop에서 개발을 진행한다. 
      - 
    3. feature : 기능을 담당하는 브랜치를 develop에서 생성한다.
      - feature 가능하다면 자주 develop에서 pull한다 (충돌을 방지하기 위해서) 
      - feature의 기능이 완벽할때 develop에 병합한다. 
    4. release : 어떤 특정한 기능이 생성되고 이를 사용자에게 제공할때 release한다
      - ex) 새로운 기능이 추가된 x.0.1버전 등
      - 릴리즈 생성시 바로 master와 develop에 병합한다.
    5. hotfix : 긴급한 버그가 발생하였을때 hotfix 를 생성하고 여기서 해결한다.
      - 해결과 동시에 master와 develop에 병합한다. 

    충돌을 줄이는 가장 좋은 방법은 규칙을 준수하며 자주 push/pull을 진행하는것이다. 
    또한 공통 소스는 주석을 부여하고 이를 손대지 않는것이 중요하다.
    
   *git flow 연습*
   ```
    * 30fd3cf (HEAD -> master, origin/master, hotfix-some, develop) add hofix
    * 23be14b (tag: 1.0, release-1.0) add release-1.0 doc
    * f9135be (feature-some) fearture some
    * f6ed002 create branch deveop flow
    * 838e5cc (origin/dev) create branch dev & README.md
    * 1281a31 branch test & git concept sutdy
    *   062faf7 master commit 작성
    |\
    | * 39aedf6 branch에 commit 작성
    * | 60e438f master commit 작성
    |/
    * 660fd11 stduy git 원리
    * 493ee0c 3
    * a7572e2 2
    * ab83f89 1
   ```
