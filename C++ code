#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <ctime>

using namespace std;

// 定义学生就餐卡
struct MealCard {
    string cardNumber;//卡号
    string name;//姓名
    double balance;//余额
    bool isLost;//状态
    string password;
    string time;
};

// 定义食堂菜品数据
struct MenuItem {
    string number;//编号
    string name;//名称
    double price;//金额
    int consumptionTimes;//消费次数
};


// 原型函数
void adminLogin();//管理员登录
void studentLogin();//学生登录
void registerMealCard();//注册学生就餐卡
void rechargeMealCard();//充值卡片
void reportLossMealCard();//挂失
void addMenuItem();//增加菜单
void deleteMenuItem();//减少菜单
void modifyMenuItem();//修改菜单
void searchMenuItem();//搜索菜单
void showSalesRanking();//展示销售余额
void showUsageStatistics();//展示消费记录
void showMenu();//展示菜单
void purchaseMenuItem(MealCard& mealCard);//购买菜品
void deleteCardnum();//直接删除卡号

// 容器全局变量
vector<MealCard> mealCards;
vector<MenuItem> menuItems;


int main() {
    //从文件加载餐卡和菜单项数据
    ifstream mealCardFile("mealCards.txt");//创建一个名为“mealCards.txt”的ifstream对象并将文件名"mealCards.txt"作为参数传递给它
    if (mealCardFile.is_open()) {//检查ifstream对象是否成功打开文件
        string cardNumber, name, balanceString, isLostString,time;//
        double balance;
        bool isLost;//定义变量
        while (mealCardFile >> cardNumber >> name >> balanceString >> isLostString >> time) {
            balance = stod(balanceString);//使用"mealCardFile"对象从文件中读取一行数据，并将读取到的数据存储到变量中。
            isLost = (isLostString == "true");//使用了一个三目运算符，如果"isLostString"等于"true"，则"isLost"将被设置为true，否则为false。
            MealCard mealCard = { cardNumber, name, balance, isLost,time};//读取并赋值
            mealCards.push_back(mealCard);//将这个对象添加到"mealCards"向量中。最后再添加的方法

        }
        mealCardFile.close();//关闭文件
    }

    ifstream menuItemFile("menuItems.txt");//创建一个名为“menuItems.txt”的ifstream对象并将文件名"mealCards.txt"作为参数传递给它
    if (menuItemFile.is_open()) {//检查ifstream对象是否成功打开文件
        string number, name, priceString, consumptionTimesString ;
        double price;
        int consumptionTimes;
        while (menuItemFile >> number >> name >> priceString >> consumptionTimesString ) {//将字符串类型的"priceString"和"consumptionTimesString"转换成double类型和int类型，分别存储到"price"和"consumptionTimes"变量中。
            price = stod(priceString);
            consumptionTimes = stoi(consumptionTimesString);//创建一个名为"menuItem"的对象，该对象包含四个属性：编号"number"、名称"name"、价格"price"和消费次数"consumptionTimes"。
            MenuItem menuItem = { number, name, price, consumptionTimes };//将"menuItem"对象添加到名为"menuItems"的vector容器中。
            menuItems.push_back(menuItem);//将这个对象添加到"mealItem"向量中。
        }
        menuItemFile.close();//关闭文件
    }

    // 主要显示区
    while (true) {
        cout << "       欢迎进入餐厅自动化管理系统!" << endl;
        cout << "🍳🥞🧇🥓🥩🍗🍖🌭🍔🍟🍕🥪🥙🌮🥗🥘"<<endl;
        showMenu();
        cout << "请选择你的身份:" << endl;
        cout << "1. 管理员（密码：admin）" << endl;
        cout << "2. 学生" << endl;
        cout << "3. 退出" << endl;
        cout << "只需要输入序号即可" <<endl;
        int choice;
        cout << "输入: ";
        cin >> choice;
        switch (choice) {
            case 1:
                adminLogin();//选1进入管理员界面
                break;
            case 2:
                studentLogin();//选2进入学生界面
                break;
            case 3:
                // 将餐卡和菜单项数据保存到文件中,写操作
                ofstream mealCardOutputFile("mealCards.txt");
                if (mealCardOutputFile.is_open()) {//检查是否打开界面
                    for (MealCard mealCard : mealCards) {
                        mealCardOutputFile << mealCard.cardNumber << " " << mealCard.name << " " << mealCard.balance << " " << mealCard.isLost << mealCard.time << endl;//将文件写入
                    }
                    mealCardOutputFile.close();//关闭文件
                }
                ofstream menuItemOutputFile("menuItems.txt");
                if (menuItemOutputFile.is_open()) {//检查是否打开界面
                    for (MenuItem menuItem : menuItems) {
                        menuItemOutputFile << menuItem.number << " " << menuItem.name << " " << menuItem.price << " " << menuItem.consumptionTimes << endl;//将文件写入
                    }
                    menuItemOutputFile.close();//关闭文件
                }
                cout << "感谢您使用食堂自动化管理系统，再见!" << endl;//结束餐厅自动管理系统系统
                cout << "🍜🍲🍛🍣🍱🥟🦪🍤🍥🥠🥮🍢🍡🍧🍨🍦" << endl;
                break;
            
        }
        
    }

    return 0;
}

