---
title: cfg 之 训练日
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [cfg 之 训练日](#cfg-之-训练日)
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
  - [alias +highjump		"+jump; +duck;"](#alias-highjumpjump-duck)
  - [alias -highjump		"-jump; -duck;"](#alias--highjump-jump--duck)
  - [](#-2)
  - [alias +jumpthrow	"+jump;-attack;-attack2";](#alias-jumpthrowjump-attack-attack2)
  - [alias -jumpthrow 	-jump;](#alias--jumpthrow--jump)
  - [bind MOUSE3 +jumpthrow;](#bind-mouse3-jumpthrow)
  - [echo alias +shownet "+showscores; net_graphheight 64"](#echo-alias-shownet-showscores-net_graphheight-64)
  - [echo alias -shownet "-showscores; net_graphheight 9999"](#echo-alias--shownet--showscores-net_graphheight-9999)
  - [echo 点击tab显示计分板同时显示网络条件](#echo-点击tab显示计分板同时显示网络条件)
  - [echo bind tab "+shownet"](#echo-bind-tab-shownet)
  - [](#-3)
  - [echo 静步清除血迹](#echo-静步清除血迹)
  - [echo bind shift "+speed; r_cleardecals"](#echo-bind-shift-speed-r_cleardecals)
  - [echo 小地图不以自己为中心](#echo-小地图不以自己为中心)
  - [cl_radar_always_centered 0](#cl_radar_always_centered-0)
  - [echo 切换左右手](#echo-切换左右手)
  - [bindToggle f2 "cl_righthand"](#bindtoggle-f2-cl_righthand)
  - [echo 显示雷包位置](#echo-显示雷包位置)
  - [bindToggle v gameinstructor_enable](#bindtoggle-v-gameinstructor_enable)
  - [echo 快速扔雷](#echo-快速扔雷)
  - [bind h "use weapon_c4; drop"](#bind-h-use-weapon_c4-drop)
  - [echo 显示队友装备](#echo-显示队友装备)
  - [+cl_show_team_equipment](#cl_show_team_equipment)
  - [echo 显示伤害信息](#echo-显示伤害信息)
  - [con_filter_text damage](#con_filter_text-damage)
  - [con_filter_text_out Plater:](#con_filter_text_out-plater)
  - [con_filter_enable 2](#con_filter_enable-2)
  - [developer 1](#developer-1)
  - [echo 放小声音](#echo-放小声音)
  - [bind f1 				"toggle voice_scale 1 0.1"](#bind-f1-toggle-voice_scale-1-01)
  - [](#-4)
  - [bind "kp_ins"			"buy vest;buy vesthelm;"](#bind-kp_insbuy-vestbuy-vesthelm)
  - [bind "kp_del"			"buy decoy;"](#bind-kp_delbuy-decoy)
  - [bind "kp_end"			"buy fiveseven;buy tec9;"](#bind-kp_endbuy-fivesevenbuy-tec9)
  - [bind "kp_downarrow"		"buy p250;"](#bind-kp_downarrowbuy-p250)
  - [bind "kp_pgdn"			"buy deagle;give weapon_deagle"](#bind-kp_pgdnbuy-deaglegive-weapon_deagle)
  - [bind "kp_leftarrow"		"buy p90;"](#bind-kp_leftarrowbuy-p90)
  - [bind "kp_5"				"buy mp7;"](#bind-kp_5buy-mp7)
  - [bind "kp_rightarrow"	"buy famas;buy galilar;"](#bind-kp_rightarrowbuy-famasbuy-galilar)
  - [bind "kp_home"			"buy m4a1;buy ak47;give weapon_ak47;give weapon_m4a1;"](#bind-kp_homebuy-m4a1buy-ak47give-weapon_ak47give-weapon_m4a1)
  - [bind "kp_uparrow"		"buy sg556;buy aug;"](#bind-kp_uparrowbuy-sg556buy-aug)
  - [bind "kp_pgup"			"buy awp;give weapon_awp"](#bind-kp_pgupbuy-awpgive-weapon_awp)
  - [bind "kp_enter"			"buy hegrenade;give weapon_hegrenade"](#bind-kp_enterbuy-hegrenadegive-weapon_hegrenade)
  - [bind "kp_plus"			"buy flashbang;give weapon_flashbang"](#bind-kp_plusbuy-flashbanggive-weapon_flashbang)
  - [bind "kp_minus"			"buy smokegrenade;give weapon_smokegrenade"](#bind-kp_minusbuy-smokegrenadegive-weapon_smokegrenade)
  - [bind "kp_multiply"		"buy incgrenade;buy molotov; give weapon_incgrenade; give weapon_molotov;"](#bind-kp_multiplybuy-incgrenadebuy-molotov-give-weapon_incgrenade-give-weapon_molotov)
  - [bind "kp_slash"			"buy defuser;"](#bind-kp_slashbuy-defuser)
  - [bind "alt"				"+highjump; r_cleardecals"](#bind-althighjump-r_cleardecals)
  - [bind "n"				"timeleft;toggle spec_hide_players 1 0;"](#bind-ntimelefttoggle-spec_hide_players-1-0)
  - [bind "f9"				"demoui"](#bind-f9demoui)
  - [bind MWHEELDOWN 		"+jump"](#bind-mwheeldown-jump)
  - [bind MOUSE5 			"use weapon_flashbang"](#bind-mouse5-use-weapon_flashbang)
  - [bind MOUSE4 			"use weapon_smokegrenade"](#bind-mouse4-use-weapon_smokegrenade)
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
