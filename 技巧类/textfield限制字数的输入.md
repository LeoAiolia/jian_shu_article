当截取限制的字符最后一个是表情时，如果使用下面的方法

    contentString = [contentString substringToIndex:limitNum];

则会将表情截半，保存会崩溃，可以使用下面的方法来替换

    + (void)textField:(UITextField*)textField limitWordNum:(NSInteger)limit {
      if (textField.markedTextRange == nil) {
        NSString* contentString = textField.text;
        if (contentString.length > limit) {
          // 防止将emoj表情截取一半导致崩溃。 因emoji长度不一定是2位也有可能是4位
          NSRange rangeIndex =
          [contentString rangeOfComposedCharacterSequenceAtIndex:limit];
          contentString = [contentString substringToIndex:(rangeIndex.location)];
        }
        textField.text = contentString;
      }
    }

这种做法将会导致最后只有一个字时，如果输入表情，则无法输入进去。
