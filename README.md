#include <stdio.h>
#include <string.h>
#include <conio.h>
#include <stdlib.h>
#include<Windows.h>
#define PERR(bSuccess, api){if(!(bSuccess)) printf("%s:Error %d from %s on line %d\n", __FILE__, GetLastError(), api, __LINE__);}
void MyCls(HANDLE);
void clrscr(void)
{
	HANDLE hStdOut = GetStdHandle(STD_OUTPUT_HANDLE);
	MyCls(hStdOut);
	return;
}
void MyCls(HANDLE hConsole)
{
	COORD coordScreen = { 0,0 };//设置清屏后光标返回的屏幕左上角坐标
	BOOL bSuccess;
	DWORD cCharsWritten;
	CONSOLE_SCREEN_BUFFER_INFO csbi;//保存缓冲区信息
	DWORD dwConSize;//当前缓冲区可容纳的字符数
	bSuccess = GetConsoleScreenBufferInfo(hConsole, &csbi);//获得缓冲区信息
	PERR(bSuccess, "GetConsoleScreenBufferInfo");
	dwConSize = csbi.dwSize.X * csbi.dwSize.Y;//缓冲区容纳字符数目
											  //用空格填充缓冲区
	bSuccess = FillConsoleOutputCharacter(hConsole, (TCHAR)' ', dwConSize, coordScreen, &cCharsWritten);
	PERR(bSuccess, "FillConsoleOutputCharacter");
	bSuccess = GetConsoleScreenBufferInfo(hConsole, &csbi);//获得缓冲区信息
	PERR(bSuccess, "ConsoleScreenBufferInfo");
	//填充缓冲区属性
	bSuccess = FillConsoleOutputAttribute(hConsole, csbi.wAttributes, dwConSize, coordScreen, &cCharsWritten);
	PERR(bSuccess, "FillConsoleOutputAttribute");
	//光标返回屏幕左上角坐标
	bSuccess = SetConsoleCursorPosition(hConsole, coordScreen);
	PERR(bSuccess, "SetConsoleCursorPosition");
	return;
}
//定义clrscr()

void chushi();
void stu_UI();
void admin_UI();
void search();
//函数声明

int A = 0;  //判断是否录入数据
char ID[11];//学生学号

void SetPos(int x, int y)
{
	COORD pos;
	HANDLE handle;
	pos.X = x;
	pos.Y = y;
	handle = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleCursorPosition(handle, pos);
}
//光标移动

HANDLE hInput;  /*  获取标准输入设备句柄 */
INPUT_RECORD inRec;/*  返回数据记录 */
DWORD numRead; /*  返回已读取的记录数 */
int Y, X;/* X和Y的坐标 */
int input()
{
	while (1) {
		COORD pos = { 0,0 };
		ReadConsoleInput(hInput, &inRec, 1, &numRead);
		pos = inRec.Event.MouseEvent.dwMousePosition;
		Y = (int)pos.Y;
		X = (int)pos.X;
		if (inRec.EventType == MOUSE_EVENT && inRec.Event.MouseEvent.dwButtonState == FROM_LEFT_1ST_BUTTON_PRESSED) /* 鼠标左键单击 */
		{
			if (X>30 && X<40 && Y == 0)
				return 1;
			else if (X>30 && X<45 && Y == 1)
				return 2;
			else if (X>30 && X<45 && Y == 2) 
				return 3;
			else if (X>30 && X<45 && Y == 3) 
				return 4;
			else if (X>30 && X<45 && Y == 4) 
				return 5;
			else if (X>30 && X<45 && Y == 5)
				return 6;
			else if (X>30 && X<45 && Y == 6) 
				return 7;
			else if (X>30 && X<45 && Y == 7) 
				return 8;
			else if (X>30 && X<45 && Y == 8) 
				return 9;
			else if (X>30 && X<45 && Y == 9)
				return 10;
			else if (X>30 && X<45 && Y == 10) 
				return 11;
			else if (X>30 && X<45 && Y == 11)
				return 12;
			else if (X>30 && X<45 && Y == 12) 
				return 13;
			else if (X>30 && X<45 && Y == 13) 
				return 14;
			else if (X>30 && X<45 && Y == 14) 
				return 15;
			else if (X>30 && X<45 && Y == 15) 
				return 16;
			else if (X>30 && X<45 && Y == 16) 
				return 17;
		}
	}
}

void HideCursor()//隐藏控制台的光标   
{
	CONSOLE_CURSOR_INFO cursor_info = { 1, 0 };
	SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cursor_info);
}

typedef struct The_users
{
	char id[11];//学号
	char pwd[20];//密码
}Users;
//账号密码结构体

typedef struct The_admin
{
	char pwd[20];//密码
}Admin;
//管理员密码结构体

