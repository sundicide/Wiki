# Oreilly - Linux Fundamentals

## Basics

`CTRL + L` => Clean terminal

```bash
whoami # 로그인 정보

hostname --help | less # less를 써서 page by page로 볼 수 있다.
hostname -I # all addresses for the host

date # 현재 날짜와 시간, 타임존

passwd # change password
su - # su는 switch user인데 뒤에 user name을 넣어야 하는데 이처럼 아무것도 안넣으면 root account로 접속을 시도한다
# - 는 open login shell을 의미한다.
# target user에 full environment가 set된다.
# 그래서 su와 - 는 같이 자주 쓰인다.
sudo -i # 우분투에서는 이렇게 써야 한다.
sudo passwd # root password를 변경할 수 있다.
# sudo는 이번에만 admin권한을 얻는 것

touch hello # write 권한이 있는지를 체크하기 위해 자주 쓰인다.

cd # home dir로 이동

last # 시스템에 마지막으로 로그인한 기록들을 보여준다.
```

## Man

man page에서
- `[]`: 안에 있는 것은 optional이라는 것이다
- `a|b`: a와 b 둘 중 하나를 써야 한다는 것이다.
- `...`:  여러 옵션을 사용 할 수 있다는 것이다
- 타이틀에 여러 섹션이 있는데 자주 쓰이는 것은 다음과 같다.
  - 1은 end-user commands
  - 8은 administrator(root) commands
  - 5: configuration file

```bash
man whoami

man lvcreate # 8번 command를 보기 위한 예시.

man -k user # 어떤 man page를 봐야 할지 모르겠다면 키워드 검색을 할 수 있다. -k 옵션으로
man -k user | wc # wc는 word count를 의미한다. 그래서 결과 리스트가 몇 개 인지 볼 수 있다.
# 결과는 221 2230 15109 같이 표시되는데
# 221 line, 2230 words, 15109 characters를 의미한다.
man man # man 사용 법 보는 법
man -k user | grep 8 # 결과 리스트에서 8 섹션만 filtering하고 싶을 때
# grep은 filtering utility이다.
man -k user | grep 8 | grep create # 두 번 필터링 적용

sudo mandb # man에는 db가 있는데 이를 업데이트하기 위한 커맨드

apropos man # apropos는 man -k를 한 번에 쓰는 효과이다.
```

man 페이지에서 **bold**로 그려지는 것들은 man page에 항목이 존재한다는 의미이다.

## info, pinfo

`man whoami`를 쳐보면 맨 아래에 전체 정보를 얻기 위해서는 아래와 같은 명령어를 수행하라고 나와 있다.
```bash
info '(coreutils) whoami invocation'
```

역사적으로 일부 명령어들은 man에 또 일부는 info에 document 되었다.
그래서 이런 일이 생긴 것이다.

근데 info 보다는 pinfo 사용을 권한다.

pinfo에서 페이지를 볼 때 단축키가 있는데  `U`는 go up, `N`은 next, `P`는 previous이다

`pinfo -a whomi`를 치면 바로 whoami 페이지로 들어간다.

하지만 대부분의 커맨드들은 `man` 페이지에 있어서 info류를 쓸 일은 잘 없다.

```bash
man --help # short info
```

/usr/share/doc에는 커맨드들에 대한 추가 정보가 있다.
해당 폴더에 들어가면 여러 패키지들이 있는데 해당 패키지 폴더에 들어가면 여러 파일이 있고 추가적인 정보를 얻을 수 있다.

## Overview: Help Systems in Linux
1. man
2. info, pinfo
3. {command} —help
4. /usr/share/doc


# Lesson3: Essential File Management Tools

## Understanding the Linux FIle System Hierarchy
모든 Linux file system은 `/`로 시작한다.
`/`는 root dir이며 모든 것들의 시작 점이다.

하위에는 여러 것들이 있는데
- bin: binaries
- home: home dir
- var: log files 등등
- usr: user, program files

Linux File system에서 중요한 컨셉 중에 하나는 mount이다.
mount: storage device를 separate dir에 붙이는 것을 의미한다.

