## upper_bound和lower_bound用法（史上最全）

两者都是定义在头文件&lt;algorithm&gt;里。用**二分查找**的方法在一个排好序的数组中进行查找。既然是二分，时间复杂度就是O(logN)。

### 基础用法

```
upper_bound(begin, end, value)
```

在**从小到大**的排好序的数组中，在数组的 **[begin, end)** 区间中二分查找第一个**大于**value的数，找到返回该数字的地址，没找到则返回end。

```
lower_bound(begin, end, value)
```

在**从小到大**的排好序的数组中，在数组的 **[begin, end)** 区间中二分查找第一个**大于等于**value的数，找到返回该数字的地址，没找到则返回end。

### 用greater&lt;type&gt;()重载

```
upper_bound(begin, end, value, greater<int>())
```

在**从大到小**的排好序的数组中，在数组的 **[begin, end)** 区间中二分查找第一个**小于**value的数，找到返回该数字的地址，没找到则返回end。

```
lower_bound(begin, end, value, greater<int>())
```

在**从大到小**的排好序的数组中，在数组的 **[begin, end)** 区间中二分查找第一个**小于等于**value的数，找到返回该数字的地址，没找到则返回end。

前两部分的代码：

```
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
int main() {
    // 1.基础用法
    vector<int> increasing = {1,2,3,4,5};
    int pos = upper_bound(increasing.begin(), increasing.end(), 3) - increasing.begin();
    cout << increasing[pos] << endl;   // 4（第一个大于3的数）
    pos = lower_bound(increasing.begin(), increasing.end(), 3) - increasing.begin();
    cout << increasing[pos] << endl;   // 3（第一个大于等于3的数）
    bool res = upper_bound(increasing.begin(), increasing.end(), 5) == increasing.end();
    cout << res << endl;               // 1 (true，即：第一个大于5的数不存在)
    // 2.greater重载
    vector<int> decreasing = {5,4,3,2,1};
    pos = upper_bound(decreasing.begin(), decreasing.end(), 3, greater<int>()) - decreasing.begin();
    cout << decreasing[pos] << endl;   // 2（第一个小于3的数）
    pos = lower_bound(decreasing.begin(), decreasing.end(), 3, greater<int>()) - decreasing.begin();
    cout << decreasing[pos] << endl;   // 3（第一个小于等于3的数）
    res = lower_bound(decreasing.begin(), decreasing.end(), 0, greater<int>()) == decreasing.end();
    cout << res << endl;               // 1 (true，即：第一个小于等于0的数不存在)
    return 0;
}
```

到这里还是比较好理解的，不加greater是大于/大于等于，加上greater就是小于/小于等于。upper_bound是不带等号的，lower_bound就是带等号的。一般博客到这里就结束了，如果想要更加灵活的使用，那么请接着看。

### 进阶用法（自定义匿名函数）

#### upper_bound进阶

```
upper_bound(begin, end, value, cmp)
bool cmp(value, element)
```

upper_bound的第四个参数是自定义的匿名函数cmp，返回值为bool类型，cmp有两个参数，一个是value，对，你没看错，就是upper_bound的第3个参数value，另一个是element，也就是查找过程中与value比较的那个数。upper_bound返回的就是[begin, end)区间中第一个满足cmp(value, element)为true的数。下面看两个例子，注意看注释，方便理解。

```
pos = upper_bound(increasing.begin(), increasing.end(), 3, [](int value, int element) -> bool {
    return value < element;
}) - increasing.begin();           // 等价于基础用法中的第1句
cout << increasing[pos] << endl;   // 4（value是3，第一个大于value的数）
```

其中`[](int value, int element) -> bool {return value < element;}`就是匿名函数，对于c++匿名函数的用法这里就不说了，自行百度。

```
pos = upper_bound(increasing.begin(), increasing.end(), 3, [](int value, int element) -> bool {
    return value <= element;
}) - increasing.begin();           // 等价于基础用法中的第2句
cout << increasing[pos] << endl;   // 3（value是3，第一个大于等于value的数）
```