void Create_users()
{
	FILE *fp;
	if ((fp = fopen("users.txt", "rb")) == NULL)                 //如果此文件不存在
	{
		if ((fp = fopen("users.txt", "wb+")) == NULL)
		{
			printf("无法建立文件!");
		}
	}
}
//学生账号密码文件

void revise_pwd()
{
	Users a, b;//a储存键入的账号信息，b储存从文件中读取的账号信息
	FILE *fp;
	fp = fopen("users.txt", "rt+");
	fread(&b, sizeof(struct The_users), 1, fp); //读入一个结构体字符块 到b
	while (1)
	{
		if (strcmp(ID, b.id) == 0) //输入学号与储存学号对比
		{
			break;
		}
		else
		{
			fread(&b, sizeof(struct The_users), 1, fp);
		}
	}
	fseek(fp, -sizeof(struct The_users), 1);
	strcpy(a.id, b.id);
	printf("请输入修改后的密码：");
	int x = 0;
	char c = 0;
	while (x < 20 && c != 13)   //13是回车符的ASCII码
	{
		c = getch();
		if (c != 13 && c != 8)
		{
			putchar('*');
			a.pwd[x++] = c;
		}
		else if (c == 8 && x>0)//实现退格
		{
			putchar('\b');
			putchar(' ');
			putchar('\b');
			x--;
		}
	}
	if (x == 0)
	{
		printf("\n密码不能为空!");
		printf("\n按任意键返回初始界面!");
		getch();
		clrscr();
		fclose(fp);
		chushi();
	}
	a.pwd[x] = '\0';
	x = 0;
	c = 0;
	char pwd_temp[20];
	printf("\n请再次输入密码:");
	while (x < 20 && c != 13)   //13是回车符的ASCII码
	{
		c = getch();
		if (c != 13 && c != 8)
		{
			putchar('*');
			pwd_temp[x++] = c;
		}
		if (c == 8 && x>0)//实现退格
		{
			putchar('\b');
			putchar(' ');
			putchar('\b');
			x--;
		}
	}
	pwd_temp[x] = '\0';
	if (strcmp(a.pwd, pwd_temp) == 0)
	{
		fwrite(&a, sizeof(struct The_users), 1, fp);
		printf("\n密码修改成功!");
		printf("\n按任意键返回初始界面!");
		getch();
		clrscr();
		fclose(fp);
		chushi();
	}
	else
	{
		printf("\n两次密码不一致!");
		printf("\n按任意键返回学生界面!");
		getch();
		clrscr();
		fclose(fp);
		stu_UI();
	}
}
//学生账号改密码

void revise_admin()
{
	Admin a, b;
	FILE *fp;
	fp = fopen("admin.txt", "rt+");
	printf("请输入修改后的密码：");
	int x = 0;
	char c = 0;
	while (x < 20 && c != 13)   //13是回车符的ASCII码
	{
		c = getch();
		if (c != 13 && c != 8)
		{
			putchar('*');
			a.pwd[x++] = c;
		}
		else if (c == 8 && x>0)//实现退格
		{
			putchar('\b');
			putchar(' ');
			putchar('\b');
			x--;
		}
	}
	if (x == 0)
	{
		printf("\n密码不能为空!");
		printf("\n按任意键返回管理界面!");
		getch();
		clrscr();
		fclose(fp);
		admin_UI();
	}
	a.pwd[x] = '\0';
	x = 0;
	c = 0;
	char pwd_temp[20];
	printf("\n请再次输入密码:");
	while (x < 20 && c != 13)   //13是回车符的ASCII码
	{
		c = getch();
		if (c != 13 && c != 8)
		{
			putchar('*');
			pwd_temp[x++] = c;
		}
		if (c == 8 && x>0)//实现退格
		{
			putchar('\b');
			putchar(' ');
			putchar('\b');
			x--;
		}
	}
	pwd_temp[x] = '\0';
	if (strcmp(a.pwd, pwd_temp) == 0)
	{
		fwrite(&a, sizeof(struct The_admin), 1, fp);
		printf("\n密码修改成功!");
		printf("\n按任意键返回初始界面!");
		getch();
		clrscr();
		fclose(fp);
		chushi();
	}
	else
	{
		printf("\n两次密码不一致!");
		printf("\n按任意键返回管理界面!");
		getch();
		clrscr();
		fclose(fp);
		admin_UI();
	}
}
//管理员改密码

void Create_admin()
{
	FILE *fp;
	char ch;
	Admin a;
	if ((fp = fopen("admin.txt", "rb")) == NULL)                 //如果此文件不存在
	{
		if ((fp = fopen("admin.txt", "wb+")) == NULL)
		{
			printf("无法建立文件!");
		}

	}
	fp = fopen("admin.txt", "rb+");
	ch = fgetc(fp);
	if (ch == EOF)
	{
		int x;
		char t = 48;
		for (x = 0; x < 6; x++)
		{
			a.pwd[x] = t + 1;
			t++;
		}
		a.pwd[x] = '\0';
		fwrite(&a, sizeof(struct The_admin), 1, fp);
	}
	fclose(fp);
}
//管理员密码文件

