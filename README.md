# YARP gui2 定制（sm8850 / pudding）

基于 `TWRP-Test/android_bootable_recovery` 的 `twrp-16.0` 分支，对 gui2 界面做两处改动，
用 GitHub Actions 出 `recovery.img`。

## 改动

1. 首页加「文件」和「重启」两张卡片，「文件」排第一位，返回路径按来源分流
2. 切卡槽改成滑动确认：备份页的 `Set slot A/B` 只记录选择，滑到底才写激活槽
   （重启页原本就是滑到底才切槽 + 重启，未改）

补丁：`patches/0001-gui2-home-and-slot-swipe.patch`（11 文件，+90/−6）

## 跑法

Actions → `Build YARP GUI2 (sm8850)` → Run workflow，默认参数即可：

| 参数 | 默认值 |
|---|---|
| MANIFEST_URL | `https://github.com/TWRP-Test/platform_manifest_twrp_aosp` |
| MANIFEST_BRANCH | `twrp-16.0` |
| DEVICE_TREE_URL | `https://github.com/YuKongA/twrp_device_xiaomi_sm8850` |
| DEVICE_TREE_BRANCH | `twrp-16.0` |
| DEVICE_PATH | `xiaomi/sm8850` |
| DEVICE_NAME | `sm8850` |
| LUNCH_TARGET | `sm8850` |
| BUILD_PARTITION | `recovery` |

产物：`artifacts/*.img` + Release 附件。

## 本机验证范围

- 四个语言包与 `i18n.cpp` 用 `clang++ -std=c++20 -fsyntax-only` 全部通过
- `action_definitions.cpp` 用桩头语法检查通过
- 未做整包编译与真机验证

## 说明

- 补丁基线 commit：`a93a12ec107265ab9c3c7fe3e4f96c8740a01337`
- CI 里 `git apply --3way` 失败会退回 `patch -p1`，日志可见 `git diff --stat`
