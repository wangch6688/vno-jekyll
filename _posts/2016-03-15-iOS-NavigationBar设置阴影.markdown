###NavigationBar 添加阴影(shadow)---

1.设置阴影的颜色

	self.navgiationController.navigationBar.layer.shadowColor = [UIColor blackColor].CGColor;
2.设置阴影偏移范围

	self.navigationController.navigationBar.layer.shadowOffset = CGSizeMake(0, 10);

3.设置阴影颜色的透明度

	self.navigationController.navigationBar.layer.shadowOpacity = 0.2;

4.设置阴影半径

	self.navigationController.navigationBar.layer.shadowRadius = 16;

5.设置阴影路径

	self.navigationController.navigationBar.layer.shadowPath = [UIBezierPath bezierPathWithRect:self.navigationController.navigationBar.bounds].CGPath;