void registers()
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	int x = 0, y = 0;
	for (x = 0; x < 80; x++)
	{
		SetPos(x, y);
		printf("*");
		Sleep(20);
	}
	Create_users();
	Create_admin();
	SetPos(33, 0);
	printf("注\n");
	Sleep(200);
	SetPos(35, 0);
	printf("册\n");
	Sleep(200);
	SetPos(37, 0);
	printf("界\n");
	Sleep(200);
	SetPos(39, 0);
	printf("面\n");
	Users a, b;//a储存键入的账号信息，b储存从文件中读取的账号信息
	FILE *fp;
	fp = fopen("users.txt", "r");
	fread(&b, sizeof(struct The_users), 1, fp); //读入一个结构体字符块 到b
	SetPos(25,5);
	printf("请输入 学 号:");
	scanf("%s", &a.id);
	while (1)
	{
		if (strcmp(a.id, b.id)) //输入学号与储存学号对比
		{
			if (!feof(fp))    //未读取到最后一个账号继续读取对比
			{
				fread(&b, sizeof(struct The_users), 1, fp);
			}
			else
				break;
		}
		else
		{
			MessageBox(hwnd, "学号已被注册", "注册失败", 0);
			clrscr();
			fclose(fp);
			registers();
		}
	}
	SetPos(25, 6);
	printf("请输入 密 码:");
	int A = 0;
	char c = 0;
	while (A < 20 && c != 13)   //13是回车符的ASCII码
	{
		c = getch();
		if (c != 13 && c != 8)
		{
			putch('*');
			a.pwd[A++] = c;
		}
		else if (c == 8 && A>0)//实现退格
		{
			putchar('\b');
			putchar(' ');
			putchar('\b');
			A--;
		}
	}
	if (A == 0)
	{
		MessageBox(hwnd, "密码不能为空", "注册失败", 0);
		getch();
		clrscr();
		fclose(fp);
		registers();
	}
	a.pwd[A] = '\0';
	A = 0;
	c = 0;
	char pwd_temp[20];
	SetPos(25, 7);
	printf("再次输入密码:");
	while (A < 20 && c != 13)   //13是回车符的ASCII码
	{
		c = getch();
		if (c != 13 && c != 8)
		{
			putchar('*');
			pwd_temp[A++] = c;
		}
		if (c == 8 && A>0)//实现退格
		{
			putchar('\b');
			putchar(' ');
			putchar('\b');
			A--;
		}
	}
	pwd_temp[A] = '\0';
	if (strcmp(a.pwd, pwd_temp) == 0)
	{
		fp = fopen("users.txt", "a");
		fwrite(&a, sizeof(struct The_users), 1, fp);
		MessageBox(hwnd, "注册成功", "提示框", 0);
		clrscr();
		fclose(fp);
		chushi();
	}
	else
	{
		MessageBox(hwnd, "两次密码不一致", "注册失败", 0);
		clrscr();
		fclose(fp);
		registers();
	}
}
//注册系统

void  login()
{
	int x = 0, y = 0;
	for (x = 0; x < 80; x++)
	{
		SetPos(x, y);
		printf("*");
		Sleep(20);
	}
	SetPos(33, 0);
	printf("登\n");
	Sleep(200);
	SetPos(35, 0);
	printf("录\n");
	Sleep(200);
	SetPos(37, 0);
	printf("界\n");
	Sleep(200);
	SetPos(39, 0);
	printf("面\n");
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	Users a, b;//a储存键入的账号信息，b储存从文件中读取的账号信息
	FILE *fp;
	fp = fopen("users.txt", "r");
	fread(&b, sizeof(struct The_users), 1, fp); //读入一个结构体字符块 到b
	SetPos(28, 5);
	printf("学号:");
	scanf("%s", &a.id);
	while (1)
	{
		if (strcmp(a.id, b.id) == 0) //输入学号与储存学号对比
		{
			break;
		}
		else
		{
			if (!feof(fp))    //未读取到最后一个账号继续读取对比
			{
				fread(&b, sizeof(struct The_users), 1, fp);
			}
			else
			{
				MessageBox(hwnd, "此用户名不存在", "请重新登录", 0);
				clrscr();
				fclose(fp);
				chushi();
			}
		}
	}
	SetPos(28, 6);
	printf("密码:");
	int A = 0;
	char c = 0;
	while (A < 20 && c != 13)   //13是回车符的ASCII码
	{
		c = getch();
		if (c != 13 && c != 8)
		{
			putchar('*');
			a.pwd[A++] = c;
		}
		else if (c == 8 && A>0)//实现退格
		{
			putchar('\b');
			putchar(' ');
			putchar('\b');
			A--;
		}
	}
	if (A == 0)
	{
		MessageBox(hwnd, "密码为空", "请重新登录", 0);
		clrscr();
		fclose(fp);
		chushi();
	}
	a.pwd[A] = '\0';
	if (strcmp(a.pwd, b.pwd) == 0)            //如果密码匹配
	{
		fclose(fp);
		strcpy(ID, a.id);
		MessageBox(hwnd, "登录成功", "欢迎使用", 0);
		clrscr();
		fclose(fp);
		stu_UI();
	}
	else
	{
		MessageBox(hwnd, "密码错误", "请重新登录", 0);
		clrscr();
		fclose(fp);
		chushi();
	}
}
//登陆系统