mount를 하면 특정 폴더를 해당 storage에 넣을 수 있다.
storage를 root 폴더에 넣기 위해서는 mount를 한다.
mount는 network를 통해서도 가능하다.(NFS 등등)

`/`내 폴더들을 좀 더 자세하게 살펴보면
binary files는 `bin`과 `sbin`에 들어가는데 bin은 `regular binaries`, sbin은 `system binaries`가 들어간다. 그래서 sbin은 root command 들, bin은 regular user commands가 들어간다.

`/boot`는 booting을 위해 쓰인다. 폴더 내부를 보면 linux kernel 파일(vmlinuz~~)이 존재하는데 linux system의 심장에 해당한다.

`/dev`는 device를 위해 쓰이며 hardware에 access를 할 수 있게 해준다. sda는 hard disk이고 sda1, sda2 등으로 hard disk partition이 보인다.

`/etc`는 configuration file들이 들어 있다. 내부에는 대게 볼 수 있는 파일들이 있는데 `os-release` 파일을 보면 Linux service에 존재하는 current version에 대한 설명들이 들어 있다.
`passwd` 파일에는 등록된 유저들에 대한 정보가 담겨있다.

`/home`은 user별 home dir이 들어 있다.

`/media`와 `/mnt`는 일반적으론 쓰이지 않는다. mount를 위해 쓰인다.

`/opt`는 application을 위해 쓰이는데 종종 비어 있다.

`/proc`은 linux kernel 에 대한 정보가 들어 있다. linux 시스템에 대한 자세한 정보를 알고 싶을 때 볼 수 있다. 예를 들면 **cpuinfo** 를 보면 cpu에 대한 자세한 정보가 들어 있다. **meminfo** memory information을 볼 수 있다.

`/root`는 root user의 home dir이다.

`/run`은 new temp dir이다.

`/srv`는 보통 비어 있다. services라는 의미인데 service를 위한 document roots로 사용할 수 있다.

`/sys`는 hardware information이 들어 있다.

`/tmp`는 temp dir인데 old dir이다. reboot할 때마다 지워진다.

`/var` OS에 의해 사용되는 폴더인데 자주 사용되는 폴더는 `/var/log`이다.

## cp

copy

```shell
# cp [source] [target]
cp /etc/hosts .

# copy recursive(with all sub directories)
cp -R /tmp .

# clear current dir
rm -rf *

# destination 이 폴더명인지 아닌지에 따라 결과가 달라진다.
cp /etc/hosts /home/whoever # /home은 폴더이지만 /home안에는 /whoever라는 폴더는 없기에 /etc/hosts 파일은 /home/whoever 라는 파일로 복사된다.
cp /etc/hosts /home/user # dest인 /home/user는 폴더이다. 그렇기 때문에 /etc/hosts 파일은 /home/use/hosts 라는 파일로 복사된다.
# 위와 같은 혼란을 방지하기 위해서 폴더일 경우에는 아래와 같이 뒤에 /를 붙여 주는 것이 안전하다.
cp /etc/hosts /home/user/

# 위와 같은 원리로
# 아래 명령어는 타겟이 올바른 directory가 아니라는 메세지와 함께 실패한다.
cp /etc/hosts /home/nonexists/

# 하지만 아래 명령어는 성공한다. nonexists라는 파일을 새로 생성하는 것이므로
cp /etc/hosts /home/nonexists

# -R 을 통해 모든 하위폴더를 복사할 수 있다.
cp -R /tmp/ .

# 아래와 같은 상황에서는 뒤에 /를 넣거나 안넣거나 차이는 없다.
# 하지만 scp를 사용하게 되면 차이가 있긴 하다.
cp -R /tmp .
```

## Working with Directories

```shell
# 이전 경로로 이동
cd -

# -p 옵션으로 필요 시  parent dir도 만들 수 있다.
mkdir -p files/morefile
```


## Understanding Hard and Symbolic Links
Linux에서는 link가 2가지 있다.

- hard link
- symbolic(=soft) link

