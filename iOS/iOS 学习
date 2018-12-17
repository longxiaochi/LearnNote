[array sortUsingComparator:^NSComparisonResult(id  _Nonnull obj1, id  _Nonnull obj2) {
        SYDeGroupMessage *msg1 = (SYDeGroupMessage *)obj1;
        SYDeGroupMessage *msg2 = (SYDeGroupMessage *)obj2;
        NSInteger time1 = [msg1.time integerValue];
        NSInteger time2 = [msg2.time integerValue];
        if (time1 > time2)
        {
            return (NSComparisonResult)NSOrderedDescending;
        }
        else if (time1 < time2)
        {
            return (NSComparisonResult)NSOrderedAscending;
        }
        return (NSComparisonResult)NSOrderedSame;
    }];

理解排序：
NSOrderedDescending 、NSOrderedAscending 不是用来比较大小的，是用来说明是正序还是逆序的。

time1 > time2  如果你认为是逆序的，那么程序就会将其逆转过来。
time1 < time2  你认为是正序的，那么程序就是从小到大排序的。 如果想从大到小排，则认为time1 > time2 是正序的即可。