void  admin_login()
{
	int x = 0, y = 0;
	for (x = 0; x < 80; x++)
	{
		SetPos(x, y);
		printf("*");
		Sleep(20);
	}
	SetPos(33, 0);
	printf("管\n");
	Sleep(200);
	SetPos(35, 0);
	printf("理\n");
	Sleep(200);
	SetPos(37, 0);
	printf("员\n");
	Sleep(200);
	SetPos(39, 0);
	printf("登\n");
	SetPos(41,0);
	printf("录\n");
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	Admin a, b;
	FILE *fp;
	fp = fopen("admin.txt", "r");
	fread(&b, sizeof(struct The_admin), 1, fp);
	SetPos(28, 6);
	printf("请输入密码:");
	int A = 0;
	char c = 0;
	while (A < 20 && c != 13)   //13是回车符的ASCII码
	{
		c = getch();
		if (c != 13 && c != 8)
		{
			putchar('*');
			a.pwd[A++] = c;
		}
		else if (c == 8 && A>0)//实现退格
		{
			putchar('\b');
			putchar(' ');
			putchar('\b');
			A--;
		}
	}
	a.pwd[A] = '\0';
	if (strcmp(a.pwd, b.pwd) == 0)            //如果密码匹配
	{
		fclose(fp);
		MessageBox(hwnd, "登录成功", "欢迎使用", 0);
		clrscr();
		fclose(fp);
		admin_UI();
	}
	else
	{
		MessageBox(hwnd, "密码错误", "请重新登录", 0);
		clrscr();
		fclose(fp);
		chushi();
	}
}
//管理员登录

typedef struct data
{
	char name[20];//姓名
	char id[20];//学号
	int Math;//数学成绩
	int Physics;//物理成绩
	int English;//英语成绩
	struct data *next;//下一个节点的指针
}STUDENT;
//学生数据链表

void Create_data()
{
	char ch;
	FILE *fp;
	if ((fp = fopen("data.txt", "rt")) == NULL)                 //如果此文件不存在
	{
		if ((fp = fopen("data.txt", "wt+")) == NULL)
		{
			printf("无法建立文件!");
		}
	}
	fp = fopen("data.txt", "rb+");
	ch = fgetc(fp);
	fclose(fp);
	if (ch == EOF)
	{
		A = 1;
		FILE *fp;
		fp = fopen("data.txt", "a+");
		STUDENT a_0;
		strcpy(a_0.name, "");
		strcpy(a_0.id, "");
		a_0.Math = 0;
		a_0.Physics = 0;
		a_0.English = 0;
		fwrite(&a_0, sizeof(struct data), 1, fp);
		fclose(fp);
	}
}
//检测有无历史文件

STUDENT *extraction()
{
	STUDENT *p, *head, *q;//head指针为链表的头结点，是找到链表的唯一依据，如果head指针丢失，那么整个链表就找不到了;p指针总是指向新申请的结点;q指针总是指向尾节点
	STUDENT temp;//定义结构体别名
	FILE *fp;
	p = (STUDENT *)malloc(sizeof(STUDENT));  // p指向新开辟的节点内存
	head = p;    //开辟头结点内存      头结点中没有学生成绩信息
	q = p;       //开辟尾节点内存   q指针总是指向尾节点
	q->next = NULL; // //标志链表的结束
	fp = fopen("data.txt", "rb+");
	while (fread(&temp, sizeof(STUDENT), 1, fp) != 0)//从文件中读结构体块
	{
		p = (STUDENT*)malloc(sizeof(STUDENT)); // p指向新开辟的节点内存
		strcpy(p->name, temp.name);
		strcpy(p->id, temp.id);
		p->Math = temp.Math;
		p->Physics = temp.Physics;
		p->English = temp.English;
		q->next = p;  //把新节点挂到原尾节点之后
		q = q->next;  //q指针指向新的尾节点
	}
	q->next = NULL;//标志链表的结束 
	fclose(fp);
	head = head->next;
	return head;
}
//读取文件到链表

