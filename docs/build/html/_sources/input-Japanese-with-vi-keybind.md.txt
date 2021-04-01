# vi keybindで挿入モードから抜ける時に日本語OFF
DATE: 2021-03-25

## 想定

### 環境
- Linux  
直接インストールされているか，WindowsのWSL2のVM

- vi keybindに対応したエディタ  
(Neo)Vim, (Doom) Emacs, VSCodeVim

- 日本語入力のIME  
Linuxではfcitx，WindowsではMicrosoft IME．  

### 状況
1. `i`キーでINSERTモードに入り，`全角/半角`キーを押して日本語入力
2. `Esc`キーまたは`Ctrl`+`[`キーでNORMALモードに戻る
3. `全角/半角`キーを押し忘れて日本語入力ONのままで，COMMANDモードに入れず煩わしい

## fcitx(-remote) in Linux

### Linux
- `.zprofile`

``` shell
if [ -e /mnt/c/WINDOWS/System32/wsl.exe ]; then
  export GTK_IM_MODULE='fcitx'
  export QT_IM_MODULE='fcitx'
  export XMODIFIERS='@im=fcitx'
  if [ $SHLVL = 1 ] ; then
    (fcitx-autostart > /dev/null 2>&1 &)
    xset -r 49  > /dev/null 2>&1
  fi
fi
```

- `.zshenv`

``` shell
eval $(dbus-launch)
export DBUS_SESSION_BUS_ADDRESS
```


### (Neo)vim
use https://github.com/lilydjwg/fcitx.vim

- `.zshenv`

``` shell
if [ -e /mnt/c/WINDOWS/System32/wsl.exe ]; then
  export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}'):0.0
  export FCITX_SOCKET=/tmp/fcitx-socket-:0
fi
```

### (Doom) emacs
use https://github.com/cute-jumper/fcitx.el

- `config.el`

``` elisp
(fcitx-default-setup)
```
### VSCodeVim
- `settins.json`

```json
"vim.autoSwitchInputMethod.enable": true,
"vim.autoSwitchInputMethod.obtainIMCmd": "/usr/bin/fcitx-remote",
"vim.autoSwitchInputMethod.switchIMCmd": "/usr/bin/fcitx-remote {im}",
"vim.autoSwitchInputMethod.defaultIM": "-c"
```

## zenhan.exe in Windows

### Windows
- `powershell`

```console
$ iwr -useb get.scoop.sh | iex
$ scoop install zenhan
```

- `.zprofile`

``` shell
if [ -e /mnt/c/WINDOWS/System32/wsl.exe ]; then
  export zenhan='/mnt/c/Users/USER/scoop/apps/zenhan/current/zenhan.exe'
fi
```

### (Neo)vim
- `.vimrc`

``` vim
autocmd InsertLeave * :call system('${zenhan} 0')
autocmd CmdlineLeave * :call system('${zenhan} 0')
```
### (Doom) emacs
- `config.el`

``` elisp
(add-hook 'evil-normal-state-entry-hook
  (lambda ()
    (if (string= "2\\n" (shell-command-to-string "${zenhan} 0"))
        (shell-command "${zenhan} 0"))))
```
### VSCodeVim
- `settins.jsonn`

```json
"vim.autoSwitchInputMethod.enable": true,
"vim.autoSwitchInputMethod.defaultIM": "0",
"vim.autoSwitchInputMethod.obtainIMCmd": "C:\\Users\\USER\\scoop\\apps\\zenhan\\current\\zenhan.exe 0",
"vim.autoSwitchInputMethod.switchIMCmd": "C:\\Users\\USER\\scoop\\apps\\zenhan\\current\\zenhan.exe {im}",
```
