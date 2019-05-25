# Definition:

**Abstraction**: The process of picking out (abstracting) common features of objects and procedures.
**Class**: A category of objects. The class defines all the common properties of the different objects that belong to it.
***Encapsulation***: The process of combining elements to create a new entity. A procedure is a type of encapsulation because it combines a series of computer instructions.
**Information hiding**: The process of hiding details of an object or function. Information hiding is a powerful programming technique because it reduces complexity.
***Inheritance***: a feature that represents the "is a" relationship between different classes.
**Interface**: the languages and codes that the applications use to communicate with each other and with the hardware.
**Messaging**: Message passing is a form of communication used in parallel programming and object-oriented programming.
**Object**: a self-contained entity that consists of both data and procedures to manipulate the data.
***Polymorphism***: A programming language's ability to process objects differently depending on their data type or class.
**Procedure**: a section of a program that performs a specific task.

# Example

    class Student(object):

        # __init__是一个特殊方法用于在创建对象时进行初始化操作
        # 通过这个方法我们可以为学生对象绑定name和age两个属性
        def __init__(self, name, age):
            self.name = name
            self.age = age

        def study(self, course_name):
            print('%s正在学习%s.' % (self.name, course_name))

        # PEP 8要求标识符的名字用全小写多个单词用下划线连接
        # 但是很多程序员和公司更倾向于使用驼峰命名法(驼峰标识)
        def watch_av(self):
            if self.age < 18:
                print('%s只能观看《熊出没》.' % self.name)
            else:
                print('%s正在观看岛国爱情动作片.' % self.name)



    def main():
        # 创建学生对象并指定姓名和年龄
        stu1 = Student('骆昊', 38)
        # 给对象发study消息
        stu1.study('Python程序设计')
        # 给对象发watch_av消息
        stu1.watch_av()
        stu2 = Student('王大锤', 15)
        stu2.study('思想品德')
        stu2.watch_av()


    if __name__ == '__main__':
        main()


    
    
    