void increase()
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	int x = 0, y = 0;
	for (x = 0; x < 80; x++)
	{
		SetPos(x, y);
		printf("*");
		Sleep(20);
	}
	Create_users();
	Create_admin();
	SetPos(33, 0);
	printf("信");
	Sleep(200);
	SetPos(35, 0);
	printf("息");
	Sleep(200);
	SetPos(37, 0);
	printf("录");
	Sleep(200);
	SetPos(39, 0);
	printf("入");
	Sleep(200);
	SetPos(41, 0);
	printf("界");
	Sleep(200);
	SetPos(43, 0);
	printf("面");
	FILE *fp;
	STUDENT a_0, a,b;
	strcpy(a.name, "");
	SetPos(25, 4);
	printf("请输入姓名：");
	scanf("%s", a.name);
	SetPos(25, 6);
	printf("请输入学号：");
	scanf("%s", a.id);
	SetPos(25, 8);
	printf("请输入数学成绩：");
	while (0 == scanf("%d", &a.Math))
	{
		while ('\n' != getchar())
		{
		}
		MessageBox(hwnd, "请勿输入非数字", "录入失败", 0);
		clrscr();
		admin_UI();
	}
	SetPos(25, 10);
	printf("请输入物理成绩：");
	while (0 == scanf("%d", &a.Physics))
	{
		while ('\n' != getchar())
		{
		}
		MessageBox(hwnd, "请勿输入非数字", "录入失败", 0);
		clrscr();
		admin_UI();
	}
	SetPos(25, 12);
	printf("请输入英语成绩：");
	while (0 == scanf("%d", &a.English))
	{
		while ('\n' != getchar())
		{
		}
		MessageBox(hwnd, "请勿输入非数字", "录入失败", 0);
		clrscr();
		admin_UI();
	}
	fp = fopen("data.txt", "r");
	fread(&b, sizeof(struct data), 1, fp); //读入一个结构体字符块 到b
	while (1)
	{
		if (strcmp(a.id, b.id)) //输入学号与储存学号对比
		{
			if (!feof(fp))    //未读取到最后一个账号继续读取对比
			{
				fread(&b, sizeof(struct The_users), 1, fp);
			}
			else
				break;
		}
		else
		{
			MessageBox(hwnd, "该学号已录入过信息", "录入失败", 0);
			clrscr();
			fclose(fp);
			admin_UI();
		}
	}
	fclose(fp);
	if (a.Math > 100 || a.Math < 0)
	{
		MessageBox(hwnd, "请输入正确的成绩（0-100）", "录入失败", 0);
		clrscr();
		admin_UI();
	}
	if (a.Physics > 100 || a.Physics < 0)
	{
		MessageBox(hwnd, "请输入正确的成绩（0-100）", "录入失败", 0);
		clrscr();
		admin_UI();
	}
	if (a.English > 100 || a.English < 0)
	{
		MessageBox(hwnd, "请输入正确的成绩（0-100）", "录入失败", 0);
		clrscr();
		admin_UI();
	}
	fp = fopen("data.txt", "a+");
	fwrite(&a, sizeof(struct data), 1, fp);
	fclose(fp);
	A = 0;
	MessageBox(hwnd, "录入成功", "提示框", 0);
	clrscr();
	admin_UI();
}
//添加学生信息

void Print_List(STUDENT *head)
{
	STUDENT *p;
	p = head->next; //跳过无数据的头结点
	printf(" 姓名    学号    数学成绩  物理成绩  英语成绩\n");
	while (p != NULL)
	{

		printf("%-6s ", p->name);
		printf("%3s ", p->id);
		printf("%6d ", p->Math);
		printf("%9d ", p->Physics);
		printf("%9d\n", p->English);
		p = p->next;//指向下一个节点      
	}
}
//链表的输出

void search_all(STUDENT *head)
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	STUDENT *p, *q, *T;
	T = head;
	for (p = head->next; p != NULL; p = p->next)
	{
		for (q = p->next; q != NULL; q = q->next)
		{
			if (strcmp(q->id, p->id)<0)
			{
				char name_t[20];
				strcpy(name_t, q->name);
				strcpy(q->name, p->name);
				strcpy(p->name, name_t);
				char id_t[20];
				strcpy(id_t, q->id);
				strcpy(q->id, p->id);
				strcpy(p->id, id_t);
				int t;
				t = q->Math;
				q->Math = p->Math;
				p->Math = t;
				t = q->Physics;
				q->Physics = p->Physics;
				p->Physics = t;
				t = q->English;
				q->English = p->English;
				p->English = t;
			}
		}
	}
	Print_List(T);
	MessageBox(hwnd, "可按任意键返回", "查询完成", 0);
	getch();
	clrscr();
	search();
}
//按学号从小大大排序

