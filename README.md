## 实验内容

**学生成绩管理系统**
某班有最多不超过30人(具体人数由键盘输入)参加期末考试，最多不超过6门（具体门数由键盘输入）。请使用结构体数组、排序查找算法以及模块化程序设计方法编程实现如下菜单驱动的学生成绩管理系统：
1. 输入每个学生的学号、姓名和各科考试成绩；
2. 计算每门课程的总分和平均分；
3. 计算每个学生的总分和平均分；
4. 按每个学生的总分由高到低排出名次表；
5. 按每个学生的总分由低到高排出名次表；
6. 按学号由小到大排出成绩表；
7. 按姓名的字典顺序排出成绩表；
8. 按学号查询学生排名及其考试成绩；
9. 按姓名查询学生排名及其考试成绩；
10 按优秀(90~100分)、良好(80~89分)、中等(70~79分)、及格(60~69分)、不及格(0~59分)5个类别，对每门课程分别统计每个类别的人数以及所占的百分比；
11. 输出每个学生的学号、姓名、各科考试成绩，以及每门课程的总分和平均分；
12. 将每个学生的记录信息写入文件；
13. 从文件中读出每个学生的记录信息并显示。

## 实验要求

要求程序运行后先显示如下菜单，并提示用户输入选项

1. Input record
2. Calculate total and average score of every course
3. Calculate total and average score of every student
4. Sort in descending order by total score of every student
5. Sort in ascending order by total score of every student
6. Sort in ascending order by number
7. Sort in dictionary order by name
8. Search by number
9. Search by name
10. Statistic analysis for every course
11. List record
12. Write to a file
13. Read from file
0. Exit
Please enter your choice:

