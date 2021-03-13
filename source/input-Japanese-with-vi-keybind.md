# vi keybindで挿入モードから抜ける時に日本語OFF

## 目的
vi keybindを使うエディタにおいて，挿入モードで日本語入力した後に，
normalモードでコマンドを入力しようとして失敗する煩わしさを解消する．

使っているLinuxがWindowsのWSl2上か気にしなくていいように，
できる限り設定を共通化する．

## fcitx

### linux
`.zprofile`

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
### (Neo)vim

add this plugin https://github.com/lilydjwg/fcitx.vim

`.zshenv`

``` shell
if [ -e /mnt/c/WINDOWS/System32/wsl.exe ]; then
  export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}'):0.0
  export FCITX_SOCKET=/tmp/fcitx-socket-:0
fi
```

### (Doom) emacs
add this package https://github.com/cute-jumper/fcitx.el

`config.el`

``` elisp
(fcitx-default-setup)
```
### VSCodeVim
`settins.json`

```json
"vim.autoSwitchInputMethod.enable": true,
"vim.autoSwitchInputMethod.obtainIMCmd": "/usr/bin/fcitx-remote",
"vim.autoSwitchInputMethod.switchIMCmd": "/usr/bin/fcitx-remote {im}",
"vim.autoSwitchInputMethod.defaultIM": "-c"
```

## zenhan

### windows
`powershell`

```console
$ iwr -useb get.scoop.sh | iex
$ scoop install zenhan
```

`.zprofile`

``` shell
if [ -e /mnt/c/WINDOWS/System32/wsl.exe ]; then
  export zenhan='/mnt/c/Users/USER/scoop/apps/zenhan/current/zenhan.exe'
fi
```

### (Neo)vim
`.vimrc`

``` config
autocmd InsertLeave * :call system('${zenhan} 0')
autocmd CmdlineLeave * :call system('${zenhan} 0')
```
### (Doom) emacs
`config.el`

``` elisp
(add-hook 'evil-normal-state-entry-hook
  (lambda ()
    (if (string= "2\\n" (shell-command-to-string "${zenhan} 0"))
        (shell-command "${zenhan} 0"))))
```
### VSCodeVim
`settins.jsonn`

```json
"vim.autoSwitchInputMethod.enable": true,
"vim.autoSwitchInputMethod.defaultIM": "0",
"vim.autoSwitchInputMethod.obtainIMCmd": "C:\\Users\\USER\\scoop\\apps\\zenhan\\current\\zenhan.exe 0",
"vim.autoSwitchInputMethod.switchIMCmd": "C:\\Users\\USER\\scoop\\apps\\zenhan\\current\\zenhan.exe {im}",
```