inode: 모든 파일은 단 하나의 inode를 갖고 있는데 파일의 정보들을 담고 있으며, `ls -l`로 확인 가능하다.
inode는 block을 가리키는데, block은 disc에 저장된 file information이다.
그리고 사람들은 name으로써 파일을 인식하는데 file name은 inode를 가리킨다.

> name -> inode -> block
> name2 ----|

linux에서는 inode에 하나 이상의 name을 부여할 수 있다. name1, name2와 같이. 그리고 이렇게 부여된 name들을 `hard link`라고 한다. name1과 name2에는 차이가 없고 단지 이름만 다를 뿐이다. 한 쪽을 제거하더라도 다른 한 쪽은 그대로 있다. 그렇기 때문에 name1에 내용을 수정하면 name2에도 반영된다. (같은 inode를 가리키고 있기 때문에)

symbolic link는 이름을 갖는데 name을 가리킨다.(한 단계 더 레벨이 생긴다.)

> sym1 -> name -> inode -> block
> sym2 -----|

그렇기 때문에 hard link(=name)이 사라지게 되면 symbolic link를 사용할 수 없게 된다.

이렇게 보면 Hard Link만 써야 할 것처럼 보이지만, Hard Link에 한계가 있다.
1. no cross device: mount를 했을 때 hard link1은 1번 device에, hard link2는 2번 device에 만드는 것을 할 수 없다. Hard links는 same storage device(same partition)에서만 가능하다.
2. no directories: hard link는 two files에서만 가능하고 two directories는 불가능하다.


```shell
# make hard link
ln /etc/hosts hosts

# 2개의 inode 숫자가 동일함을 볼 수 있다.
# 또한 link count 도 표시되는 것을 볼 수 있다(여기서는 2개이다.)
# 하나 더 link를 만들면 3이 된다.
ls -li /etc/hosts hosts

# make symbolic link
ln -s hosts symlink

# 실험을 위해 symlink를 다른 곳으로 옮기고
mv symlink /tmp
# sym link상태를 확인 해보면 빨간색 글씨로 보이는 것을 알 수 있다.
# 이는 해당 sym link가 unavailable하다는 의미인데
# 위에서 symlink를 만들 때 상대 경로로 만들었기 때문에, 현재 위치에서는 동작하지 않는 이유이다.
# 그래서 symlink를 만들때에는 절대 경로를 사용해야 한다.
ls -ls /tmp/symlink

# ls -l 을 보면 여러 sym link들이 보이는데 경로가 바뀌더라도 파일을 관리하는 곳은 그대로 사용하기 위해 쓴다.
```

## Finding Files with find

```shell
# / 및 하위 폴더에서 "hosts" 란 name을 갖는 것을 찾는다.
find / -name "hosts"

# / 및 하위 폴더에서 "host" 란 name을 갖는 것을 찾는다.
# host란 정확한 단어를 찾기에 hosts란 단어를 갖는 것들은 찾지 않는다.
find / -name "host"

# amy란 유저가 만든 파일들을 찾는다.
find / -user amy

# 이를 이용하면 특정 유저가 만든 파일들을 한 곳에 복사할 수 있다.
# ;으로 하나의 커맨드를 끝내는 것을 알릴 수 있다.
# find 구문에서 -exec는 앞에 명령어의 결과를 이용해 뒤에 오는 명령어를 실행한다는 의미인데
# {} 가 앞에 명령어의 결과가 들어갈 곳이다.(여기에서는 amy가 만든 파일들이다.)
# 그렇기 때문에 find 구문을 해석해보면 amy란 유저가 만든 파일들을 /root/amy에 모두 복사한다가 된다.
# exec 커맨드는 오직 하나의 커맨드만 실행 가능하며 \; 로 끝나야 한다는 것에 주의해야 한다.
mkdir /root/amy; find / -user amy -exec cp {} /root/amy \;


find / -size +100M

# 이렇게 하면 Permsssion denied 등 몇몇 error 메세지가 발생하는데 아래와 같이 해결 가능하다.
# 2는 standard error file descriptor를 의미하는 command output 인데 2>/dev/null은 모든 error를 null device로 보낸다는 것이기에, 에러 메세지를 감출 수 있다.
find / -size +100M 2>/dev/null


# type은 찾고 싶은 타입을 의미하는데 f는 파일을 의미한다.
find / -type f -size +100M

# grep은 text를 포함하는 파일을 찾을 때 쓴다.
# 그래서 아래처럼 grep amy 는 amy라는 텍스트를 갖는 파일을 찾는다.
# 그리고 -l은 오직 file name만 가져오라는 것을 의미하는데 그렇기 때문에 /etc 내 파일들 중 content에 amy란 단어를 포함한 파일의 파일명만 얻게 된다.
find /etc -exec grep -l amy {} \; -exec cp {} root/amy/ \; 2>/dev/null


# 아래 명령어는 127.0.0.1 이란 텍스트를 갖는 파일들을 찾는 예제이다.
# xargs 는 | 와 함께 쓰여서 위에서 보던 -exec에서 쓰이던 {} 같이 앞에서 수행한 결과가 담긴다.
# *는 wildcard인데 single quote로 감싼 이유는 bash shell이 이를 해석할게 아니라 find가 해석하길 바라기 때문이다.
find /etc -name '*' -type f | xargs grep "127.0.0.1"
```


