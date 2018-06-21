# DeadlockAlgorithm
四舍六入五成双算法。

> 规则

（1）被修约的数字小于5时，该数字舍去；
（2）被修约的数字大于5时，则进位；
（3）被修约的数字等于5时，要看5前面的数字，若是奇数则进位，若是偶数则将5舍掉，即修约后末尾数字都成为偶数；若5的后面还有不为“0”的任何数，则此时无论5的前面是奇数还是偶数，均应进位。

> 举例

9.8249=9.82, 9.82671=9.83
9.8350=9.84, 9.8351 =9.84
9.8250=9.82, 9.82501=9.83

> OC代码实现

```
- (double)get4s6rWithMoney:(double)money {
    if (money < 0.0) {
        return 0.0;
    }
    
    NSString *moneyStr = [NSString stringWithFormat:@"%lf", money];
    NSString *beforeDotNumber = [moneyStr componentsSeparatedByString:@"."].firstObject;
    NSString *dotNumber = [moneyStr componentsSeparatedByString:@"."].lastObject;
    NSInteger subDivision = dotNumber.intValue / (int)pow(10, 5) % 10;
    NSInteger percentile = dotNumber.intValue / (int)pow(10, 4) % 10;
    NSInteger thousands = dotNumber.intValue / (int)pow(10, 3) % 10;
    /// 判断5后面还是否是大于0的数
    BOOL isLarge = dotNumber.integerValue - subDivision * (int)pow(10, 5) - percentile * (int)pow(10, 4) - thousands * (int)pow(10, 3);
    
    NSString *tempStr = @"";
    if (thousands > 5) { // 进1
        tempStr = [NSString stringWithFormat:@"%.2f", beforeDotNumber.integerValue + subDivision / 10.0 + percentile / 100.0 + 0.01];
    } else if (thousands < 5) { // 舍去
        tempStr = [NSString stringWithFormat:@"%@.%ld%ld", beforeDotNumber, subDivision, percentile];
    } else {
        if (isLarge) { // 进1
            tempStr = [NSString stringWithFormat:@"%.2f", beforeDotNumber.integerValue + subDivision / 10.0 + percentile / 100.0 + 0.01];
        } else {
            if (percentile % 2 == 0 || percentile == 0) { // 偶数舍去
                tempStr = [NSString stringWithFormat:@"%@.%ld%ld", beforeDotNumber, subDivision, percentile];
            } else { // 进1
                tempStr = [NSString stringWithFormat:@"%.2f", beforeDotNumber.integerValue + subDivision / 10.0 + percentile / 100.0 + 0.01];
            }
        }
    }
    return dotNumber.doubleValue;
}
```
