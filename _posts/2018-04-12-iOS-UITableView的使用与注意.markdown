###tableView的使用与注意---
layout: post
title: Sample Post
date: 2016-02-15 15:32:24.000000000 +09:00
---

关于tableview的基本使用不再赘述，只针对项目中可能出现的一些问题。
1.tableView的分割线处理方法
    开发中会出现，明明已经将分割线隐藏了，但是当选中cell的时候，还是会出现分割线，这里提供几个方法对分割线进行处理。

    {% highlight ruby %}
    //遍历取得UITableViewCellSeparatorView，设置隐藏
    - (void)setSelected:(BOOL)selected animated:(BOOL)animated {
        for(UIView *view in self.subviews){
            if([view isMemberOfClass:NSClassFromString(@"UITableViewCellSeparatorView")])
                view.hidden = YES;
        }
        [super setSelected:selected animated:animated];
    }
    - (void)setHighlighted:(BOOL)highlighted animated:(BOOL)animated {
        for(UIView *view in self.subviews){
        if([view isMemberOfClass:NSClassFromString(@"_UITableViewCellSeparatorView")])
            view.hidden = YES;
        }
        [super setHighlighted:highlighted animated:animated];
    }
    {% endhighlight %}

2. tableview取消选中效果
    1> 直接对选中的cell的背景颜色进行设置

    //tableView中设置想要的cell颜色
    {% highlight ruby %}
    -  (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
        PayVehicleCell * cell = [tableView cellForRowAtIndexPath:indexPath];
        cell.backgroundColor = [UIColor whiteColor];
    }

    //cell中对UITableViewCellSelectedBackground进行移除

    - (void)setEditing:(BOOL)editing animated:(BOOL)animated {
        //重写此方法，作用为当进入编辑模式时候运行customMultipleChioce方法
        [super setEditing:editing animated:animated];
        if (editing) {
            [self customMultipleChioce];
        }
    }

    -(void)layoutSubviews {
        //重写此方法，作用为当cell重新绘制的时候运行customMultipleChioce方法
        [super layoutSubviews];
        [self customMultipleChioce];
    }

    //对于一些tableView自身控件出现的视图问题，可以先打开层级，查清视图类别，在视图中进行遍历，再自定义处理
    -(void)customMultipleChioce {
    for (UIControl *control in self.subviews) {
            if ([control isMemberOfClass:NSClassFromString(@"UITableViewCellSelectedBackground")]) {
                [control removeFromSuperview];
            }
        }
    }
    {% endhighlight %}