## Archiving Files with tar

tar: Tape ARchiver

예전에 tape에 백업해서 생긴 용어이다.

- **tar -cvf my_archive.tar /home**
  - 폴더 압축
  - c: create, v: verbose, f: archive 할 files
- **tar -xvf my_archive**
  - 압축 해제
  - 위 방식은 현재 디렉토리에 푼다는 것이다
  - **-C** 를 이용해 output path를 변경할 수 있다.
- **tar -tvf** 는 archive의 내용을 보여준다.
- compression기능도 지원한다. `-z(gzip)` `-j(bzip)`

```shell
# 옵션 앞에 -는 안붙여도 된다.
# 또한 한 번에 여러 폴더를 압축할 수 도 있다.
tar cvf home.tar /home /root

# 콘텐츠를 볼 수 있다.
# 보면 파일의 경로들이 상대경로로 지정되어있음을 알 수 있는데
# 압축 해제 시 상대 경로로 저장되게 하기 위함이다.
tar tvf home.tar

# 원하는 폴더에 압축 해제 할 수 있다.
tar xvf home.tar -C /tmp

# compression archive
tar czvf /tmp/home.tgz /home /root
tar cjvf /tmp/home.tbz /home /root
```

- **gzip**은 가장 일반적으로 쓰인다.
- **zip**은 윈도우에서 잘 쓰인다.
- 다른 종류들의 압축도 있다.

```shell
# dd는 block device를 copy하는 커맨드이다.
# 여기에선 dev zero block device(zeros를 생성할 때 사용하는)를 사용한다.
# of=bigfile1 으로 output을 지정
# bs로 a box size가 1M로 지정
# count로 1024개를 지정
# 결과는 1GB의 파일이 만들어진다.
dd if=/dev/zero of=bigfile1 bs=1M count=1024
dd if=/dev/zero of=bigfile2 bs=1M count=1024
dd if=/dev/zero of=bigfile3 bs=1M count=1024


# 압축을 해보자.
# 압축하고 나면 원본 파일이 사라졌음을 알 수 있다.
# bigfile1 파일이 사라지고 bigfile1.gz 파일이 생성됨
gzip bigfile1

# 압축해제는 다음과 같이 g unzip
gunzip bigfile1.gz

# time을 이용해 걸린 시간을 측정 수 있다.
time gzip bigfile1

time bzip2 bigfile2

# zip은 위의 2개와 달리 원본 파일은 그대로 두고 압축파일을 생성한다.
time zip bigfile3.zip bigfile3

# 아래 같이 압축파일의 확장자를 날려버리면 실제로는 압축파일인데 일반 파일처럼 보인다.
mv bigfile1.gz bigfile1

# 그래서 아래 커맨드로 파일의 상세정보를 확인할 수 있다.
file bigfile1

```