## 实验源码
```C
#define _CRT_SECURE_NO_WARNINGS 1
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#define MaxCourseNum 6
#define MaxStuNum 30
#define Max_ID 11
#define Max_Name 16
#define LevelNum 6
// 结构体
typedef struct student
{
    char ID[Max_ID];
    char Name[Max_Name];
    char Tag;
    float CourseScore[MaxCourseNum];
    float ScoreSum;
    float ScoreAvg;
} STU;

// 子函数声明
int Ascending(float a, float b);
int Descending(float a, float b);
void SwapFloat(float *a, float *b);
void SwapString(char a[], char b[]);

int menu(void);
void InputStudent(STU STUDENTS[], int *StuNum, int *CourseNum);
void SumAvg_CourseScore(STU STUDENTS[], int StuNum, int CourseNum);
void SumAvg_Stu(STU STUDENTS[], int StuNum, int CourseNum);
void Sort_ByTotalScore(STU STUDENTS[], int StuNum, int CourseNum, int (*compare)(float a, float b));
void Sort_ByID(STU STUDENTS[], int StuNum, int CourseNum);
void Sort_ByName(STU STUDENTS[], int StuNum, int CourseNum);
void Search_ByID(STU STUDENTS[], int StuNum, int CourseNum);
void Search_ByName(STU STUDENTS[], int StuNum, int CourseNum);

void StatisticAnalysis(STU STUDENTS[], int StuNum, int CourseNum);

void Print_StuIfo(STU STUDENTS[], int StuNum, int CourseNum);

void WritetoFile(STU STUDENTS[], int StuNum, int CourseNum);
void ReadfromFile(STU STUDENTS[], int *StuNum, int *CourseNum);
//-------------------------------主函数-------------------------------------------
int main(void)
{

    STU STUDENTS[MaxStuNum];
    int StuNum = 0, CourseNum = 0;
    int time = 0;
    int index;
    while (1)
    {
        index = menu();
        switch (index)
        {
        case 1: // 输入每个学生的学号、姓名和各科考试成绩
            InputStudent(STUDENTS, &StuNum, &CourseNum);
            break;

        case 2: // 计算每门课程的总分和平均分
            SumAvg_CourseScore(STUDENTS, StuNum, CourseNum);
            break;

        case 3: // 计算每个学生的总分和平均分
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);
            Print_StuIfo(STUDENTS, StuNum, CourseNum);
            break;

        case 4: // 按每个学生的总分由高到低排出名次表
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);

            Sort_ByTotalScore(STUDENTS, StuNum, CourseNum, Descending);
            printf("----------Sort by score in decending----------\n");
            Print_StuIfo(STUDENTS, StuNum, CourseNum);
            break;
        case 5: // 按每个学生的总分由低到高排出名次表
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);

            Sort_ByTotalScore(STUDENTS, StuNum, CourseNum, Ascending);
            printf("----------Sort by score in ascending----------\n");
            Print_StuIfo(STUDENTS, StuNum, CourseNum);
            break;
        case 6: // 按学号由小到大排出成绩表
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);

            Sort_ByID(STUDENTS, StuNum, CourseNum);
            printf("----------Sort by ID----------\n");
            Print_StuIfo(STUDENTS, StuNum, CourseNum);
            break;
        case 7: // 按姓名的字典顺序排出成绩表
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);

            Sort_ByName(STUDENTS, StuNum, CourseNum);
            printf("----------Sort by Name----------\n");
            Print_StuIfo(STUDENTS, StuNum, CourseNum);
            break;
        case 8: // 按学号查询学生排名及其考试成绩
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);

            Search_ByID(STUDENTS, StuNum, CourseNum);
            break;
        case 9: // 按姓名查询学生排名及其考试成绩
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);

            Search_ByName(STUDENTS, StuNum, CourseNum);
            break;
        case 10: // 按优秀(90~100分)、良好(80~89分)、中等(70~79分)、及格(60~69分)、不及格(0~59分)5个类别，对每门课程分别统计每个类别的人数以及所占的百分比
            StatisticAnalysis(STUDENTS, StuNum, CourseNum);
            break;
        case 11: // 输出每个学生的学号、姓名、各科考试成绩，以及每门课程的总分和平均分
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);
            Print_StuIfo(STUDENTS, StuNum, CourseNum);
            break;
        case 12: // 将每个学生的记录信息写入文件
            SumAvg_Stu(STUDENTS, StuNum, CourseNum);

            WritetoFile(STUDENTS, StuNum, CourseNum);
            break;
        case 13: // 从文件中读出每个学生的记录信息并显示
            ReadfromFile(STUDENTS, &StuNum, &CourseNum);
            Print_StuIfo(STUDENTS, StuNum, CourseNum);
            break;
        case 0:
            return 0;
        default:
            break;
        }
    }
    return 0;
}

//-------------------------------子函数-------------------------------------------
// RANK
int Ascending(float a, float b)
{

    return a < b;
}
int Descending(float a, float b)
{

    return a > b;
}
// SWAP
void SwapFloat(float *a, float *b)
{
    float t;
    t = *a;
    *a = *b;
    *b = t;
}
void SwapString(char a[], char b[])
{
    char t[Max_ID];
    strcpy(t, a);
    strcpy(a, b);
    strcpy(b, t);
}

// 菜单
int menu(void)
{

    printf("---------------------------Menu---------------------------\n");
    printf("\
1. Input record\n\
2. Calculate total and average score of every course\n\
3. Calculate total and average score of every student\n\
4. Sort in descending order by total score of every student\n\
5. Sort in ascending order by total score of every student\n\
6. Sort in ascending order by number\n\
7. Sort in dictionary order by name\n\
8. Search by number\n\
9. Search by name\n\
10.Statistic analysis for every course\n\
11.List record\n\
12.Write to a file\n\
13.Read from file\n\
0. Exit\n");
    printf("\nPlease enter your choice:\n");
    int index;
    scanf("%d", &index);
    return index;
}
// 学生数据输入
void InputStudent(STU STUDENTS[], int *StuNum, int *CourseNum)
{
    printf("----------------------Input----------------------\n");

    while (1)
    {
        printf("Input student number(n<=%d)：", MaxStuNum);
        scanf("%d", StuNum);
        printf("Input course number(n<=%d)：", MaxCourseNum);
        scanf("%d", CourseNum);
        if (*StuNum < 0 || *CourseNum < 0 || *StuNum > MaxStuNum || *CourseNum > MaxCourseNum)
        {
            printf("ERROR! Please Input agian\n");
            continue;
        }
        else
        {
            break;
        }
    }

    for (int i = 0; i < *StuNum; i++)
    {
        printf("Please input the information of Student %d\n", i + 1);
        printf("Input ID:\n");
        scanf(" %s", STUDENTS[i].ID);
        printf("Input Name:\n");
        scanf(" %s", STUDENTS[i].Name);

        for (int k = 0; k < *CourseNum; k++)
        {
            printf("Input Course %d:\n", k + 1);
            scanf("%f", &STUDENTS[i].CourseScore[k]);
        }
    }
}
// 课程总分与平均分
void SumAvg_CourseScore(STU STUDENTS[], int StuNum, int CourseNum)
{
    if (StuNum == 0)
    {
        printf("未找到数据，请先录入数据！\n");
        return;
    }
    else
    {
        printf("----------------------Course----------------------\n");
        float CourseSum[MaxCourseNum] = {0};
        for (int i = 0; i < CourseNum; i++)
        {
            for (int k = 0; k < StuNum; k++)
            {
                CourseSum[i] += STUDENTS[k].CourseScore[i];
                // printf("科目%d的%d人是:%10.2f\n", i+1,k+1, STUDENTS[k].CourseScore[i]);
            }
        }
        for (int i = 0; i < CourseNum; i++)
        {
            printf("The score of Course %d:%10.0f\taverage:%10.2f\n", i + 1, CourseSum[i], CourseSum[i] / StuNum);
        }
    }
}
// 学生总分与平均分
void SumAvg_Stu(STU STUDENTS[], int StuNum, int CourseNum)
{
    for (int i = 0; i < StuNum; i++)
    {
        STUDENTS[i].ScoreSum = 0;
        for (int k = 0; k < CourseNum; k++)
        {
            STUDENTS[i].ScoreSum += STUDENTS[i].CourseScore[k];
        }
        STUDENTS[i].ScoreAvg = STUDENTS[i].ScoreSum / CourseNum;
        // printf("stu_sum = %.2f\n", STUDENTS[i].ScoreSum);
    }
}
// 总分排序
void Sort_ByTotalScore(STU STUDENTS[], int StuNum, int CourseNum, int (*compare)(float a, float b))
{
    int i, j, index, k;
    for (i = 0; i < StuNum - 1; i++)
    {

        index = i;

        for (j = i + 1; j < StuNum; j++)
        {
            if ((*compare)(STUDENTS[j].ScoreSum, STUDENTS[index].ScoreSum))
            {
                index = j;
            }
        }
        if (index != i)
        {
            SwapString(STUDENTS[i].ID, STUDENTS[index].ID);
            SwapString(STUDENTS[i].Name, STUDENTS[index].Name);
            SwapFloat(&STUDENTS[i].ScoreSum, &STUDENTS[index].ScoreSum);
            SwapFloat(&STUDENTS[i].ScoreAvg, &STUDENTS[index].ScoreAvg);
            for (k = 0; k < CourseNum; k++)
            {
                SwapFloat(&STUDENTS[i].CourseScore[k], &STUDENTS[index].CourseScore[k]);
            }
        }
    }
}
// 按学号排序
void Sort_ByID(STU STUDENTS[], int StuNum, int CourseNum)
{
    int i, j, index, k;
    for (i = 0; i < StuNum - 1; i++)
    {
        index = i;
        for (j = i + 1; j < StuNum; j++)
        {
            if (strcmp(STUDENTS[index].ID, STUDENTS[j].ID) > 0)
            {
                index = j;
            }
        }
        if (index != i)
        {
            SwapString(STUDENTS[i].ID, STUDENTS[index].ID);
            SwapString(STUDENTS[i].Name, STUDENTS[index].Name);
            SwapFloat(&STUDENTS[i].ScoreSum, &STUDENTS[index].ScoreSum);
            SwapFloat(&STUDENTS[i].ScoreAvg, &STUDENTS[index].ScoreAvg);
            for (k = 0; k < CourseNum; k++)
            {
                SwapFloat(&STUDENTS[i].CourseScore[k], &STUDENTS[index].CourseScore[k]);
            }
        }
    }
}
// 按姓名排序
void Sort_ByName(STU STUDENTS[], int StuNum, int CourseNum)
{
    int i, j, index, k;
    for (i = 0; i < StuNum - 1; i++)
    {              // i=0,1,2,3
        index = i; // index=0
        for (j = i + 1; j < StuNum; j++)
        { // j=1-4
            if (strcmp(STUDENTS[index].Name, STUDENTS[j].Name) > 0)
            {
                index = j;
            }
        }
        if (index != i)
        {
            SwapString(STUDENTS[i].ID, STUDENTS[index].ID);
            SwapString(STUDENTS[i].Name, STUDENTS[index].Name);
            SwapFloat(&STUDENTS[i].ScoreSum, &STUDENTS[index].ScoreSum);
            SwapFloat(&STUDENTS[i].ScoreAvg, &STUDENTS[index].ScoreAvg);
            for (k = 0; k < CourseNum; k++)
            {
                SwapFloat(&STUDENTS[i].CourseScore[k], &STUDENTS[index].CourseScore[k]);
            }
        }
    }
}

// 按照学号搜索
void Search_ByID(STU STUDENTS[], int StuNum, int CourseNum)
{
    char SearchID[Max_ID];
    printf("Please input the ID you want to search:");
    // 使用fgets函数来读取输入的字符串，最多读取Max_ID-1个字符
    // fgets函数会把换行符也读入，所以要把它替换成字符串结束符'\0'
    getchar();
    fgets(SearchID, Max_ID, stdin);
    if (SearchID[strlen(SearchID) - 1] == '\n')
    {
        SearchID[strlen(SearchID) - 1] = '\0';
    }
    if (SearchID[0] == '\0')
    {
        printf("Invalid ID!\n");
        return;
    }
    printf("ID\tName\t");
    for (int k = 0; k < CourseNum; k++)
    {
        printf("Course%d\t", k + 1);
    }
    printf("Sum\tAverage\n");
    for (int i = 0; i < StuNum; i++)
    {
        if (strcmp(STUDENTS[i].ID, SearchID) == 0)
        {
            printf("%s\t%s\t", STUDENTS[i].ID, STUDENTS[i].Name);
            for (int j = 0; j < CourseNum; j++)
            {
                printf("%.0lf\t", STUDENTS[i].CourseScore[j]);
            }
            printf("%.0lf\t%.2lf\n", STUDENTS[i].ScoreSum, STUDENTS[i].ScoreAvg);
            return;
        }
    }
    printf("NOT FOUND!\n");
}
// 按照姓名搜索
void Search_ByName(STU STUDENTS[], int StuNum, int CourseNum)
{
    char SearchName[Max_ID];
    int found = 0;
    printf("Please input the Name you want to search:");
    // 使用fgets函数来读取输入的字符串，最多读取Max_ID-1个字符
    // fgets函数会把换行符也读入，所以要把它替换成字符串结束符'\0'
    getchar();
    fgets(SearchName, Max_Name, stdin);
    if (SearchName[strlen(SearchName) - 1] == '\n')
    {
        SearchName[strlen(SearchName) - 1] = '\0';
    }
    if (SearchName[0] == '\0')
    {
        printf("Invalid ID!\n");
        return;
    }
    printf("ID\tName\t");
    for (int k = 0; k < CourseNum; k++)
    {
        printf("Course%d\t", k + 1);
    }
    printf("Sum\tAverage\n");
    for (int i = 0; i < StuNum; i++)
    {
        if (strcmp(STUDENTS[i].Name, SearchName) == 0)
        {
            printf("%s\t%s\t", STUDENTS[i].ID, STUDENTS[i].Name);
            for (int j = 0; j < CourseNum; j++)
            {
                printf("%.0lf\t", STUDENTS[i].CourseScore[j]);
            }
            printf("%.0lf\t%.2lf\n", STUDENTS[i].ScoreSum, STUDENTS[i].ScoreAvg);
            found = 1;
        }
    }
    if (found == 0)
    {
        printf("NOT FOUND!\n");
    }
}

// 统计
void StatisticAnalysis(STU STUDENTS[], int StuNum, int CourseNum)
{
    int Level[LevelNum];

    for (int i = 0; i < CourseNum; i++)
    {
        memset(Level, 0, sizeof(Level));
        printf("Course %d\n", i + 1);
        for (int j = 0; j < StuNum; j++)
        {
            if (STUDENTS[j].CourseScore[i] >= 0 && STUDENTS[j].CourseScore[i] < 60)
            {
                Level[0]++;
            }
            else if (STUDENTS[j].CourseScore[i] < 70)
            {
                Level[1]++;
            }
            else if (STUDENTS[j].CourseScore[i] < 80)
            {
                Level[2]++;
            }
            else if (STUDENTS[j].CourseScore[i] < 90)
            {
                Level[3]++;
            }
            else if (STUDENTS[j].CourseScore[i] < 100)
            {
                Level[4]++;
            }
            else if (STUDENTS[j].CourseScore[i] == 100)
            {
                Level[5]++;
            }
        }
        printf("Level\tNumber\tProportion\n");
        for (int k = 0; k < LevelNum; k++)
        {
            if (k == 0)
            {
                printf("<60\t%d\t%.2f%%\n", Level[k], (float)Level[k] / StuNum * 100);
            }
            else if (k == 5)
            {
                printf("%d\t%d\t%.2f%%\n", (k + 5) * 10, Level[k], (float)Level[k] / StuNum * 100);
            }
            else
            {
                printf("%d-%d\t%d\t%2.f%%\n", (k + 5) * 10, (k + 5) * 10 + 9, Level[k], (float)Level[k] / StuNum * 100);
            }
        }
        printf("\n");
    }
}

// 打印学生信息
void Print_StuIfo(STU STUDENTS[], int StuNum, int CourseNum)
{
    int i, j;
    printf("ID\tName\t");
    for (i = 0; i < CourseNum; i++)
    {
        printf("Course%d\t", i + 1);
    }
    printf("Sum\tAverage\n");
    for (i = 0; i < StuNum; i++)
    {
        printf("%s\t%s\t", STUDENTS[i].ID, STUDENTS[i].Name);
        for (j = 0; j < CourseNum; j++)
        {
            printf("%.0lf\t", STUDENTS[i].CourseScore[j]);
        }
        printf("%.0lf\t%.2lf\n", STUDENTS[i].ScoreSum, STUDENTS[i].ScoreAvg);
    }
}

// 输出到txt
void WritetoFile(STU STUDENTS[], int StuNum, int CourseNum)
{
    FILE *fp;
    if ((fp = fopen("student.txt", "w")) == NULL)
    {
        printf("Fail to open");
        exit(0);
    }
    fprintf(fp, "%d\t%d\n", StuNum, CourseNum);
    for (int i = 0; i < StuNum; i++)
    {
        fprintf(fp, "%10s%15s", STUDENTS[i].ID, STUDENTS[i].Name);
        for (int j = 0; j < CourseNum; j++)
        {
            fprintf(fp, "%10.0f", STUDENTS[i].CourseScore[j]);
        }
        fprintf(fp, "%10.0f%10.2f\n", STUDENTS[i].ScoreSum, STUDENTS[i].ScoreAvg);
    }
    fclose(fp);
}
// 读入txt
void ReadfromFile(STU STUDENTS[], int *StuNum, int *CourseNum)
{
    FILE *fp;
    if ((fp = fopen("student.txt", "r")) == NULL)
    {
        printf("Fail to open");
        exit(0);
    }
    fscanf(fp, "%d\t%d", StuNum, CourseNum);
    for (int i = 0; i < *StuNum; i++)
    {
        fscanf(fp, "%10s", STUDENTS[i].ID);
        fscanf(fp, "%15s", STUDENTS[i].Name);

        for (int j = 0; j < *CourseNum; j++)
        {
            fscanf(fp, "%10f", &STUDENTS[i].CourseScore[j]);
        }
        fscanf(fp, "%10f%10f\n", &STUDENTS[i].ScoreSum, &STUDENTS[i].ScoreAvg);
    }
    fclose(fp);
}

```
