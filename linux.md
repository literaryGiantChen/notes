## 基本命令

tail 【实时看日志】

```
tail -f *.log 【实时查看日志,开发环境还行，生产就算了，日志猛刷啊】
tail -f error.log 【实时看异常日志还是可以的】
```

vi 【编辑文本】

```
vi x.log 【强大的vi命令】
:wq 保存退出
:q! 退出不保存
Shift+g 跳至当前文本最后一行 【看最新的日志，都在最下面】
g+g 跳至当前文本第一行
```

grep 【专抓日志，grep是必备日志分析命令】

```
grep 【强大的grep，搜日志就靠它了】
grep -r '关键字如商品ID' *.log 【使用频率最高】
grep '关键字如商品ID' *.log | grep 免费商品 【条件结果中，在加条件筛选下 】
grep '关键字如商品ID' *.log >> anan.txt 【相关日志输入到一个txt中，下载到本地慢慢看，我最喜欢】
grep -A 2 '商品ID' *.log 【显示商品ID及后5行】
grep -B 2 '商品ID' *.log 【显示商品ID及上5行】
grep -C 2 '商品ID' *.log 【显示商品ID及上下5行】
grep '商品ID' *.log --col  【高亮显示商品ID，非常醒目啊】
```

杀僵尸进程 部分程序员，肯定喜欢下面命令

```
ps -ef | grep java 【先查java进程ID】
kill -9 java进程ID 【生产环境谨慎使用】
```

host 查物域名IP

```
host 域名 【查具体IP】
```