// 管理员登录系统
void adminLogin() {
    string password;
    time_t now;
    struct tm *timenow;
    time(&now);//time函数读取现在的时间(国际标准时间非北京时间)，然后传值给now
    timenow = localtime(&now);//localtime函数把从time取得的时间now换算成电脑时间
    string BJshijian;
    BJshijian = asctime(timenow);
    cout << "请输入管理员密码:" << endl;
    cin >> password;
    if (password == "admin") {
        while (true) {
            cout << "欢迎进入管理员模式" << endl;
            cout << "当前北京时间：" << BJshijian <<endl;
            cout << "请选择您的需求:" << endl;
            cout << "1. 注册就餐卡\t\t\t";
            cout << "2. 充值就餐卡" << endl;
            cout << "3. 挂失就餐卡\t\t\t";
            cout << "4. 增加菜品选项" << endl;
            cout << "5. 删除菜品\t\t\t";
            cout << "6. 修改菜品" << endl;
            cout << "7. 搜索菜品\t\t\t";
            cout << "8. 显示销售排名" << endl;
            cout << "9. 使用情况统计\t\t";
            cout << "10. 展示菜单" << endl;
            cout << "11. 删除卡号\t\t\t";
            cout << "12. 退出" << endl;
            cout << "只需要输入序号即可" <<endl;
            cout << "输入： ";
            int choice;
            cin >> choice;
            switch (choice) {
                case 1:
                    registerMealCard();
                    break;
                case 2:
                    rechargeMealCard();
                    break;
                case 3:
                    reportLossMealCard();
                    break;
                case 4:
                    addMenuItem();
                    break;
                case 5:
                    deleteMenuItem();
                    break;
                case 6:
                    modifyMenuItem();
                    break;
                case 7:
                    searchMenuItem();
                    break;
                case 8:
                    showSalesRanking();
                    break;
                case 9:
                    showUsageStatistics();
                    break;
                case 10:
                    showMenu();
                    break;
                case 11:
                    deleteCardnum();
                    break;
                case 12:
                    cout << "成功退出." << endl;
                    return;
                default:
                    cout << "错误的选项,请重试." << endl;
                    break;
            }
        }
    } else {
        cout << "不正确的密码，请重试." << endl;
    }
    
}

// 学生登录系统
void studentLogin() {
string cardNumber;
    cout << "当前时间为:" << endl;
    time_t nowtime;
    time(&nowtime);
    tm* p = localtime(&nowtime);
    cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
cout << "请输入您的卡号:" << endl;//卡号登录
cin >> cardNumber;
string password;
cout << "请输入密码:" <<endl;//密码登录
cin >> password;
    
bool found = false;
for (MealCard& mealCard : mealCards) {
if (mealCard.cardNumber == cardNumber && mealCard.password == password) {
if (mealCard.isLost) {//检查用餐卡是否处于挂失状态
cout << "此就餐卡已挂失,登录失败." << endl;
return;
}
found = true;
cout << "你好, " << mealCard.name << ". 现余额为 " << mealCard.balance << "." << endl;//当前卡片状态
purchaseMenuItem(mealCard);//进入点餐环节
break;
}
}
if (!found) {//如果没有找到卡号或者密码匹配失败
cout << "没有找到你的卡号或密码错误,请重试." << endl;
}
}

