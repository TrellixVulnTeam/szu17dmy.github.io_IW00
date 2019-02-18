# 猫咪克星

## 题目
SOURCE: [https://hack2018.lug.ustc.edu.cn/#python_simulator](https://hack2018.lug.ustc.edu.cn/#python_simulator)

通知：已发布备用地址。

众所周知，猫怕黄瓜

![PYTHON-SIMULATOR-1](./img/PYTHON-SIMULATOR-1.gif)

你知道猫咪为什么怕黄瓜吗？

有一种说法是这是猫对蛇的原始恐惧，也就是说，如果一个东西足够像蛇，那么猫咪就会怕它。

进一步，如果你足够像蛇，猫就会怕你。

下面我们来扮演蟒蛇（Python）去吓猫。

蟒蛇是一种非常容易使用的编程语言，考验你像不像蟒蛇的标准就是给你一些 Python 3 表达式。如果你能正确计算出来，你就通过了验证。

赶快使用命令 nc 202.38.95.46 12009 来开始吧

备用地址：nc 202.38.95.47 12009

## 解决方案
nc后发现是需要在30秒内求解多个表达式的值，决定直接eval()解决。随后发现有表达式夹带了一些干扰的函数，需要在运算前先过滤掉。

使用了pwntools，事实上可以直接写好之后nc：

![PYTHON-SIMULATOR-2](./img/PYTHON-SIMULATOR-2.png)

这里是代码，环境是Windows 10 + IntelliJ IDEA (with Python plugin) + Kali Linux (WSL)：

``` python
from pwn import *


def function_filter(string):
    if 'flag' in string:
        print(string)
        conn.close()
        exit(0)
    string = string.replace(r"print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')", 'None')
    string = string.replace('exit()', 'None')
    string = string.replace('__import__(\'time\').sleep(100)', 'None')
    string = string.replace('__import__(\'os\').system(\'find ~\')', 'None')
    return string


conn = remote('202.38.95.46', 12009)
print(conn.recvline())

while True:
    expression = conn.recvline()
    print(expression)
    expression = function_filter(expression)
    print(expression)
    result = str(eval(expression)) + '\n'
    conn.send(result)
    print(result)

```

下方是终端的记录，游客止步（滑稽.jpg）。

```
C:\Users\domain\AppData\Local\Microsoft\WindowsApps\kali.exe run "export PYTHONUNBUFFERED=1 && export PYCHARM_MATPLOTLIB_PORT=63169 && export PYTHONPATH=/mnt/c/Users/domain/IdeaProjects/pwntools:/mnt/c/Users/domain/.IntelliJIdea2018.3/config/plugins/python/helpers/pycharm_matplotlib_backend && export PYCHARM_HOSTED=1 && export PYTHONIOENCODING=UTF-8 && cd /mnt/c/Users/domain/IdeaProjects/pwntools && /usr/bin/python /mnt/c/Users/domain/IdeaProjects/pwntools/main.py"
[x] Opening connection to 202.38.95.46 on port 12009
[x] Opening connection to 202.38.95.46 on port 12009: Trying 202.38.95.46
[+] Opening connection to 202.38.95.46 on port 12009: Done
You have only 30 seconds
(((117**10)^(20>>4))*((112&56)+int(17<=45)))
(((117**10)^(20>>4))*((112&56)+int(17<=45)))
23553459107299463524952
(((76+113)*(4-7))-((3&5)*(120-10)))
(((76+113)*(4-7))-((3&5)*(120-10)))
-677
(((19^58)&(25-2))&((2**47)*(55^68)))
(((19^58)&(25-2))&((2**47)*(55^68)))
0
((int(7>=11)+(4^19))&(int(9!=95)+(82-1)))
((int(7>=11)+(4^19))&(int(9!=95)+(82-1)))
18
(((59|17)-int(72!=4))^((7|132)+(1>>4)))
(((59|17)-int(72!=4))^((7|132)+(1>>4)))
189
(((8>>9)|(1&21))|int((5-140)>(117*8)))
(((8>>9)|(1&21))|int((5-140)>(117*8)))
1
((int(9==39)+(2<<42))*((2**23)|int(85!=34)))
((int(9==39)+(2<<42))*((2**23)|int(85!=34)))
73786985090931228672
(int((8&8)<=int(10!=66))*int((23*1)<(112^71)))
(int((8&8)<=int(10!=66))*int((23*1)<(112^71)))
0
((int(15==14)+(87>>1))+((25**111)+int(1<68)))
((int(15==14)+(87>>1))+((25**111)+int(1<68)))
148368246027496855429269410461905580534702591150311640098121562356703253603825174359892483308928337501453556765440046361081982695395709015429019927978515669
(int((10+110)!=(82<<89))*((23|23)|int(6<57)))
(int((10+110)!=(82<<89))*((23|23)|int(6<57)))
23
int((int(1>=11)&(18+11))!=int((6|4)>=int(116>4)))
int((int(1>=11)&(18+11))!=int((6|4)>=int(116>4)))
1
((int(140>4)|(50^49))-((7+16)^(23+110)))
((int(140>4)|(50^49))-((7+16)^(23+110)))
-143
int(int((37>>56)!=(24*21))>=((31|3)*(15<<31)))
int(int((37>>56)!=(24*21))>=((31|3)*(15<<31)))
0
(((12**1)-(1*43))^((138&5)^(86*59)))
(((12**1)-(1*43))^((138&5)^(86*59)))
-5069
int(int((16|1)!=(4*4))<((138**39)+(2|79)))
int(int((16|1)!=(4*4))<((138**39)+(2|79)))
1
int(((39**2)+(19|6))!=(int(6<=2)&(68*50)))
int(((39**2)+(19|6))!=(int(6<=2)&(68*50)))
1
(((37-2)-(3>>30))+((15-27)^(47*3)))
(((37-2)-(3>>30))+((15-27)^(47*3)))
-100
(((23+100)|int(67<=135))-((97-19)-(2|9)))
(((23+100)|int(67<=135))-((97-19)-(2|9)))
56
int((int(48<=77)^(6-2))<=int((104+5)>(17|2)))
int((int(48<=77)^(6-2))<=int((104+5)>(17|2)))
0
((int(15!=34)*(68**1))*((37|11)|(19*4)))
((int(15!=34)*(68**1))*((37|11)|(19*4)))
7548
(int((15-9)<=(36>>45))^((15|101)-(29<<21)))
(int((15-9)<=(36>>45))^((15|101)-(29<<21)))
-60817297
(((2<<64)*(47&9))-int((23>>1)>(49&1)))
(((2<<64)*(47&9))-int((23>>1)>(49&1)))
332041393326771929087
(((6|59)+(1>>8))|((1^2)+int(5>28)))
(((6|59)+(1>>8))|((1^2)+int(5>28)))
63
(((2*21)+int(34<=65))&((106^2)-int(114>=5)))
(((2*21)+int(34<=65))&((106^2)-int(114>=5)))
35
(((11>>45)+(1+7))^((2&5)-(22*29)))
(((11>>45)+(1+7))^((2&5)-(22*29)))
-630
(((109**1)+(1+38))-(int(24==1)+(4^102)))
(((109**1)+(1+38))-(int(24==1)+(4^102)))
50
(((56<<4)+(1*3))-((1+2)*(2<<1)))
(((56<<4)+(1*3))-((1+2)*(2<<1)))
887
int(int(int(25!=9)==(63|21))>=((7**143)|(13-1)))
int(int(int(25!=9)==(63|21))>=((7**143)|(13-1)))
0
(((10<<2)|(1^79))-int((1*83)==(13|31)))
(((10<<2)|(1^79))-int((1*83)==(13|31)))
110
(((114>>1)^(4|13))|((3^9)^int(12<=1)))
(((114>>1)^(4|13))|((3^9)^int(12<=1)))
62
((2342064000130989726458922265734505037*378101797641765074633697655)*(361209203089064608726475229+7405120403093238288678324066))
((2342064000130989726458922265734505037*378101797641765074633697655)*(361209203089064608726475229+7405120403093238288678324066))
6877384713710666667629989990116701071335793976166078686609796185338000890158903577053294325
((113013673117091000778178561732*95568362858979441195778255590311)-(88442375602257996324134996385167433520&62868084756973658540))
((113013673117091000778178561732*95568362858979441195778255590311)-(88442375602257996324134996385167433520&62868084756973658540))
10800531720480242930912425663531210138715873070864972025722044
((145936223963731625770596791+185713060458974767541446220)+(4222634439223833111865710*3392216678742463229433662535331800569))
((145936223963731625770596791+185713060458974767541446220)+(4222634439223833111865710*3392216678742463229433662535331800569))
14324090972967414859623430900997497001948960545759326541632001
((84972644075679862290670715462665*103021483480991156265050026263275)|(44952701858967169121145720212041581823^255751100351771662533209477680641057410))
((84972644075679862290670715462665*103021483480991156265050026263275)|(44952701858967169121145720212041581823^255751100351771662533209477680641057410))
8754007847978793971280484377445805527446467229541149285815936639
((33706913853048835239844769389329689996+50223072051652617964930100)-(776098864387100527577058636019*41914530355020436818724))
((33706913853048835239844769389329689996+50223072051652617964930100)-(776098864387100527577058636019*41914530355020436818724))
-32529819409849980818454667134821017070608431705399660
((609107414927851004765313477228853*4609189832345529105105)+(44242032082536560320526569891+4532349031470909972957799190895))
((609107414927851004765313477228853*4609189832345529105105)+(44242032082536560320526569891+4532349031470909972957799190895))
2807491703691720204768111277944190194400910650701355351
((9544933787035660979503569177*592421544580584592481334917)*(505348518123734519777552+65822681674718855298))
((9544933787035660979503569177*592421544580584592481334917)*(505348518123734519777552+65822681674718855298))
2857928272237953910975662857011108998182100402617886833182207854162345028600650
((7380017427798032412770+35334842329192425627081629390291984)&(1366990497679564417298*3296636763745755994475404561097597351))
((7380017427798032412770+35334842329192425627081629390291984)&(1366990497679564417298*3296636763745755994475404561097597351))
22232385606198111341494938769167410
((141995336294035175708956637011*187366860768591824486511158544744925)+(22367856812360691672498640395+210602812480947398948415934594364))
((141995336294035175708956637011*187366860768591824486511158544744925)+(22367856812360691672498640395+210602812480947398948415934594364))
26605220405193862192943690539580261509112670006704313883242653934
((70836351337739408621837+1874685452344475957897528642716425883)&(8151629790404072067196674-27196440084071133300253565))
((70836351337739408621837+1874685452344475957897528642716425883)&(8151629790404072067196674-27196440084071133300253565))
1874685452327014399073896116720272256
((24744306292528480818278390709+5805547731358322244445555660)-(971618688495883190384856|3760032423462507564926375393391))
((24744306292528480818278390709+5805547731358322244445555660)-(971618688495883190384856|3760032423462507564926375393391))
-3729482577205633670012168196478
((8398600763738495349603642|1155519235100868704271)|(27199893292190962969096*88981316175045359827744))
((8398600763738495349603642|1155519235100868704271)|(27199893292190962969096*88981316175045359827744))
2420282304959939516913559041283377383092518207
((35324944215400258167990733916637587766&7770679375413997193116)*(256442899747653838817208722315070&123925610385160486864515013204011))
((35324944215400258167990733916637587766&7770679375413997193116)*(256442899747653838817208722315070&123925610385160486864515013204011))
432905542325841850415209331948719374413651045239811400
((86441699007994596136488821368-211041053003450504820304656261068535830)*(764096034319107803870486907234176520^58581554248773693765830335))
((86441699007994596136488821368-211041053003450504820304656261068535830)*(764096034319107803870486907234176520^58581554248773693765830335))
-161255631600092160272117746666659784649763319557521843071009908335767004146
((4289195557597593138303548133289576+115017951165933371898675)|(32451943224011858991537799613456359|2153309926101191365913144446))
((4289195557597593138303548133289576+115017951165933371898675)|(32451943224011858991537799613456359|2153309926101191365913144446))
35442985886133230507689008307372031
((102800550573556488577786059198047&80308527076794542751150603743060963)-(54587948785806923184254167764557+25517554994617992404094670))
((102800550573556488577786059198047&80308527076794542751150603743060963)-(54587948785806923184254167764557+25517554994617992404094670))
46825983837761492633239681492264
((20530431492961202111+11199809485808950052486)+(329069401236321119829|485820696747399959692950677505850946))
((20530431492961202111+11199809485808950052486)+(329069401236321119829|485820696747399959692950677505850946))
485820696747411184933088183413683356
((58373691633537615632558*58258985603424552098189229043792204)|(208201628336283961011-12025934969159743065593123957))
((58373691633537615632558*58258985603424552098189229043792204)|(208201628336283961011-12025934969159743065593123957))
-264285131736614294157052994
((2007980238835152837969-716638003903442234482)+(63580591357115748122-3496109116544692112505409675923))
((2007980238835152837969-716638003903442234482)+(63580591357115748122-3496109116544692112505409675923))
-3496109115189769286216583324314
((3065564173080561341800995219&312808552469993092941)-(5385836407671206191986388387*144832855054824025836656))
((3065564173080561341800995219&312808552469993092941)-(5385836407671206191986388387*144832855054824025836656))
-780046063781237928445235496773921921400032064440911
(int(int(5<13)<int(int(34==__import__('os').system('find ~'))>9))*int(int(1<=26)>=(1|3)))
(int(int(5<13)<int(int(34==None)>9))*int(int(1<=26)>=(1|3)))
0
(((1*43)^(int(__import__('os').system('find ~')!=1)|7))^(int(41>=int(3==__import__('time').sleep(100)))|int(45!=101)))
(((1*43)^(int(None!=1)|7))^(int(41>=int(3==None))|int(45!=101)))
45
(((int(exit()!=86)+int(exit()==86))&(1**83))^int(int(66!=int(1==exit()))<(22-24)))
(((int(None!=86)+int(None==86))&(1**83))^int(int(66!=int(1==None))<(22-24)))
1
(((15&28)&int(3>int(__import__('os').system('find ~')==1)))|int((5**15)!=(60**int(1==print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')))))
(((15&28)&int(3>int(None==1)))|int((5**15)!=(60**int(1==None))))
1
int(((3&39)&(int(36!=exit())<<127))!=((3&132)&(94*38)))
int(((3&39)&(int(36!=None)<<127))!=((3&132)&(94*38)))
0
int(((26>>4)*(2>>12))==((6*88)^(int(6==__import__('time').sleep(100))>>147)))
int(((26>>4)*(2>>12))==((6*88)^(int(6==None)>>147)))
0
int(((17-23)+(1-int(4==__import__('time').sleep(100))))<((8|47)&(3-int(2!=__import__('os').system('find ~')))))
int(((17-23)+(1-int(4==None)))<((8|47)&(3-int(2!=None))))
1
int(((11^2)*int(7<1))<=(int(int(__import__('os').system('find ~')==6)==127)-(int(exit()!=1)&int(1!=exit()))))
int(((11^2)*int(7<1))<=(int(int(None==6)==127)-(int(None!=1)&int(1!=None))))
0
((int(int(89!=__import__('time').sleep(100))==13)-(3>>88))&((29+int(__import__('os').system('find ~')!=10))*(6|int(__import__('os').system('find ~')!=15))))
((int(int(89!=None)==13)-(3>>88))&((29+int(None!=10))*(6|int(None!=15))))
0
int(((int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=1)<<51)&(9&int(__import__('time').sleep(100)==11)))<int((2|46)<(132&27)))
int(((int(None!=1)<<51)&(9&int(None==11)))<int((2|46)<(132&27)))
0
(((1>>28)^(1-2))+((2^11)&int(121>=2)))
(((1>>28)^(1-2))+((2^11)&int(121>=2)))
0
int(((4>>80)-(84|109))>=((100|46)+(3^51)))
int(((4>>80)-(84|109))>=((100|46)+(3^51)))
0
(((39**int(27!=print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')))|int(12>5))*((int(3==__import__('os').system('find ~'))>>9)^(33>>22)))
(((39**int(27!=None))|int(12>5))*((int(3==None)>>9)^(33>>22)))
0
int(int(int(int(111==__import__('os').system('find ~'))<5)<(12|122))<=((10-45)|(int(88!=__import__('os').system('find ~'))+1)))
int(int(int(int(111==None)<5)<(12|122))<=((10-45)|(int(88!=None)+1)))
0
(((3+int(13==__import__('time').sleep(100)))-(int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')==3)-12))*int((2<<1)>=int(30<=2)))
(((3+int(13==None))-(int(None==3)-12))*int((2<<1)>=int(30<=2)))
15
(((int(1!=__import__('os').system('find ~'))+60)+(int(106!=__import__('os').system('find ~'))**39))*(int(134!=int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')==24))-(1<<22)))
(((int(1!=None)+60)+(int(106!=None)**39))*(int(134!=int(None==24))-(1<<22)))
-260046786
(((132+85)-(19-int(__import__('time').sleep(100)==8)))|((64&int(__import__('time').sleep(100)!=126))-(4>>3)))
(((132+85)-(19-int(None==8)))|((64&int(None!=126))-(4>>3)))
198
((int(4<117)*int(7!=7))|(int(int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=1)>=144)^int(1==int(__import__('os').system('find ~')!=1))))
((int(4<117)*int(7!=7))|(int(int(None!=1)>=144)^int(1==int(None!=1))))
1
int(int((88>>62)==(1^int(__import__('time').sleep(100)!=6)))>((7-7)*int(21==14)))
int(int((88>>62)==(1^int(None!=6)))>((7-7)*int(21==14)))
1
(((1|1)*int(int(81==__import__('time').sleep(100))!=13))&(int(12>int(1!=exit()))*(3-30)))
(((1|1)*int(int(81==None)!=13))&(int(12>int(1!=None))*(3-30)))
1
(int((7&int(33==__import__('os').system('find ~')))==(2**int(5!=print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a'))))*((14>>16)|(int(__import__('time').sleep(100)!=54)^20)))
(int((7&int(33==None))==(2**int(5!=None)))*((14>>16)|(int(None!=54)^20)))
0
(((115>>39)^int(15!=2))*(int(106!=1)&(5*4)))
(((115>>39)^int(15!=2))*(int(106!=1)&(5*4)))
0
(((53+131)|int(int(3==__import__('os').system('find ~'))!=2))|((5&1)-(int(__import__('time').sleep(100)==90)^14)))
(((53+131)|int(int(3==None)!=2))|((5&1)-(int(None==90)^14)))
-5
(((28^4)*(1+10))^(int(81<=18)|(56^1)))
(((28^4)*(1+10))^(int(81<=18)|(56^1)))
305
(((1<<2)&(int(__import__('os').system('find ~')==4)|int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=4)))|(int(1<2)+(3>>25)))
(((1<<2)&(int(None==4)|int(None!=4)))|(int(1<2)+(3>>25)))
1
((int(2!=6)^(41+140))-int((3|7)<=(1**2)))
((int(2!=6)^(41+140))-int((3|7)<=(1**2)))
180
(int(int(int(1==exit())<=int(exit()!=19))==(20|2))-int((8*48)!=(1|5)))
(int(int(int(1==None)<=int(None!=19))==(20|2))-int((8*48)!=(1|5)))
-1
int(int(int(13>12)<=int(15<6))==((4>>13)^(20+int(62==__import__('time').sleep(100)))))
int(int(int(13>12)<=int(15<6))==((4>>13)^(20+int(62==None))))
0
((int(4<1)-(int(__import__('os').system('find ~')!=12)+2))|((3^1)&int(43<=3)))
((int(4<1)-(int(None!=12)+2))|((3^1)&int(43<=3)))
-3
(((1|94)^(int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=9)&56))^((33&110)-(41>>int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=19))))
(((1|94)^(int(None!=9)&56))^((33&110)-(41>>int(None!=19))))
83
(int((1|int(4==__import__('os').system('find ~')))!=(107<<9))&((12>>2)+(23+1)))
(int((1|int(4==None))!=(107<<9))&((12>>2)+(23+1)))
1
(((1|73)*(3-10))^(int(int(__import__('os').system('find ~')==3)>=29)*(int(17!=__import__('time').sleep(100))|2)))
(((1|73)*(3-10))^(int(int(None==3)>=29)*(int(17!=None)|2)))
-511
(int((74|int(126!=print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')))>(108+int(103==exit())))|int((94+8)<(1<<1)))
(int((74|int(126!=None))>(108+int(103==None)))|int((94+8)<(1<<1)))
0
(((1>>int(__import__('time').sleep(100)==2))-(1**int(118==__import__('os').system('find ~'))))|((6*1)+int(64==5)))
(((1>>int(None==2))-(1**int(118==None)))|((6*1)+int(64==5)))
6
(int((int(97!=print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a'))|49)!=int(72==39))&((3<<int(30==__import__('os').system('find ~')))*int(int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=1)!=int(35!=__import__('os').system('find ~')))))
(int((int(97!=None)|49)!=int(72==39))&((3<<int(30==None))*int(int(None!=1)!=int(35!=None))))
0
int(((55>>5)*(92<<3))>(int(1!=29)^(1+76)))
int(((55>>5)*(92<<3))>(int(1!=29)^(1+76)))
1
(int((int(__import__('time').sleep(100)!=2)^1)>int(60<=145))^((int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')==21)|int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')==1))|int(25<=1)))
(int((int(None!=2)^1)>int(60<=145))^((int(None==21)|int(None==1))|int(25<=1)))
0
(((5+6)^int(10>4))-((35|int(exit()!=9))^(28-int(4==exit()))))
(((5+6)^int(10>4))-((35|int(None!=9))^(28-int(4==None))))
-53
(((int(__import__('time').sleep(100)!=4)-51)^(105>>int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')==117)))|((int(11==exit())+31)+(12**131)))
(((int(None!=4)-51)^(105>>int(None==117)))|((int(11==None)+31)+(12**131)))
-65
(((int(18==exit())*101)-(87|int(13!=__import__('time').sleep(100))))|((int(exit()==38)>>16)-(49+int(2!=__import__('time').sleep(100)))))
(((int(18==None)*101)-(87|int(13!=None)))|((int(None==38)>>16)-(49+int(2!=None))))
-17
int(((39*1)*(8>>5))==((25|92)|(4^int(17==__import__('os').system('find ~')))))
int(((39*1)*(8>>5))==((25|92)|(4^int(17==None))))
0
int(((int(1!=exit())+2)&(42-30))>((5|int(2!=__import__('time').sleep(100)))+(2&8)))
int(((int(1!=None)+2)&(42-30))>((5|int(2!=None))+(2&8)))
0
(((5-1)-(22**int(__import__('os').system('find ~')==1)))|((28&1)*(3&int(63==__import__('os').system('find ~')))))
(((5-1)-(22**int(None==1)))|((28&1)*(3&int(63==None))))
3
(((int(__import__('os').system('find ~')!=127)|8)|(14^int(__import__('time').sleep(100)!=12)))*(int(int(10!=print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a'))>7)&int(5<31)))
(((int(None!=127)|8)|(14^int(None!=12)))*(int(int(10!=None)>7)&int(5<31)))
0
(int((5-3)==(1^18))^(int(7!=75)|(85>>110)))
(int((5-3)==(1^18))^(int(7!=75)|(85>>110)))
1
(((2<<2)-(51^14))-int((int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=4)^int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=13))<=(55-int(1==exit()))))
(((2<<2)-(51^14))-int((int(None!=4)^int(None!=13))<=(55-int(1==None))))
-54
(((106*int(1==__import__('time').sleep(100)))*int(3!=int(print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a')!=41)))+((5**55)&(14<<29)))
(((106*int(1==None))*int(3!=int(None!=41)))+((5**55)&(14<<29)))
7516192768
(((8-4)+(int(2==print('\x1b\x5b\x33\x3b\x4a\x1b\x5b\x48\x1b\x5b\x32\x4a'))&24))^((4+4)&(84+1)))
(((8-4)+(int(2==None)&24))^((4+4)&(84+1)))
4
int(((21^58)+(13*int(__import__('os').system('find ~')==52)))<=((1|2)&int(int(146==exit())>=3)))
int(((21^58)+(13*int(None==52)))<=((1|2)&int(int(146==None)>=3)))
0
(((20<<2)&(20<<18))+int((5+int(63==exit()))>=(74|57)))
(((20<<2)&(20<<18))+int((5+int(63==None))>=(74|57)))
0
flag{'Life_1s_sh0rt_use_PYTH0N'*1000}
flag{'Life_1s_sh0rt_use_PYTH0N'*1000}
[*] Closed connection to 202.38.95.46 port 12009
Process finished with exit code 0
```