void search_id(STUDENT *head)
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	char t[20];
	printf("请输入要查询的学号：");
	scanf("%s", t);
	STUDENT *p;
	for (p = head;; p = p->next)
	{
		if (strcmp(t, p->id) == 0)
		{
			printf(" 姓名    学号    数学成绩  物理成绩  英语成绩\n");
			printf("%s ", p->name);
			printf("%3s ", p->id);
			printf("%7d ", p->Math);
			printf("%8d ", p->Physics);
			printf("%9d\n", p->English);
			break;
		}
		if (p->next == NULL)
		{
			MessageBox(hwnd, "未找到指定学生", "查询失败", 0);
			clrscr();
			search();
		}
	}
	MessageBox(hwnd, "可按任意键返回", "查询完成", 0);
	getch();
	clrscr();
	search();
}
//按学号查找

void search_name(STUDENT *head)
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	char t[20];
	printf("请输入要查询的姓名：");
	scanf("%s", t);
	STUDENT *p;
	for (p = head;; p = p->next)
	{
		if (strcmp(t, p->name) == 0)
		{
			printf(" 姓名    学号    数学成绩  物理成绩  英语成绩\n");
			printf("%s ", p->name);
			printf("%3s ", p->id);
			printf("%7d ", p->Math);
			printf("%8d ", p->Physics);
			printf("%9d\n", p->English);
			break;
		}
		if (p->next== NULL)
		{
			MessageBox(hwnd, "未找到指定学生", "查询失败", 0);
			clrscr();
			search();
		}
	}
	MessageBox(hwnd, "可按任意键返回", "查询完成", 0);
	getch();
	clrscr();
	search();
}
//按姓名查找

void search_Math(STUDENT *head)
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	STUDENT *p, *q, *T;
	T = head;
	for (p = head->next; p != NULL; p = p->next)
	{
		for (q = p->next; q != NULL; q = q->next)
		{
			if (q->Math>p->Math)
			{
				char name_t[20];
				strcpy(name_t, q->name);
				strcpy(q->name, p->name);
				strcpy(p->name, name_t);
				char id_t[20];
				strcpy(id_t, q->id);
				strcpy(q->id, p->id);
				strcpy(p->id, id_t);
				int t;
				t = q->Math;
				q->Math = p->Math;
				p->Math = t;
				t = q->Physics;
				q->Physics = p->Physics;
				p->Physics = t;
				t = q->English;
				q->English = p->English;
				p->English = t;
			}
		}
	}
	Print_List(T);
	MessageBox(hwnd, "可按任意键返回", "查询完成", 0);
	getch();
	clrscr();
	search();
}
//按数学成绩排序

void search_Physics(STUDENT *head)
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	STUDENT *p, *q, *T;
	T = head;
	for (p = head->next; p != NULL; p = p->next)
	{
		for (q = p->next; q != NULL; q = q->next)
		{
			if (q->Physics>p->Physics)
			{
				char name_t[20];
				strcpy(name_t, q->name);
				strcpy(q->name, p->name);
				strcpy(p->name, name_t);
				char id_t[20];
				strcpy(id_t, q->id);
				strcpy(q->id, p->id);
				strcpy(p->id, id_t);
				int t;
				t = q->Math;
				q->Math = p->Math;
				p->Math = t;
				t = q->Physics;
				q->Physics = p->Physics;
				p->Physics = t;
				t = q->English;
				q->English = p->English;
				p->English = t;
			}
		}
	}
	Print_List(T);
	MessageBox(hwnd, "可按任意键返回", "查询完成", 0);
	getch();
	clrscr();
	search();
}
//按物理成绩排序

void search_English(STUDENT *head)
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	STUDENT *p, *q, *T;
	T = head;
	for (p = head->next; p != NULL; p = p->next)
	{
		for (q = p->next; q != NULL; q = q->next)
		{
			if (q->English>p->English)
			{
				char name_t[20];
				strcpy(name_t, q->name);
				strcpy(q->name, p->name);
				strcpy(p->name, name_t);
				char id_t[20];
				strcpy(id_t, q->id);
				strcpy(q->id, p->id);
				strcpy(p->id, id_t);
				int t;
				t = q->Math;
				q->Math = p->Math;
				p->Math = t;
				t = q->Physics;
				q->Physics = p->Physics;
				p->Physics = t;
				t = q->English;
				q->English = p->English;
				p->English = t;
			}
		}
	}
	Print_List(T);
	MessageBox(hwnd, "可按任意键返回", "查询完成", 0);
	getch();
	clrscr();
	search();
}
//按英语成绩排序

