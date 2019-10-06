---
title: 使用AppleScript获取网易云正在播放的曲目
date: 2019-10-02 16:47:37
tags: applescript
---

之前在搞powerlevel9k时看到过有人使用applescript来获取iTunes正在播放的曲目，如下：

```bash
#! /bin/bash
osascript -e 'tell application "iTunes" to if player state is playing then artist of current track & " - " & name of current track'
```

不过我平时还是用网易云的多些，想弄个网易云的ver。iTunes提供了很完善的applescript文档，网易云却没有，所以只好考虑通过傻办法来试出来网易云当前曲目的位置

##  傻办法

> PS：
>
> 1. window的名称可能略有不同，由于使用的英文系统所以是"NeteaseMusic"，而中文应该是网易云音乐
> 2. 该脚本在网易云mac版2.2.0上运转良好，后续或以前的版本可能具体的位置会有变化

```bash
#! /bin/bash
osascript -e 'tell application "System Events"
	tell window "NeteaseMusic" of process "NeteaseMusic"
		UI elements
	end tell
end tell'
```

在终端中运行这个脚本会返回

```
group 1 of window NeteaseMusic of application process NeteaseMusic, button 1 of window NeteaseMusic of application process NeteaseMusic, button 2 of window NeteaseMusic of application process NeteaseMusic, button 3 of window NeteaseMusic of application process NeteaseMusic
```

然后我们把group 1加到脚本里

```bash
#! /bin/bash
osascript -e 'tell application "System Events"
	tell group 1 of window "NeteaseMusic" of process "NeteaseMusic"
		UI elements
	end tell
end tell'
```

会发现输出变成了

```
group 1 of group 1 of window NeteaseMusic of application process NeteaseMusic
```

然后逐步的重复上述步骤，根据输出对脚本进行修改

## 最终脚本

```bash
#! /bin/bash
osascript -e 'on is_running(appName)
	tell application "System Events" to (name of processes) contains appName
end is_running
set neteaseRunning to is_running("NeteaseMusic")
tell application "System Events"
	if neteaseRunning then
		get title of UI element 4 of group 9 of UI element 1 of scroll area 1 of group 1 of group 1 of window "NeteaseMusic" of process "NeteaseMusic"
	end if
end tell'
```

## 后续：把脚本使用在Powerlevel9k & 10k上

### powerlevel9k

``` bash
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=（... custom_now_playing ...）
# 把上面的脚本内容放在nowplaying里
POWERLEVEL9K_CUSTOM_NOW_PLAYING='~/.dotfiles/bin/nowplaying'
```

### powerlevel10k

``` bash
# ~/.p10k.zsh
function prompt_now_playing_song() {
    local song=`osascript -e 'on is_running(appName) 
      tell application "System Events" to (name of processes) contains appName 
      end is_running 
      set neteaseRunning to is_running("NeteaseMusic") 
      tell application "System Events" 
      if neteaseRunning then 
      get title of UI element 4 of group 9 of UI element 1 of scroll area 1 of group 1 of group 1 of window "NeteaseMusic" of process "NeteaseMusic" 
      end if
      end tell'`
    if [[ -n $song ]]; then
      p10k segment -f 208 -i '🎵' -t "$song"
    fi 
}
```

## TODO

* BUG: 刚开启应用的时候获取不到元素

* 