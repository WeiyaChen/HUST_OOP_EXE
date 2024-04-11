# 问题 A: 机器人PK

## 题目描述

编写一个基础机器人类，包含属性有机器名、血量、攻击力、被动技能（必定触发）：

- 对群机器人,机器名为aoe，血量为1000、攻击力为100，被动技能：攻击对所有目标造成攻击力50%的伤害
- 对单机器人,机器名为single，血量为800、攻击力为80，被动技能：有50%概率可再次攻击


基础面板属性：血量、攻击力
机器人pk采用回合制，一回合中各个机器人随机顺序进行攻击，多目标时优先攻击血量最低单位，血量小于等于0时机器人阵亡，一方全部阵亡时对方获胜。

1、随机两个机器人（机器人类型可重复），进行1v1 pk；
2、（选做）现新增加一种机器人：裂变机器人,机器名为fission，血量为500、攻击力为80，被动技能：机器人阵亡后，可分裂为3个迷你机器人（迷你机器人继承分裂前基础面板属性的五分之一，以及除分裂外的所有被动技能）
随机两个机器人（机器人类型可重复），进行1v1 pk；
3、（选做）随机选六个机器人（机器人类型可重复），进行3v3 pk。



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
aoe血量剩余1000
single血量剩余1000

Round 1
aoe血量剩余920
single血量剩余750
```
aoe阵亡，single获胜

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