void delete_0(STUDENT *head)
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	STUDENT b;//a储存键入的学生信息，b储存从文件中读取的学生信息
	FILE *fp,*ft;
	fp = fopen("data.txt", "rt+");
	ft=fopen("temp.txt","wt");
	if(fp==NULL || ft==NULL)
	{
        printf("错误！\n");
        return;
    }
	printf("请输入要删除的学号：");
	char id[20];
	scanf("%s", id);
	 //读入一个结构体字符块 到b
	while (fread(&b, sizeof(STUDENT), 1, fp))
	{
		if (strcmp(id, b.id) != 0) //输入学号与储存学号对比
		{
			fwrite(&b, sizeof(STUDENT), 1, ft);
		}
		else
		{
			
		}
	}
	fclose(fp);
	fclose(ft);
	char c;
	fp=fopen("data.txt", "w");
	ft=fopen("temp.txt","r");
	while ((c = fgetc(ft)) != EOF)//当读取的字符不是文件的结束符
    {
        fputc(c,fp);
    }
    fclose(ft);
    fclose(fp);
    remove("temp.txt");
	printf("\n删除成功!");
	printf("\n按任意键返回管理员界面!");
	getch();
	clrscr();
	admin_UI();
} 
 //删除学生数据 
  
void search_myself(STUDENT *head)
{
	STUDENT *p, *q, *T;
	for (p = head; p != NULL; p = p->next)
	{
		if (strcmp(ID, p->id) == 0)
		{
			printf(" 姓名    学号    数学成绩  物理成绩  英语成绩\n");
			printf("%s ", p->name);
			printf("%3s ", p->id);
			printf("%7d ", p->Math);
			printf("%8d ", p->Physics);
			printf("%9d\n", p->English);
			break;
		}
		if (p == NULL)
		{
			printf("未找到输入学号的学生数据\n");
			printf("\n按任意键返回查询界面!");
			getch();
			clrscr();
			search_id(extraction());
		}
	}
	printf("查询完毕!");
	printf("\n按任意键返回学生界面!");
	getch();
	clrscr();
	stu_UI();
}
//查询自己的成绩

void search()
{
	int rt;
	HideCursor(); //隐藏控制台的光标   
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	hInput = GetStdHandle(STD_INPUT_HANDLE); /*  输入设备句柄 */
	if (A == 1)
	{
		MessageBox(hwnd, "未录入数据", "查询失败", 0);
		clrscr();
		admin_UI();
	}
	int x = 0, y = 0;
	for (x = 0; x < 80; x++)
	{
		SetPos(x, y);
		printf("*");
		Sleep(20);
	}
	Create_users();
	Create_admin();
	SetPos(33, 0);
	printf("信");
	Sleep(200);
	SetPos(35, 0);
	printf("息");
	Sleep(200);
	SetPos(37, 0);
	printf("查");
	Sleep(200);
	SetPos(39, 0);
	printf("询");
	Sleep(200);
	SetPos(41, 0);
	printf("界");
	Sleep(200);
	SetPos(43, 0);
	printf("面");
	SetPos(30, 4);
	printf("显示全部学生数据");
	SetPos(33, 6);
	printf("按学号查询");
	SetPos(33, 8);
	printf("按姓名查询");
	SetPos(31, 10);
	printf("按数学成绩排序");
	SetPos(31, 12);
	printf("按物理成绩排序");
	SetPos(31, 14);
	printf("按英语成绩排序");
	SetPos(34, 16);
	printf("退出查询");
	while (1) {

		ReadConsoleInput(hInput, &inRec, 1, &numRead); /* 读取1个输入事件 */
		switch (inRec.EventType)
		{
		case MOUSE_EVENT: {
			rt = input();
			switch (rt)
			{
			case 5:
				clrscr();
				search_all(extraction());
				break;
			case 7:
				clrscr();
				search_id(extraction());
				break;
			case 9:
				clrscr();
				search_name(extraction());
				break;
			case 11:
				clrscr();
				search_Math(extraction());
				break;
			case 13:
				clrscr();
				search_Physics(extraction());
				break;
			case 15:
				clrscr();
				search_English(extraction());
				break;
			case 17:
				clrscr();
				admin_UI();
				break;

			}
		}
						  break;
		}
	}
}
//查询

void revise_data()
{
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	STUDENT a, b;//a储存键入的学生信息，b储存从文件中读取的学生信息
	FILE *fp;
	fp = fopen("data.txt", "rt+");
	printf("请输入要修改的成绩的学号：");
	scanf("%s", a.id);
	fread(&b, sizeof(STUDENT), 1, fp); //读入一个结构体字符块 到b
	while (1)
	{
		if (strcmp(a.id, b.id) == 0) //输入学号与储存学号对比
		{
			break;
		}
		else
		{
			fread(&b, sizeof(STUDENT), 1, fp);
		}
	}
	fseek(fp, -sizeof(STUDENT), 1);
	printf("该学生的成绩为\n");
	printf(" 姓名    学号    数学成绩  物理成绩  英语成绩\n");
	printf("%s ", b.name);
	printf("%3s ", b.id);
	printf("%7d ", b.Math);
	printf("%8d ", b.Physics);
	printf("%9d\n", b.English);
	printf("请输入修改后的成绩(不变则原样输入)\n");
	printf("数学成绩");
	scanf("%d", &a.Math);
	printf("\n物理成绩");
	scanf("%d", &a.Physics);
	printf("\n英语成绩");
	scanf("%d", &a.English);
	strcpy(a.name, b.name);
	fwrite(&a, sizeof(STUDENT), 1, fp);
	MessageBox(hwnd, "修改成功未", "", 0);
	getch();
	clrscr();
	fclose(fp);
	admin_UI();
}
//修改学生成绩

