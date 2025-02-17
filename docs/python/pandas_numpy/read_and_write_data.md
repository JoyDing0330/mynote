### Create a dataframe from list

``` py
list1 = ['A', 'B', 'C', 'D', 'E']
list2 = [1, 2, 3, 4, 5]

df = pd.DataFrame(list(zip(list1, list2)), columns =['Name', 'value'])
```

### Create a dataframe from dict

``` py
d = {f'getNthHighestSalary({N})': [111, 222, 333]}
df = pd.DataFrame(data=d)
```