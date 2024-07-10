# 1. Строка `s`, число `k`
Дается строка `s` и число `k`. Найти длину максимальной подстроки, на которой не более `k` различных букв. 

```python
s  = input()
k = int(input())

right = 0
uniq_cnt = 0
obs_max = 0

dee = {}

for left in range(len(s)):
    while right < n and uniq_cnt <= k:
        new = s[right]
        if new in dee:
            dee[new] += 1
            uniq_cnt += 1
        else:
            dee[new] = 1
        
        if right - left + 1 > obs_max:
            obs_max = right - left + 1
    
    if dee[s[left]] == 1:
        dee.pop(s[left])
        uniq_cnt -= 1
    else:
        dee[s[left]] -= 1
        
```

# 2. Строка-палиндром
# 3. Нулевая сумма подотрезка
Дается массив целых чисел `a_1, a_2, a_3, ..., a_n`. 
Отрезок назовем *хорошим*, если сумма чисел в нём равна нулю. Найдите максимальную по длине *хороший* подотрезок. 
Например, `1 2 -1 1 -1 -1` $\to$ `5`

$\blacktriangleright$ Главное: уследить, что если где-то префиксная сумма равна 0, это тоже надо учесть. Поэтому в начале массифа префиксных сумм положим сумму пустого кол-ва элементов - то есть 0. С таким псевдоэлементом подход "встретил число - посмотри, встречал ли его раньше" будет корректным всегда.

```python
nums = list(map(int, input().split()))

prefixsum = [0]
curmax = 0

for i, num in enumerate(nums):
    prefixsum.append(prefixsum[i] + num)
    
dee = {}

for i in range(len(nums) + 1):
    new = prefixsum[i]
    
    if new not in dee:
        dee[new] = [i, None]
        
    else:
        dee[new][1] = i
        
        if i == 0:
            bias = 0
        else:
            bias = 1
        
        if i - dee[new][0] + bias > curmax:
            curmax = i - dee[new][0] + bias
            
print(curmax)
```

# 4.  Подотрезки длины `k`
Найти количество подстрок длины `k`, состоящих из уникальных элементов.

```python
class Solution(object):
    def longestSubstring(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        dee = {}
        unique = 0
        n = len(s)

        appr_windows = 0

        for i in range(k):
            if s[i] in dee:
                dee[s[i]] += 1
            else:
                dee[s[i]] = 1
                unique += 1
 
        if unique == k:
            appr_windows += 1
    
        for i in range(1, n-k+1):
            if dee[s[i-1]] == 1:
                dee.pop(s[i-1])
                unique -= 1
            else:
                dee[s[i-1]] -= 1
     
            if s[i+k-1] in dee:
                dee[s[i+k-1]] += 1
            else:
                dee[s[i+k-1]] = 1
                unique += 1   
    
            if unique == k:
                appr_windows += 1
        
        return appr_windows
```

# 5. Хотя бы `k` повторов
Длиннейшая подстрока, в которой каждый символ повторяется хотя бы `k` раз.
Не доделал.
# 6. Бинарный массив, удалить один элик
Дается двоичный массив, найдите наиболее длинную подпоследовательность из 1, если нужно обязательно удалить 1 элемент в массиве.
**Эту задачу я решил на собеседовании у Гриши!**
`[1,1,0,1] -> 3`
`[0,1,1,1,0,1,1,0,1] -> 5`

```python
def solution(nums):
    n = len(nums)
    obs_max = 0
    current = 0
    zero_cnt = 0
    right = 0
    
    one_flag = True
#    zero_flag = True
    
#    for j in range(n):
#        if nums[j] == 0:
#            one_flag = False
#        if nums[j] == 1:
#            zero_flag = False
            
#    if one_flag:
#        return n-1
    
#    if zero_flag:
#        return 0
        
    
    for left in range(n):
        while right < n and zero_cnt < 2:
            
            if nums[right] == 0:
                one_flag = False
            
            if nums[right] == 0:
                zero_cnt += 1
            else:
                current += 1
                
        if nums[left] == 1:
            current -= 1
        else:
            zero_cnt -= 1
            
        obs_max = max(obs_max, current)
        
        
    if one_flag:
        return n-1
        
    return obs_max
```

