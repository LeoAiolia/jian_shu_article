创作灵感：
   工作中经常会用到一些需要和朋友分享的内容，如邀请码了。虽然功能很简单，但是这些东西不需要和数据进行交互，可以直接将其封装成一个控件，有需要的时候可以直接调用岂不痛快。提高代码的可重复性。

我所封装的这个控件的思路主要是继承UIlabel，通过长按或者双击的手势，来达到触发的效果，然后通过富文本，给文字加一个底色，这样就可以知道自己已经选中了文字，以此达到目的，废话不多说，直接上代码。

    #import <UIKit/UIKit.h>
    @interface CSRCopyLabel : UILabel
    @end
以下是.m文件

    #import "CSRCopyLabel.h"

    @implementation CSRCopyLabel

    -(BOOL)canBecomeFirstResponder
    {
        return YES;
    }
    // 可以响应的方法
    -(BOOL)canPerformAction:(SEL)action withSender:(id)sender
    {
        return (action == @selector(copy:));
    }
    //UILabel默认是不接收事件的，我们需要自己添加touch事件
    -(void)attachTapHandler
    {
       self.userInteractionEnabled = YES;  //用户交互的总开关
        //双击
        UITapGestureRecognizer *touch = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(handleTap:)];
        touch.numberOfTapsRequired = 2;
        [self addGestureRecognizer:touch];
    //长按手势
        UILongPressGestureRecognizer *longPressGesture = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(handleTap:)];
    [self addGestureRecognizer:longPressGesture];
    }

    //绑定事件
    - (id)initWithFrame:(CGRect)frame
    {
        self = [super initWithFrame:frame];
        if (self)
        {
            [self attachTapHandler];
        }
        return self;
    }
    //同上
    -(void)awakeFromNib
    {
        [super awakeFromNib];
        [self attachTapHandler];  
    
    }
    -(void)handleTap:(UIGestureRecognizer*)recognizer
    {
        [self becomeFirstResponder];
        UIMenuItem *copyLink = [[UIMenuItem alloc] initWithTitle:@"复制" action:@selector(copy:)];
        [[UIMenuController sharedMenuController] setMenuItems:[NSArray arrayWithObjects:copyLink, nil]];
        [[UIMenuController sharedMenuController] setTargetRect:self.frame inView:self.superview];
       [[UIMenuController sharedMenuController] setMenuVisible:YES animated: YES];
    
        //加个背景色
        NSMutableAttributedString *str = [[NSMutableAttributedString alloc]initWithString:self.text];
        NSDictionary *dic = @{NSBackgroundColorAttributeName:GreyTwoLevel, };
        [str addAttributes:dic range:NSMakeRange(0, self.text.length)];
        self.attributedText = str;
    
    }
    //针对于响应方法的实现
    -(void)copy:(id)sender
    {
        UIPasteboard *pboard = [UIPasteboard generalPasteboard];
        pboard.string = self.text;
        //取消背景颜色
        [self cancleLabelBackColor];
    }
    //取消label文字的背景颜色
    - (void)cancleLabelBackColor
    {
        NSMutableAttributedString *str = [[NSMutableAttributedString alloc]initWithString:self.text];
        NSDictionary *dic = @{NSBackgroundColorAttributeName:[UIColor whiteColor], };
        [str addAttributes:dic range:NSMakeRange(0, self.text.length)];
        self.attributedText = str;
    }
    + (void)cancleLabelBackColor
    {
        [[self alloc] cancleLabelBackColor];
    }

    @end

具体的用法也很简单，直接导入头文件，就像使用UILabel一样就可以使用，长按复制功能是默认开启的，但有时候可能不需要这个功能，直接将其userInteractionEnabled设置为NO即可。 




