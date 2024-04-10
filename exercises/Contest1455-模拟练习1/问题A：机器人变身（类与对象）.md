# 问题 A: 机器人PK

## 题目描述

编写一个机器人类，包含属性有机器名、血量、攻击力、暴击率（出手时计算暴击，暴击造成双倍伤害）、闪避率（闪避时受攻击方免疫此次伤害）、被动技能（必定触发）：

- 对群机器人,机器名为aoe，血量为1000、攻击力为100、暴击率为百分之三十，闪避率为百分之十，被动技能：攻击对所有目标造成攻击力50%的伤害（可暴击）
- 对单机器人,机器名为sinle，血量为800、攻击力为80、暴击率为百分之三十，闪避率为百分之十，被动技能：暴击时可再次攻击（可连续触发，对方闪避时也可生效）
- 裂变机器人,机器名为fission，血量为500、攻击力为80、暴击率为百分之零，闪避率为百分之十，被动技能：机器人阵亡后，可分裂为3个迷你机器人（迷你机器人继承分裂前基础面板属性的五分之一，以及所有被动技能）

基础面板属性：血量、攻击力、暴击率、闪避率
机器人pk采用回合制，一回合中各个机器人随机顺序进行攻击，多目标时优先攻击血量最低单位，血量小于等于0时机器人阵亡，一方全部阵亡时对方获胜。

1、随机两个机器人（机器人类型可重复），进行1v1 pk；
2、随机抽取六个机器人（机器人类型可重复），进行3v3 pk；
3、(选做)现机器人功能升级，开发出了合体功能（合体基础面板属性叠加，被动技能全部合成）。随机抽取六个机器人（机器人类型可重复），随机分成两组，每组三个机器人进行合体，进行1v1 pk。


## 输入

无

## 输出

先输出对战机器人阵容以及其属性

输出回合数

输出对战后，机器人的状态

......

输出最后的胜利方



## 样例输出
```
X001--H--5--250--25--25
X002--D--5--25--25--50
X003--D--5--25--25--50
The number of robot transform is 2
```

## 优秀代码示例
```C++
// Author: 2016150143 黄鸿凯
#include <iostream>
#include <string>

using namespace std;

class Robot {
private:
    char type;
    int level;
    string name;
    int health, attack, defence;
public:
    // 构造函数。默认为普通机器人，默认等级为1
    Robot(string name, char type = 'N', int level = 1)
        :name(name), type(type), level(level) {
        // 调用init函数对机器人进行初始化
        init(type);
    }
    // 输出函数
    void print() {
        cout << name << "--" << type << "--" << level << "--" << health << "--" << attack << "--" << defence << endl;
    }
    char getType() { return type; }
    // 初始化机器人函数，会根据参数的值来决定机器人的类型
    void init(char type = 0) {
        // 如果没有参数，则按照现有的类型进行初始化
        if (type == 0) type = this->type;
        this->type = type;
        // 先按照普通机器人的标准初始化
        health = attack = defence = level * 5;
        // 然后根据具体类型赋予其不同的属性
        if (type == 'A') attack = level * 10;
        else if (type == 'D') defence = level * 10;
        else if (type == 'H') health = level * 50;
    }
};

// 机器人类型转换函数
bool change(Robot& a, char typeChanged){
    // 如果和当前类型相同则返回false
    if (a.getType() == typeChanged) return false;
    // 否则按照指定类型对机器人重新初始化
    a.init(typeChanged);
    return true;
}

int main(){
    int T;
    cin >> T;
    int ans = 0;
    while (T--) {
        string name;
        char type,typeChanged;
        int level;
        cin >> name >> type >> level >> typeChanged;
        Robot robot(name, type, level);
        if (change(robot, typeChanged)) ans++;
        robot.print();
    }
    cout << "The number of robot transform is " << ans << endl;
}
```