# 7. Палиндром максимальной длины
Дается строка `s`. Найти максимальную по длине подстроку, которая является палиндромом. 
Например, для `abccbd` ответом будет `bccb`.
Задачка на ДП. Посмотрим потом.
# 8.  Один свап
Дается массив из целых чисел, вернуть `True`, если можно за один `swap` двух чисел сделать массив отсортированным (по возрастанию или по убыванию). В массиве могут быть повторы. *В итоге решал для случая, когда повторов нет.*
```python
def sorted_checker(arr):
    flag = True
    for j in range(1, len(arr)):
        if arr[j] < arr[j-1]:
            flag = False
    return flag

def one_swap(nums):
    
    ind_arr = []
    case_cnt = 0
    
    n = len(nums)
    if n == 1:
        return False
    
    for i in range(1, n):
        if nums[i] < nums[i-1]:
            case_cnt += 1
            ind_arr.append(i)
            
    if case_cnt == 2:
        nums[ind_arr[0]], nums[ind_arr[1]] = nums[ind_arr[1]], nums[ind_arr[0]]
        
        return sorted_checker(nums)
        
    if cnt == 1:
        nums[ind_arr[0]-1], nums[ind_arr[0]] = nums[ind_arr[0]], nums[ind_arr[0]-1]
        return sorted_checker(nums)
        
    return False
    
    
nums = list(map(int, input()))

result = one_swap(nums) or one_swap(nums[::-1])

print(result)         
```
# 9. Массив строк
Дается массив строк. Найдите такое минимальное число mn, что если мы оставим в каждой строке первые mn букв то все строки будут различными. 
(если длина строки меньше чем mn, мы берем всю строку) 

Два формата решения - зависят от того, как считается хеш.
Если мы в обычных условиях, и про хеш-функцию ничего не знаем, то у нас нет оснований считать, что она считается за $O(1)$ от строки любой длины - т. о., считаем хеш считается всё же за $O(maximum\_length)$, где $maximum\_length$ - длинна длиннейшей строки.
Тогда подойдёт линейный поиск, который исходно придумал я - с асимптотикой $O(m*maximum\_length)$:
```python
def min_prefix(strs):
    lens = [len(string) for string in strs]
    max_len  = max(lens)
    
    for i in range(max_len):
        unique = 0
        current_members = 0
        chars = {}
        
        for s in strs:
            if len(str) > i:
                current_members += 1
                
                if s[i] not in chars:
                    unique += 1
                    chars.add(s[i])
                    
        if unique == current_members:
            print(i+1)
            break
                    
                    
strs = []
r = int(input())

for _ in range(r):
    strs.append(input())
    
min_prefix(strs)
```