// 注册卡
void registerMealCard() {
    MealCard mealCard;
    cout << "已经注册过的卡号:" << endl;
    for(MealCard existingMealCard : mealCards){
        cout << existingMealCard.cardNumber << " ";
    }
    cout << endl;
    cout << "请输入卡号:" << endl;
    cin >> mealCard.cardNumber;//注册时将卡号写入文件
    for (MealCard existingMealCard : mealCards) {
        if (existingMealCard.cardNumber == mealCard.cardNumber) {//判读是否卡号重复
            cout << "此卡号已经存在，注册失败." << endl;
            return;
        }
    }
    cout << "请输入密码" << endl;
    cin >> mealCard.password;//将密码写入文件
    cout << "请输入持卡人姓名:" << endl;
    cin >> mealCard.name;//将姓名写入文件
    cout << "请输入初始余额:" << endl;
    cin >> mealCard.balance;//将初始余额写入文件
    mealCard.isLost = false;//处于挂失状态则失败
    mealCards.push_back(mealCard);
    cout << "注册成功." << endl;
    cout << "注册时间:";
    time_t nowtime;
    time(&nowtime);
    tm* p = localtime(&nowtime);
    cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;}

// 充钱
void rechargeMealCard() {
    string cardNumber;
    cout << "请输入卡号以充值:" << endl;
    cin >> cardNumber;//要充值的卡号
    bool found = false;
    for (MealCard& mealCard : mealCards) {
        if (mealCard.cardNumber == cardNumber) {//判断卡号是否存在·
            found = true;
            double amount;
            cout << "请输入充值金额:" << endl;
            cin >> amount;
            mealCard.balance += amount;//增加余额
            cout << "充值成功！现余额: " << mealCard.balance << endl;
            cout << "充值时间:";
            time_t nowtime;
            time(&nowtime);
            tm* p = localtime(&nowtime);
            cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
            break;
        }
    }
    if (!found) {
        cout << "没有找到卡，请重试." << endl;
    }
}

// 挂失
void reportLossMealCard() {
    string cardNumber;
    cout << "请输入卡号以挂失:" << endl;
    cin >> cardNumber;//输入卡号挂失
    bool found = false;
    for (MealCard& mealCard : mealCards) {
        if (mealCard.cardNumber == cardNumber) {//判断是否存在卡号
            found = true;
            mealCard.isLost = true;//判断卡的状态是否为挂失状态
            cout << "成功挂失." << endl;
            cout << "挂失时间:";
            time_t nowtime;
            time(&nowtime);
            tm* p = localtime(&nowtime);
            cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
            break;
        }
    }
    if (!found) {
        cout << "没有找到卡，请重试." << endl;
    }
}

// 增加菜品
void addMenuItem() {
    MenuItem menuItem;
    cout << "请输入增加菜品的序号:" << endl;
    cin >> menuItem.number;//输入新增菜品的序号到文件里
    for (MenuItem existingMenuItem : menuItems) {
        if (existingMenuItem.number == menuItem.number) {
            cout << "此序号已经存在，增加失败." << endl;
            return;
        }
    }
    cout << "请输入菜名:" << endl;
    cin >> menuItem.name;
    cout << "请输入菜品价格:" << endl;
    cin >> menuItem.price;//输入菜品的名字以及价格
    menuItem.consumptionTimes = 0;
    menuItems.push_back(menuItem);
    cout << "成功增加菜品." << endl;
    cout << "增加时间:";
    time_t nowtime;
    time(&nowtime);
    tm* p = localtime(&nowtime);
    cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
    
}

