# Bai 2 (Session 05): Tai cau truc lich su commit bang Interactive Rebase

## 1. Muc tieu
- Gom 3 commit nho thanh 1 commit duy nhat (squash).
- Doi message thanh: `feat: hoan thien module authentication` (reword qua man hinh gop message).
- Xoa bo commit rac `temp.txt` (drop).
- Ket qua: `git log --oneline` chi con 1 commit sach, khong con `temp.txt`.

## 2. Moi truong thuc hanh
- Thu muc test rieng trong project de khong lam ban code Java:
  `ss5/bai2/rebase-practise/` (`git init` rieng tai day).
- Branch: `main`.
- Editor: Vim trong Git Bash (man hinh `rebase-todo` + man hinh `COMMIT_EDITMSG`).

> Ly do dung `git rebase -i --root` thay vi `git rebase -i HEAD~4` nhu de bai:
> repo test nay chi co dung 4 commit, khong co commit nen -> `HEAD~4` bao
> `fatal: invalid upstream 'HEAD~4'`. `--root` la cach dung de rebase ca 4 commit tu root.

## 3. Tao 4 commit vun (tai hien de bai)

```bash
echo "// auth v1" > auth.js
git add auth.js
git commit -m "feat: khoi tao module auth"

echo "// fix typo" >> auth.js
git add auth.js
git commit -m "fix typo"

echo "// utility" >> auth.js
git add auth.js
git commit -m "adds utility functions"

echo "debug" > temp.txt
git add temp.txt
git commit -m "add temp file for debug"

git log --oneline
# e50529d feat: khoi tao module auth
# 8053dd7 fix typo
# 9c1c7b0 adds utility functions
# 0e3fc6d add temp file for debug
```

## 4. Interactive Rebase

```bash
git rebase -i --root
```

### 4.1. Man hinh 1: `rebase-todo` (BAT BUOC chup man hinh nop bai)

Ban dau Git mo:

```
pick e50529d feat: khoi tao module auth
pick 8053dd7 fix typo
pick 9c1c7b0 adds utility functions
pick 0e3fc6d add temp file for debug
```

Sua thanh (giong de bai yeu cau):

```
pick e50529d feat: khoi tao module auth
squash 8053dd7 fix typo
squash 9c1c7b0 adds utility functions
drop 0e3fc6d add temp file for debug
```

- `squash` (viet tat `s`): gop commit 2 + 3 vao commit 1.
- `drop` (viet tat `d`): xoa bo commit `temp.txt`.
- Luu + dong Vim: `Esc` -> `:wq`.

> Chen anh tai day:
> `![man hinh rebase-todo pick squash drop](images/rebase-todo.png)`

### 4.2. Man hinh 2: `COMMIT_EDITMSG` (reword)

Ngay sau khi `:wq` man hinh 1, Git mo tiep man hinh 2 trong cung cua so:

```
# This is a combination of 3 commits.
# This is the 1st commit message:
feat: khoi tao module auth
# This is the commit message #2:
fix typo
# This is the commit message #3:
adds utility functions
```

Xoa het 3 dong cu, chi de lai dung 1 dong:

```
feat: hoan thien module authentication
```

`Esc` -> `:wq` de hoan tat.

Neu de nguyen cac dong `# This is a combination...` thi message se hong
(da gap thuc te, phai sua lai bang `git commit --amend -m "..."`).

## 5. Kiem tra sau rebase (ket qua thuc te tren may)

```text
$ git log --oneline
bea6766 feat: hoan thien module authentication

$ git show --stat HEAD --oneline
bea6766 feat: hoan thien module authentication
 auth.js | 3 +++
 1 file changed, 3 insertions(+)

$ dir /b
.gitattributes
.gitignore
.gradle
auth.js
build.gradle
gradle
gradlew
gradlew.bat
HELP.md
rebase-practise.iml
settings.gradle
src

$ git branch --show-current
main
```

Nhan xet:
- Chi con duy nhat 1 commit `bea6766 feat: hoan thien module authentication`.
- Khong con `fix typo`, `adds utility functions`, `add temp file for debug`.
- `dir /b` khong con `temp.txt` -> `drop` thanh cong.
- `auth.js` van giu du 3 dong noi dung duoc gop (noi dung thuc te):

```text
// auth v1 /usr/bin/bash.00 spent
// fix typo
// utility
```

> Chen anh tai day:
> `![log sach sau rebase](images/log-sau-rebase.png)`

## 6. Loi hay gap khi lam tren Windows/Git Bash

1. Paste dinh chu rac tu terminal (`LSP`, `tokens`, `$0.00 spent`) -> `git add` bao
   `fatal: pathspec 'LSP' did not match`. Khac phuc: go tay tung lenh, dung Git Bash
   rieng ngoai IntelliJ.
2. Dung lenh `cmd` (`rmdir /s /q`, `del`, `dir /b`) trong Git Bash -> bao loi.
   Trong Git Bash phai dung: `rm -rf .git`, `rm -f auth.js temp.txt`, `ls`.
3. `git rebase -i HEAD~4` bao `fatal: invalid upstream 'HEAD~4'` khi repo chi co
   dung 4 commit. Khac phuc: `git rebase -i --root`.
4. Thoat Vim: `Esc` -> `:wq` (luu), `:q!` (huy man hinh), dung tat cua so khi
   dang `(main|REBASE)`. Neu ket: `git rebase --abort` / `git rebase --continue`.
5. Trong luc `REBASE` dung mo terminal khac chay `git log/rebase` chen vao.
6. Tuyet doi khong `git add .` trong `rebase-practise/` vi co nhieu file copy thua
   (`build.gradle`, `src/`...) hien dang `Untracked files`.

## 7. Cau truc nop

```text
homework/session_05/ex2/
  README.md            <- file nay
  images/
    rebase-todo.png    <- anh man hinh pick/squash/drop (bat buoc)
    log-sau-rebase.png <- anh git log --oneline chi con 1 commit
```
