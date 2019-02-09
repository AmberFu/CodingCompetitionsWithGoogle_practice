```python
def generate_digitList(i):
    ilist = []
    while i >= 10:
        ilist.append(i%10)
        i = i // 10
    ilist.append(i)
    return ilist

def is_even_num(i):
    while i >= 10:
        iLastdig = i % 10
        if iLastdig % 2 != 0:
            return False
        i = i // 10
    return True if i % 2 == 0 else False


def is_even(n):
    return True if n % 2 == 0 else False

def generateNum(iList):
    i = 1
    ans = 0
    for n in iList:
        ans += (i*n)
        i *= 10
    return ans

def add_process(i):
    iList = generate_digitList(i)
    ilen = len(iList)
    afterodd = False
    roundUp = False
    for dig in range(ilen-1,-1,-1):
        iseven = is_even(iList[dig])
        if afterodd:
            iList[dig] = 0
        elif (afterodd==False) and (iseven==False):
            afterodd = True
            if iList[dig] == 9:
                if ilen-1 == dig:
                    # odd in first place: 900 -> 2000
                    iList.append(2)
                    iList[dig] = 0
                else:
                    ### befor dig must be even: 88892 -> 200000, 246892 -> 248000
                    iList[dig] = 0
                    ### loop to check if thenumber need to round up:
                    if iList[dig+1] == 8:
                        roundUp = True
                        for roundup_i in range(dig+1, ilen,1):
                            if roundUp and iList[roundup_i] == 8:
                                if roundup_i == ilen-1:
                                    ### The Last one is 8, so going to round up: 89 -> 200
                                    iList[roundup_i] = 0
                                    iList.append(2)
                                else:
                                    iList[roundup_i] = 0
                                    roundUp = True
                            elif roundUp and iList[roundup_i] != 8:
                                iList[roundup_i] += 2
                                roundUp = False
                    else:
                        iList[dig+1] += 2
            else:
                iList[dig] += 1
    nextEven = generateNum(iList)
    # print('nextEven (+) : ', nextEven)
    return nextEven-i

    
def minus_process(i):
    iList = generate_digitList(i)
    ilen = len(iList)
    afterodd = False
    for dig in range(ilen-1,-1,-1):
        iseven = is_even(iList[dig])
        if afterodd:
            iList[dig] = 8
        elif (afterodd==False) and (iseven==False):
            afterodd = True
            ### deal with odd: 122 -> 088
            iList[dig] -= 1
    nextEven = generateNum(iList)
    # print('nextEven (-) : ', nextEven)
    return i-nextEven
    
### main:
t = int(input())
for i in range(1, t + 1):
    add_ans = 0
    minus_ans = 0
    inputN = int(input())
    # print('---\ninputN: ', inputN)
    if is_even_num(inputN) == False:
        add_ans = add_process(inputN)
        minus_ans = minus_process(inputN)
    ans = add_ans if add_ans <= minus_ans else minus_ans
    print("Case #{}: {}".format(i, ans))
```

Practice Submissions: 
> Attempt 5 | check, check | Feb 10 2019, 00:14
>

Online IDE: https://www.jdoodle.com/python3-programming-online