没想到，upper_bound竟然用出了lower_bound的效果！这就是自定义函数的优点了，使用灵活，这里只是举一个例子展示一下，对于更复杂的情况，比如在一个有序的vector&lt;vector&lt;int&gt;&gt;中，想查找第一个满足第2个元素大于value的vector，匿名函数就可以写成：

```
[](int value, vector<int> element) -> bool {return value < element[1];}
```

#### lower_bound进阶

```
lower_bound(begin, end, value, cmp)
bool cmp(element, value)
```

注意，lower_bound的匿名函数element和value的顺序反过来了！lower_bound返回的是[begin, end)区间中第一个使cmp(element, value)为**false**的数。

```
pos = lower_bound(increasing.begin(), increasing.end(), 3, [](int element, int value) -> bool {
    return element < value;
}) - increasing.begin();           // 等价于基础用法中的第2句
cout << increasing[pos] << endl;   // 3（value是3，第一个大于等于value的数）
```

```
pos = lower_bound(increasing.begin(), increasing.end(), 3, [](int element, int value) -> bool {
    return element <= value;
}) - increasing.begin();           // 等价于基础用法中的第1句
cout << increasing[pos] << endl;   // 4（value是3，第一个大于value的数）
```

可以看出，lower_bound也可以用出upper_bound的效果。对于更复杂的情况，读者可以自己探索。

以上，任何一个用法都需要有序，否则无法进行二分查找。

### 所有代码

```
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    // 1.基础用法
    vector<int> increasing = {1,2,3,4,5};

    int pos = upper_bound(increasing.begin(), increasing.end(), 3) - increasing.begin();
    cout << increasing[pos] << endl;   // 4（第一个大于3的数）

    pos = lower_bound(increasing.begin(), increasing.end(), 3) - increasing.begin();
    cout << increasing[pos] << endl;   // 3（第一个大于等于3的数）

    bool res = upper_bound(increasing.begin(), increasing.end(), 5) == increasing.end();
    cout << res << endl;               // 1 (true，即：第一个大于5的数不存在)

    // 2.greater重载
    vector<int> decreasing = {5,4,3,2,1};

    pos = upper_bound(decreasing.begin(), decreasing.end(), 3, greater<int>()) - decreasing.begin();
    cout << decreasing[pos] << endl;   // 2（第一个小于3的数）

    pos = lower_bound(decreasing.begin(), decreasing.end(), 3, greater<int>()) - decreasing.begin();
    cout << decreasing[pos] << endl;   // 3（第一个小于等于3的数）

    res = lower_bound(decreasing.begin(), decreasing.end(), 0, greater<int>()) == decreasing.end();
    cout << res << endl;               // 1 (true，即：第一个小于等于0的数不存在)

    // 3.进阶用法
    pos = upper_bound(increasing.begin(), increasing.end(), 3, [](int value, int element) -> bool {
        return value < element;
    }) - increasing.begin();           // 等价于基础用法中的第1句
    cout << increasing[pos] << endl;   // 4（value是3，第一个大于value的数）

    pos = upper_bound(increasing.begin(), increasing.end(), 3, [](int value, int element) -> bool {
        return value <= element;
    }) - increasing.begin();           // 等价于基础用法中的第2句
    cout << increasing[pos] << endl;   // 3（value是3，第一个大于等于value的数）

    pos = lower_bound(increasing.begin(), increasing.end(), 3, [](int element, int value) -> bool {
        return element < value;
    }) - increasing.begin();           // 等价于基础用法中的第2句
    cout << increasing[pos] << endl;   // 3（value是3，第一个大于等于value的数）

    pos = lower_bound(increasing.begin(), increasing.end(), 3, [](int element, int value) -> bool {
        return element <= value;
    }) - increasing.begin();           // 等价于基础用法中的第1句
    cout << increasing[pos] << endl;   // 4（value是3，第一个大于value的数）
    
    return 0;
}
```

