## TableView 简单学习

### 介绍tableview常用的方法。

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView

    上面方法用于指定整个tableView有多少个块(section)。

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)
    
    上面的方法，返回每个块（section）中有多少行。

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
    
    上面方法，为tableview提供cell, 根据indexPath.section、indexPath.row 来指定哪块(section)中的哪一行（rows) 的cell。

- (nullable NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section

    上面方法，用于设置tableview的Header的标题。如果希望在头部添加View, 则可以使用- (nullable UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section这个方法。

- (nullable NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section

    上面方法，用于设置tableview的footer的标题。如果希望在尾部添加View, 则可以使用- (nullable UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section这个方法。

- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath

    上面方法，用于设置tableView的cell是否支持编辑模式，可根据indexPath.section indexPath.row来为每一行设置。在设置[tableview setEdit:YES] 进入编辑模式。


- (BOOL)tableView:(UITableView *)tableView canMoveRowAtIndexPath:(NSIndexPath *)indexPath

    上面方法与tableview cell的移动相关，使用较少，以后再说。


- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath

    上面方法，用于设置tableview 每一行cell的高度。

- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section

    上面方法，用于设置tableview 每个section的headerView的高度。

- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section

    上面方法，用于设置tableview 每个section的footerView的高度。

- (nullable UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section

    上面方法，用于设置tableview 每个section的HeaderView, 可以自定义view，此时titleForHeaderInSection 这个方法无效。

- (nullable UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section

    上面方法，用于设置tableview 每个section的FooterView, titleForFooterInSection 这个方法无效。

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath 

    上面方法，在tableview的cell被点击时调用，根据indexPath.section, indexPath.row来定位哪一个位置。


### 注意点
- 设置tableview的headerView只需要设置高度即可。
- tableview的HeaderView和FooterView 和Section的 headerView与footerView 在两种模式下的差异：
    - UITableViewStylePlain: tableView的headerView和footerView会随着tableview的滑动而滚动。但是每个块（section）的headerview和footerview 滑动时会停留在顶部或者底部。

    - UITableViewStyleGrouped：无论tableview的headerView或者是块(section)的headerView 都会随着tableview的滚动而滚动。footerView与一样。

- tableview 的样式 UITableViewStylePlain 和 UITableViewStyleGrouped的区别
    - UITableViewStylePlain

      1. 有多个块时，头部自带停留效果。(默认)
      2. 多个块时，section之间默认没有间距(需要的话，可以通过设置headerView和footerView来实现)


    - UITableViewStyleGrouped

       1. 有多个块时，头部没有停留效果。
       2. 多个块时，section之间默认是有间距的。(不满意，可以通过设置headerView和footerView来设置)
       3. sectionHeaderHeight/sectionFooterHeight这2个属性只在Grouped类型，且未实现代理方法tableView:heightForHeaderInSection: 时有效，在plain风格下设置无效。故在使用UITableView过程中尽量使用代理方法设置sectionHeader和sectionFooter的高度。

链接：https://www.jianshu.com/p/764ed5aa46cf
