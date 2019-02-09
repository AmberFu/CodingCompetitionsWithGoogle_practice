```python
def check_odd(i):
    last_dig = i % 10
    if i >= 10:
        if last_dig % 2 == 0:
            check_odd(i//10)
        else:
            return False
    elif last_dig % 2 == 0:
        return True
    else:
        return False

def generate_digitList(i):
    ilist = []
    while i >= 10:
        ilist.append(i%10)
        i = i // 10
    ilist.append(i)
    return ilist

def generateNum(iList):
    i = 1
    ans = 0
    for n in iList:
        ans += (i*n)
        i *= 10
    return ans

def add_process(i):
    # if N=86912, then Y=88000.
    iList = generate_digitList(i)
    ilen = len(iList)
    afterodd = False
    roundUp = False
    for dig in range(ilen-1,-1,-1):
        iseven = check_odd(iList[dig])
        if afterodd:
            iList[dig] = 0
        elif (afterodd==False) and (iseven==False):
            afterodd = True
            if iList[dig] == 9:
                if ilen-1 == dig:
                    # odd in first place: 900 -> 2000
                    iList.append(2)
                    # print("append 2 ", iList)
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
    print('nextEven (+) : ', nextEven)
    return nextEven-i

    
def minus_process(i):
    # if N=4436271, then X=4428888.
    iList = generate_digitList(i)
    ilen = len(iList)
    afterodd = False
    for dig in range(ilen-1,-1,-1):
        iseven = check_odd(iList[dig])
        if afterodd:
            iList[dig] = 8
        elif (afterodd==False) and (iseven==False):
            afterodd = True
            ### deal with odd: 122 -> 088
            iList[dig] -= 1
    nextEven = generateNum(iList)
    print('nextEven (-) : ', nextEven)
    return i-nextEven
    
### main:
# For example, if N=4436271, 
# then X=4428888. (-7383)
# others X=4440000 (+3729)
t = int(input())
add_ans = 0
minus_ans = 0
for i in range(1, t + 1):
    inputN = int(input())
    print('inputN: ', inputN)
    if not check_odd(inputN):
        add_ans = add_process(inputN)
        minus_ans = minus_process(inputN)
    ans = add_ans if add_ans <= minus_ans else minus_ans
    print("Case #{}: {}".format(i, ans))
```
https://www.jdoodle.com/python3-programming-online