Но вот если считать, что хеш всегда считается за $O(1)$, можно выкатить решение, где мы *забинарим* нужную длину префикса и асимптотику получим $O(m*\log(maximum\_length)$.
Идею "забинарить" нам предлагают Поступашки.

```python
def checker(strs, i):
    unique = 0
    uniq_str = {}
    
    for s in strs:
        if s not in uniq_str:
            uniq_str.add(s)
            unique += 1
    return unique == len(strs)
        

def binary_min_pref(strs):
    lens = [len(string) for string in strs]
    max_len  = max(lens)
    
    l = 0
    r = max_len
    
    while l < r:
        m = (l+r)//2
        
        if checker(strs, m):
            r = m
        else:
            l = m + 1
    return l
```

Но вот беда: даже если предположить, что хеш, скажем, линеен по буквам строки - на более умную, чем тупой пересчёт с нуля, поправку хеша, мы тоже будем тратить  порядка $maximum\_length$ времени, ведь бинпоиск скачет довольно хаотично. Поэтому на самом деле асимптотика тут $O(m*maximum\_length*\log (maximum\_length)$, что хуже на логарифмический множитель по сравнению с тем, что предалагаю я.

А вот что Дима мне рассказал про умный хеш - что можно его в каком-то смысле циклическим сделать (как он говорит, как в Рабине-Карпе):
![[Pasted image 20240702215158.png]]
# 10. Чередующаяся подпоследовательность
Дается массив a. Найдите длину максимально чередующейся подпоследовательности. 
Максимальная чередующая подпоследовательность - это набор индексов `i1, i2, .., ik`, 
где `k` максимальное и `a[i1] < a[i2] > a[i3] < a[i4] > a[i5] < .... a[ik]`, а также `i1 < i2 < i3 < ... < ik`.

```python
def solver(a):
    n = len(a)
    
    for i in range(1, n):
        if a[i] > a[i-1]:
            start = i
            break
        else:
            return 1
            
    current = True
    cnt = 1
            
    for i in range(start+1, n):
        res = a[i] > a[i-1]
        
        if result == current:
            continue
        else:
            current = result
            cnt += 1 

    return cnt
```
# 11. Не слишком большое число
Дается массив положительных целых чисел `a` длины $n \leq 10^5$.
Набор чисел называется хорошим, если любое число из набора не больше суммы двух других чисел из набора.
Найдите хороший набор из массива с максимальной суммой. 
Например,
```
5
3 2 5 4 1 -> возьмём числа 3, 2, 5, 4


5
1 2 4 8 16 ->возьмем числа 8, 16
```

Мне помогли понять, что происходит, вот такие массивы:
```
[5, 4, 3, 2, 1]

[16, 9, 7, 1, 1, 1]

[6, 5, 3, 1]
```

Решение:
```python
def solver(a):
    a.sort()
    n = len(a)
    
    if n <= 2:
        return sum(a)
    
    right = 2
    obs_max = 0
    cur_sum = 0
    
    for left in range(n):
        cur_sum += a[left]
        
        if right - left < 2:
            right = left + 2
        
        while right < n and a[right] + a[right-1] > a[left]:
            cur_sum += a[right-1]
        
        obs_max = max(obs_max, cur_sum)   
        cur_sum -= a[left]
        
    return obs_max
```
Можно было обойтись и без префиксных сумм, просто аккуратно каждый раз обновлять `cur_sum`.  Но из-за кусочка кода:
```python
if right - left < 2:
            right = left + 2
```
это немножко летело, и я решил, дабы не заморачиваться, переписать под префиксумму (правда уже с 30-й по 40-ю минуту).
Правда, так мы стали $O(n)$ по памяти.

Итоговая сложность: $O(n\log n)$ по времени, $O(n)$ по памяти, но можно и версию с $O(1)$ памяти написать.
# 12. ~~Пельмени~~ кассы времени
[Задача с Шада](https://t.me/algoses/39).
Имеется n касс. Каждая касса работает определенное время работы. Найдите количество секунд, когда все кассы работают одновременно.
Формат времени работы каждой кассы определяется 6 числами:
`h, m, s, h1, m1, s1` (час, минута, секунда открытия и закрытие кассы соответственно).

```python
def solver(filename='input.txt'):
    latest_start = 0
    soonest_end = None
    
    with open(filename) as file:
        for line in file:
            h, m, s, h1, m1, s1 = list(map(int, line.split()))
            
            start = 3600*h + 60*m + s
            end = 3600*h1 + 60*m1 + s1
            
            latest_start = max(latest_start, start)
            
            if soonest_end == None:
                soonest_end = end
            else:
                soonest_end = min(soonest_end, end)
                
        if soonest_end < latest_start:
            return 0
        else:
            return soonest_end - latest_start
```

$O(n)$ времени, $O(1)$ памяти - даже не придётся сохранить весь этот массив.


# 13.  [что-то нерешённое](https://t.me/algoses/67)
```python
Задача ШАДа
Дают n отрезков (1 <= n <= 1e5).  Каждый отрезок это два числа l, r (1 <= l <= r <= 1e5).
Для каждого натурального числа x, которое состоит хотя бы в одном отрезке,
посчитать, в скольких отрезках содержится число x.

Например: 
5
1 10
2 9
3 8
4 7
5 5
Ответ 
1 1
2 2
3 3
4 4
5 5
6 4
7 4
8 3
9 2
10 1

Скажем, число 9 встречается только в первом и во втором отрезке. 

O(max(r-l))*O(n)*O(n)
числа    массивы  проверка

Вывод ответа: O(n)*O(max(r-l)) => быстрее просто нельзя

5 10
11 141

preparation:

nums: [(l, r), ..., (l, r)] -> sort by r -> sort by l 

exploring:
    
binary search by l (bisect_left) дичь
binary search by r (bisect_right) дичь

binary search by r (bisect_left) дичь

---> count --> otp_arr


def bisect_left_by_left(arr):
    l = 


def solver(arr):
    
    for arr in nums:
        for num in range(arr[0], arr[1]):
            if num in dee:
                dee[num] +=1
                continue
            else:
                dee[num] = 1
                
    print([key, dee[key] for key in sorted(dee.keys)])
    
    return
```
