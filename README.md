# iOSTips
iOS开发中常用到的小技巧

### 获取 UITexView 高度

```Objective-C
- (CGSize)getStringRectInTextView:(NSString *)string InTextView:(UITextView *)textView {
    //实际textView显示时我们设定的宽
    UITextView* localTextView = textView;
    CGFloat contentWidth = CGRectGetWidth(localTextView.frame);
    //但事实上内容需要除去显示的边框值
    CGFloat broadWith    = (localTextView.contentInset.left + localTextView.contentInset.right
                            + localTextView.textContainerInset.left
                            + localTextView.textContainerInset.right
                            + localTextView.textContainer.lineFragmentPadding/*左边距*/
                            + localTextView.textContainer.lineFragmentPadding/*右边距*/);
    
    CGFloat broadHeight  = (localTextView.contentInset.top
                            + localTextView.contentInset.bottom
                            + localTextView.textContainerInset.top
                            + localTextView.textContainerInset.bottom);
    //由于求的是普通字符串产生的Rect来适应textView的宽
    contentWidth -= broadWith;
    
    CGSize InSize = CGSizeMake(contentWidth, MAXFLOAT);
    
    NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc]init];
    paragraphStyle.lineBreakMode = localTextView.textContainer.lineBreakMode;
    NSDictionary *dic = @{NSFontAttributeName:localTextView.font, NSParagraphStyleAttributeName:[paragraphStyle copy]};
    
    CGSize calculatedSize =  [string boundingRectWithSize:InSize options:NSStringDrawingUsesLineFragmentOrigin | NSStringDrawingUsesFontLeading attributes:dic context:nil].size;
    
    CGSize adjustedSize = CGSizeMake(ceilf(calculatedSize.width),calculatedSize.height + broadHeight);
    return adjustedSize;
}
```
