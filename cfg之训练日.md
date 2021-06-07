---
title: cfg 之 训练日
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [cfg 之 训练日](#cfg-%E4%B9%8B-%E8%AE%AD%E7%BB%83%E6%97%A5)
  - [net_graph 1](#net_graph-1)
  - [net_graphpos 1](#net_graphpos-1)
  - [net_graphproportionalfont 0](#net_graphproportionalfont-0)
  - [fps_max 999](#fps_max-999)
  - [m_customaccel 0](#m_customaccel-0)
  - [sensitivity 1.1](#sensitivity-11)
  - [zoom_sensitivity_ratio_mouse 1](#zoom_sensitivity_ratio_mouse-1)
  - [cl_disablefreezecam 1](#cl_disablefreezecam-1)
  - [cl_cmdrate "128"](#cl_cmdrate-128)
  - [cl_updaterate "128"](#cl_updaterate-128)
  - [rate "128000"](#rate-128000)
  - [](#)
  - [cl_viewmodel_shift_left_amt "0"](#cl_viewmodel_shift_left_amt-0)
  - [cl_viewmodel_shift_right_amt "0"](#cl_viewmodel_shift_right_amt-0)
  - [cl_bob_lower_amt "0"](#cl_bob_lower_amt-0)
  - [cl_bobamt_lat "0"](#cl_bobamt_lat-0)
  - [cl_bobamt_vert "0"](#cl_bobamt_vert-0)
  - [](#-1)
  - [alias +highjump		"+jump; +duck;"](#alias-highjump%09%09jump-duck)
  - [alias -highjump		"-jump; -duck;"](#alias--highjump%09%09-jump--duck)
  - [](#-2)
  - [alias +jumpthrow	"+jump;-attack;-attack2";](#alias-jumpthrow%09jump-attack-attack2)
  - [alias -jumpthrow 	-jump;](#alias--jumpthrow-%09-jump)
  - [bind MOUSE3 +jumpthrow;](#bind-mouse3-jumpthrow)
  - [echo alias +shownet "+showscores; net_graphheight 64"](#echo-alias-shownet-showscores-net_graphheight-64)
  - [echo alias -shownet "-showscores; net_graphheight 9999"](#echo-alias--shownet--showscores-net_graphheight-9999)
  - [echo 点击tab显示计分板同时显示网络条件](#echo-%E7%82%B9%E5%87%BBtab%E6%98%BE%E7%A4%BA%E8%AE%A1%E5%88%86%E6%9D%BF%E5%90%8C%E6%97%B6%E6%98%BE%E7%A4%BA%E7%BD%91%E7%BB%9C%E6%9D%A1%E4%BB%B6)
  - [echo bind tab "+shownet"](#echo-bind-tab-shownet)
  - [](#-3)
  - [echo 静步清除血迹](#echo-%E9%9D%99%E6%AD%A5%E6%B8%85%E9%99%A4%E8%A1%80%E8%BF%B9)
  - [echo bind shift "+speed; r_cleardecals"](#echo-bind-shift-speed-r_cleardecals)
  - [echo 小地图不以自己为中心](#echo-%E5%B0%8F%E5%9C%B0%E5%9B%BE%E4%B8%8D%E4%BB%A5%E8%87%AA%E5%B7%B1%E4%B8%BA%E4%B8%AD%E5%BF%83)
  - [cl_radar_always_centered 0](#cl_radar_always_centered-0)
  - [echo 切换左右手](#echo-%E5%88%87%E6%8D%A2%E5%B7%A6%E5%8F%B3%E6%89%8B)
  - [bindToggle f2 "cl_righthand"](#bindtoggle-f2-cl_righthand)
  - [echo 显示雷包位置](#echo-%E6%98%BE%E7%A4%BA%E9%9B%B7%E5%8C%85%E4%BD%8D%E7%BD%AE)
  - [bindToggle v gameinstructor_enable](#bindtoggle-v-gameinstructor_enable)
  - [echo 快速扔雷](#echo-%E5%BF%AB%E9%80%9F%E6%89%94%E9%9B%B7)
  - [bind h "use weapon_c4; drop"](#bind-h-use-weapon_c4-drop)
  - [echo 显示队友装备](#echo-%E6%98%BE%E7%A4%BA%E9%98%9F%E5%8F%8B%E8%A3%85%E5%A4%87)
  - [+cl_show_team_equipment](#cl_show_team_equipment)
  - [echo 显示伤害信息](#echo-%E6%98%BE%E7%A4%BA%E4%BC%A4%E5%AE%B3%E4%BF%A1%E6%81%AF)
  - [con_filter_text damage](#con_filter_text-damage)
  - [con_filter_text_out Plater:](#con_filter_text_out-plater)
  - [con_filter_enable 2](#con_filter_enable-2)
  - [developer 1](#developer-1)
  - [echo 放小声音](#echo-%E6%94%BE%E5%B0%8F%E5%A3%B0%E9%9F%B3)
  - [bind f1 				"toggle voice_scale 1 0.1"](#bind-f1-%09%09%09%09toggle-voice_scale-1-01)
  - [](#-4)
  - [bind "kp_ins"			"buy vest;buy vesthelm;"](#bind-kp_ins%09%09%09buy-vestbuy-vesthelm)
  - [bind "kp_del"			"buy decoy;"](#bind-kp_del%09%09%09buy-decoy)
  - [bind "kp_end"			"buy fiveseven;buy tec9;"](#bind-kp_end%09%09%09buy-fivesevenbuy-tec9)
  - [bind "kp_downarrow"		"buy p250;"](#bind-kp_downarrow%09%09buy-p250)
  - [bind "kp_pgdn"			"buy deagle;give weapon_deagle"](#bind-kp_pgdn%09%09%09buy-deaglegive-weapon_deagle)
  - [bind "kp_leftarrow"		"buy p90;"](#bind-kp_leftarrow%09%09buy-p90)
  - [bind "kp_5"				"buy mp7;"](#bind-kp_5%09%09%09%09buy-mp7)
  - [bind "kp_rightarrow"	"buy famas;buy galilar;"](#bind-kp_rightarrow%09buy-famasbuy-galilar)
  - [bind "kp_home"			"buy m4a1;buy ak47;give weapon_ak47;give weapon_m4a1;"](#bind-kp_home%09%09%09buy-m4a1buy-ak47give-weapon_ak47give-weapon_m4a1)
  - [bind "kp_uparrow"		"buy sg556;buy aug;"](#bind-kp_uparrow%09%09buy-sg556buy-aug)
  - [bind "kp_pgup"			"buy awp;give weapon_awp"](#bind-kp_pgup%09%09%09buy-awpgive-weapon_awp)
  - [bind "kp_enter"			"buy hegrenade;give weapon_hegrenade"](#bind-kp_enter%09%09%09buy-hegrenadegive-weapon_hegrenade)
  - [bind "kp_plus"			"buy flashbang;give weapon_flashbang"](#bind-kp_plus%09%09%09buy-flashbanggive-weapon_flashbang)
  - [bind "kp_minus"			"buy smokegrenade;give weapon_smokegrenade"](#bind-kp_minus%09%09%09buy-smokegrenadegive-weapon_smokegrenade)
  - [bind "kp_multiply"		"buy incgrenade;buy molotov; give weapon_incgrenade; give weapon_molotov;"](#bind-kp_multiply%09%09buy-incgrenadebuy-molotov-give-weapon_incgrenade-give-weapon_molotov)
  - [bind "kp_slash"			"buy defuser;"](#bind-kp_slash%09%09%09buy-defuser)
  - [bind "alt"				"+highjump; r_cleardecals"](#bind-alt%09%09%09%09highjump-r_cleardecals)
  - [bind "n"				"timeleft;toggle spec_hide_players 1 0;"](#bind-n%09%09%09%09timelefttoggle-spec_hide_players-1-0)
  - [bind "f9"				"demoui"](#bind-f9%09%09%09%09demoui)
  - [bind MWHEELDOWN 		"+jump"](#bind-mwheeldown-%09%09jump)
  - [bind MOUSE5 			"use weapon_flashbang"](#bind-mouse5-%09%09%09use-weapon_flashbang)
  - [bind MOUSE4 			"use weapon_smokegrenade"](#bind-mouse4-%09%09%09use-weapon_smokegrenade)
  - [exec Coen_crosshair_NiKo.cfg](#exec-coen_crosshair_nikocfg)
  - [](#-5)
  - [bind l "noclip"](#bind-l-noclip)
  - [](#-6)
  - [sv_grenade_trajectory 1](#sv_grenade_trajectory-1)
  - [bindToggle o "sv_showimpacts"](#bindtoggle-o-sv_showimpacts)
  - [mp_autoteambalance 0](#mp_autoteambalance-0)
  - [mp_freezetime 0](#mp_freezetime-0)
  - [mp_maxrounds 60](#mp_maxrounds-60)
  - [mp_limitteams 0](#mp_limitteams-0)
  - [mp_startmoney 20000](#mp_startmoney-20000)
  - [mp_maxmoney 50000](#mp_maxmoney-50000)
  - [](#-7)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



[TOC]

# cfg 之 训练日

首先上我的 `cfg`

```ini
net_graph 1
net_graphpos 1
net_graphproportionalfont 0
fps_max 999
m_customaccel 0
sensitivity 1.1
zoom_sensitivity_ratio_mouse 1

cl_disablefreezecam 1

cl_cmdrate "128"
cl_updaterate "128"
rate "128000"

cl_viewmodel_shift_left_amt "0"
cl_viewmodel_shift_right_amt "0"
cl_bob_lower_amt "0"
cl_bobamt_lat "0"
cl_bobamt_vert "0"


alias +highjump		"+jump; +duck;" 
alias -highjump		"-jump; -duck;"

alias +jumpthrow	"+jump;-attack;-attack2";
alias -jumpthrow 	-jump;
bind MOUSE3 +jumpthrow;

echo alias +shownet "+showscores; net_graphheight 64"
echo alias -shownet "-showscores; net_graphheight 9999"
echo 点击tab显示计分板同时显示网络条件
echo bind tab "+shownet"

echo 静步清除血迹
echo bind shift "+speed; r_cleardecals"
echo 小地图不以自己为中心
cl_radar_always_centered 0
echo 切换左右手
bindToggle f2 "cl_righthand"
echo 显示雷包位置
bindToggle v gameinstructor_enable
echo 快速扔雷
bind h "use weapon_c4; drop"
echo 显示队友装备
+cl_show_team_equipment
echo 显示伤害信息
con_filter_text damage
con_filter_text_out Plater:
con_filter_enable 2
developer 1
echo 放小声音
bind f1 				"toggle voice_scale 1 0.1"

bind "kp_ins"			"buy vest;buy vesthelm;"
bind "kp_del"			"buy decoy;"
bind "kp_end"			"buy fiveseven;buy tec9;"
bind "kp_downarrow"		"buy p250;"
bind "kp_pgdn"			"buy deagle;give weapon_deagle"
bind "kp_leftarrow"		"buy p90;"
bind "kp_5"				"buy mp7;"
bind "kp_rightarrow"	"buy famas;buy galilar;"
bind "kp_home"			"buy m4a1;buy ak47;give weapon_ak47;give weapon_m4a1;"
bind "kp_uparrow"		"buy sg556;buy aug;"
bind "kp_pgup"			"buy awp;give weapon_awp"
bind "kp_enter"			"buy hegrenade;give weapon_hegrenade"
bind "kp_plus"			"buy flashbang;give weapon_flashbang"
bind "kp_minus"			"buy smokegrenade;give weapon_smokegrenade"
bind "kp_multiply"		"buy incgrenade;buy molotov; give weapon_incgrenade; give weapon_molotov;"
bind "kp_slash"			"buy defuser;"
bind "alt"				"+highjump; r_cleardecals"
bind "n"				"timeleft;toggle spec_hide_players 1 0;"
bind "f9"				"demoui"
bind MWHEELDOWN 		"+jump"
bind MOUSE5 			"use weapon_flashbang"
bind MOUSE4 			"use weapon_smokegrenade"
exec Coen_crosshair_NiKo.cfg

bind l "noclip"

sv_grenade_trajectory 1
bindToggle o "sv_showimpacts"
mp_autoteambalance 0
mp_freezetime 0
mp_maxrounds 60
mp_limitteams 0
mp_startmoney 20000
mp_maxmoney 50000


```

下面我们来一一解释

## net_graph 1

显示网络, fps, var, tickrate, loss 等状态信息图表, `1` 对应开启, `0` 是关闭

## net_graphpos 1

状态信息图表的位置, 状态值有 

- `0`
- `1`
- `2`

## net_graphproportionalfont 0

状态信息中的字体是否跟界面 UI 成比例. 

- `0` 是不成比
- `1` 是成比例

## fps_max 999

最大 fps 设置为 999

## m_customaccel 0

关闭鼠标加速

## sensitivity 1.1

鼠标灵敏度设置为 1.1

## zoom_sensitivity_ratio_mouse 1
开镜鼠标灵敏度设置为 1

## cl_disablefreezecam 1
 关闭死亡后的回放功能

## cl_cmdrate "128"
## cl_updaterate "128"
## rate "128000"
## 
## cl_viewmodel_shift_left_amt "0"
## cl_viewmodel_shift_right_amt "0"
## cl_bob_lower_amt "0"
## cl_bobamt_lat "0"
## cl_bobamt_vert "0"

## 
## alias +highjump		"+jump; +duck;" 
## alias -highjump		"-jump; -duck;"
## 
## alias +jumpthrow	"+jump;-attack;-attack2";
## alias -jumpthrow 	-jump;
## bind MOUSE3 +jumpthrow;





## echo alias +shownet "+showscores; net_graphheight 64"
## echo alias -shownet "-showscores; net_graphheight 9999"
## echo 点击tab显示计分板同时显示网络条件
## echo bind tab "+shownet"
## 
## echo 静步清除血迹
## echo bind shift "+speed; r_cleardecals"
## echo 小地图不以自己为中心
## cl_radar_always_centered 0
## echo 切换左右手
## bindToggle f2 "cl_righthand"
## echo 显示雷包位置
## bindToggle v gameinstructor_enable
## echo 快速扔雷
## bind h "use weapon_c4; drop"
## echo 显示队友装备
## +cl_show_team_equipment
## echo 显示伤害信息
## con_filter_text damage
## con_filter_text_out Plater:
## con_filter_enable 2
## developer 1
## echo 放小声音
## bind f1 				"toggle voice_scale 1 0.1"
## 
## bind "kp_ins"			"buy vest;buy vesthelm;"
## bind "kp_del"			"buy decoy;"
## bind "kp_end"			"buy fiveseven;buy tec9;"
## bind "kp_downarrow"		"buy p250;"
## bind "kp_pgdn"			"buy deagle;give weapon_deagle"
## bind "kp_leftarrow"		"buy p90;"
## bind "kp_5"				"buy mp7;"
## bind "kp_rightarrow"	"buy famas;buy galilar;"
## bind "kp_home"			"buy m4a1;buy ak47;give weapon_ak47;give weapon_m4a1;"
## bind "kp_uparrow"		"buy sg556;buy aug;"
## bind "kp_pgup"			"buy awp;give weapon_awp"
## bind "kp_enter"			"buy hegrenade;give weapon_hegrenade"
## bind "kp_plus"			"buy flashbang;give weapon_flashbang"
## bind "kp_minus"			"buy smokegrenade;give weapon_smokegrenade"
## bind "kp_multiply"		"buy incgrenade;buy molotov; give weapon_incgrenade; give weapon_molotov;"
## bind "kp_slash"			"buy defuser;"
## bind "alt"				"+highjump; r_cleardecals"
## bind "n"				"timeleft;toggle spec_hide_players 1 0;"
## bind "f9"				"demoui"
## bind MWHEELDOWN 		"+jump"
## bind MOUSE5 			"use weapon_flashbang"
## bind MOUSE4 			"use weapon_smokegrenade"
## exec Coen_crosshair_NiKo.cfg
## 
## bind l "noclip"
## 
## sv_grenade_trajectory 1
## bindToggle o "sv_showimpacts"
## mp_autoteambalance 0
## mp_freezetime 0
## mp_maxrounds 60
## mp_limitteams 0
## mp_startmoney 20000
## mp_maxmoney 50000
## 