// 删除菜品
void deleteMenuItem() {
    string number;
    cout << "请输入需要删除菜品的序号:" << endl;
    cin >> number;//输入菜品的序号
    bool found = false;
    for (int i = 0; i < menuItems.size(); i++) {
        if (menuItems[i].number == number) {//判断菜品序号是否存在
            found = true;
            menuItems.erase(menuItems.begin() + i);
            cout << "成功删除." <<endl;
            cout << "删除时间:";
            time_t nowtime;
            time(&nowtime);
            tm* p = localtime(&nowtime);
            cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
            break;
        }
    }
    if (!found) {
        cout << "没有找到菜品，请重试." << endl;
    }
}
void deleteCardnum() {//直接删除餐卡
    string number;
    cout << "请输入需要删除的卡号:" << endl;
    cin >> number;//输入菜品的序号
    bool found = false;
    for (int i = 0; i < mealCards.size(); i++) {
        if (mealCards[i].cardNumber == number) {//判断菜品序号是否存在
            found = true;
            mealCards.erase(mealCards.begin() + i);
            cout << "成功删除." <<endl;
            cout << "删除时间:";
            time_t nowtime;
            time(&nowtime);
            tm* p = localtime(&nowtime);
            cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
            break;
        }
    }
    if (!found) {
        cout << "没有找到卡，请重试." << endl;
    }
}
// 修改菜品
void modifyMenuItem() {
    string number;
    cout << "请输入要修改菜品的序号:" << endl;
    cin >> number;//输入菜品的序号
    bool found = false;
    for (MenuItem& menuItem : menuItems) {
        if (menuItem.number == number) {//判断菜品序号是否存在
            found = true;
            cout << "请输入新的名字 (current name: " << menuItem.name << "):" << endl;
            cin >> menuItem.name;
            cout << "请输入新的价格 (current price: " << menuItem.price << "):" << endl;
            cin >> menuItem.price;
            cout << "成功修改菜品." << endl;//修改菜品属性
            cout << "修改时间:";
            time_t nowtime;
            time(&nowtime);
            tm* p = localtime(&nowtime);
            cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
            break;
        }
    }
    if (!found) {
        cout << "没有找到菜品，请重试." << endl;
    }
}
// 寻找菜品
void searchMenuItem() {
    string keyword;
    cout << "请输入菜品的关键词:" << endl;
    cin >> keyword;//通过输入菜品关键词寻找菜品
    bool found = false;
    for (MenuItem menuItem : menuItems) {
        if (menuItem.name.find(keyword) != string::npos) {//用find函数对STL容器进行查找,npos作为find（）返回的类型
            found = true;
            cout << menuItem.number << " " << menuItem.name << " " << menuItem.price << " " << menuItem.consumptionTimes << endl;//输出寻找到的菜名
        }
    }
    if (!found) {
        cout << "没有找到." << endl;
    }
    cout << "寻找时间:";
    time_t nowtime;
    time(&nowtime);
    tm* p = localtime(&nowtime);
    cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
    
}
// 显示销售排名
void showSalesRanking() {
    vector<MenuItem> sortedMenuItems = menuItems;
    sort(sortedMenuItems.begin(), sortedMenuItems.end(), [](const MenuItem& a, const MenuItem& b) {//通过sort函数对给定区间进行前后排序
        return a.consumptionTimes > b.consumptionTimes;
    });
    cout << "销售排名:" << endl;
    cout <<"序号"<<"菜名"<<"价钱"<<"售量"<<endl;
    for (MenuItem menuItem : sortedMenuItems) {
        cout << menuItem.number << " " << menuItem.name << " " << menuItem.price << " " << menuItem.consumptionTimes << endl;//输出属性
        cout << "排名时间：";
        time_t nowtime;
        time(&nowtime);
        tm* p = localtime(&nowtime);
        cout << p->tm_year+1900 << "年" << p->tm_mon + 1 << "月" << p->tm_mday << "日 " << p->tm_hour << ":" << p-> tm_min << ":" << p-> tm_sec <<endl;
        
    }
}
// 展示消费记录
void showUsageStatistics() {
    double totalBalance = 0;
    int totalConsumptionTimes = 0;
    string cardnum,timenow,xiaofeijine,cainum;
    for (MealCard mealCard : mealCards) {
        totalBalance += mealCard.balance;
    }
    for (MenuItem menuItem : menuItems) {
        totalConsumptionTimes += menuItem.consumptionTimes;
    }
    for (MealCard mealItem : mealCards){
        cardnum = mealItem.cardNumber;
    }
    for (MealCard mealItem : mealCards){
        timenow = mealItem.time;
    }
    for (MenuItem menuItem : menuItems){
        cainum = menuItem.number;
    }
    for (MenuItem menuItem : menuItems){
        xiaofeijine = menuItem.price;
    }
    cout << "卡号为:" << cardnum << "的号主"<<endl;
    cout << "使用数据:" << endl;
    cout << "总余额: " << totalBalance << endl;
    cout << "总消费: " << totalConsumptionTimes << endl;
    
}
// 展示菜单
void showMenu() {
    cout << "餐厅菜单:" << endl;
    for (MenuItem menuItem : menuItems) {
        cout << menuItem.number << " " << menuItem.name << " " << menuItem.price << " " << menuItem.consumptionTimes << endl;
    }
}
// 购买菜品
void purchaseMenuItem(MealCard& mealCard) {
    showMenu();
    string number,timexiaofei;
    cout << "请输入你需要购买菜品的序号:" << endl;
    cin >> number;
    bool found = false;
    for (MenuItem& menuItem : menuItems) {
        if (menuItem.number == number) {
            found = true;
            if (mealCard.balance < menuItem.price) {
                cout << "余额不足，购买失败." << endl;
                return;
            } else {
                mealCard.balance -= menuItem.price;
                menuItem.consumptionTimes++;
                cout << "成功购买现余额: " << mealCard.balance << endl;
                time_t now;
                struct tm *timenow;
                time(&now);
                timenow = localtime(&now);
                timexiaofei = asctime(timenow);
                break;
            }
        }
    }
    if (!found) {
        cout << "没有找到，请重试." << endl;
    }
}