# Inheritance
    class Person(object):
        """人"""

        def __init__(self, name, age):
            self._name = name
            self._age = age

        @property
        def name(self):
            return self._name

        @property
        def age(self):
            return self._age

        @age.setter
        def age(self, age):
            self._age = age

        def play(self):
            print('%s正在愉快的玩耍.' % self._name)

        def watch_av(self):
            if self._age >= 18:
                print('%s正在观看爱情动作片.' % self._name)
            else:
                print('%s只能观看《熊出没》.' % self._name)


    class Student(Person):
        """学生"""

        def __init__(self, name, age, grade):
            super().__init__(name, age)
            self._grade = grade

        @property
        def grade(self):
            return self._grade

        @grade.setter
        def grade(self, grade):
            self._grade = grade

        def study(self, course):
            print('%s的%s正在学习%s.' % (self._grade, self._name, course))


    class Teacher(Person):
        """老师"""

        def __init__(self, name, age, title):
            super().__init__(name, age)
            self._title = title

        @property
        def title(self):
            return self._title

        @title.setter
        def title(self, title):
            self._title = title

        def teach(self, course):
            print('%s%s正在讲%s.' % (self._name, self._title, course))


        def main():
            stu = Student('王大锤', 15, '初三')
            stu.study('数学')
            stu.watch_av()
            t = Teacher('骆昊', 38, '老叫兽')
            t.teach('Python程序设计')
            t.watch_av()


        if __name__ == '__main__':
            main()


  在上面的代码中，我们将Pet类处理成了一个抽象类，所谓抽象类就是不能够创建对象的类，这种类的存在就是专门为了让其他类去继承它。Python从语法层面并没有像Java或C#那样提供对抽象类的支持，但是我们可以通过abc模块的ABCMeta元类和abstractmethod包装器来达到抽象类的效果，如果一个类中存在抽象方法那么这个类就不能够实例化（创建对象）。上面的代码中，Dog和Cat两个子类分别对Pet类中的make_voice抽象方法进行了重写并给出了不同的实现版本，当我们在main函数中调用该方法时，这个方法就表现出了多态行为（同样的方法做了不同的事情）。
  
  
  # Another Interesting Example
  
    from abc import ABCMeta, abstractmethod
    from random import randint, randrange


    class Fighter(object, metaclass=ABCMeta):
        """战斗者"""

        # 通过__slots__魔法限定对象可以绑定的成员变量
        __slots__ = ('_name', '_hp')

        def __init__(self, name, hp):
            """初始化方法

            :param name: 名字
            :param hp: 生命值
            """
            self._name = name
            self._hp = hp

        @property
        def name(self):
            return self._name

        @property
        def hp(self):
            return self._hp

        @hp.setter
        def hp(self, hp):
            self._hp = hp if hp >= 0 else 0

        @property
        def alive(self):
            return self._hp > 0

        @abstractmethod
        def attack(self, other):
            """攻击

            :param other: 被攻击的对象
            """
            pass


    class Ultraman(Fighter):
        """奥特曼"""

        __slots__ = ('_name', '_hp', '_mp')

        def __init__(self, name, hp, mp):
            """初始化方法

            :param name: 名字
            :param hp: 生命值
            :param mp: 魔法值
            """
            super().__init__(name, hp)
            self._mp = mp

        def attack(self, other):
            other.hp -= randint(15, 25)

        def huge_attack(self, other):
            """究极必杀技(打掉对方至少50点或四分之三的血)

            :param other: 被攻击的对象

            :return: 使用成功返回True否则返回False
            """
            if self._mp >= 50:
                self._mp -= 50
                injury = other.hp * 3 // 4
                injury = injury if injury >= 50 else 50
                other.hp -= injury
                return True
            else:
                self.attack(other)
                return False

        def magic_attack(self, others):
            """魔法攻击

            :param others: 被攻击的群体

            :return: 使用魔法成功返回True否则返回False
            """
            if self._mp >= 20:
                self._mp -= 20
                for temp in others:
                    if temp.alive:
                        temp.hp -= randint(10, 15)
                return True
            else:
                return False

        def resume(self):
            """恢复魔法值"""
            incr_point = randint(1, 10)
            self._mp += incr_point
            return incr_point

        def __str__(self):
            return '~~~%s奥特曼~~~\n' % self._name + \
                '生命值: %d\n' % self._hp + \
                '魔法值: %d\n' % self._mp


    class Monster(Fighter):
        """小怪兽"""

        __slots__ = ('_name', '_hp')

        def attack(self, other):
            other.hp -= randint(10, 20)

        def __str__(self):
            return '~~~%s小怪兽~~~\n' % self._name + \
                '生命值: %d\n' % self._hp


    def is_any_alive(monsters):
        """判断有没有小怪兽是活着的"""
        for monster in monsters:
            if monster.alive > 0:
                return True
        return False


    def select_alive_one(monsters):
        """选中一只活着的小怪兽"""
        monsters_len = len(monsters)
        while True:
            index = randrange(monsters_len)
            monster = monsters[index]
            if monster.alive > 0:
                return monster


    def display_info(ultraman, monsters):
        """显示奥特曼和小怪兽的信息"""
        print(ultraman)
        for monster in monsters:
            print(monster, end='')


    def main():
        u = Ultraman('骆昊', 1000, 120)
        m1 = Monster('狄仁杰', 250)
        m2 = Monster('白元芳', 500)
        m3 = Monster('王大锤', 750)
        ms = [m1, m2, m3]
        fight_round = 1
        while u.alive and is_any_alive(ms):
            print('========第%02d回合========' % fight_round)
            m = select_alive_one(ms)  # 选中一只小怪兽
            skill = randint(1, 10)   # 通过随机数选择使用哪种技能
            if skill <= 6:  # 60%的概率使用普通攻击
                print('%s使用普通攻击打了%s.' % (u.name, m.name))
                u.attack(m)
                print('%s的魔法值恢复了%d点.' % (u.name, u.resume()))
            elif skill <= 9:  # 30%的概率使用魔法攻击(可能因魔法值不足而失败)
                if u.magic_attack(ms):
                    print('%s使用了魔法攻击.' % u.name)
                else:
                    print('%s使用魔法失败.' % u.name)
            else:  # 10%的概率使用究极必杀技(如果魔法值不足则使用普通攻击)
                if u.huge_attack(m):
                    print('%s使用究极必杀技虐了%s.' % (u.name, m.name))
                else:
                    print('%s使用普通攻击打了%s.' % (u.name, m.name))
                    print('%s的魔法值恢复了%d点.' % (u.name, u.resume()))
            if m.alive > 0:  # 如果选中的小怪兽没有死就回击奥特曼
                print('%s回击了%s.' % (m.name, u.name))
                m.attack(u)
            display_info(u, ms)  # 每个回合结束后显示奥特曼和小怪兽的信息
            fight_round += 1
        print('\n========战斗结束!========\n')
        if u.alive > 0:
            print('%s奥特曼胜利!' % u.name)
        else:
            print('小怪兽胜利!')


    if __name__ == '__main__':
        main()
        
        
        
        
        
        
        
        
        
        
        
            """
    某公司有三种类型的员工 分别是部门经理、程序员和销售员
    需要设计一个工资结算系统 根据提供的员工信息来计算月薪
    部门经理的月薪是每月固定15000元
    程序员的月薪按本月工作时间计算 每小时150元
    销售员的月薪是1200元的底薪加上销售额5%的提成
    """
    from abc import ABCMeta, abstractmethod


    class Employee(object, metaclass=ABCMeta):
        """员工"""

        def __init__(self, name):
            """
            初始化方法

            :param name: 姓名
            """
            self._name = name

        @property
        def name(self):
            return self._name

        @abstractmethod
        def get_salary(self):
            """
            获得月薪

            :return: 月薪
            """
            pass


    class Manager(Employee):
        """部门经理"""

        def get_salary(self):
            return 15000.0


    class Programmer(Employee):
        """程序员"""

        def __init__(self, name, working_hour=0):
            super().__init__(name)
            self._working_hour = working_hour

        @property
        def working_hour(self):
            return self._working_hour

        @working_hour.setter
        def working_hour(self, working_hour):
            self._working_hour = working_hour if working_hour > 0 else 0

        def get_salary(self):
            return 150.0 * self._working_hour


    class Salesman(Employee):
        """销售员"""

        def __init__(self, name, sales=0):
            super().__init__(name)
            self._sales = sales

        @property
        def sales(self):
            return self._sales

        @sales.setter
        def sales(self, sales):
            self._sales = sales if sales > 0 else 0

        def get_salary(self):
            return 1200.0 + self._sales * 0.05


    def main():
        emps = [
            Manager('刘备'), Programmer('诸葛亮'),
            Manager('曹操'), Salesman('荀彧'),
            Salesman('吕布'), Programmer('张辽'),
            Programmer('赵云')
        ]
        for emp in emps:
            if isinstance(emp, Programmer):
                emp.working_hour = int(input('请输入%s本月工作时间: ' % emp.name))
            elif isinstance(emp, Salesman):
                emp.sales = float(input('请输入%s本月销售额: ' % emp.name))
            # 同样是接收get_salary这个消息但是不同的员工表现出了不同的行为(多态)
            print('%s本月工资为: ￥%s元' %
                  (emp.name, emp.get_salary()))


    if __name__ == '__main__':
        main()