void chushi()
{
	int x = 0, y = 0;
	for (x = 0; x < 80; x++)
	{
		SetPos(x, y);
		printf("*");
		Sleep(20);
		}
	Create_users();
	Create_admin();
	SetPos(33, 0);
	printf("学");
	Sleep(200);
	SetPos(35, 0);
	printf("生");
	Sleep(200);
	SetPos(37, 0);
	printf("管");
	Sleep(200);
	SetPos(39, 0);
	printf("理");
	Sleep(200);
	SetPos(41, 0);
	printf("系");
	Sleep(200);
	SetPos(43, 0);
	printf("统");
	SetPos(35, 4);
	printf("学生注册");
	Sleep(200);
	SetPos(35, 6);
	printf("学生登陆");
	Sleep(200);
	SetPos(35, 8);
	printf("管理员登陆");
	int rt;
	HideCursor(); //隐藏控制台的光标   
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	hInput = GetStdHandle(STD_INPUT_HANDLE); /*  输入设备句柄 */
	while (1) {

		ReadConsoleInput(hInput, &inRec, 1, &numRead); /* 读取1个输入事件 */
		switch (inRec.EventType)
		{
		case MOUSE_EVENT: {
			rt = input();
			switch (rt)
			{
			case 5:
				clrscr();
				registers();
				break;
			case 7:
				clrscr();
				login();
				break;
			case 9:
				clrscr();
				admin_login();
				break;

			}
		}
						  break;
		}
	}
}
//初始界面

void stu_UI()
{
	Create_data();
	if (A == 1)
	{
		printf("尚未录入信息");
		printf("\n按任意键返回初始界面!");
		getch();
		clrscr();
		chushi();
	}
	printf("******************学生界面******************\n");
	printf("                按1 查询成绩\n");
	printf("                按2 修改密码\n");
	printf("                按3 退出登录\n");
	printf("********************************************\n");
	int a;
	scanf("%d", &a);
	switch (a)
	{
	case  1:search_myself(extraction());
	case  2:revise_pwd();
	case  3:clrscr(); chushi();
	}
}
//学生界面

void admin_UI()
{
	int x = 0, y = 0;
	for (x = 0; x < 80; x++)
	{
		SetPos(x, y);
		printf("*");
		Sleep(20);
	}
	Create_users();
	Create_admin();
	SetPos(33, 0);
	printf("管");
	Sleep(200);
	SetPos(35, 0);
	printf("理");
	Sleep(200);
	SetPos(37, 0);
	printf("员");
	Sleep(200);
	SetPos(39, 0);
	printf("界");
	Sleep(200);
	SetPos(41, 0);
	printf("面");
	Sleep(200);
	SetPos(43, 0);
	printf("统\n");
	Create_data();
	SetPos(32, 4);
	printf("添加学生信息\n");
	Sleep(200);
	SetPos(32, 6);
	printf("查看学生信息\n");
	Sleep(200);
	SetPos(32, 8);
	printf("修改学生成绩\n");
	Sleep(200);
	SetPos(32, 10);
	printf("删除学生数据");
	Sleep(200);
	SetPos(34, 12);
	printf("修改密码\n");
	Sleep(200);
	SetPos(34, 14);
	printf("退出登录\n");
	int rt;
	HideCursor(); //隐藏控制台的光标   
	HWND hwnd = FindWindow("ConsoleWindowClass", NULL);/*  控制台窗口句柄 */
	hInput = GetStdHandle(STD_INPUT_HANDLE); /*  输入设备句柄 */
	while (1) {

		ReadConsoleInput(hInput, &inRec, 1, &numRead); /* 读取1个输入事件 */
		switch (inRec.EventType)
		{
		case MOUSE_EVENT: {
			rt = input();
			switch (rt)
			{
			case 5:
				clrscr();
				increase();
				break;
			case 7:
				clrscr();
				search();
				break;
			case 9:
				clrscr();
				revise_data();
				break;
			case 11:
				clrscr();
				delete_0(extraction());
				break;
			case 13:
				clrscr();
				revise_admin();
				break;
			case 15:
				clrscr();
				chushi();
				break;
			}
		}
						  break;
		}
	}
}
//管理界面

int main()
{
	chushi();

}
学生管理系统
