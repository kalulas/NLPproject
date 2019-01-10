# NLP_final_project

### Brief Introduction

**./half_done：**

filename.txt 未处理文本材料

filename_sent.txt 几何并处理，分句后的文本材料



**./pre_treatment.py:** 

text_pre_treat():

文本材料预处理，删去空行，标题，页注，并去除空行，进行分句

输入为filename.txt，输出为filename_sent.txt

name_pre_treat():

人名素材处理 （HarryNameEnglish.txt -> HarryNameEnglishOuptu.txt）



**./human_name_extraction.py:** 

**name_dict**结构：{姓名，出场行数列表[]}

**family_name_dict**结构：{姓，[家族出场次数，[各个成员信息(姓名，出场行数列表)]]}

**appearance_dict**结构：{行数 / 行数±radius 区间段落，出现的角色}

**threshold:** [需要调参] 出现次数小于等于此值的姓名信息键值对将被视为无效

**appear_dict_radius:**[需要调参] 出场字典的投影范围大小

extract_name():

人名提取，并记录初次登场行数

family_name_extract():

根据提取到的人名得到所有姓氏

build_family_tree():

根据前两个函数得到的结果将提取到的姓名整合到家族字典树中

build_appearance_dict():

先创建 行数 -> 出现角色 的映射 appearance_dict

再把 当前行数±radius（闭区间）的出现角色映射到当前行数上来形成 appearance_proj_dict



**TODO**

1. 文本预处理: 分句，去除大标题，空行，替换符号，Page，归一一下放到pre里面去 **finished**
2. 异常的省略号导致的分句问题 **finished**
3. 对异常姓名的一个处理：提及次数就一两次的其实在关系提取中也不重要，可以直接把出现次数少的都筛选掉，建议在构建家族字典树之前对姓名字典进行筛选。**存在问题：**小样本下一个出现次数少的也可能拥有重要信息，比如一个人的人名只出现了一次。**finished**
4. 在提取姓名的时候不仅记录初次出场，应该记录所有出场，根据这些出场信息整合一个“行数->出现人员”之后再投影成“段落（几行）->出现人员”的字典 **finished**
5. 出场顺序的序列输出利用pandas **finished**，姓名字典的输出 **finished**，提及占比的词云 **finished**
6. 打印信息做成一个整合的参数可选的函数，输入参数字典可能不同 **finished**
7. 创建姓名字典进度条 **finished**
8. 添加选择文件读取功能 **finished**
9. 利用**出现投影字典**人物间关系亲密疏远的一个图/ 直接用gephi画图 save csv **halfdone**
10. 应该用标准化 **finished**
11. figure_selected 需要调参 做曲线
12. 关系提取开始

**参考：**

词云 http://www.cnblogs.com/Sinte-Beuve/p/7617517.html

关系可视化？http://www.cnblogs.com/Sinte-Beuve/p/7679392.html



---------人物关系部分--------

进行文本分类的特征提取需不需要去掉上下引号

需要找到人物共同出场部分的话可能记录所有出现句子序号会有用，不需要再次遍历 **确实这么做了**

PART1 根据两个人物出现在同一句话或同一段对话中的次数多少来判断联系强弱

PART2 根据关系找出人名实体对，再去找包含这个人名对的所有实例，利用这些实例来做关系提取

需要进行有监督学习，特征提取可以考虑使用词袋

<i>（见12/6课上CS224n内容）</i>



**2019/01/10**

异常姓名的筛选处理完毕

姓名字典name_dict改版

出场字典以及出场投影字典完成

交互cmd制作中



**2018/12/28**

现在pre_treatment.py可以做分句了，不过似乎需要修改（如省略号问题），book1.txt->book1_sent.txt

human_name_extraction.py现在输入输出格式为book1_sent.txt -> book1_sent_name_extract.txt

似乎分句后有得有失，具体表现还要自己检查



**2018/12/7**

Mr. Harry Potter / Miss Hermione Granger问题（在姓名提取中解决 **finished**

现在使用一个三元组来进行名字检测，具体判断逻辑见下方

现在名字识别已经几乎没有问题

![name_detect_logic](D:\QQPCmgr\Desktop\Courses\大三\能量炮\NLPproject\name_detect_logic.jpg)



**2018/12/6 [3] **

家族树建立 **finished**

human_name_extraction.py 

family_name_extract()根据name_dict得到所有可能的人物姓氏 代表家族

build_family_tree()根据name_dict与family_name_dict得到一个以 name:（mentioned, member）为成员的字典，其中member包含name_dict中信息，即初次登场，出场次数，以及名字

Mr/Mrs Ron Weasley处理（没在家族树里面）**finished**



**2018/12/6 [2]**

需要记录出现的 名 / 姓 / 姓名 / Mr Mrs Miss敬称 记录出场行数，记录出场次数 **finished**

现在human_name_extraction.py extract_name() 得到人物的初次登场，出场次数，以及名字

（存在姓/名/姓名/敬称重复）



**2018/12/6 [1] **

更新nltk语料库，自己添加语料不然无法复现（chap2载入自己语料库）**finished**

添加的姓名现在保存在HarryNameEnglishOutput_save.txt中，处理函数在PreTreatment.py中


