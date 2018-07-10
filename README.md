//
//  main.c
//  testC
//
//  Created by faterap on 16/8/3.
//  Copyright © 2016年 faterap. All rights reserved.
//

#include <stdio.h>
#include <string.h>
#include <stdlib.h>

char* add(char* number1, char* number2, char outBase);
char* minus(char* number1, char* number2, char outBase);
//char* bitsetAdd(char* b1, char* b2);
//char* bitsetMinus(    char* b1, char* b2);
int getSingleChBase(char number);
char* baseTransform(char* number, int oldBase, int newBase);


int main(int argc, const char * argv[]) {
//    char* ret = baseTransform("8", 10, 2);
    
    char* ret = (char*)malloc(sizeof(char) * 10);
    memset(ret,'0', sizeof(char) * 10);
    ret[0] = '1';
    ret[9] = 'o';
    printf("%s",ret);
    printf("\n");
    return 0;
}

char* calc(char opr, char* number1, char* number2, char outBase){
    switch (opr) {
        case '+':
            return add(number1, number2, outBase);
        case '-':
            return minus(number1, number2, outBase);
        default:
            return "";
    }
}

char* add(char* number1, char* number2, char outBase){
    int num1Base = getBase(number1);
    int num2Base = getBase(number2);
    
    char* num1 = (char*)malloc(1000);
    char* num2 = (char*)malloc(1000);
    
    // substring from position 1
    strncpy(num1, number1 + 1, strlen(number1 - 1));
    strncpy(num2, number2 + 1, strlen(number2 - 1));
    
    char* binary1,binary2;
    binary1 = num1Base != 'b'? baseTransform(num1, num1Base, 2):number1;
    binary2 = num2Base != 'b'? baseTransform(num2, num2Base, 2):number2;
    
//    char* binaryRet = bitsetAdd(binary1, binary2);
    char* binaryRet;
    
    // convert number to desired base
    return baseTransform(binaryRet, 2, getSingleChBase(outBase));
}

char* minus(char* number1, char* number2, char outBase){
    int num1Base = getBase(number1);
    int num2Base = getBase(number2);
    
    // substring from position 1
    char* num1 = (char*)malloc(1000);
    char* num2 = (char*)malloc(1000);
    
    strncpy(num1, number1 + 1, strlen(number1 - 1));
    strncpy(num2, number2 + 1, strlen(number2 - 1));
    
    char* binary1,binary2;
    binary1 = num1Base != 'b'? baseTransform(num1, num1Base, 2):number1;
    binary2 = num2Base != 'b'? baseTransform(num2, num2Base, 2):number2;
    
//    char* binaryRet = bitsetAdd(binary1, binary2);
    char* binaryRet;
    
    // convert number to desired base
    return baseTransform(binaryRet, 2, getSingleChBase(outBase));
}

//char* bitsetAdd(char* b1, char* b2){
//    int length
//    char* result;
//
//    int carry = 0;  // Initialize carry
//
//    // Add all bits one by one
//    for (int i = length-1 ; i >= 0 ; i--)
//    {
//        int firstBit = first.at(i) - '0';
//        int secondBit = second.at(i) - '0';
//
//        // boolean expression for sum of 3 bits
//        int sum = (firstBit ^ secondBit ^ carry)+'0';
//
//        result = (char)sum + result;
//
//        // boolean expression for 3-bit addition
//        carry = (firstBit & secondBit) | (secondBit & carry) | (firstBit & carry);
//    }
//
//    // if overflow, then add a leading 1
//    if (carry)
//        result = '1' + result;
//
//
//    return result;
//}

//char* bitsetMinus(char* b1, char* b2){
//
//}

int getBase(char * number){
    return getBase(number[0]);
}

int getSingleChBase(char number){
    switch (number) {
        case 'b':
            return 2;
        case 'o':
            return 8;
        case 'd':
            return 10;
        case 'x':
            return 16;;
        default:
            // illegal input, should print ERROR
            return 0;
    }
}

char* baseTransform(char* number, int oldBase, int newBase){
    int start[1000], ans[1000], res[1000];
    
    //1. 各个数位还原为数字形式
    int len = strlen(number);
    start[0] = len;
    for(int i=1;i<= len;i++)
    {
        if(number[i-1] >= '0' && number[i-1] <= '9')
        {
            start[i] = number[i-1] - '0';
        }
    }
    
    //2. transform
    memset(res,0,sizeof(res));//余数初始化为空
    int y,i,j;
    //模n取余法，(总体规律是先余为低位，后余为高位)
    while(start[0] >= 1)
    {//只要被除数仍然大于等于1，那就继续“模2取余”
        y=0;
        i=1;
        ans[0]=start[0];
        //
        while(i <= start[0])
        {
            y = y * oldBase + start[i];
            ans[i++] = y/newBase;
            y %= newBase;
        }
        res[++res[0]] = y;//这一轮运算得到的余数
        i = 1;
        //找到下一轮商的起始处
        while((i<=ans[0]) && (ans[i]==0)) i++;
        //清除这一轮使用的被除数
        memset(start,0,sizeof(start));
        //本轮得到的商变为下一轮的被除数
        for(j = i;j <= ans[0];j++)
            start[++start[0]] = ans[j];
        memset(ans,0,sizeof(ans)); //清除这一轮的商，为下一轮运算做准备
    }
    
    // 3. print result string
    char* ret = (char*)malloc(1000);
    memset(ret,'0', sizeof(ret));
    for(i = res[0];i >= 1;--i){
        ret[999] = '1';
    }
    
    return ret;
}

